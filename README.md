# Projeto Final - Python - CODERHOUSE
Construção de um pipeline de dados, que consiste em várias etapas: extração, tratamentos, alertas, deploy e documentação.  O objetivo geral do projeto é permitir que os dados brutos sejam coletados, processados, analisados e disponibilizados para uso em diferentes áreas de negócios.

Grupo: Bianca Carvalho, Gabriel Cruz, Isabella Gimenez, Leticia Prado

# Documentação do Projeto Final

## Visão Geral
Este código consiste em uma aplicação Python que utiliza a API [restcountries](https://restcountries.com/) para obter informações sobre países, como região, nome, capital, moeda, população, área, latitude e longitude. 

## Bibliotecas Utilizadas
- **pandas**: Utilizada para manipulação e análise de dados, especialmente para ler os dados JSON obtidos da API e gravá-los em arquivos CSV.
- **requests**: Usada para fazer solicitações HTTP para a API restcountries.
- **csv**: Biblioteca padrão do Python para manipulação de arquivos CSV.
- **datetime**: Utilizada para manipulação de datas e horários.
- **plyer**: Utilizada para enviar notificações.
- **os**: Biblioteca padrão do Python para funcionalidades relacionadas ao sistema operacional.
- **sqlite3**: Banco de dados para armazenamento de informações
- ***ast***: Módulo na biblioteca que ajuda os aplicativos Python a processar árvores da gramática de sintaxe abstrata do Python

## Passos do Código

### 1. Importação de Bibliotecas
```python
import pandas as pd
import requests
import csv
from datetime import datetime
from plyer import notification
import os
import sqlite3
import ast
```

### 2. Solicitação e Processamento dos Dados da API
O código faz solicitações à API restcountries para obter informações sobre países em diferentes aspectos, como região, nome, capital, moeda, população, área, latitude e longitude.

### 3. Manipulação dos Dados e Gravação em Arquivos CSV
Após obter os dados da API, o código converte os resultados JSON em DataFrames pandas e os grava em arquivos CSV.

### 4. Função de Notificação de Alerta
O código inclui uma função `alerta()` que verifica se os arquivos CSV foram salvos com sucesso na pasta de destino. Dependendo do resultado, uma notificação de alerta é enviada.

### 5. Chamada da Função de Notificação
A função de notificação é chamada no final do script para enviar a notificação de alerta.

## Execução
Para executar este código, basta executar cada célula de código sequencialmente em um ambiente Python com as bibliotecas necessárias instaladas. Certifique-se de substituir o caminho da pasta de destino na função `alerta()` com o caminho correto do seu sistema.

## Observações
- Este código depende da disponibilidade e estabilidade da API restcountries.
- Certifique-se de que o ambiente Python em que está executando o código tenha acesso à internet para fazer solicitações à API.
- As notificações só serão enviadas se a biblioteca plyer estiver instalada e configurada corretamente no ambiente Python.

### 6. Importação de Bibliotecas e Leitura dos Arquivos CSV
O código importa a biblioteca `sqlite3` para trabalhar com o SQLite e a biblioteca `pandas` para manipulação de dados. Em seguida, ele lê os arquivos CSV gerados na etapa anterior para DataFrames pandas.

```python
import sqlite3
import pandas as pd

# Leitura dos arquivos CSV

### 7. Tratamento e Renomeação das Colunas nos DataFrames
Os DataFrames são tratados para garantir que os dados estejam formatados corretamente. Em seguida, as colunas são renomeadas para facilitar a compreensão dos dados.

# Renomeação das colunas nos DataFrames


### 8. Análise dos Tipos de Variáveis
A função `info()` do pandas é utilizada para analisar os tipos de variáveis em cada DataFrame, bem como a contagem de valores não nulos em cada coluna.

# Análise dos tipos de variáveis em cada DataFrame


### 9. Análise dos Valores Ausentes
A função `isna().sum()` do pandas é utilizada para analisar a quantidade de valores ausentes em cada coluna de cada DataFrame.

# Análise dos valores ausentes em cada DataFrame

### 10. Criação do Banco de Dados sqlite3 e Inserção dos Dados
Um banco de dados sqlite3 é criado utilizando a biblioteca `sqlite3`. Em seguida, uma tabela chamada "paises" é criada para armazenar os países com suas respectivas informações. Os dados são inseridos na tabela utilizando um loop sobre as linhas do DataFrame.

# Criação do banco de dados sqlite3

### 11. Importação de Bibliotecas e Leitura dos Arquivos CSV
Mais uma vez é importado as bibliotecas e feita a leitura dos arquivos csv para que estes mesmos possam ser tratados na parte final do projeto

### 12. Tratamento das bases
O tratamento de bases é feito com extração de informações, criação de novas colunas e depois, exclusão de colunas específicas. Algumas linhas são removidas também, as que possuem valor nulo. O começo do tratamento das bases pode ser visto abaixo.

#BASE REGIAO

# Aplicar a função para extrair o nome do país, capital e região e criar novas colunas

#exclusao colunas "Moeda" e "ccn3"

### 13. Salvando as bases tratadas em CSV e analisando os tipos de variáveis. 
O próximo passo é salvar todas as bases tratadas e novos arquivos csv, além disso, imprime-se os tipos de variáveis em cada dataframe. Posteriormente, a verficiação será feita sem base com dados nulos

### 14. Junção das tabelas
Este paço tem como intenção juntar as 3 tabelas em uma só, criando um único documento com todas as colunas e linhas relevantes.

# primeiro join (regiao e moda) usando a chave "cod"

# segundo join (merged_df com area) usando a chave "cod"

# Exibir o DataFrame resultante

### 15. Criando a tabela no banco de dados 

Aqui é onde iniciamos a criação da tabela dentro do banco de dados. 

Primeiro faremos a conexão com o banco e informamos que, se necessário, será feita uma substituição da tabela existente para a nova, com as informações tratadas.

### 16. Seleção e visualização das tabelas

Por fim, como último passo, selecionamos a tabela criada e imprimimos ela inteira para averiguar se todas as informações foram corretas, e então, podemos fazer a visualização da tabela montada com todas as colunas e linhas organizadas

## ✒️ Autores

Grupo: Bianca Carvalho, Gabriel Cruz, Isabella Gimenez, Leticia Prado

## 📄 Licença

Este projeto está sob a licença (sua licença) - veja o arquivo [LICENSE.md](https://github.com/biaacarvalhoo27/projetofinal_python/blob/main/LICENSE) para detalhes.