# lab05MSAzureAI
Laboratório final do curso Microsofto Azure AI fundamentals

A criação de conteúdo com o Copilot é algo que nos dá um universo de possibilidades para a criação de qualquer aplicação. 
Vejo que essa ferramenta eleva o nível da programação, desde que usada com responsabilidade e ética. Abaixo segue um código em Python que utiliza o Copilot para apresentar a previsão do tempo para uma equipe de obras de construção civil. 

Primeiro, você precisará instalar a biblioteca requests para fazer requisições HTTP e a biblioteca smtplib para enviar e-mails. Você pode instalá-las usando os seguintes comandos:

pip install requests
pip install secure-smtplib

Aqui está o código de exemplo:

Python

import requests
import smtplib
from email.mime.text import MIMEText

def obter_dados_meteorologicos():
    cidade = "Anápolis"
    estado = "Goiás"
    url = f"https://www.cptec.inpe.br/previsao-tempo/go/{cidade.lower()}"

    try:
        response = requests.get(url)
        response.raise_for_status()
        dados = response.json()

        # Exemplo: extraindo a temperatura e previsão para hoje
        temperatura_maxima = dados["temperatura"]["maxima"]
        temperatura_minima = dados["temperatura"]["minima"]
        previsao_manha = dados["previsao"]["manha"]
        previsao_tarde = dados["previsao"]["tarde"]
        previsao_noite = dados["previsao"]["noite"]

        mensagem = f"Previsão para {cidade}, {estado}:\n"
        mensagem += f"Temperatura máxima: {temperatura_maxima}°C\n"
        mensagem += f"Temperatura mínima: {temperatura_minima}°C\n"
        mensagem += f"Previsão para a manhã: {previsao_manha}\n"
        mensagem += f"Previsão para a tarde: {previsao_tarde}\n"
        mensagem += f"Previsão para a noite: {previsao_noite}"

        enviar_email(mensagem)

    except requests.RequestException as e:
        print(f"Erro ao obter dados meteorológicos: {e}")

def enviar_email(mensagem):
    remetente = "seu_email@gmail.com"
    senha = "sua_senha"
    destinatarios = ["funcionario1@email.com", "funcionario2@email.com"]

    msg = MIMEText(mensagem)
    msg["Subject"] = "Previsão Meteorológica para a Obra"
    msg["From"] = remetente
    msg["To"] = ", ".join(destinatarios)

    try:
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login(remetente, senha)
        server.sendmail(remetente, destinatarios, msg.as_string())
        server.quit()
        print("E-mail enviado com sucesso!")

    except smtplib.SMTPException as e:
        print(f"Erro ao enviar e-mail: {e}")

if __name__ == "__main__":
    obter_dados_meteorologicos()
