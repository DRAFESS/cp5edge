# cp5edge
README - Resultados e Processo com Postman e Google Colab
Este guia descreve os resultados obtidos com os códigos e o processo de obtenção e visualização de dados de luminosidade usando o Postman e o Google Colab.

Resultados
Criamos um código Python que usa a biblioteca requests para fazer uma solicitação GET a uma URL externa que fornece dados de luminosidade.
Processamos os dados JSON obtidos para extrair informações de luminosidade e timestamps.
Utilizamos a biblioteca matplotlib para criar um gráfico da luminosidade em função do tempo.
Limitamos o gráfico a um intervalo de 15 minutos a partir do tempo mais recente para tornar a visualização mais útil.
Os rótulos de tempo foram ajustados para melhor legibilidade.
Processo com o Postman e Google Colab
Passo 1: Obtenção de Dados com o Postman
Utilizamos o Postman para criar uma solicitação GET à URL que fornece os dados de luminosidade.
Enviámos a solicitação e verificamos se os dados JSON foram retornados corretamente.
Copiamos o código de solicitação HTTP gerado pelo Postman para usar no Google Colab.
Passo 2: Criação de um Gráfico no Google Colab
Abrimos o Google Colab e criamos um novo notebook.
Colamos o código de solicitação HTTP do Postman em uma célula no notebook.
Importamos as bibliotecas necessárias, incluindo requests, json, matplotlib, datetime e timedelta.
Criamos uma célula de código e colamos o código Python fornecido pelo Chat-GPT. Executamos a célula de código para obter os dados da URL e criar o gráfico de luminosidade em função do tempo.
Ajustamos o código conforme necessário para personalizar o gráfico de acordo com nossas preferências, como rótulos de eixo, títulos e cores.
Salvamos o notebook no Google Colab ou fizemos o download para tê-lo localmente.
Código utilizado:
import requests
import json
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

Defina a URL da qual você deseja obter as leituras de luminosidade
url = "http://46.17.108.113:8666/STH/v1/contextEntities/type/Lamp/id/urn:ngsi-ld:Lamp:001/attributes/luminosity?hOffset=1&lastN=90"

Defina cabeçalhos
headers = {
    'fiware-service': 'smart',
    'fiware-servicepath': '/'
}

Faça a solicitação HTTP
response = requests.get(url, headers=headers)

Verifique se a solicitação foi bem-sucedida
if response.status_code == 200:
    data = json.loads(response.text)['contextResponses'][0]['contextElement']['attributes'][0]['values']

Extrair as leituras de luminosidade e timestamps
    luminosity_values = []
    timestamps = []

    for entry in data:
        timestamp = datetime.strptime(entry['recvTime'], '%Y-%m-%dT%H:%M:%S.%fZ')
        luminosity = entry['attrValue']
        timestamps.append(timestamp)
        luminosity_values.append(luminosity)
Ordene os dados com base no tempo
    sorted_data = sorted(zip(timestamps, luminosity_values), key=lambda x: x[0])
    timestamps, luminosity_values = zip(*sorted_data)

Crie o gráfico
    plt.figure(figsize=(10, 6))
    plt.plot(timestamps, luminosity_values, marker='o', linestyle='-')
    plt.xlabel('Tempo')
    plt.ylabel('Luminosidade')
    plt.title('Luminosidade em função do tempo')
    plt.grid(True)
    plt.tight_layout()

Limite o gráfico a 15 minutos
    plt.xlim([timestamps[0], timestamps[0] + timedelta(minutes=15)])

Rotacione os rótulos de tempo para melhor legibilidade
    plt.xticks(rotation=45)

Mostrar o gráfico
    plt.show()
else:
    print("Falha na solicitação HTTP:", response.status_code)
