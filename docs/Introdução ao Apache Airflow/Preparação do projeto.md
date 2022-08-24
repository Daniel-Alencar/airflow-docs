# Preparação do projeto

## Passos iniciais

Antes de tudo, temos que ter instalado o python3 em nosso computador. Logo, execute o seguinte comando:

```python
sudo apt-get install python3
```

Para verificar isto, podemos colocar o seguinte comando no terminal:

```jsx
python3 --version
```

Após isto, precisamos do 'pip3' instalado (gerenciador de pacotes do python). Para isto:

```bash
sudo apt-get install python3-pip
```

E em seguida, para verificar se deu tudo certo:

```bash
pip3 --version
```

Agora, devemos utilizar a virtualenv, e para isto devemos instalá-la:

```bash
sudo pip3 install virtualenv
```

E, mais uma vez:

```bash
virtualenv --version
```

## Preparação do Projeto

Criando a pasta do projeto e entrando nela no terminal:

```jsx
mkdir airflow-test
cd airflow-test
```

O airflow deverá ser instalado na virtualenv. Para criar um ambiente virtual, digite o seguinte comando (lembre-se de substituir o parâmetro NOME):

```jsx
virtualenv NOME
```

Agora, podemos ativar a virtualenv que acabamos de criar, para isto execute:

```bash
source NOME/bin/activate
```