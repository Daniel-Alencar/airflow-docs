# 2 - Entendendo e Criando DAGs

## Primeiramente, o que são DAGs?

Em primeiro lugar, devemos saber que DAG não é um conceito novo. Na verdade, as DAGs são conhecidas desde o desenvolvimento das **[Teorias de grafos](https://es.wikipedia.org/wiki/Teor%C3%ADa_de_grafos)** em matemática e que mais tarde foram usadas na computação devido à sua enorme utilidade neste campo. Quando falamos sobre DAG, estamos a falar sobre grafos com duas propriedades muito interessantes: **são dirigidos e acíclicos**.

Em primeiro lugar, **um grafo é direcionado, quando todos os nós (ou vértices) que fazem parte do grafo são conectados por arestas que indicam uma direção bem definida**.

Segundo, **falamos de um grafo acíclico, quando estamos diante de um grafo onde não há ciclos de deslocamento para o mesmo.** Por outras palavras, é impossível ir de um vértice do grafo, passar pelos demais vértices e terminar no vértice onde a jornada começou.

Assim, as DAGs têm certas propriedades que são vitais para que funcionem bem:

1. **Têm um ponto de partida (origem) e um ponto de chegada ou final.** Ao serem direcionados, isso garante que a nossa rota vai sempre de um ponto de origem a um ponto final, e não podemos retornar por essa rota. Se a construção dessa estrutura for aplicada consecutivamente, estaremos a criar um histórico incremental dentro da DAG, como acontece em blockchain.
2. **Modificar uma relação entre vértices reescreve toda a DAG, porque a sua estrutura e peso foram alterados.** Isto é equivalente a se modificarmos um bloco na blockchain, o resultado será uma blockchain diferente daquele ponto em diante.
3. **São paralelizáveis.** Uma DAG pode ter geração paralela e caminhos de valores diferentes entre vértices diferentes. Isto otimiza a sua geração e a capacidade de verificar a relação entre os vértices e as informações que eles podem conter.
4. **São redutíveis.** Uma propriedade exclusiva das DAGs é que sua estrutura pode ser reduzida a um ponto ideal onde seu caminho atende a todos os relacionamentos especificados nele sem qualquer perda. Basicamente, significa que é possível reduzir as relações dos vértices (ou blocos) a um ponto mínimo onde tal redução não afete a capacidade de verificar a informação de qualquer vértice a qualquer momento. Isso é especialmente útil.

## Entendendo as DAGs no Airflow

Abrindo o software Airflow, temos uma página semelhante a esta.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled.png)

Cada um destes elementos da lista representa uma DAG diferente.

Como exemplo, clicaremos na DAG ‘tutorial’ e na aba Graph View (para termos uma visualização em gráfico da nossa DAG).

Então teremos a seguinte tela.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%201.png)

Perceba que temos 3 blocos em nossa DAG tutorial: **print_date, sleep e templated**.

Cada um destes blocos representa uma *task*. Task é alguma tarefa fundamental a ser executada para que o processo todo seja concluído. É uma unidade para construção do todo.

Pelo gráfico também podemos visualizar a ordem de execução das tasks. Temos a execução da task **print_date** e em seguida as execuções de **sleep** e **templated**. Se executássemos a DAG, veríamos esta sequência de execução através do código de cores mostrado acima.

As principais cores que devemos nos atentar na execução de cada task são as seguintes:

- Cinza: Tarefa está na fila de execução.
- Verde claro: Tarefa está executando.
- Verde escuro: Tarefa completou a sua execução com sucesso.
- Vermelho: A execução da tarefa falhou em algum ponto.

Além disso, existem diferentes tipos de Tasks. Estas tasks podem ser uma execução de uma função python, podem ser a execução de algum comando no terminal e outros. Veremos estes detalhes mais adiante.

## Execução das DAGs

Para executar uma DAG, podemos clicar no botão na parte superior esquerda do programa.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%202.png)

Para acompanharmos mais detalhadamente a execução de alguma task, podemos clicar na Task que queremos analisar.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%203.png)

Em seguida, clicar no botão ‘Log’.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%204.png)

E então, teremos uma visualização mais detalhada do que está acontecendo. Nesta parte, podemos acompanhar os erros que alguma task pode ter disparado também.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%205.png)

Beleza. Após aprendermos o que são as DAGs e como elas podem ser visualizadas e manipuladas no Airflow, agora, podemos criar a nossa primeira DAG.

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

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%206.png)

Após clicar em nossa DAG e executá-la, podemos clicar no ‘Log’ da nossa task e assim visualizar de fato o que foi executado. Perceba a saída `Hello world` na tela.

![Untitled](2%20-%20Entendendo%20e%20Criando%20DAGs/Untitled%207.png)

No próximo artigo iremos criar uma DAG um pouco mais complexa.

## Referências

[O que é um DAG?](https://academy.bit2me.com/pt/que-es-un-dag/)

[Tutorial Airflow para engenharia de dados](https://www.youtube.com/watch?v=4DGRqMoyrPk)