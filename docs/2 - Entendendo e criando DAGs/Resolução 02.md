# Resolução 02

Uma possível resolução para o Exercício 02:

```python
from airflow import DAG
import datetime as dt

from airflow.operators.python import PythonOperator

import requests
import json

def captura_conta_dados():
    url = "https://api.adviceslip.com/advice"
    response = requests.get(url)
    print(response)

    todos = json.loads(response.content)

    print("\n\n")
    print(todos['slip']['advice'])
    print("\n\n")

with DAG('exercício2', start_date = dt.datetime(2022,8,16),
schedule_interval = '30 6 * * *', catchup = False) as dag:
  
  Captura_dados = PythonOperator(
    task_id = 'Capturar_dados',
    python_callable = captura_conta_dados
  )

  Captura_dados
```