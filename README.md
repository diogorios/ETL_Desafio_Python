# ETL_Desafio_Python
Baseado nos dados de um planilha estou transformando os dados, enviando uma mensagem por e-mail aos clientes com mais de 15 anos de conta bancária.


# Passo 01
!pip install pandas

# Passo 02
import pandas as pd

df = pd.read_csv('/content/sample_data/Customer.csv')
user_ids = df['UserID'].tolist()
user_Names = df['Name'].tolist()
user_Customer_Since = df['Customer_Since'].tolist()
user_Balances = df['Balance'].tolist()
user_Messages = df['Message'].tolist()
print(user_ids)
print(user_Names)
print(user_Customer_Since)
print(user_Balances)
print(user_Messages)

# Passo 03
import pandas as pd
from datetime import datetime

df['Customer_Since'] = pd.to_datetime(df['Customer_Since'], format='%d/%m/%Y')
data_de_referencia = datetime(2023, 9, 3)  
clientes_ha_mais_18_anos = []

for index, row in df.iterrows():
    qtd_Anos = (data_de_referencia - row['Customer_Since']).days // 365 
    if qtd_Anos >= 18:
        row['Send_Email'] = 'TRUE'
        row['Message'] = 'Obrigado pela parceria de 15 anos'
    else:
        row['Send_Email'] = 'FALSE'

    clientes_ha_mais_18_anos.append(row)

df_clientes = pd.DataFrame(clientes_ha_mais_18_anos)

print(df_clientes)

# Salva o DataFrame atualizado em um arquivo CSV
df_clientes.to_csv('/content/sample_data/Clientes_Atualizados.csv', index=False)

# Gera um link para download do arquivo
from google.colab import files
files.download('/content/sample_data/Clientes_Atualizados.csv')

## Tabela ORIGINAL ##
UserID,Name,Customer_Since,Balance,Send_Email,Message
1,Diogo,10/05/2002,850,
2,Rios,20/06/2005,700,
3,Tiago,20/06/2007,612,
4,Maria,08/01/2023,311,
5,Carlos,20/04/2007,224,


## Tabela com ETL ##
UserID,Name,Customer_Since,Balance,Send_Email,Message
1,Diogo,2002-05-10,850,TRUE,Obrigado pela parceria de 15 anos
2,Rios,2005-06-20,700,TRUE,Obrigado pela parceria de 15 anos
3,Tiago,2007-06-20,612,FALSE,
4,Maria,2023-01-08,311,FALSE,
5,Carlos,2007-04-20,224,FALSE,
