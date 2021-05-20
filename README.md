import pandas as pd
from twilio.rest import Client

# Your Account SID from twilio.com/console
account_sid = "AC4f209123cb1bd8ece87d1922b3c37e95"
# Your Auth Token from twilio.com/console
auth_token  = "1f060569e4c612d19e436c85d71094ef"
client = Client(account_sid, auth_token)

# Abrir os 6 arquivos em Excel
lista_meses = ['Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho']
for mes in lista_meses:
    tabela_vendas = pd.read_excel(f'{mes}.xlsx')
    if (tabela_vendas['Vendas'] > 55000).any():
        vendedor = tabela_vendas.loc[tabela_vendas['Vendas'] > 55000, 'Vendedor'].values[0]
        vendas = tabela_vendas.loc[tabela_vendas['Vendas'] > 55000, 'Vendas'].values[0]
        print(f'No mês {mes} alguém bateu a meta. Vendedor: {vendedor}, Vendas: {vendas}')
        message = client.messages.create(
            to="xxxxxxxxx",
            from_="xxxxxxxxx",
            body=f'No mês {mes} alguém bateu a meta. Vendedor: {vendedor}, Vendas: {vendas}')
print(message.sid)
