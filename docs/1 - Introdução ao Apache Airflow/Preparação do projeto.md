# Preparação do projeto

Agora que já sabemos um pouco sobre os problemas que o software Apache Airflow consegue resolver, podemos trabalhar diretamente com ele.

Para fazer isto, podemos instalá-lo localmente em nosso computador. Esta e a próxima aula tem a intenção de realizar este propósito.

<aside>
⚠️ A instalação do Airflow foi feita em um Linux Mint 20.2 Cinnamon. Porém, a instalação deve ocorrer normalmente (ou com poucas mudanças) também em outros sistemas baseados no Ubuntu.

</aside>

## Passos iniciais

Antes de tudo, temos que ter instalado o python3 em nosso computador. Logo, execute o seguinte comando no terminal:

```python
sudo apt-get install python3
```

Para verificar isto, podemos colocar o seguinte comando no terminal:

```jsx
python3 --version
```

Após isto, precisamos do 'pip3' instalado (gerenciador de pacotes do python). Para isto, execute:

```bash
sudo apt-get install python3-pip
```

E em seguida, para verificar se deu tudo certo:

```bash
pip3 --version
```

Para o nosso projeto, não iremos trabalhar com o python global do sistema, para isto, precisamos de uma área específica com o python separado do python do nosso sistema. Iremos fazer isto através de uma biblioteca de criação de ambientes virtuais python, uma delas é a ‘virtualenv’.

Para instalá-la devemos executar o seguinte comando:

```bash
sudo pip3 install virtualenv
```

E, mais uma vez, devemos verificar a instalação:

```bash
virtualenv --version
```

## Preparação do Projeto

Criando a pasta do projeto e entrando nela no terminal:

```jsx
mkdir airflow-test
cd airflow-test
```

O Airflow deverá ser instalado dentro de um ambiente virtual. Para criar um ambiente virtual, digite o seguinte comando (lembre-se de substituir o parâmetro NOME):

```jsx
virtualenv NOME
```

Agora, podemos ativar a virtualenv que acabamos de criar, para isto execute:

```bash
source NOME/bin/activate
```