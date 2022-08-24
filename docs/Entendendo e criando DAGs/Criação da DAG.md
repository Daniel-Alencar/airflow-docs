# Criação da DAG

## Criação de DAG

Primeiramente, devemos criar uma pasta chamada ‘dags’ dentro da pasta `AIRFLOW_HOME`, definida anteriormente no manual de instalação. Como no nosso manual de instalação utilizamos `AIRFLOW_HOME=~/airflow`, nossa pasta deverá ser criada dentro da pasta airflow.

```bash
mkdir ~/airflow/dags
```

Após isto, criamos um arquivo python neste diretório com qualquer nome, por exemplo, chamarei o arquivo de `teste.py`.

Utilize o editor de texto de sua preferência, no meu caso, usarei o VS code.

Em seguida, coloque o seguinte texto no arquivo:

```python
from airflow import DAG
from datetime import datetime

with DAG('teste', start_date = datetime(2022,5,23),
				schedule_interval = '30 * * * *', catchup = False) as dag:
```

Nas duas primeiras linhas temos as importações necessárias. Estes módulos (airflow e datetime) já vem com o próprio airflow, de forma que ainda não precisamos instalar nada.

A terceira linha é a criação da DAG em si. Como podemos perceber, precisamos definir alguns parâmetros para criação da DAG. Em ordem, eles são:

1. name: define o nome que aparecerá na lista de DAGs do Airflow. No nosso caso, ‘teste’.
2. start_date: define o início da execução da DAG. No nosso caso, 23/05/2022.
3. schedule_interval: define de quanto em quanto tempo a DAG deve ser executada. Este parâmetro utiliza o mesmo padrão utilizado no crontab do sistema UNIX.
    
    Em nosso caso, de 30 em 30 minutos a DAG é executada.
    
4. catchup: define se deve executar ou não todas as DAGs que não foram executadas desde o start_date até o período atual. No nosso caso, definimos que não queremos executar.

<aside>
💡 A DAG só é executada automaticamente em: **tempo do start_date** + **tempo do schedule_interval**.

</aside>

### Entendendo Operators

Um Operator é o operador da task que iremos realizar, é o que vai definir o meu tipo de task.

Por exemplo, temos: PythonOperator, BranchPythonOperator, BashOperator etc.

Isto pode ser entendido melhor na criação de tasks.

### Criando uma task

Continuando o código anterior, acrescentamos alguns comandos:

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
```

Perceba que dentro do bloco da DAG criada (variável ‘dag’), definimos uma variável chamada ‘helloWorld’. Esta variável referencia uma task do tipo PythonOperator, o que significa que esta task em específico irá rodar instruções python.

Dentro do PythonOperator devemos definir dois parâmetros:

- task_id: Identificador da task.
- python_callable: Função python que será executada nessa task.

Perceba que a função python ‘hello’ (que referenciamos) está definida mais acima no código. Ela é uma função python normal que printa na tela “Hello World”.

Finalmente, só temos que definir mais uma coisa para podermos executar esta DAG. Devemos informar a ordem de execução das tarefas. E fazemos isto no final.

Como neste exemplo só temos uma tarefa que será executada, devemos apenas informar o nome dela dentro do código. E assim, a DAG fica:

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

<aside>
💡 Perceba que no final temos o nome ‘helloWorld’ dentro do bloco da DAG.

</aside>

## Executando

Para visualizarmos nossa DAG dentro do airflow, basta atualizarmos a tela inicial do programa. Perceba que a DAG ‘teste’ já aparece na lista.

![Untitled](Criac%CC%A7a%CC%83o%20da%20DAG/Untitled.png)

Após clicar em nossa DAG e executá-la, podemos clicar no ‘Log’ da nossa task e assim visualizar de fato o que foi executado. Perceba a saída `Hello world` na tela.

![Untitled](Criac%CC%A7a%CC%83o%20da%20DAG/Untitled%201.png)

No próximo artigo iremos criar uma DAG um pouco mais complexa.