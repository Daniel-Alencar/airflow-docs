# Preparação do Airflow

Com o nosso ambiente preparado, estamos prontos para a instalação efetiva do Airflow.

<aside>
⚠️ Ao copiar os comandos, talvez haja alguma quebra de linha no texto. Isto acontece no material em PDF.

</aside>

## Criação do projeto

Define o diretório padrão do Airflow:

```jsx
export AIRFLOW_HOME=~/airflow
```

Determine a versão do Airflow que instalaremos:

```jsx
AIRFLOW_VERSION=2.0.1
```

Determine a versão do Python utilizada pela sua máquina:

```jsx
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
```

Define uma variável que é um link que contém todas as dependências do Airflow de acordo com a versão do airflow escolhida e a versão do python3 da sua máquina:

```jsx
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
```

Instalação final do Airflow:

```jsx
pip3 install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

## Configurações do projeto

Inicializa o banco de dados padrão:

```jsx
airflow db init
```

Se por acaso, for obtido o erro `AttributeError: module 'wtforms.fields' has no attribute 'TextField'`, podemos instalar a biblioteca `flask-ldap-login` em sua versão 0.3.0.

Isto resolverá o problema. Ao que parece, esta biblioteca usa uma versão mais antiga da biblioteca wtforms que ainda continha o atributo ‘TextField’. Pode ser que simplesmente instalando a biblioteca wtforms nessa versão especifica resolva o problema. A versão 0.8.2 da wtforms ainda continha o atributo ‘TextField’.

## Correção do erro

Execute os seguintes comandos no seu terminal (dentro da pasta do airflow) caso tenha enfrentado o erro dito acima.

```jsx
pip3 install flask-ldap-login==0.3.0
```

```jsx
deactivate
```

```bash
source NOME/bin/activate
```

Dentro da pasta definida por `AIRFLOW_HOME`, apague: 

- logs
- airflow.cfg
- airflow.db
- webserver_config.py

para que novos arquivos venham ser criados a partir do comando abaixo:

```jsx
airflow db init
```

Com isso, o problema não deve aparecer mais.

## Criação de usuário

Crie um usuário admin substituindo os parâmetros USERNAME, FIRSTNAME, LASTNAME e EMAIL.

```jsx
airflow users create --username USERNAME --firstname FIRSTNAME --lastname LASTNAME --role Admin --email EMAIL
```

Após isto, defina a senha de autenticação que vai ser pedida no terminal.

## Inicializando interface

Aqui utilizaremos a porta 8080, porém, caso não seja possível utilizar esta porta, pode-se alterar o valor também.

Este é o comando que inicializa o Airflow.

```jsx
airflow webserver -p 8080
```

Deixe o comando acima rodando e abra outro terminal na mesma pasta ‘airflow-test’.

Neste terminal, habilite também o ambiente virtual criado anteriormente com o seguinte comando:

```jsx
source NOME/bin/activate
```

Após isto, digite o comando:

```jsx
airflow scheduler
```

## Utilizando a interface

Inicializando [http://localhost:8080/](http://localhost:8080/) no navegador, teremos a seguinte tela:

![Untitled](Preparac%CC%A7a%CC%83o%20do%20Airflow/Untitled.png)

Entre com o seu usuário e senha.

E assim, a próxima tela de exibição será a seguinte.

![Untitled](Preparac%CC%A7a%CC%83o%20do%20Airflow/Untitled%201.png)

E pronto, temos o Airflow totalmente configurado para ser executado em nosso sistema.

Como exemplo, podemos executar um destes arquivos da lista.

Procure o exemplo ‘tutorial’ e clique nele:

![Untitled](Preparac%CC%A7a%CC%83o%20do%20Airflow/Untitled%202.png)

Após isto, clique no botão superior esquerdo para executar.

![Untitled](Preparac%CC%A7a%CC%83o%20do%20Airflow/Untitled%203.png)

E pronto, já estamos executando uma DAG em nosso airflow. No próximo artigo entenderemos melhor a ideia de DAGs.

## Terminando a execução

Para fecharmos o airflow em nosso sistema, devemos parar as execuções do `airflow webserver -p 8080` e `airflow scheduler` em nossos terminais. Para fazer isto, podemos clicar CTRL + C nos dois terminais. E assim, o airflow fecha a sua execução.

## Adendo (talvez não precise ser feito)

Na próxima vez que for rodar o airflow com o comando `airflow webserver -p 8080` e `airflow scheduler` pode ser que ocorra o seguinte erro:

`sqlite3.OperationalError: no such table: dag`

Isso aconteceu porque as tabelas ab_* não foram criadas no `airflow db init`. Todas essas tabelas são para controle de acesso baseado em função – RBAC.

Para resolver este problema, edite o arquivo `airflow.cfg` colocando/modificando a seguinte linha de código.

```python
[webserver]
rbac = True
```

Digite novamente:

```python
airflow db init
```

E após isto, recrie o usuário:

```jsx
airflow users create --username USERNAME --firstname FIRSTNAME --lastname LASTNAME --role Admin --email EMAIL
```

E rode o airflow novamente:

```python
airflow webserver -p 8080
```

```jsx
airflow scheduler
```

Com isto, você já está apto a utilizar o airflow sempre que quiser.