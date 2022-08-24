# DAGs no Apache Airflow

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

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled.png)

Cada um destes elementos da lista representa uma DAG diferente.

Como exemplo, clicaremos na DAG ‘tutorial’ e na aba Graph View (para termos uma visualização em gráfico da nossa DAG).

Então teremos a seguinte tela.

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled%201.png)

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

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled%202.png)

Para acompanharmos mais detalhadamente a execução de alguma task, podemos clicar na Task que queremos analisar.

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled%203.png)

Em seguida, clicar no botão ‘Log’.

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled%204.png)

E então, teremos uma visualização mais detalhada do que está acontecendo. Nesta parte, podemos acompanhar os erros que alguma task pode ter disparado também.

![Untitled](DAGs%20no%20Apache%20Airflow/Untitled%205.png)

Beleza. Após aprendermos o que são as DAGs e como elas podem ser visualizadas e manipuladas no Airflow, agora, podemos criar a nossa primeira DAG.