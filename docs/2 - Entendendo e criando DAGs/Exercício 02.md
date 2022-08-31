# Exercício 02

Já que agora você entendeu o que é uma DAG e como manipulá-la de forma básica no Airflow, podemos fazer alguns exercícios.

### DAG para conselho diário

A Advide Slip é uma API que lhe retorna um conselho para a sua vida.

Exemplo, ao fazer a seguinte requisição:

```python
https://api.adviceslip.com/advice
```

Temos o retorno de algum conselho aleatório da API:

```json
{
  "slip": {
    "id": 202,
    "advice": "Never waste an opportunity to tell someone you love them."
  }
}
```

A sua tarefa é criar uma DAG com uma tarefa apenas, que faça uma requisição para esta API e retorne um conselho todo dia as 6:30h da manhã.

Você terá que pesquisar sobre o crontab do sistema UNIX para descrever o parâmetro schedule_interval corretamente. Eis um breve resumo de funcionamento.

![Untitled](Exercicio%2002/Untitled.png)

<aside>
⚠️ Recomendamos utilizar o raciocínio do exercício acima para criar DAGs com algum tipo de agendamento. Pense em alguma informação que você possa querer saber em algum momento do dia e implemente para ser executada na hora e nos dias que desejar.

</aside>