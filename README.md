# Boas Práticas no Desenvolvimento iOS

## Projeto base

Esse documento inicialmente tem por base os tópicos abordados em https://github.com/futurice/ios-good-practices, porém adaptado para as práticas utilizadas na Ilhasoft.

## Iniciando

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

