# Definindo problema

Vista a criação de uma DAG simples. Podemos criar uma DAG um pouco mais complexa para visualizarmos a sequência de tasks a serem executadas.

Comecemos de onde paramos. Copie o código da DAG que foi criada no artigo anterior para um outro arquivo que irei chamar de `new_teste`. Este arquivo deve estar dentro da pasta ‘dags’ (pasta que foi criada anteriormente para armazenar DAGs criadas). Assim, temos o seguinte código em nosso arquivo.

```python
from airflow import DAG
from datetime import datetime

from airflow.operators.python import PythonOperator

def hello():
  print("Hello world")

with DAG('teste', start_date = datetime(2022,5,23),
         schedule_interval = '30 * * * *', catchup = False) as dag:
  
  helloWorld = PythonOperator(
    task_id = 'Hello_World',
    python_callable = hello
  )

  helloWorld
```

Porém, desta vez, deixaremos somente a parte da DAG criada (mudando apenas o parâmetro ‘name’ para o nome ‘new_teste’). Assim, ficamos com isto em nosso código:

```python
from airflow import DAG
from datetime import datetime

with DAG('new_teste', start_date = datetime(2022,5,23),
         schedule_interval = '30 * * * *', catchup = False) as dag:
```

## Definindo um problema

Digamos que seja muito importante eu saber se a quantidade de repositórios no meu Github é maior do que algum determinado valor, digamos, 30. Logo, seria interessante termos uma DAG que cuide disso para mim.

Perceba que podemos dividir o problema em mini-problemas que seriam as nossas *tasks*.

Uma possível divisão de tasks seria:

- Task 1: Busca a quantidade de repositórios que eu tenho no Github.
- Task 2: Analisa esta quantidade.
- Task 3: Faz alguma coisa (caso a quantidade seja maior).
- Task 4: Faz alguma outra coisa (caso a quantidade seja menor).

Primeiro executamos a Task 1, logo em seguida a Task 2 (pois depende da primeira task). E com base na análise do valor na segunda Task, devo executar ou a Task 3 ou Task 4.

Na próxima aula iremos construir a solução para o problema apresentado.