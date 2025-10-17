# Introdução

É fato que o exercício da análise de dados  e criação de visuais é executado por uma extensa gama de profissionais, inclusive aqueles mais próximos ao negócio. Por conta disso, uma resolução de um mesmo problema pode contar diversas abordagens. Essa característica é um dos fatores determinantes, porém não o único, para a não adoção extensa de padrões de construção.

Geralmente os padrões são definidos e seguidos por equipes ou até empresas inteiras, mas não é algo de todo o mercado. Isso não é realidade para todos os ramos de atuação da Tecnologia da Informação (TI), observe, por exemplo, os padrões da linguagem Python (PEP 8) e suas ferramentas de análise estática de código  (_linters_), ou até mesmo as conversões de nomenclatura e escrita adotadas e criadas por comunidades como a do Java e JavaScript.

Dessa forma, esse texto tem como o objetivo estabelecer determinados padrões, especificamente sob a ótica de utilização do software Power BI no desenvolvimento local (desktop). Além disso, as orientações apontadas podem ser transpostas para outros contextos do Power BI Service, com as devidas adaptações e ressalvas.

Por último, é importante ressaltar que o texto é voltado para equipes técnicas e não usuários de "Self-Service BI".
## Por que adotar padrões?

A adoção de padrões proporciona consistência no desenvolvimento. A consistência, além de usualmente promover um grau maior de organização, possibilita com que manutenção, quando feita por profissionais diferentes, seja mais simples e direta.

# Formatação

## Nomenclatura

A nomenclatura deve priorizar nomes descritivos e significativos, capazes de indicar de forma clara a função ou o conteúdo do objeto.

Recomenda-se evitar o uso de caracteres especiais, tais como acentos, cedilha (`ç`) ou símbolos fora do padrão alfanumérico utf-8. Além disso, deve-se restringir o uso ao alfabeto latino básico (A–Z, a–z) e números (0–9).

Outro ponto de atenção é a evitação de caracteres ambíguos, como a letra “l” minúscula (`l`), que pode ser facilmente confundida com o número “1” ou com a letra maiúscula “I” dependendo da fonte utilizada.

### Abreviações

O uso de abreviações em nomes deve ser evitado, pois reduz a clareza e dificulta a manutenção do código ou do modelo por terceiros. Abreviações pouco óbvias podem gerar interpretações equivocadas e comprometer a legibilidade em revisões futuras.

Exceções aceitáveis são acrônimos consolidados e amplamente reconhecidos no contexto da empresa ou aqueles relacionados à inteligência temporal (como _YTD_, _MTD_, _QTD_, _PY_). 

Nos demais casos, prefira nomes completos e descritivos, mesmo que mais longos, pois a clareza é mais importante do que economizar caracteres.

## Consultas do Power Query

Neste guia classificaremos as consultas do Power Query com  duas naturezas distintas.  Dessa forma, as consultas poderão representar tabelas finais ou tabelas intermediárias. As tabelas intermediárias são aquelas manipuladas (mescladas, acrescentadas, etc.) para produzir uma tabela final. Por sua vez, as tabelas finais serão aquelas utilizadas extensivamente no modelo, tanto para exibir visuais quanto para realizar cálculos. Os dois casos serão abordados.

### Tabelas finais

#### Tabelas fato e dimensões comuns

 * Case: D_UNDERSCORE_UPPERCASE_SINGULAR em tabelas dimensão. F_UNDERSCORE_UPPERCASE_PLURAL em tabelas fato.

Em algumas situações é possível utilizar o D_UNDERSCORE_UPPERCASE_PLURAL para dimensão visando manter a consistência, por exemplo, em casos que a dimensão já foi escrita no plural na origem.

 * Exemplo: D_STORE ; D_CLIENT; F_SALES; F_TRANSACTIONS
 
 * Motivo:

O prefixo D ou F será adotado visando uma celeridade na identificação da função da tabela no modelo, além disso, o UNDERSCORE_UPPERCASE foi escolhido para representar visualmente que as mesmas estão em um grau de "importância de objeto" superior aos demais objetos do relatório.

Ao contrário do que corriqueiramente se recomenda (utilizar nomes descritivos e amigáveis ao consumidor do relatório) aqui optou-se por manter a formalidade e utilizar a nomenclatura sem espaços, se possível, conservando o nome que foi utilizado na origem.

#### Tabelas de medidas

A orientação é de que as medidas estejam organizadas em tabelas específicas, sendo possível ter mais de uma tabela de medidas no modelo para o caso de medidas necessárias para tabelas fato diferentes. 

Dessa forma, a formatação escolhida para as tabelas de medidas é:

* Case: Title Case

* Exemplo: Medidas Faturamento

* Motivo:

Optou-se pelo Title Case tradicional devido à não necessidade de demonstrar uma hierarquia visual, sendo mais necessário ser descritivo quanto à função daquela tabela no modelo.

##### Pastas e Subpastas

Recomenda-se também que as tabelas de medidas tenham sua organização subdivida entre pastas e subpastas, separando assim as medidas por escopo e organizando de forma eficiente o visual.

* Case: 1. Title Casae

* Exemplo: 1. Metricas Base ; 4. Representatividade

* Motivo:

A necessidade de utilização de um número antes da pasta e subpasta serve para criar uma ordem entre as pastas, já que, faz mais sentido atribuir uma ordem com base na fluxo dos cálculos (ou outra ordem lógica de negócio) do que utilizar a ordenação alfabética. Então, do modo escolhido é possível criar uma hierarquia visual entre pastas e subpastas se aproveitando da organização alfanumérica.

Caso seja necessário criar medidas para debug, é recomendado criar uma pasta com a nomenclatura "0. Debug" para medidas de debug.

### Tabelas Intermediárias
#### Consultas auxiliares

* Case: aux_pq_underscore_lowercase

* Exemplo: aux_pq_clientes_de_para

* Motivo:

Escolheu-se um prefixo regular para que na visualização do modelo de dados todas as consultas estejam próximas e sejam facilmente encontradas.

Com relação ao nome que será utilizado depois do prefixo "aux_pq_" cabe ao construtor do relatório o bom senso de escolher um nome significativo e não excessivamente grande.

Quaisquer consultas que não sejam uma tabela final e os dados não sejam utilizados no visual ou construção de cálculos no relatório devem estar oculta no modelo.

## DAX

### Medidas

* Case: (local) Title Case; (local) Title Case TIMEINTELLIGENCE 

* Exemplo: (local) Revenue ; (local) Revenue PYTD

* Motivo:

O motivo da escolha do prefixo "(local)" é para indicar que as medidas foram feitas dentro do escopo de um relatório, ou seja, Power BI Desktop. Isso serve para diferenciar as medidas do relatório e não poluir um modelo semântico em específico. 

Caso as medidas sejam do modelo semântico aceito, não há necessidade do prefixo. Então, em cenários corporativos centralizados, recomenda-se omitir o prefixo

Por sua vez, o Title Case é indicado pois as medidas são exibidas nos elementos visuais. Dessa forma, para a sua utilização rápida em um visual bastaria apagar o "(local)".

Além disso, no caso de de lidar com múltiplas tabelas fatos em que medidas serão semelhantes, é aceitável indicar na medida a qual tabela ela pertence. Por exemplo, no caso de F_INTERN_SALES e F_MARKET serem tabelas fatos distintas, seria aceitável: "(local) INS Revenue" e "(local) MKT Revenue". 

As abreviações são aceitas para conceitos de inteligência temporal (que são dados avaliados sobre diferentes contextos de tempo), porém, recomendamos evitar abreviações fora disso. Por exemplo, ao invés de  "(local) Avg Rev" o mais adequado seria "(local) Average Revenue".

Para a criação de medidas é preferível que as mesmas estejam separadas em uma tabela própria, não somente por questão de organização, mas também porque caso a tabela em que a medida foi criada precise ser apagada do modelo, a medida não é apagada junto. Sim, existem casos em que é necessário apagar e recriar uma tabela.

### Palavras Reservadas

* Case: UPPERCASE

* Exemplo: CALCULATE()

* Motivo:

O DAX não é _case-sensitive_ para a suas palavras reservadas. A recomendação é utilizar exclusivamente maiúsculas para evidenciar o código e facilitar o _scan_ visual em fórmulas longas, mesmo que já exista um uma cor diferente para essas palavras. 
### Variáveis

* Case: \_PrefixUnderscoreCamelCase

* Exemplo: VAR \_LastYear

* Motivo:

Não há um consenso de como as variáveis dentro do DAX devem ser escritas, além disso, há um [artigo de 2019](https://www.sqlbi.com/blog/marco/2019/01/15/naming-variables-in-dax/) do Marco Russo que fala sobre uma possível necessidade de utilizar underscores antes de cada variável para evitar possíveis problemas de colisão.

Apesar de colisão de nomes ser um cenário incomum (até mesmo improvável), aqui entende-se que é interessante utilizar o um único _underscore_ por uma questão visual. Com o _underscore_, fica mais fácil identificar rapidamente que um valor se trata de uma variável. 

Neste manual foi escolhido a utilização do _underscore_, porém, caso seja consenso de sua equipe, é possível não utilizá-lo. O mais importante é manter a consistência.

Por sua vez, o CamelCase foi escolhido por aparecer consistentemente nos materiais de de Ferrari & Russo, como no livro "The Definitive Guide to Dax: Business Intelligence for Microsoft Power Bi, SQL Server Analysis Services, and Excel".

### Identação

Estilo: _Hanging Ident_

Exemplo: 

```
(local) KPI MoM % =

VAR _Atual = [KPI]

VAR _Anterior = CALCULATE(
	[KPI],
	DATEADD(D_CALENDAR[AT_DATE], -1, MONTH)
)

RETURN ROUND(DIVIDE(_Atual - _Anterior, _Anterior ), 2)
```

Motivo:

Em sentenças curtas, até 79 caracteres (mesmo número definido para sentenças em python pela [PEP 008]([https://peps.python.org/pep-0008/])), é aceitável escrever a construção inteira em uma linha. Quando o número de caracteres tende a ultrapassar esse valor, é recomendado utilizar o estilo _Hanging Ident_. Adendo, o editor de código DAX dentro do Power BI não mostra o número de caracteres de cada linha, cabe ao desenvolvedor ficar atento a esse detalhe.

O estilo escolhido se caracteriza por não haver argumentos na mesma linha em que a função construtora foi aberta, deve ser utilizada uma nova linha para separar os argumentos. O motivo principal é para distinguir visualmente uma determinada função de seus argumentos, adicionando um nível extra de identação. 

É escolhido também o fechamento do parêntese alinhado com o primeiro caractere da linha que inicia a construção multilinhas.

No caso do _return_ usualmente as sentenças são menores, preferencialmente escreva em uma linha só. Caso ultrapasse os 79 caracteres, vale revisar como o _return_ está sendo escrito ou até mesmo utilizar o _hanging ident_ dentro do _return_.

### Quebra de linha para operadores binários

Estilo: Leading Operator

Exemplo:

````
(local) Medida =

VAR _Resultado = CALCULATE(
    [(local) Medida1 com um nome realmente longo]
    + [(local) Medida2 com um nome realmente longo]
    + [(local) Medida3 com um nome realmente longo]
)

RETURN _Resultado
````

Motivo:

Exibir o operador sempre antes, no início da linha,  possibilita identificar de forma mais rápida como o cálculo está sendo executado. Em expressões curtas é aceitável manter em linha única separando o operador dos operandos por espaços.

