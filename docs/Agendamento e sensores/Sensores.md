# Sensores

Além de execuções automáticas realizadas em intervalos de tempo, podemos querer disparar uma DAG sempre que um critério condicional for atendido. No Airflow, podemos fazer isso através de Sensores.

Um sensor é um tipo especial de operador que verifica continuamente (em um intervalo de tempo) se uma certa condição é verdadeira ou falsa.

- Se verdadeira, o sensor tem seu estado alterado para bem-sucedido e o restante do pipeline é executado.
- Se falsa, o sensor continua tentando até que a condição seja verdadeira ou um tempo limite (timeout) for atingido.

Um sensor muito utilizado é o `FileSensor` que verifica a existência de um arquivo e retorna verdadeiro caso o arquivo exista. Caso contrário, o `FileSensor` retorna `False` e refaz a checagem após um intervalo de 60 segundos (valor padrão). Este ciclo permanece até que o arquivo venha a existir ou um tempo limite seja atigindo (por padrão, 7 dias).

Podemos configurar o intervalo de reavalição através do argumento `poke_interval` que espera receber, em segundos, o período de espera entre cada checagem. Já o timeout é configurado por meio do argumento `timeout`.

```python
from airflow.sensors.filesystem import FileSensor

wait_for_file = FileSensor(
    task_id = "wait_for_file",
    filepath = "/data/file.csv",
    poke_interval = 10,  # 10 seconds
    timeout = 5 * 60,  # 5 minutes
)
```

**Nota:**

O `FileSensor` suporta [wildcards](https://ahayasic.github.io/apache-airflow-in-a-nutshell/content/building_pipelines/scheduling_and_sensors/#) (e.g. astericos [`*`]), o que nos permite criar padrões de correspondência nos nomes dos arquivos.

<aside>
⚠️ Chamamos de *poking* a rotina realizada pelo sensor de execução e checagem contínua de uma condição.

</aside>

### Condições Personalizadas

Há diversos cenários em que as condições de execução de um pipeline são mais complexas do que a existência ou não de um arquivo. Em situações como essa, podemos recorrer a uma implementação baseada em Python através do `PythonSensor` ou então criarmos nosso próprio sensor.

O `PythonSensor` assim como o `PythonOperator` executa uma função Python. Contudo, essa função deve retornar um valor booleano: `True` indicando que a condição foi cumprida, `False` caso contrário.

Já para criarmos nosso próprio sensor, basta estendermos a classe `BaseSensorOperator`. Informações sobre a criação de componentes personalizados são apresentados na seção [Criando Componentes Personalizados](https://ahayasic.github.io/apache-airflow-in-a-nutshell/content/going_deeper/creating_custom_components/)

### Sensores & Deadlock

O tempo limite padrão de sete dias dos sensores possui uma falha silenciosa.

Suponha uma DAG cujo `schedule_interval` é de um dia. Se ao longo do tempo as condições não foram atendidas, teremos um acumulo de DAGs e tarefas para serem executadas considerável.

O problema deste cenário é que o Airflow possui um limite máximo de tarefas que ele consegue executar paralalemente. Portanto, caso o limite seja atingido, as tarefas ficarão bloqueadas e nenhuma execução será feita. Este comportamento é denominado *sensor deadlock*.

Embora seja possível aumentar o número de tarefas executáveis em paralelo, as práticas recomendadas para evitar esse tipo de situação são:

- Definir um *timeout* menor que o `schedule_interval`. Com isso, as execuções irão falhar antes da próxima começar.
- Alterar a forma como os sensores são acionados pelo *scheduler*. Por padrão, os sensores trabalham no modo `poke` (o que pode gerar o *deadlock*). Através do argumento `mode` presente na classe do sensor em questão, podemos alterar o acionamento do sensor para `reschedule`.
    
    No modo `reschedule`, o sensor só permanecerá ativo como uma tarefa enquanto estiver fazendo as verificações. Ao final da verificação, caso o critério de sucesso não seja cumprido, o *scheduler* colocará o sensor em estado de espera, liberando assim uma posição (*slot*) para outras tarefas serem executadas.