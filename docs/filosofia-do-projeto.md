---
title: Filosofia do Projeto
nav_order: 2
layout: default
---

# Introdução

<br>

Antes de mais nada, para contribuir com esse projeto, bem como para entender as decisões tomadas, é necessário estar a par da filosofia adotada. Dessa forma, o texto abaixo busca  explicar a direção que o projeto segue, além disso, é necessário ressaltar que o projeto assume que não há como alcançar uma coerência perfeita, então, trabalha de maneira realista e não dogmática.

Continuando, notadamente o texto se inspira na [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/) e a maneira elegante com que o texto é escrito, bem como suas decisões. Toda via, é preciso compreender que um texto de tal natureza para o mundo de dados, e especificamente para a plataforma Power BI, tem penetração muito menor.

Isso acontece porque o ecossistema de dados, especialmente no contexto do Power BI, ainda carece de uma cultura consolidada de padronização de trabalho semelhante à encontrada nas comunidades de desenvolvimento. Além disso, há uma gama de profissionais não técnicos (usuários de negócio, por exemplo)  utilizando as ferramentas de _self-service_ sem supervisão.

Por conta dessa conjuntura, é comum que o profissional técnico de Business Intelligence (BI), especialmente aqueles que trabalham em consultoria, herdem projetos construídos de forma "peculiar", onde a manutenção se torna custosa e, em alguns casos, inviável. Além disso, mesmo que o projeto esteja bem construído, profissionais diferentes têm formas diferentes de trabalhar e, por conta disso, é possível acabar com um projeto que não segue padrão algum.

Então, surge a pergunta: não tem padrão é necessariamente um problema? A resposta imediata é que não, todavia, a existência de um padrão proporciona alívio da carga cognitiva ao desenvolver um projeto, além do mais, proporciona coerência interna entre times.

Isto posto, este projeto busca implementar padrões com os objetivos de promover a coerência de equipes, bem como acelerar o trabalho de construção de projetos.  Continuando, os objetivos serão alcançados por meio do estabelecimento de guidelines para que os profissionais sigam durante o desenvolvimento.

# Pilares 

<br>

A manutenção deste projeto se sustenta nos pilares abaixo, citados por ordem de prioridade. Qualquer texto adicionado irá levá-los em consideração.

## Viabilidade de Adoção

<br>

Nenhum padrão é útil se não for aplicável no mundo real. A proposta deste projeto não é criar um sistema idealizado e excessivamente rigoroso, mas um conjunto de diretrizes que possam ser adotadas por equipes reais, em contextos reais, com restrições reais de tempo, maturidade técnica e governança.

Portanto:

 - As regras devem ser simples de entender;

 - Devem exigir baixo custo de implementação;

 - Não devem depender de ferramentas externas complexas;

 - Não devem criar fricção desnecessária no fluxo de desenvolvimento.

A prioridade é a  adesão prática.

## Clareza Visual

<br>

O projeto assume que a leitura rápida e o scan visual são fundamentais. Grande parte do tempo de um desenvolvedor não é escrevendo código, mas lendo código. Portanto, as decisões de formatação privilegiam identificação rápida do tipo de objeto e a hierarquia visual clara entre entidades.

## Coerência Interna

<br>

O _guideline_ busca garantir que, dentro de um mesmo projeto ou equipe, exista:

 - Consistência de nomenclatura;

 - Consistência de identação;

 - Consistência estrutural;

 - Consistência na organização lógica.

Mesmo que outro time adote outro padrão, o importante é que cada projeto seja internamente consistente.

