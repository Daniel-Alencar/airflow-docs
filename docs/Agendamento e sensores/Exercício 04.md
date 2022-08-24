# 4 - Exercício

## Aprimoramento de média dos alunos

Como deve ter percebido, este será um exercício para aprimoramento da solução do exercício anterior. Você deve primeiramente verificar se existe uma arquivo CSV no diretório antes de efetivamente fazer a média dos alunos (com FileSensor).

Regule esta verificação para ser feita de 5 em 5 segundos (caso o arquivo não exista). Com o timeout de 5 minutos.

Caso o arquivo notas.csv exista no diretório, prossiga normalmente:

- Recolha as notas dos alunos.
- Calcule a média de cada aluno.
- Preencha um arquivo CSV com as médias dos alunos.

## Resolução

```python
from airflow import DAG
import datetime as dt
import csv

from airflow.operators.python import PythonOperator
from airflow.sensors.filesystem import FileSensor
from airflow.operators.bash import BashOperator

def calcular_medias():
  medias = []
  with open('notas.csv') as ficheiro:
    reader = csv.reader(ficheiro)
    for linha in reader:
      soma = 0
      for coluna in linha:
        soma += int(coluna)
      medias.append(soma / 3)
  
  return medias

def fazer_CSV(task_instance):
  medias = task_instance.xcom_pull(task_ids = 'Calcular_medias')
  print(medias)
  with open('./medias.csv', 'w') as csvfile:
    for media in medias:
      csv.writer(csvfile, delimiter=',').writerow([media])

with DAG('exercício4', start_date = dt.datetime(2022,8,16),
         schedule_interval = '0 9 * * *', catchup = False) as dag:

  Wait_for_file = FileSensor(
    task_id = "wait_for_file",
    filepath = "/mnt/31f07ee3-e7ad-49de-a4d4-9295fa9b72ec/Arquivos/MEGA/Documents/Projetos pessoais/IC/airflow-test/notas.csv",
    poke_interval = 5,
    timeout = 5 * 60,
  )
  
  Calcular_medias = PythonOperator(
    task_id = 'Calcular_medias',
    python_callable = calcular_medias
  )

  Fazer_CSV = PythonOperator(
    task_id = 'Fazer_CSV',
    python_callable = fazer_CSV
  )

  Message = BashOperator(
    task_id = 'Message',
    bash_command = 'echo "As médias foram calculadas com sucesso!"'
  )

  Wait_for_file >> Calcular_medias >> Fazer_CSV >> Message
```