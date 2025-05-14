O problema da Mochila Binária  
======

<font color="gray">Grupo 3 - Bloco B (Henrique Puppi, Gustavo Takahashi, Marcos Costa, Maria Luisa Sevilha)</font>

Objetivo do Handout  
---------  

O objetivo essencial do handout é explicar o problema da mochila binária, proposto e entrelaçado com os conceitos apresentados na disciplina [Desafios de Programação](https://ensino.hashi.pro.br/desprog/).

- Relembrar o processo de construção de funções recursivas;
- Relembrar o processo de cálculo de complexidade;
- Relembrar conceitos relacionados à programação dinâmica;  
- Associar o problema a situações reais;  

Conceitualização do problema  
---------  

Imagine que exista, à nossa frente, um conjunto de *n* elementos. Cada um desses elementos tem um peso (*p*) e um valor (*v*), que não apresentam nenhuma relação entre si. Além desses elementos, suponha que possua-se uma mochila à qual é associada uma capacidade máxima (*C*) de peso que essa pode carregar. O problema surge quando precisamos levar o **máximo valor possível** na mochila, respeitando o limite de capacidade.

O problema é conhecido como **mochila binária**, pois não é possível fracionar nenhum dos itens: cada item será levado por inteiro (1) ou não (0), gerando 2 possibilidades por item. A versão em que os itens podem ser fracionados é chamada de [Problema Fracionário da Mochila ou Problema Contínuo da Mochila](https://en.wikipedia.org/wiki/Continuous_knapsack_problem#:~:text=In%20theoretical%20computer%20science%2C%20the,the%20value%20of%20the%20selected).

A mochila binária é particularmente relevante para a otimização de processos ou aquisições. Imaginemos a compra de uma série de CPUs, cada qual com um custo (peso) e uma capacidade de produção (valor). Como há um *budget* limitado (capacidade), precisamos decidir quais CPUs comprar para maximizar o desempenho total.

![CPU](cpu.png)

Primeiras ideias para resolução  
---------  

Dito isso, fica a pergunta: como escolher o que levar na mochila?  

Bom, vamos pensar primeiro em abordagens intuitivas, sem pensar muito nas consequências. 

Para ilustrar as abordagens a seguir, vamos considerar uma mochila com **capacidade igual a 40 unidades de peso** e quatro itens disponíveis:

| Item | Peso | Valor |
|------|------|-------|
| A    | 10   | 10    |
| B    | 18   | 18    |
| C    | 22   | 24    |
| D    | 26   | 30    |

Uma primeira possibilidade seria escolher sempre o item mais **leve**, de forma a levar a maior quantidade possível de itens. 

??? Checkpoint

Faça uma tentativa de completar a Mochila, considerando a sugestão dada.

Essa é a melhor solução possível? Tente calcular o maior valor total possível para esse caso.

::: Gabarito
Ao selecionar sempre o item mais leve até que a capacidade da mochila não seja suficiente, temos o seguinte resultado:

1. Seleciona **A** (peso total = 10, valor total = 10).  
2. Seleciona **B** (peso total = 28, valor total = 28).  
3. Não cabe **C** (28 + 22 > 40).  
  

   **Fim**: valor total = 28 (não é o maior possível).



Nesse caso, o maior valor que pode ser levado é **42**. Ele pode ser obtido ao selecionar os itens **B** (peso 18, valor 18) e **C** (peso 22, valor 24), cuja soma de pesos fica exatamente em **40** e soma de valores em **42**.
:::

???
A estratégia de sempre escolher o item mais leve ignora completamente o **valor** de cada item.  

Assim, ela pode encher a mochila com itens de **baixo valor**, impedindo que sejam incluídos itens um pouco mais pesados que, em conjunto, dariam um **valor total maior**.  

Ok. Já vimos que a abordagem anterior não dá certo. Então, vamos tentar ir por outro caminho: escolher sempre o item de **maior valor**.

??? Checkpoint
Para o mesmo caso, tente resolver o problema dessa forma. Qual foi o valor total obtido? Dessa vez, ele é o maior possível?

::: Gabarito 
Seguindo esse método:  

1. Seleciona **D** (peso total = 26, valor total = 30).  
2. Seleciona **A** (peso total = 36, valor total = 40).  
3. Não cabe **C** (36 + 22 > 40).  
  

    **Fim**: valor total = 40 (não é o maior possível).
:::

???
O problema é que a estratégia de sempre escolher o item de maior valor ignora
completamente o **peso** de cada item.  

Com essa estratégia, a mochila pode acabar cheia com poucos itens **pesados**, deixando de fora combinações de itens ligeiramente mais **leves** que, juntos, poderiam dar um valor total maior — como **B + C**, que juntos valem **42**.  

Vamos tentar mais uma vez. Já vimos que não podemos considerar só um dos atributos e ignorar o outro. Portanto, vamos selecionar os itens a partir de uma relação entre eles: a partir de agora, escolheremos sempre o item que tem o **maior valor por unidade de peso** (pense como se fosse uma espécie de **“custo benefício”**).  

A ideia é, para cada item, **dividir o valor pelo peso**, para obter sua **“densidade”**. Então, preencher a mochila escolhendo sempre o item mais **“denso”**.

??? Checkpoint
Bom, então, faça isso. Esse método finalmente maximizou o valor total da
mochila?

::: Gabarito 
Calculando a “densidade” de cada item:  

- Item D: 30 / 26 ≈ 1,15
- Item C: 24 / 22 ≈ 1,09
- Item B: 18 / 18 = 1,00
- Item A: 10 / 10 = 1,00  
  
  

Preenchendo a mochila:  

1. Seleciona **D** (peso total = 26, valor total = 30).  
2. Tenta selecionar **C**, mas 26 + 22 > 40 (capacidade), logo não cabe. 
3. Tenta selecionar **B**, mas 26 + 18 > 40 (capacidade), logo não cabe.
4. Seleciona **A** (peso total = 26 + 10 = 36, valor total = 30 + 10 = 40).

  

    **Fim**: valor total = 40 (não é o maior possível).
:::

???

Ao executar o último método, você deve ter percebido que sobraram **4 unidades de peso** na mochila. Esse pequeno “buraco” faz toda a diferença. Se tivéssemos escolhido **B** (peso 18, valor 18) e **C** (peso 22, valor 24), usaríamos exatamente **40 de peso** e alcançaríamos valor **42**. Ou seja, mesmo que **D** tenha a **melhor taxa individual de retorno**, sua escolha deixa **espaço ocioso** que não pode ser bem aproveitado. Esse é o grande problema dessa estratégia: ela não leva em consideração o **espaço** na mochila, e portanto não garante o melhor uso da capacidade disponível.  
  

Ok. Você já deve estar ficando frustrado. Você provavelmente deve estar pensando “Isso já está ficando complicado demais! Se testarmos todas as combinações possíveis, alguma delas será a correta, não é mesmo?”, e sim, isso é verdade. Essa com certeza é a abordagem mais intuitiva: resolver o problema na **“força bruta”**.


Abordagem força bruta  
---------

Podemos utilizar da força bruta para gerar todas as possibilidades de mochila e escolher a combinação que gera o maior valor total.  

Com todas as possibilidades criadas, é necessário “apenas”, armazenar o conjunto que devolve o maior valor gerado, sem ultrapassar a capacidade máxima, até que todas as opções tenham sido percorridas.  

??? Checkpoint
Dada a descrição da abordagem, sem fazer cálculos, qual você imagina que seja a complexidade do algoritmo?

Dica: pense que, para cada elemento, a duas possibilidades (ele pode ser incluído ou não)

::: Gabarito 
Como todas as combinações devem ser testadas, a complexidade do algoritmo está associada ao processo de formação de todas essas combinações. Como cada elemento pode ou não ser adicionado, então a complexidade do elemento deve ser $O(2^n)$.

Se ainda não estiver convencido que essa é a complexidade desse algoritmo, veja a tabela abaixo, que demonstra todas as possíveis combinações. Nessa, *0* representa não incluir e *1*, incluir. A coluna de *i* conta quantas combinações são possíveis.

Combinações com 3 itens (*n* = 3):

| a | b | c | i |
|---|---|---|---|
| 0 | 0 | 0 | 1 |
| 1 | 0 | 0 | 2 |
| 0 | 1 | 0 | 3 |
| 0 | 0 | 1 | 4 |
| 1 | 1 | 0 | 5 |
| 1 | 0 | 1 | 6 |
| 0 | 1 | 1 | 7 |
| 1 | 1 | 1 | 8 |

Se ainda não estiver convencido dessa resposta, tente simular essa mesma tabela com mais ou menos itens.

:::

???  

Visualizando essa solução, nos deparamos com um desafio: como geraremos todas as possíveis combinações? Como que conseguiremos armazenar, para cada uma delas, o valor e o peso? Para resolver usando a força bruta, utilizaremos de um algoritmo recursivo. 

Montando o algoritmo de forma recursiva  
---------
Para começar a produzir a solução do problema da mochila, vamos resumir a lógica que o algoritmo deverá seguir. 

A ideia principal é que, para cada elemento dentro da lista de itens, devemos conferir se ele cabe ou não na mochila. Se ele não couber, o ignoramos. Se ele couber, o próximo passo é comparar as "duas" opções: todas as combinações da mochila que incluem o item e todas aquelas que o excluem.

Assim, quando descrevemos comparar os valores incluindo ou não um determinado elemento, precisamos comparar  **todas as possíveis combinações sem o elemento e com o elemento**. Para cada uma delas, primeiro determina-se se essa respeita a capacidade limite e, depois, o seu valor que será comparado. 

Para melhor entender essa descrição, observe a demonstração visual, com apenas três itens.

:demo

Para implementar essa solução, utilizaremos da recursão. Essa ferramenta explora, para cada item, as escolhas de “incluir” ou “não incluir”. Assim, essa possível forma de resolução **repete subproblemas**.

Assim, dada a seguinte estrutura de cada item e a função [[max]], pode-se começar a implementação desejada.

Estrutura [[item]]:
```c
typedef struct {
    int valor;
    int peso;
} item;
```
Função [[max]]:
```c
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

Para construir o algoritmo recursivo, vamos seguir o roteiro descrito na [Aula 2 de Desafios de Programação](https://ensino.hashi.pro.br/desprog/aula/2/).

**Passo 1** (*feito*): entenda o que a função recebe e o que deveria fazer

```c
int mochila_r(item itens[], int n, int cap) {
    // Escreva aqui o caso base da função
}
```

Nesse caso, a função recursiva receberá uma lista de itens que estão disponíveis, a quantidade de itens e a capacidade máxima da mochila.

**Passo 2** (*feito*): adicione uma chamada recursiva ao código da função.

```c
int mochila_r(item itens[], int n, int cap) {
    mochila_r(???);
}
```

**Passo 3**: passe para a chamada recursiva um parâmetro menor.

??? Checkpoint
Pense nos dois possíveis casos para cada item. Se fossemos excluir o item, como ficaria a chamada recursiva? E se fossemos colocá-lo na mochila?

::: Gabarito
Se fossemos excluir o item, apenas a quantidade de itens (*n*) deveria diminuir. Afinal, estamos olhando para um item a menos.

```c
int mochila_r(item itens[], int n, int cap) {
    mochila_r(itens, n - 1, cap);
}
```

Por outro lado, se fossemos incluir o elemento, devemos considerar que, para o próximo item, a **capacidade estará reduzida** (terá o peso daquele item na mochila). Assim, para esse caso, a capacidade da mochila também deve ser diminuída.

```c
int mochila_r(item itens[], int n, int cap) {
    mochila_r(itens, n - 1, cap - itens[n-1].peso);
}
```
:::
???

Então, a recursão precisa lidar com **dois caminhos!** Dessa forma, devem haver duas chamadas recursivas - uma para cada escolha possível.

```c
int mochila_r(item itens[], int n, int cap) {
    mochila_r(itens, n - 1, cap);
    mochila_r(itens, n - 1, cap - itens[n-1].peso);
}
```

Não se preocupe, por enquanto, com a ordem em que essas chamadas serão realizadas. Lideremos com isso no próximo passo.

**Passo 4**: não simularás e terás fé.

??? Checkpoint
Acredite que a função recursiva está fazendo o que deveria. O que cada chamada recursiva deveria retornar?

Dica: Pense em alto nível, lembre que cada chamada da função recursiva representa uma possível escolha. Não se preocupe com o código por enquanto.

::: Gabarito
Como cada chamada recursiva representa uma escolha, então, se acreditarmos que a função fuciona como deve, a primeira chamada deve retornar o valor máximo que não inclui o item. Já a segunda chamada recursiva deve retornar o valor máximo que espera incluir o item, isto é, que conta que parte da capacidade estará ocupada por ele.

Assim, podemos armazenar esses dois valores em duas variáveis [[excluir]] e [[incluir]].

```c
int mochila_r(item itens[], int n, int cap) {
    int excluir = mochila_r(itens, n - 1, cap);
    int incluir = itens[n-1].valor + mochila_r(itens, n - 1, cap - itens[n-1].peso);
}
```

*Nota: perceba que para o valor de [[incluir]] é necessário somar o valor do item mais o maior valor da combinação na qual ele cabe.*
:::
???

**Passo 5** (*feito*): você tem fé na resposta da chamada recursiva, então use-a.

Com esses dois valores em mãos e muita fé, falta apenas uma etapa: compará-los. Como queremos o maior valor possível para ser carregado na mochila, vamos utilizar da função [[max]]. De forma que a função fique assim:

```c
int mochila_r(item itens[], int n, int cap) {
    int excluir = mochila_r(itens, n - 1, cap);
    int incluir = itens[n-1].valor + mochila_r(itens, n - 1, cap - itens[n-1].peso);

    return max(excluir, incluir);
}
```

**Passo 6**: isole o caso em que o parâmetro é o menor possível.

??? Checkpoint
Se estivéssemos excluindo todos os itens, qual seria o caso descrito pelo passo? E se, estivéssemos incluindo todos, existe outro parâmetro que pode atingir um mínimo?

::: Gabarito
A recursão vai precisar parar quando **não houver mais itens** ou quando **não houver mais espaço**:

```c
int mochila_r(item itens[], int n, int cap) {
    if (n == 0 || cap == 0) {
    }

    int excluir = mochila_r(itens, n - 1, cap);
    int incluir = itens[n-1].valor + mochila_r(itens, n - 1, cap - itens[n-1].peso);

    return max(excluir, incluir);
}
```
:::
???

**Passo 7** (*feito*): a solução desse caso é trivial, então calcule ela direto.

Bom, se não há mais espaço ou itens, o retorno deverá ser 0. Ou seja: não há mais valor para ser agregado.

```c
int mochila_r(item itens[], int n, int cap) {
    if (n == 0 || cap == 0) {
        return 0;
    }

    int excluir = mochila_r(itens, n - 1, cap);
    int incluir = itens[n-1].valor + mochila_r(itens, n - 1, cap - itens[n-1].peso);

    return max(excluir, incluir);
}
```

Então, agora a função de recursão está pronta? *Quase*.

??? Checkpoint
Há um pequeno problema no cálculo da variável [[incluir]]. Pense sobre a capacidade. Ela poderia, em algum momento, ser negativa? Tente entender como lidar com esse caso.

Dica: Volte a lógica inicial de quando começamos a construir o algoritmo. Tem algum passo que estamos ignorando?

::: Gabarito
Quando a capacidade é negativa, significa que um item que não cabia na mochila foi adicionado. Assim, o cálculo da variável [[incluir]] fica incorreto. Poderíamos mudar o caso base, mas esse ajuste não lida com o fato de que o item passado, que não cabia, foi adicionado na mochila. 

Dessa forma, é necessária uma outra condicional, que verifica **se o item cabe na mochila**.

```c
int mochila_r(item itens[], int n, int cap) {
    if (n == 0 || cap == 0) {
        return 0;
    }

    if (itens[n - 1].peso > cap) {
        return mochila_r(itens, n - 1, cap);
    } 

    int incluir = itens[n - 1].valor + mochila_r(itens, n - 1, cap - itens[n - 1].peso);
    int excluir = mochila_r(itens, n - 1, cap);
    
    return max(incluir, excluir);
}
```
:::
???

Agora sim! O algoritmo montado funciona corretamente. Mas, como foi proposto, ele resolve o problema na **força bruta**. Qual será a sua complexidade? Será que isso pode ser um limitante?

Cálculo de complexidade desse algoritmo
---------

Vamos fazer o cálculo, como aprendemos em aula, passo a passo de um algoritmo recursivo. 

Como o algoritmo recebe vários parâmetros, vamos analisar apenas em função do responsável pela recursão, o *n*.

**Passo 1**

??? Checkpoint
Faça o sistema de complexidade.

Dica: Faça para o *pior caso*, onde o item atual cabe na mochila, e temos que escolher se ele entra, ou não.

::: Gabarito

$$
f(n) = \begin{cases} 1 & \text{se } n = 0 \\ 2 \cdot f(n-1) + 1 & \text{se } n > 0 \end{cases}
$$

:::
???

**Passo 2**

Fazer a árvore de recursão (essa fornecerei para vocês).

![Arvore Complexidade](arvore_complexidade.png)

**Passo 3**

??? Checkpoint
Estime a altura (<font color="red">*h*</font>) da árvore.

::: Gabarito

Cada chamada recursiva reduz *n* em 1 enquanto for maior que 0.

No antepenúltimo andar *(h-2)*, ainda não chegamos na base.

  $$
  n - 1 \cdot (h-2) > 0
  $$
  
  $$
  n - h + 2 > 0
  $$

  $$
  h < n + 2
  $$

  $$
  h = O(n), \hspace{0.3em}ou\hspace{0.3em}seja, h <= c\hspace{0.3em}n
  $$

:::
???

**Passo 4**

??? Checkpoint
Estime a soma de <font color="red">vermelhos</font> (retornos).

::: Gabarito

Ao longo dos andares, temos:

$$
(1, 2, 4, \dots, 2^{h-1})
$$

Soma de PG:
- Primeiro elemento: *1*;
- Razão: *2*;
- Quantidade de elementos: *h*.

$$
= 1 \cdot \frac{2^h - 1}{2 - 1} 
$$
$$  
= 2^h - 1
$$

:::
???

**Passo 5 - Conclusão**

Como $h <= c\hspace{0.3em}n$, pelas regras de simplificação, podemos concluir que a complexidade é:


$$
\mathbf{O(2^n)}
$$

Ou seja, a complexidade do algoritmo aumenta exponencialmente conforme a quantidade de elementos que estão disponíveis. Isso significa que, dependendo do tamanho do vetor [[itens]] é impraticável utilizar dessa solução. Para isso, temos uma outra estatégia: a **Programação dinâmica**.

Programação dinâmica 
---------

Uma complexidade $O(2^n)$ é um tanto grande, não? Isso significa que o número de possibilidades dobra a cada novo item, crescendo muito rápido — e tornando o problema inviável para conjuntos grandes.

Então, precisamos de uma abordagem melhor. Para isso, vamos relembrar um conceito que vimos na [APS 3](https://ensino.hashi.pro.br/desprog/aps/3/): **Programação Dinâmica**.

Na APS 3, trabalhamos com um problema inspirado na Biologia Computacional: medir a distância de edição entre duas sequências de DNA. A ideia era descobrir o número mínimo de operações necessárias para transformar uma sequência na outra, considerando inserções, remoções ou substituições de caracteres.

![DNA](dna.gif)

Inicialmente, esse problema pode ser resolvido com **recursão**, mas isso rapidamente se torna um empecilho — a quantidade de chamadas cresce exponencialmente. 

Foi aí que usamos a **Programação dinâmica**, uma técnica que quebra o problema em subproblemas menores, resolve cada um deles uma única vez, e **guarda as soluções** para reutilizar depois. Com isso, transformamos um algoritmo de complexidade altíssima em algo muito mais eficiente. 

A ideia da Programação Dinâmica é **evitar calcular a mesma coisa várias vezes**.

Esse mesmo raciocínio pode ser aplicado em diversos contextos — inclusive no nosso problema atual.

??? Checkpoint
Considere o exemplo inicial das CPUs. Imagine que existam 5 CPUs disponíveis para compra, cada uma com um custo e um desempenho associado. O orçamento disponível é 8.

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |
| C   | 4     | 5           |
| D   | 5     | 7           |
| E   | 2     | 2           |

Quantas combinações diferentes precisamos analisar para saber qual conjunto de CPUs fornece o maior desempenho **sem ultrapassar o orçamento**?  
Você percebe que, ao testar as combinações, **a mesma capacidade restante aparece várias vezes**?

Pense:
- Se escolhemos a CPU B (custo 3), restam 5 de orçamento.
- Se escolhemos a CPU D (custo 5), restam 3 de orçamento.
- Se escolhemos A e E (custos 1 + 2), também restam 5 de orçamento.
  
Essas subcomparações de combinações com a **mesma capacidade restante** (como 5 ou 3) são repetidos em diferentes caminhos de decisão. Reflita sobre as implicações disso.

::: Gabarito  
Com 5 itens, o número total de subconjuntos possíveis seria 2⁵ = 32 combinações.  
Muitas dessas combinações diferentes geram os **mesmos subproblemas de capacidade restante**, o que resulta em **cálculos redundantes**.

A Programação Dinâmica evita esses cálculos repetidos **armazenando em uma tabela os melhores desempenhos possíveis para cada orçamento e conjunto parcial de itens**, garantindo que cada subproblema seja resolvido apenas **uma única vez**.
:::
???


**{red}(Ou seja, em vez de recalcular tudo do zero, o algoritmo vai preenchendo essa tabela passo a passo, aproveitando as respostas já conhecidas.)**

No caso da Mochila Binária, montamos uma tabela onde:
- **Cada linha** representa os **itens considerados até o momento**.

- **Cada coluna** representa a **capacidade disponível da mochila**.

- **Cada célula** guarda o **valor máximo possível de desempenho** que conseguimos atingir naquela situação (com aquele subconjunto de itens e aquele orçamento).

Ok, talvez isso tenha ficado um pouco abstrato, vamos montar a tabela para o exemplo do caso anterior.


## Construção da Tabela da Mochila Binária

### Dados dos itens (CPUs)

<br>

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |
| C   | 4     | 5           |
| D   | 5     | 7           |
| E   | 2     | 2           |

Orçamento máximo: **8**

Ao preencher cada célula da tabela, que representa um item (linha) e um determinado orçamento (coluna), siga este raciocínio:

![Diagrama](Diagrama.jpg)

!!! Aviso  
As linhas da tabela representam **combinações** de itens considerados até dado ponto, não itens isolados.
!!!

---

### Vamos primeiro construir uma versão preliminar da tabela, sem preencher seus dados:

<br>

| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma       | ? | ? | ? | ? | ? | ? | ? | ? | ? |
| A                 | ? | ? | ? | ? | ? | ? | ? | ? | ? |
| A, B              | ? | ? | ? | ? | ? | ? | ? | ? | ? |
| A, B, C           | ? | ? | ? | ? | ? | ? | ? | ? | ? |
| A, B, C, D        | ? | ? | ? | ? | ? | ? | ? | ? | ? |
| A, B, C, D, E     | ? | ? | ? | ? | ? | ? | ? | ? | ? |

??? Checkpoint
A primeira linha representa a situação em que nenhum item foi considerado ainda. Nesse caso, qual é o valor máximo de desempenho para cada orçamento?

| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| **Nenhuma**         | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |


::: Gabarito
Se nenhum item foi considerado ainda, para qualquer orçamento, o valor máximo de desempenho é **0**.

| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| **Nenhuma**         | **0** | **0** | **0** | **0** | **0** | **0** | **0** | **0** | **0** |

:::

???


---

### Ótimo. Agora que a primeira linha já está preenchida, vamos preencher as outras, uma de cada vez. A próxima linha representa a situação em que apenas a CPU A é considerada.

<br>



| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |


??? Checkpoint

Nessa situação, qual é o valor máximo de desempenho para cada orçamento?

| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **A**             | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |


::: Gabarito
| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **A**             | **0** | **1** | **1** | **1** | **1** | **1** | **1** | **1** | **1** |

Para qualquer orçamento (exceto o 0), o valor máximo de desempenho é **1**. Isso é verdade porque, como a CPU A cabe em todos esses orçamentos, ela é a melhor escolha possível, pois não há outra CPU sendo considerada ainda.
:::

???

---

### Ainda está muito fácil. Vamos dar um "up" na dificuldade. Considere agora duas CPUs: A e B.  

<br>

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |

<br>

??? Checkpoint
Preencha a linha para esse caso. Dica: antes de olhar para o diagrama, **pense em alto nível**.

| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A             | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| **A, B**             | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |

::: Gabarito
| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A             | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| **A, B**             | **0** | **1** | **1** | **4** | **5** | **5** | **5** | **5** | **5** |

Até o orçamento 2, **só podemos encaixar A**, e portanto o desempenho máximo se mantém em **1**. Porém, quando chegamos no orçamento 3, a situação muda: podemos **incluir B**. Assim, o desempenho máximo é **4** (desempenho de B). Isso também muda a partir do orçamento 4, em que podemos incluir **ambas**, transformando o valor máximo em **4(B) + 1(A) = 5**.
:::

???

---

<br>

??? Checkpoint
Preencha a próxima linha, para o caso ABC. 

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |
| C   | 4     | 5           |

<br>
  
| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A             | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B             | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| **A, B, C**         | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |

::: Gabarito
| CPUs consideradas/ Orçamento | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
|  Nenhuma          | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A             | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B             | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| **A, B, C**         | **0** | **1** | **1** | **4** | **5** | **6** | **6** | **9** | **10** |

- Orç. 0 – 2 → só cabe a CPU A (custo 1), portanto o desempenho máximo permanece 1.

- Orç. 3 → passa a caber a CPU B (custo 3, desp. 4); trocar A por B eleva o máximo para 4.

- Orç. 4 → duas alternativas empatadas em 5:

    -- A + B (1 + 3 = 4 → 1 + 4 = 5)

    -- C sozinho (custo 4, desp. 5).

- Orç. 5 → melhora ao combinar A + C (1 + 4 = 5 → 1 + 5 = 6).

- Orç. 6 → nenhuma nova combinação supera 6 (B + C ainda custa 7).

- Orç. 7 → agora cabe B + C (3 + 4 = 7 → 4 + 5 = 9).

- Orç. 8 → cabe o trio A + B + C (1 + 3 + 4 = 8) atingindo o desempenho máximo de 10.
:::

???

---

<br>

??? Checkpoint
Preencha a próxima linha, para o caso ABCD. 

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |
| C   | 4     | 5           |
| D   | 5     | 7           |

<br>
  
| CPUs consideradas | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma           | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A                 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B              | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| A, B, C           | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 | 10 |
| **A, B, C, D**         | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |

::: Gabarito
| CPUs consideradas | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma           | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A                 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B              | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| A, B, C           | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 | 10 |
| **A, B, C, D**         | **0** | **1** | **1** | **4** | **5** | **7** | **8** | **9** | **11** |

- Orç. 0 – 2 → só cabe a CPU A (custo 1), logo o desempenho máximo permanece 1.

- Orç. 3 → passa a caber a CPU B (custo 3, desp. 4); trocar A por B eleva o máximo para 4.

- Orç. 4 → duas alternativas empatadas em 5:

    -- A + B → 1 + 3 = 4 → 1 + 4 = 5

    -- C sozinho → custo 4, desp. 5

- Orç. 5 → entra a CPU D (custo 5, desp. 7), superando qualquer outra combinação; máximo sobe para 7.

- Orç. 6 → melhor combinação é A + D (1 + 5 = 6 → 1 + 7 = 8); nenhuma outra supera 8.

- Orç. 7 → agora cabe B + C (3 + 4 = 7 → 4 + 5 = 9), que se torna o novo máximo 9.

- Orç. 8 → duas possibilidades cabem, mas a melhor é:

    -- B + D → 3 + 5 = 8 → 4 + 7 = 11 (máximo)

    -- A + B + C → 1 + 3 + 4 = 8 → 10 (não supera 11)
:::

???

---

<br>

??? Checkpoint
Preencha a próxima linha, para o caso ABCDE. 

| CPU | Custo | Desempenho |
|-----|-------|-------------|
| A   | 1     | 1           |
| B   | 3     | 4           |
| C   | 4     | 5           |
| D   | 5     | 7           |
| E   | 2     | 2           |

<br>
  
| CPUs consideradas | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma           | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A                 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B              | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| A, B, C           | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 | 10 |
| A, B, C, D        | 0 | 1 | 1 | 4 | 5 | 7 | 8 | 9 | 11 |
| **A, B, C, D, E**         | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** | **?** |

::: Gabarito
| CPUs consideradas | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma           | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A                 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B              | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| A, B, C           | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 | 10 |
| A, B, C, D        | 0 | 1 | 1 | 4 | 5 | 7 | 8 | 9 | 11 |
| **A, B, C, D, E**         | **0** | **1** | **2** | **4** | **5** | **7** | **8** | **9** | **11** |

- Orç. 0 → nenhum chip cabe; desempenho máximo 0.

- Orç. 1 → só cabe a CPU A (custo 1, desp. 1) ⇒ desempenho 1.

- Orç. 2 → agora cabe a CPU E (custo 2, desp. 2), que supera A ⇒ desempenho 2.

- Orç. 3 → cabe a CPU B (custo 3, desp. 4); trocar E por B eleva o máximo para 4.

- Orç. 4 → duas alternativas empatadas em 5:

    -- A + B → 1 + 3 = 4 → 1 + 4 = 5

    -- C sozinho → custo 4, desp. 5

- Orç. 5 → a melhor escolha passa a ser D (custo 5, desp. 7) ⇒ máximo 7.

- Orç. 6 → melhor combinação é A + D (1 + 5 = 6 → 1 + 7 = 8) ⇒ desempenho 8.

- Orç. 7 → cabe B + C (3 + 4 = 7 → 4 + 5 = 9) ⇒ desempenho 9.

- Orç. 8 → melhor combinação é B + D (3 + 5 = 8 → 4 + 7 = 11) ⇒ desempenho 11.
:::

???

---

<br>

??? Checkpoint
O que representa o valor da **última célula da tabela** (linha A, B, C, D e E; coluna 8)?

<br>

| CPUs consideradas | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------------|---|---|---|---|---|---|---|---|---|
| Nenhuma           | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A                 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| A, B              | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 | 5 |
| A, B, C           | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 | 10 |
| A, B, C, D        | 0 | 1 | 1 | 4 | 5 | 7 | 8 | 9 | 11 |
| A, B, C, D, E     | 0 | 1 | 2 | 4 | 5 | 7 | 8 | 9 | 11 |

::: Gabarito
Representa o **melhor desempenho possível com orçamento 8**, considerando **todas as CPUs disponíveis**.  
É o resultado final da programação dinâmica: a melhor combinação possível sem ultrapassar o limite de orçamento.
:::

???

Assim, saímos de uma complexidade exponencial de  $O(2^n)$ para uma complexidade polinomial de $O(n \cdot W)$, onde:

- $n$ é o número de itens, e  
- $W$ é a capacidade total da mochila.

Essa abordagem é muito mais eficiente e torna viável resolver instâncias que seriam impraticáveis com força bruta.

Agora, se quiser explorar um exemplo mais desafiador, considere o conjunto de CPUs abaixo com um orçamento total de 12 unidades


| CPU | Custo | Desempenho |
|-----|-------|------------|
| A   | 8     | 76         |
| B   | 3     | 37         |
| C   | 6     | 47         |
| D   | 5     | 60         |
| E   | 9     | 91         |
| F   | 2     | 21         |
| G   | 7     | 86         |
| H   | 4     | 36         |


:simu2

Além disso, você pode testar seus próprios conjuntos de dados e orçamentos utilizando esta calculadora interativa online:
👉 [Knapsack Calculator](https://augustineaykara.github.io/Knapsack-Calculator/) – by Augustine Aykara

## Complexidade do algoritmo de Programação Dinâmica

Com essa proposta construída, será que ela consegue ser menos complexa do que a força bruta? Vamos confirmar isso.

??? Checkpoint

Pensando em alto nível, qual a complexidade dessa proposta de resolução?

Dica: a principal atividade do algortimo é preencher uma tabela de dimensão *n* (quantidade de itens) por *W* (capacidade ou orçamento máximo).

::: Gabarito

Como a função do algoritmo é preencher a tabela e, para isso, utiliza de soluções armazenadas na mesma, a sua complexidade é de preencher a matriz. Dessa forma, como as dimensões da tabela são $W$ e $n$, temos que a complexidade do algoritmo é: **O(w*n)**

Repare que, nesse caso, ambos os elementos são recebidos pelo algoritmo (são parâmetros), de forma que não seja possível simplificar a complexidade para O(n). Para melhor entender isso, imagine que a capacidade máxima seja igual a $n$.

:::

???