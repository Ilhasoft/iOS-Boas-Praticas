# Boas Práticas no Desenvolvimento iOS

_Assim como software, este documento também deve ser mantido pelos desenvolvedores. Desta forma, poderemos ter a garantia de que ele estará sempre atualizado com as nossas necessidades e costumes diários. Convidamos todos a nos ajudarem nisto - abra uma issue ou mande um pull request._

### Motivação

Sabemos que pode ser difícil mergulhar de cabeça no mundo iOS. A estrada pode ser longa e tortuosa da construção até o lançamento dos aplicativos na AppStore. Mas não se desespere! Foi por isso que criamos este documento. Ele tem como objetivo ajudar aos iniciantes e servir como guia aos desbravadores que sempre pensam e procuram fazer as coisas da "maneira correta". Tudo que escrevemos aqui são sugestões de boas práticas, mas caso tenha uma boa razão para fazer as coisas de maneira diferente, te encorajamos a seguir em frente!

### Human Interface Guidelines

Se você não está acostumado ou vem de outra plataforma, tire um tempo para se familiarizar com o conteúdo do [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/). As guidelines possuem um bom resumo com as nomeclaturas dos componentes de UI nativos, dimensões dos ícones etc.

### Xcode

Usamos o XCode como IDE para construir os aplicativos, pois é a IDE recomendada pela Apple e possui o melhor o suporte a `Objective-C` e `Swift`.

### Criação do Projeto

Em geral, os projetos iOS da Ilhasoft seguem uma estrutura comum de devisão de pastas, no seguinte estilo:

ProjectName
- Classes
  - Controllers
  - Model
  - Util
  - Views

#### Por que não usar Storyboards?

Na Ilhasoft, optamos por não usar Storyboards pelo motivo de

## Referência

Este documento baseia seus tópicos aos abordados em [Good ideas for iOS development by Futurice developers](https://github.com/futurice/ios-good-practices), mas foi traduzido e adaptado para estar de acordo com as práticas utilizadas pelos desenvolvedores da Ilhasoft.
