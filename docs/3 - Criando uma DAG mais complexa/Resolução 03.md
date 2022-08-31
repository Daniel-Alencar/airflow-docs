# Resolução 03

Uma possível resolução para o Exercício 03:

```python
from airflow import DAG
import datetime as dt
import csv

from airflow.operators.python import PythonOperator

def calcular_medias():
  medias = []
  with open('notas.csv') as ficheiro:
    reader = csv.reader(ficheiro)
    for linha in reader:
      soma = 0
      for coluna in linha:
        soma += int(coluna)
      medias.append(soma / 3)
  
  print(medias)
  with open('./medias.csv', 'w') as csvfile:
    for media in medias:
      csv.writer(csvfile, delimiter=',').writerow([media])

with DAG('exercício3', start_date = dt.datetime(2022,8,16),
         schedule_interval = '0 9 * * *', catchup = False) as dag:
  
  Calcular_medias = PythonOperator(
    task_id = 'Calcular_medias',
    python_callable = calcular_medias
  )

  Calcular_medias
```

<aside>
⚠️ Para se desafiar um pouco, tente resolver o problema utilizando mais de uma Task!

</aside>