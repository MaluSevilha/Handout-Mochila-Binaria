O problema da Mochila Binária  
======

- Grupo 3 Bloco B (Henrique Puppi, Gustavo Takahashi, Marcos Costa, Maria Luisa Sevilha)

Objetivo do Handout  
---------  

O objetivo essencial do handout é explicar o problema da mochila binária, proposto e entrelaçado com os conceitos apresentados na disciplina Desafios de Programação.

- Relembrar conceitos relacionados a programação dinâmica;  
- Associar o problema a situações reais;  

Conceitualização do problema  
---------  

Imagine que exista, à nossa frente, um conjunto de *n* elementos. Cada um desses elementos tem um peso (*p*) e um valor (*v*), que não apresentam nenhuma relação entre si. Além desses elementos, suponha que possua-se uma mochila à qual é associada uma capacidade máxima (*C*) de peso que pode levar. O problema surge quando precisamos levar o **máximo valor possível** na mochila, respeitando o limite de capacidade.

O problema é conhecido como **mochila binária**, pois não é possível fracionar nenhum dos itens: cada item será levado por inteiro (1) ou não (0), gerando exatamente 2 possibilidades por item. A versão em que os itens podem ser fracionados é chamada de **Problema Fracionário da Mochila** ou **Problema Contínuo da Mochila**.

Ele é particularmente relevante para a otimização de processos ou aquisições: imaginemos a compra de uma série de CPUs, cada qual com um custo (peso) e uma capacidade de produção (valor). Como há um *budget* limitado (capacidade), precisamos decidir quais CPUs comprar para maximizar o desempenho total.

Primeiras ideias para resolução  
---------  

Dito isso, fica a pergunta: como escolher o que levar na mochila?  

Bom, vamos pensar primeiro nas
abordagens mais intuitivas possíveis, sem pensar muito nas consequências. A primeira
seria escolher sempre o item mais **leve**, de forma a levar a maior quantidade possível de
itens. Para ilustrar essa abordagem, vamos considerar uma mochila com **capacidade igual
a 40 unidades de peso** e quatro itens disponíveis:

| Item | Peso | Valor |
|------|------|-------|
| A    | 10   | 10    |
| B    | 18   | 18    |
| C    | 22   | 24    |
| D    | 26   | 30    |

Ao selecionar sempre o mais leve até que a
capacidade da mochila não seja suficiente, temos o seguinte resultado:

1. Seleciona **A** (peso total = 10, valor total = 10).  
2. Seleciona **B** (peso total = 28, valor total = 28).  
3. Não cabe **C** (28 + 22 > 40).  
  

    **Fim**: valor total = 28.

??? Checkpoint
A estratégia acima tem um problema. Qual? Tente calcular o maior valor total possível para esse caso.

::: Gabarito  
A estratégia de sempre escolher o item mais leve ignora completamente o
**valor** de cada item.  

Assim, ela pode encher a mochila com itens de **baixo valor**, impedindo que
você inclua itens um pouco mais pesados que, em conjunto, dariam um **valor total
maior**.  

Nesse caso, o maior valor é **42**, obtido ao selecionar os itens **B** (peso 18, valor
18) e **C** (peso 22, valor 24), cuja soma de pesos fica exatamente em **40** e soma de
valores em **18 + 24 = 42**.
:::

???
Ok. Já vimos que a abordagem anterior não dá certo. Então, vamos tentar ir por
outro caminho: escolher sempre o item de **maior valor**.


??? Checkpoint
Para o mesmo caso, tente resolver o problema dessa forma. Qual foi o valor
total que você obteve? Qual o principal problema dessa nova abordagem?

::: Gabarito 
Seguindo esse método:  

1. Seleciona **D** (peso total = 26, valor total = 30).  
2. Seleciona **A** (peso total = 36, valor total = 40).  
3. Não cabe **C** (36 + 22 > 40).  
  

    **Fim**: valor total = 40.
 
O problema é que a estratégia de sempre escolher o item de maior valor ignora
completamente o **peso** de cada item.  

Por isso, pode ocupar muito espaço com itens **pesados** de alto valor, deixando
de lado combinações de itens ligeiramente mais **leves** que, em conjunto, dariam um
valor total **ainda maior** (como **B + C**, que somam **42**).
:::

???

Vamos tentar mais uma vez. Já vimos que não podemos considerar só um dos
atributos e ignorar o outro. Portanto, vamos selecionar os itens a partir de uma relação entre
eles: a partir de agora, escolheremos sempre o item que tem o **maior valor por unidade de
peso** (pense como se fosse uma espécie de **“custo/benefício”**).  

A ideia é, para cada item,
**dividir o valor pelo peso**, para obter sua **“densidade”**. Então, preencher a mochila
escolhendo sempre o item mais **“denso”**.

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

  

    **Fim**: valor total = 40.
  

O valor total ainda **não foi maximizado**, já que o resultado foi 40, e não 42.
:::

???

Ao executar o último método, você deve ter percebido que sobraram **4 unidades de peso** na mochila. Esse pequeno “buraco” faz toda a diferença. Se tivéssemos escolhido **B** (peso 18, valor 18) e **C** (peso 22, valor 24), usaríamos exatamente **40 de peso** e alcançaríamos valor **42**. Ou seja, mesmo que **D** tenha a **melhor taxa individual de retorno**, sua escolha deixa **espaço ocioso** que não pode ser bem aproveitado. Esse é o grande problema dessa estratégia: ela não leva em consideração o **espaço** na mochila, e portanto não garante o melhor uso da capacidade disponível.  
  
  
Ok. Você já deve estar ficando frustrado. Você provavelmente deve estar pensando “Isso já está ficando complicado demais! Se testarmos todas as combinações possíveis, alguma delas será a correta, não é mesmo?”, e sim, isso é verdade. Essa com certeza é a abordagem mais intuitiva: resolver o problema na **“força bruta”**.


Abordagem força bruta  
---------

Podemos utilizar da força bruta para gerar todas as possibilidades de mochila e escolher a combinação que gera o maior valor total.  

Assim, o primeiro desafio é *criar todas as combinações*.  

!!! Aviso  
A ordem em que os itens são colocados não é relevante para a mochila, já que as únicas propriedades que importam são a soma dos pesos (para determinar se um conjunto é válido) e a soma de valores dos elementos.  

Dessa forma, **dois conjuntos são diferentes apenas se possuem elementos diferentes**.
!!!  

Com todas as possibilidades criadas, é necessário “apenas”, armazenar o conjunto que devolve o maior valor gerado, sem ultrapassar a capacidade máxima, até que todas as opções tenham sido percorridas.  

??? Checkpoint
Dada a descrição da abordagem, sem fazer cálculos, qual você imagina que seja a complexidade do algoritmo?

::: Gabarito 
Como todas as combinações devem ser testadas, a complexidade do algoritmo está associada ao processo de formação de todas essas combinações. Como cada elemento pode ou não ser adicionado, então a complexidade do elemento deve ser $O(2^n)$.
:::

???  


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

Montando o algoritmo de forma recursiva  
---------

Uma implementação recursiva explora, para cada item, as escolhas de “incluir” ou “não incluir”. Entretanto, essa versão recursiva repete subproblemas e mantém a complexidade exponencial.

#### Cálculo de complexidade desse algoritmo

Ainda $O(2^n)$, pois reexploramos combinações repetidas.

Programação dinâmica 
---------

Uma complexidade $O(2^n)$ é um tanto grande, não? Isso significa que o número de possibilidades dobra a cada novo item, crescendo muito rápido — e tornando o problema inviável para conjuntos grandes.
Então, precisamos de uma abordagem melhor. Para isso, vamos relembrar um conceito que vimos na APS 3: Programação Dinâmica.

Na APS 3, trabalhamos com um problema inspirado na Biologia Computacional: medir a distância de edição entre duas sequências de DNA. A ideia era descobrir o número mínimo de operações necessárias para transformar uma sequência na outra, considerando inserções, remoções ou substituições de caracteres.

![](dna.png)

Inicialmente, esse problema pode ser resolvido com recursão, mas isso rapidamente se torna um empecilho — a quantidade de chamadas cresce exponencialmente. Foi aí que usamos a programação dinâmica, uma técnica que quebra o problema em subproblemas menores, resolve cada um deles uma única vez, e guarda as soluções para reutilizar depois. Com isso, transformamos um algoritmo de complexidade altíssima em algo muito mais eficiente.

A APS 3 mostrou, de forma prática, como a programação dinâmica nos permite resolver problemas complexos com eficiência, ao evitar recomputações e organizar a resolução dos subproblemas de maneira sistemática. Esse mesmo raciocínio pode ser aplicado em diversos contextos — inclusive no nosso problema atual.

A ideia da Programação Dinâmica é evitar calcular a mesma coisa várias vezes.
Se formos pensar, muitos subproblemas são repetidos. Por exemplo, ao decidir se usamos ou não um item, muitas combinações acabam compartilhando a mesma capacidade restante.

A Programação Dinâmica resolve isso armazenando as soluções intermediárias em uma tabela — chamada de tabela de estados — e reutilizando essas soluções sempre que necessário.

Ou seja, em vez de recalcular tudo do zero, o algoritmo vai preenchendo essa tabela passo a passo, aproveitando as respostas já conhecidas.
No caso da Mochila Binária, montamos uma tabela onde:
Cada linha representa os itens considerados;
Cada coluna representa a capacidade disponível da mochila;
E cada célula guarda o valor máximo que conseguimos para aquela situação.

Ok, talvez tenha sido um pouco rápido demais, vamos tentar exercitar isso de forma mais simplificada antes de avançar.

Assim, saímos de uma complexidade 2^n, exponencial, para uma complexidade polinomial, de O(n * W), onde n é o número de itens e W é a capacidade da mochila.
Essa abordagem é muito mais eficiente e torna possível resolver instâncias que seriam inviáveis na força bruta.

Vamos pens

Para cada célula 
(i, w)
 (linha da CPU atual e orçamento atual):
Você precisa responder mentalmente:
Essa CPU cabe no orçamento atual w?
Se NÃO cabe (ou seja, o custo da CPU é maior que w):
Você não pode comprar essa CPU.
Então, a melhor solução é a mesma que você já tinha sem essa CPU.
Ou seja, simplesmente copia o valor da célula de cima.
Se SIM, ela cabe no orçamento w:
Agora você tem uma escolha a fazer:
Opção 1: Ignorar essa CPU e não usá-la.
Resultado: manter o melhor desempenho que já existia sem ela (valor de cima).
Opção 2: Usar essa CPU.
Resultado: somar o desempenho dessa CPU com o melhor desempenho anterior que cabe no orçamento restante (w - custo da CPU).
Você compara essas duas opções:
Quem der mais desempenho, vence


:simu


!!! Aviso  
As linhas da tabela representam **combinações** de itens já avaliadas, não itens isolados. 🤓☝  
!!!