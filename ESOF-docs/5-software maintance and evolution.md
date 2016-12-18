# Relatório 5 - Manutenção e evolução do *software*

<p align="center">
<img src="../doc/img/icon38@2x.png" alt="Logo" aligne="center">
</p>

## Índice
1. [Introdução](#introducao)
1. [Manutenção do software](#manutencao)
1. [Implementação da feature](#implementacao)
1. [Pull request](#pull)
1. [Grupo e contribuição](#grupo_contribuicao)

<a name="introducao"/>
## Introdução
Na engenharia de *software*, a manutenção ou evolução é um processo de otimização e melhoria de um projeto já antes desenvolvido. Estas melhorias podem ir desde, melhoria de funcionalidades,novas funcionalidades, correção e prevenção de erros até a adaptação do programa a diferentes mudanças do sistema.

Durante a realização deste processo, temos de ter em conta que as alterações a ser realizadas, não vão afetar negativamente o programa, realizando alterações apenas nas partes pretendidas do *software*.

Ao longo do tempo é inevitável estas alterações do software devido aos seguintes factos,
* Novos requisitos que surgem quando o *software* é utilizado.
* O ambiente de negócios muda.
* Erros que são descobertos e a necessidade de serem reparados.
* Novos computadores e equipamentos são adicionados ao sistema.
* O desempenho ou a confiabilidade do *software* podem ter de ser melhorados.

Um problema chave para todas as organizações com estes tipo de projetos, é gerir a implementação de alterações no *software*.

<a name="manutencao"/>
## Manutenção do software
Como forma de avaliar a qualidade do *uBlock*, foi utilizado a ferramenta [Better Code Hub](https://bettercodehub.com/results/epassos26/uBlock), ferramenta a qual permite avaliar vários fatores do projeto, como a legibilidade e capacidade de evolução.

Com a execução da ferramenta no nosso projeto, podemos verificar que nem todas as métricas tiveram aprovação sendo que de 10, 4 falharam e 6 foram aprovadas.

[![BCH compliance](https://bettercodehub.com/edge/badge/epassos26/uBlock)](https://bettercodehub.com)

<img src="images/better_code_hub_general.png"/>

Como podemos verificar pela análise do [Better Code Hub](https://bettercodehub.com/results/epassos26/uBlock) no que toca ao nível da escrita do código, a aplicação falha em 2 de 3 métricas, sendo estas a escrita de pequenas unidades de código (***Write Short Units of Code***) e a escrita de unidades simples de código (***Write Simple Units of Code***). Ao analisarmos estas métricas, podemos verificar que a análise é bem realizada, uma vez que para a métrica ***Write Simple Units of Code*** o programa reprova tendo um resultado bastante negativo uma vez que são dados o top 30 de ficheiros que ultrapassam o limite de linha definido pelo *Better Code Hub*, sendo que, o último ficheiro do top, possuí cerca de 5 vezes o código limite e o primeiro do top tem cerca de 36 vezes mais. Este resultado deve-se, provavelmente, ao facto de ser um aplicação *web* em que o proprietário possuí, mais ou menos, um ficheiro para cada função da aplicação. O facto de o autor, por vezes, não separar as funções em funções mais pequenas, de forma a melhorar a legibilidade do código contribui também para este resultado.

<p align="center">
<img src="images/write_short_units.png"/>
</p>

Relativamente a métrica ***Write Simple Units of Code***, a verificação de aprovação ou reprovação é determinada pela complexidade ciclomática (*McCabe complexity*), sendo que o programa reprova também nesta métrica pois possuí vários *branch points* o que torna a complexidade ciclomática acima do limite determinado pelo *Better Code Hub*. Devido a este resultado negativo, podemos verificar que se trata de um código cuja a alteração e testabilidade e torna mais complexa.

<p align="center">
<img src="images/write_simple_units.png"/>
</p>

**Cálculo da complexidade ciclomática**:

<p align="center">
<img src="images/McCabe_complexity.png"/>
</p>

Quanto a métrica ***Write Code Once*** a aplicação obteve um resultado positívo, o que leva a conclusão que em caso de *bugs* ou alterações de código, esta ação se torna mais simples uma vez que não havendo grande quantidade de código duplicado, exige apenas a alteração de este apenas uma vez.

Nas métricas seguintes, a aplicação apenas obteve resultado negativo em mais duas (na métrica ***Separate Concerns in Modules*** e na métrica ***Automate Tests***). Relativamente a métrica ***Separate Concerns in Modules*** a aplicação reprova uma vez que vários módulos são chamados por parte de outros,
o que torna mais complicado a alteração em certos módulos uma vez que a sua alteração pode provocar consequentes alterações que não são necessárias noutros que fazem uso do que foi alterado. Podemos verificar assim que a aplicação não possuí uma boa separação de responsabilidades.

<p align="center">
<img src="images/separate_concerns.png"/>
</p>

Quanto a métrica ***Automate Tests***, apesar o *Better Code Hub* não detetar teste automatizados o que leva a reprovação desta métrica, o *uBlock* faz uso do [*Travis CI*](https://travis-ci.org/) para a realização de testes sempre que é realizado um *push* para o repositório. No caso do *uBlock* os testes realizados são *scripts* em que é realizada a simulação da aplicação em diferentes browsers como o *Chrome*, *WebExt* e *Firefox*.

<p align="center">
<img src="images/automatetests.png"/>
</p>

Nas restantes métricas por analisar, podemos concluir através da métrica ***Keep Unit Interfaces Small*** que a aplicação é aprovada com facilidade uma vez que a função com mais parâmetros da aplicação possuí quatro parâmetros e o limite establecido pelo *Better Code Hub* é de dois parâmetros. Isto torna o código de melhor compreensão e uma maior flexibilidade no sentido de reúso. Através das métricas ***Couple Architecture Components Loosely*** e ***Keep Architecture Components Balanced*** podemos verificar que a aplicação possuí uma boa arquitectura nas componentes separando as componentes de diferentes níveis, sendo que a separação nos diferentes níveis é realizada de forma uniforme, facilitando a manutenção das componentes assim como a localização do código. Com a aprovação nas métricas ***Keep Your Codebase Small*** e ***Write Clean Code*** o também é possível concluir que o código possuí poucos *code smells* e pouco código morto (código que não tem utilidade).

<a name="implementacao"/>
## Implementação da *feature*

Ao navegar pelos *issues* do projeto, o grupo encontrou um [*issue*](https://github.com/gorhill/uBlock/issues/2224) que achou interessante resolver.
O utilizador *WillPittenger* que mudou do *AdBlock Latitude* para o *uBlock Origin* reparou que este não tinha atalhos, de modo a facilitar a navegação. Sendo o grupo constituído por elementos que fazem uso de atalhos e acham que a existência destes é uma mais valia para a eficiência do uso do *uBlock*, então o grupo decidiu implementar um atalho que liga o *Element Picker* de modo a selecionar que elementos de uma página o utilizador tenciona remover.
![*Element Picker*](images/element-picker.png)

Para a identificação da parte do código a editar, foram utilizadas as ferramentas de desenvolvimento que vêm nativamente com o Google Chrome. Estas permitem correr o programa passo a passo de modo a encontrar o pedaço exato de código a modificar. Aliado a isto, o grupo pesquisou como se abria o modo de seleção de elemento do programa pelo botão que já existia na extensão. A partir disto, foi possível arranjar a melhor maneira de lançar o código necessário.

Recentemente, o Firefox alterou a maneira como suporta atalhos para extensões, tendo migrado para uma maneira semelhante à do Google Chrome - através de um *manifest.json*.
Contudo, o ficheiro de configurações do uBlock para o Firefox ainda tem o sistema antigo de configuração. Devido a este facto, o grupo não conseguiu encontrar informação sobre como implementar esta *feature* no Firefox.
Apesar deste contra-tempo, o grupo conseguiu implementar o atalho para os browsers Opera, Google Chrome e para os navegadores que seguem o protocolo WebExt.

<a name="pull"/>
## *Pull Request*

Após ter finalizado a implementação, o grupo criou um [*pull request*](https://github.com/gorhill/uBlock/pull/2251) para o repositório original.
Infelizmente, o criador não é conhecido por aceitar *pull requests*, tendo explicitamente escrito no [*CONTRIBUTING.md*](https://github.com/gorhill/uBlock/blob/master/CONTRIBUTING.md) que não aceita contribuições.

<a name="grupo_contribuicao"/>
## Grupo e Contribuição

* Bernardo Belchior - up201405381 - 33,33%
* Edgar Passos - up201404131 - 33,33%
* José Pedro Monteiro - up201406458 - 33,33%
