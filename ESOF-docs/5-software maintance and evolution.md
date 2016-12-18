# Relatório 5 - Manutenção e evolução do *software*

<p align="center">
<img src="../doc/img/icon38@2x.png" alt="Logo" aligne="center">
</p>

## Índice
1. [Introdução](#introducao)
1. [Manutenção do software](#manutencao)
1. [Implementação da feature](#implementacao)
1. [Pull request](#pull)

<a name="introducao"/>
## Introdução
Na engenharia de *software*, a manutenção ou evolução é um processo de otimização e melhoria de um projeto já antes desenvolvido. Estas melhorias podem ir desde, melhoria de funcionalidades,novas funcionalidades, correção e prevenção de erros até a adaptação do programa a diferentes mudanças do sistema.

Durante a realização deste processo, temos de ter em conta que as alterações a ser realizadas, não vão afetar negativamente o programa, realizando alterações apenas nas partes pretendidas do *software*.

Ao longo do tempo é inevitável estas alterações do software devido aos seguintes factos,
* Novos requisitos que surgem quando o *software* é utilizado.
* O ambiente de negócios muda.
* Erros que são descobertos e a necessidade de seem reparados.
* Novos computadores e equipamentos são adicionados ao sistema.
* O desempenho ou a confiabilidade do *software* podem ter de ser melhorados.

Um problema chave para todas as organizações com estes tipo de projetos, é gerir a implementação de alterações no *software*.

<a name="manutencao"/>
## Manutenção do software
Como forma de avaliar a qualidade do *uBlock*, foi utilizado a ferramenta [Better Code Hub](https://bettercodehub.com/results/epassos26/uBlock), ferramenta a qual permite avaliar vários fatores do projeto, como a legibilidade e capacidade de evolução.

Com a execução da ferramenta no nosso projeto, podemos verificar que nem todas as métricas tiveram aprovação sendo que de 10, 4 falharam e 6 foram aprovadas.

[![BCH compliance](https://bettercodehub.com/edge/badge/epassos26/uBlock)](https://bettercodehub.com)

<img src="images/better_code_hub_general.png"/>


## Implementação da *feature*

Ao navegar pelos *issues* do projeto, o grupo encontrou um [*issue*](https://github.com/gorhill/uBlock/issues/2224) que acho interessante resolver.
O utilizador *WillPittenger* que mudou do *AdBlock Latitude* para o *uBlock Origin* reparou que este não tinha atalhos, de modo a facilitar a navegação.

Tendo isto em conta, o grupo implementou um atalho que liga o *Element Picker* de modo a selecionar que elementos de uma página o utilizador tenciona remover.
![*Element Picker*](images/element-picker.png)

Recentemente, o Firefox alterou a maneira como suporta atalhos para extensões, tendo migrado para uma maneira semelhante à do Google Chrome - através de um *manifest.json*.
Contudo, o ficheiro de configurações do uBlock para o Firefox ainda tem o sistema antigo de configuração. Devido a este facto, o grupo não conseguiu encontrar informação sobre como implementar esta *feature* no Firefox.
Apesar deste contra-tempo, o grupo conseguiu implementar o atalho para os browsers Opera, Google Chrome e para os navegadores que seguem o protocolo WebExt.

## *Pull Request*

Após ter finalizado a implementação, o grupo criou um [*pull request*](https://github.com/gorhill/uBlock/pull/2251) para o repositório original.
Infelizmente, o criador não é conhecido por aceitar *pull requests*, tendo explicitamente escrito no [*CONTRIBUTING.md*](https://github.com/gorhill/uBlock/blob/master/CONTRIBUTING.md) que não aceita contribuições.
