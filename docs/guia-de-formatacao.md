---
title: Guia de Formatação Completo
nav_order: 3
layout: default
---

# Introdução

<br>

É fato que o exercício da análise de dados  e criação de visuais é executado por uma extensa gama de profissionais, inclusive aqueles mais próximos ao negócio. Por conta disso, a resolução de um mesmo problema pode contar diversas abordagens. Essa característica é um dos fatores determinantes, porém não o único, para a não adoção extensa de padrões de construção.

É certo que, em alguns casos os padrões são definidos e seguidos por equipes ou até empresas inteiras, mas não é algo de todo o mercado. Entretanto, essa falta de unidade ampla não é realidade para todos os ramos de atuação da Tecnologia da Informação (TI), observe, por exemplo, os padrões da linguagem Python (PEP 8) e suas ferramentas de análise estática de código  (_linters_), ou até mesmo as conversões de nomenclatura e escrita adotadas e criadas por comunidades como a do Java e JavaScript.

Dessa forma, esse texto tem como o objetivo estabelecer determinados padrões, especificamente sob a ótica de utilização do software Power BI no desenvolvimento local (desktop), DAX e Power Query . Além disso, as orientações apontadas podem ser transpostas para outros contextos do Power BI Service, com as devidas adaptações e ressalvas.

Além disso, é preciso comentar que o conteúdo descrito não são leis (no sentido de precisarem ser seguidas de forma tirânica), mas sim sugestões para que buscam melhorar a experiência do desenvolvedor. 

Por último, é importante ressaltar que o texto é voltado para equipes técnicas e não usuários de "_self-service_ BI". 

## Por que adotar padrões?
<br>

A adoção de padrões proporciona consistência no desenvolvimento. A consistência, além de usualmente promover um grau maior de organização, possibilita com que manutenção, quando feita por profissionais diferentes, seja mais simples e direta.

Além disso, a adoção de padrões proporciona redução na carga cognitiva, tanto ao ler o código quanto a escrevê-lo, dessa forma, acelerando o desenvolvimento.

# Formatação

## Nomenclatura
<br>

A nomenclatura deve priorizar nomes descritivos e significativos, capazes de indicar de forma clara a função ou o conteúdo do objeto.

Recomenda-se evitar o uso de caracteres especiais, tais como acentos, cedilha (`ç`) ou símbolos fora do padrão alfanumérico básico (algo como ASCII/ANSI). Portanto, limitando-se ao uso dos caracteres (A–Z, a–z) e números (0–9).

Exceções aceitáveis são: 

Em medidas, o sinal de barra (`/`) para indicar uma divisão, o underline (`_`) para referenciar algum objeto (como o nome de uma coluna), o sinal de porcentagem (`%`), ou o parêntese (`()`), porém, evite se possível. O caso do parêntese é especial, fará parte de uma abordagem específica adiante.

Essa orientação decorre do próprio comportamento da linguagem DAX e do mecanismo do editor de fórmulas. Em DAX, identificadores que contêm apenas caracteres alfanuméricos simples podem ser referenciados diretamente durante a digitação, permitindo que o IntelliSense reconheça e sugira automaticamente a medida. Quando o nome contém caracteres especiais, o engine exige delimitação explícita (por exemplo, com colchetes ou aspas simples, conforme o contexto), o que altera o _parsing_ do identificador. 

Observe o caso da barra (`/`), que será nosso exemplo, usualmente busca-se referenciar uma medida pelo nome, sem necessariamente utilizar os colchetes e, ao fazer isso, o parser interpreta o caractere como operador aritmético de divisão, interrompendo a sugestão automática e exigindo ajuste manual. Isso reduz a fluidez da escrita, aumenta a chance de erros sintáticos.

Além disso, a recomendação busca evitar qualquer possível erro de encoding quando os metadados estiverem sendo manipulados fora do Power BI Desktop.

Outro ponto de atenção é a evitação de caracteres ambíguos, como a letra “l” minúscula (`l`), que pode ser facilmente confundida com o número “1” ou com a letra maiúscula “I” dependendo da fonte utilizada.

### Abreviações
<br>

As abreviações recomendadas são os acrônimos consolidados e amplamente reconhecidos no contexto da empresa, acrônimos referentes ao DAX como, por exemplo, `ALL` ou aqueles relacionados à inteligência temporal (como _YTD_, _MTD_, _QTD_, _PY_). Observe a lista descrita na seção _Naming Convention_ no [daxpatterns](https://www.daxpatterns.com/standard-time-related-calculations/).

O uso de abreviações, que não sejam os citados, deve ser evitado, pois reduz a clareza e dificulta a manutenção do código ou do modelo por terceiros. Abreviações pouco óbvias podem gerar interpretações equivocadas e comprometer a legibilidade em revisões futuras.

Em geral, prefira nomes completos e descritivos, mesmo que mais longos, pois a clareza é mais importante do que economizar caracteres.  

## Descritivo: estilos de nomenclatura
<br>

Há muitos estilos de escrita e os abaixo são comumente descritos, eles serão citados para a utilização na continuidade do texto.


 - lowercase: letras minúsculas; <br><br>

 - lower_case_with_underscores: letras minúsculas separadas por underline;<br><br>

 - UPPERCASE: letras maiúsculas;<br><br>

 - UPPER_CASE_WITH_UNDERSCORES: letras maiúsculas separadas por underline; <br><br>

  - Title Case: Palavras iniciando com letras maiúsculas com separação entre si por espaços; <br><br>


 - CapitalizedWords (CapWords, ou CamelCase): Palavras iniciando com letras maiúsculas sem separação entre si. <br>

	Nota: ao utilizar acrônimos em CapWords, deixe maiúscula todas as letras do acrônimo. Como no exemplo: SomaPYTD.

## Consultas do Power Query
<br>

Apesar de esse texto não focar especificamente em Power Query, serão abordadas algumas questões que influem no resultado final do projeto.

Seguindo, classificaremos as consultas do Power Query com  duas naturezas distintas, são elas: tabelas finais e tabelas intermediárias. As tabelas intermediárias são aquelas manipuladas (mescladas, acrescentadas, etc.) para produzir uma tabela final. Por sua vez, as tabelas finais serão aquelas utilizadas extensivamente no modelo, tanto para exibir visuais quanto para realizar cálculos. Os dois casos serão abordados.

Nota: o que aqui chamamos de tabela é chamado de consulta dentro do Power Query. Foi adotada uma nomenclatura diferente apenas para fins representativos.

A primeira recomendação é que nas tabelas intermediárias deve-se desabilitar a carga, dessa forma, não será gerada uma entidade (ou tabela final) exibida no modelo de dados.

### Tabelas finais
<br>
#### Tabelas fato, dimensões e bridges

 * Formatação: __UPPER_CASE_WITH_UNDERSCORES__ com um prefixo representando a natureza da tabela (D, F ou B) e o nome da entidade em singular ou plural. Em tabelas dimensão e bridges, singular e em tabelas fatos o plural. 

Nota: em algumas situações é aceitável utilizar o plural para dimensão visando manter a consistência, por exemplo, em casos que a dimensão já foi escrita no plural na origem.

 * Exemplo: D_STORE ; D_CLIENT ; F_SALES ; F_TRANSACTIONS ; B_CLIENT
 
 * Motivo:

O prefixo será adotado visando uma celeridade na identificação da função da tabela no modelo, além disso, o UNDERSCORE_UPPERCASE foi escolhido para representar visualmente que as mesmas estão em um grau de "importância de objeto" superior aos demais objetos do relatório (chama mais atenção).

Ao contrário do que corriqueiramente se recomenda (utilizar nomes descritivos e amigáveis ao consumidor do relatório) aqui optou-se por manter a formalidade e utilizar a nomenclatura sem espaços, se possível, conservando o nome que foi utilizado na origem, desde que esse tenha significado próprio.

<br>

#### Tabelas de medidas

É preferível que as medidas estejam organizadas em tabelas específicas, sendo possível ter mais de uma tabela de medidas no modelo para o caso de separar as medidas por contexto de tabela.

O padrão se sustenta não somente por questão de organização, mas também porque caso a tabela em que a medida foi baseada precise ser apagada do modelo, a medida não é apagada junto. Sim, existem casos em que é necessário apagar e recriar uma tabela.

Dessa forma, a formatação escolhida para as tabelas de medidas é:

* Formatação: __Title Case__

* Exemplo: Medidas Faturamento

* Motivo:

Optou-se pelo Title Case para diferenciar as tabelas de medidas para as tabelas finais, criando uma hierarquia visual.

##### Pastas e Subpastas

Recomenda-se também que as tabelas de medidas tenham sua organização subdivida entre pastas e subpastas, separando assim as medidas por escopo e organizando de forma eficiente o visual. Essa organização têm um custo, a ordem das pastas deve ser definida manualmente e reformada quando se deseja alteração, mas, aqui entende-se que é um custo pequeno frente a organização visual e lógica.

* Formatação: __Title Case__ com prefixo  (numero de ordem seguindo por um ponto final `"1."`). 

* Exemplo: 1. Metricas Base ; 4. Representatividade ; 3. Medidas Time Intelligence

* Motivo:

A necessidade de utilização de um prefixo antes da pasta e subpasta serve para criar uma ordem entre as pastas, já que, faz mais sentido atribuir uma ordem com base na fluxo dos cálculos (ou outra ordem lógica de negócio) do que utilizar a ordenação alfabética. Então, do modo escolhido é possível criar uma hierarquia visual entre pastas e subpastas se aproveitando da organização alfanumérica.

Caso seja necessário criar medidas para debug, é recomendado criar uma pasta com a nomenclatura "0. Debug" para medidas de debug. Além disso, é uma boa prática separar as medidas de base, aquelas mais simples utilizadas para construir outras medidas,  na primeira pasta.

### Tabelas Intermediárias
#### Consultas auxiliares

* Case: __lower_case_with_underscores__ com prefixo (aux).

* Exemplo: aux_clientes_de_para ; aux_clientes_inativos

* Motivo:

Escolheu-se um prefixo regular para que o _scan_ visual no Power Query seja facilitado.

Com relação ao nome de entidade que será utilizado depois do prefixo "aux_" cabe ao construtor do relatório o bom senso de escolher um nome significativo e não excessivamente grande.

Como citado anteriormente, Quaisquer consultas que não sejam uma tabela final e os dados não sejam utilizados no visual ou construção de cálculos no relatório devem estar com a carga desabilitada.

## DAX

### Medidas

* Case: __Title Case__ com prefixo (`(local)`).

Nota: caso as medidas sejam do modelo semântico aceito, ou seja, aquele utilizado regularmente pela organização e validado, não há necessidade do prefixo. Então, em cenários corporativos centralizados e organizados, recomenda-se omitir o prefixo.

* Exemplo: (local) Revenue ; (local) Revenue PYTD

* Motivo:

O motivo da escolha do prefixo "(local)" é para indicar que as medidas foram feitas dentro do escopo de um relatório, ou seja, Power BI Desktop. Isso serve para diferenciar as medidas do relatório e não poluir um modelo semântico em específico. 

Por sua vez, o Title Case é indicado pois as medidas são exibidas nos elementos visuais. Dessa forma, para a sua utilização rápida em um visual bastaria apagar o "(local)". Não somente, o padrão também é utilizado em livros reconhecidos da área, dos autores Ferrari e Russo, por exemplo.

Além disso, no caso de de lidar com múltiplas tabelas fatos em que medidas serão semelhantes, é aceitável indicar na medida a qual tabela ela pertença (até mesmo utilizar abreviações específicas). Por exemplo, no caso de F_INTERN_SALES e F_MARKET serem tabelas fatos distintas, seria aceitável: "(local) INS Revenue" e "(local) MKT Revenue". 

De resto, as abreviações são aceitas como descrito na seção [abreviações](#abreviações). Mas é importante notar que, ao invés de  "(local) Avg Rev" o mais adequado seria "(local) Average Revenue".

#### Organização interna das medidas

É necessário algumas definições a mais para organizar as medidas adequadamente, dessa forma, recomenda-se:

 - Retornar o resultado de uma medida dentro de uma variável

Este _guide_ define que é preferível na maioria das situações retornar o cálculo de uma medida utilizando as palavras reservadas `VAR/RETURN`, como no exemplo abaixo:

Correto: 
```
(local) ALL Soma Minutos Estudados = 

VAR _Result = 
CALCULATE(
	[(local) Soma Minutos Estudados]

	,REMOVEFILTERS(D_ATIVIDADE)
	,REMOVEFILTERS(D_ESTUDO)
)

RETURN _Result
```

Errado: 

```
(local) ALL Soma Minutos Estudados = 

CALCULATE(
	[(local) Soma Minutos Estudados]

	,REMOVEFILTERS(D_ATIVIDADE)
	,REMOVEFILTERS(D_ESTUDO)
)
```

Essa decisão decorre em virtude da facilitação de debug, caso necessário. Quando o resultado (e etapas intermediárias) está separado em uma variável é mais fácil isolar certos comportamentos.

Há casos específicos em que podemos suprimir essa _guideline_, que seriam em construções simples, de uma linha somente, com poucos termos. Como no exemplo:

```
(local) Ano Atual = YEAR(TODAY())

(local) Soma Minutos Estudados = SUM(F_ESTUDO[minutos_investidos])

```

Por último recomendados separar cada construção de variável e o próprio nome da medida por uma linha em branco do restante dos elementos.

### Palavras Reservadas

* Formatação: __UPPERCASE__

* Exemplo: CALCULATE()

* Motivo:

O DAX não é _case-sensitive_ para a suas palavras reservadas. A recomendação é utilizar exclusivamente maiúsculas para evidenciar o código e facilitar o _scan_ visual em fórmulas longas, mesmo que já exista uma cor diferente para essas palavras. 

### Variáveis

* Formatação: __CapitalizedWords__ com prefixo (`_`).

* Exemplo: VAR \_LastYear

* Motivo:

Não há um consenso de como as variáveis dentro do DAX devem ser escritas, além disso, há um [artigo de 2019](https://www.sqlbi.com/blog/marco/2019/01/15/naming-variables-in-dax/) do Marco Russo que fala sobre uma possível necessidade de utilizar underscores antes de cada variável para evitar possíveis problemas de colisão.

Apesar de colisão de nomes ser um cenário incomum (até mesmo improvável), aqui entende-se que é interessante utilizar o um único _underscore_ por uma questão visual. Com o _underscore_, fica mais fácil identificar rapidamente que um valor se trata de uma variável. 

Neste manual foi escolhido a utilização do _underscore_, porém, caso seja consenso de sua equipe, é possível não utilizá-lo. O mais importante é manter a consistência.

Por sua vez, o CapWords foi escolhido por aparecer consistentemente nos materiais de de Ferrari & Russo, como no livro "The Definitive Guide to Dax: Business Intelligence for Microsoft Power Bi, SQL Server Analysis Services, and Excel".

### Identação

Formatação: Identação pendurada (_Hanging Ident_)

Exemplo:

```
(local) KPI MoM % =

VAR _Atual = [KPI]

VAR _Anterior = 
CALCULATE(
	[KPI]

	,DATEADD(D_CALENDAR[AT_DATE], -1, MONTH)
)

VAR _Result = ROUND(DIVIDE(_Atual - _Anterior, _Anterior ), 2)

RETURN _Result
```

Motivo:

Em sentenças curtas, até 79 caracteres (mesmo número definido para sentenças em python pela [PEP 8]([https://peps.python.org/pep-0008/])), é aceitável escrever a construção inteira em uma linha. Quando o número de caracteres tende a ultrapassar esse valor, é recomendado utilizar o estilo _Hanging Ident_. Adendo, o editor de código DAX dentro do Power BI não mostra o número de caracteres de cada linha, cabe ao desenvolvedor ficar atento a esse detalhe.

O estilo escolhido se caracteriza por não haver argumentos na mesma linha em que a função construtora foi aberta, deve ser utilizada uma nova linha para separar os argumentos. O motivo principal é para distinguir visualmente uma determinada função de seus argumentos, adicionando um nível extra de identação. 

É escolhido também o fechamento do parêntese alinhado com o primeiro caractere da linha que inicia a construção multilinhas.

Este guide opta por pular a linha após a definição de uma variável, iniciando o cálculo logo após, entretanto, entende-se que caso seja de preferência da equipe é possível escrever a primeira palavra reservada nesta linha.

Então:

Recomendação:

```
VAR _Anterior = 
CALCULATE(
	...
)
```

Não recomendado, mas aceitável:

```
VAR _Anterior = CALCULATE(
	...
)
```

### CALCULATE

O calculate é uma das ferramentas mais importantes e utilizadas do DAX, desse modo, é imprescindível que o mesmo seja escrito de forma semântica e de fácil entendimento, já que o mesmo estará presente em várias medidas. Continuando, o guia irá abordar o calculate de acordo com seus componentes, que são:

```
CALCULATE(
	Expressão
	
	,Filtro1
	,Filtro2
	,FiltroN
)
```

Observe que há 2 componentes principais, primeiramente a expressão, que se caracteriza pelo cálculo a ser avaliado e, em seguida, o conjunto de filtros aplicados que representam o a modificação no contexto de filtro.

#### Expressão

A primeira indicação é que haja separação clara de maneira visual, por uma linha de diferença (por exemplo), da expressão e dos argumentos de filtro. Portanto:

Correto:

```
CALCULATE(
	Expressão
	
	,Filtro1
	,Filtro2
	,FiltroN
)
```

Errado:

```
CALCULATE(
	Expressão
	,Filtro1
	,Filtro2
	,FiltroN
)
```

#### Conjunto de filtros/ Contexto de filtro

Para essa seção recomendamos a utilização da formatação com a linha no vírgula no começo do argumento (também conhecido como _Leading Comma_).  Este _guideline_ é intuído pois facilita ao adicionar e remover argumentos de filtros, bem como comentá-los. Exemplificando: 

Situação 1, com leading comma:

```
CALCULATE(
	Expressão
	
	,Filtro1
	,Filtro2 
	//,FiltroN comentado de maneira fácil 
)
```

Situação 2, sem leading comma:

```
CALCULATE(
	Expressão
	
	Filtro1,
	Filtro2,
	// FiltroN, Calculate quebrado, necessário remover a vírgula anterior
)
```

Por fim, é necessário comentar que, em muitas situações, é oportuno designar o filtro a ser aplicado no calculate em uma variável própria, para isolar a lógica e facilitar a leitura.

#### Quebra de linha para operadores binários

Ao realizar um cálculo do tipo exemplificado abaixo, é preferível exibir o operador sempre antes, no início da linha, pois possibilita identificar de forma mais rápida como o cálculo está sendo executado.

Correto:

````
VAR _Result = CALCULATE(
	[(local) Medida1]
	+ [(local) Medida2]
	+ [(local) Medida3]

	,Filtro1
)

RETURN _Result
````

Errado:

````
VAR _Result = CALCULATE(
	[(local) Medida1] + [(local) Medida2]  + [(local) Medida3]
   
	,Filtro1
)

RETURN _Result
````

Observe que, ainda permanece uma linha em branco separando a expressão do contexto de filtro a ser aplicado.

### SELECTCOLUMNS/ADDCOLUMNS

#### Adição de colunas virtuais

Utilizando o `SELECTCOLUMNS` e o `ADDCOLUMNS` é possível criar colunas virtuais, que serão utilizadas unicamente dentro do contexto do DAX. Aqui seguiremos com a recomendação de seguir a formatação:

 - Formatação: __UPPER_CASE_WITH_UNDERSCORES__ com um prefixo (`@`).

 - Exemplo: "@UNITS" ; "@DATE_KEY"

 - Motivo:

Insistentemente abordamos o motivo, clareza visual. Nenhum outro objeto aqui definido irá começar com `@`, desta maneira, ao ler o código DAX rapidamente é possível identificar do que se trata.

### Switch

Assim como no calculate, o primeiro argumento do switch se trata de uma expressão a ser avaliada (em muitos casos setado como `TRUE()`), então, a recomendação se mantém: separar a expressão por uma linha em branco dos próximos argumentos. Portanto:

Correto:

```
SWITCH(
	TRUE(),

	...
)
```

Errado:

```
SWITCH(
	TRUE(),
	...
)
```

Os próximos componentes são um conjunto de valores e resultados, seguidos pelo  valor do else ao final. Dessa forma, recomendamos a escrita da seguinte maneira: escrever o primeiro valor juntamente da vírgula e, na linha abaixo, escrever o resultado correspondente. Fazer isso para cada dupla de valor e resultado separando-os por linhas e, por fim, adicionar o valor do else. Por conseguinte, seria escrito desta forma:



```
SWITCH(
	TRUE(),

	valor1, 
		resultado1
	
	valor2,
		resultado2,
	
	valorN,
		resultadoN,

	valorelse
)
```

Ficamos então com o seguinte exemplo:

```
SWITCH(
    TRUE(),

    _Margem >= 0.30,
        "Alta",

    _Margem >= 0.15,
        "Média",

    _Margem > 0,
        "Baixa",

    "Sem Margem"
)
```

Conseguimos então, de maneira rápida, observar o conjunto de valores e resultados de forma clara, dessa maneira, atingindo nosso objetivo.