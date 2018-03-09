# Boas Práticas no Desenvolvimento iOS

_Assim como software, este documento também deve ser mantido pelos desenvolvedores. Desta forma, poderemos ter a garantia de que ele estará sempre atualizado com as nossas necessidades e costumes diários. Convidamos todos a nos ajudarem nisto - abra uma issue ou mande um pull request._

## Motivação

Sabemos que pode ser difícil mergulhar de cabeça no mundo iOS. A estrada pode ser longa e tortuosa da construção até o lançamento dos aplicativos na AppStore. Mas não se desespere! Foi por isso que criamos este documento. Ele tem como objetivo ajudar aos iniciantes e servir como guia aos desbravadores que sempre pensam e procuram fazer as coisas da "maneira correta". Tudo que escrevemos aqui são sugestões de boas práticas, mas caso tenha uma boa razão para fazer as coisas de maneira diferente, te encorajamos a seguir em frente!

## Conteúdo

Se estiver procurando por algo específico, pode ir diretamente à seção.

1. [Getting Started](#getting-started)
    1. Linguagens de Programação
    1. Cocoa Framework
    1. Human Interface Guidelines
    1. IDE
1. [Criando um novo projeto](#project-setup)
    1. Versão mínima do iOS
    1. Estrutura do Projeto
    1. Localization
    1. Constants
    1. Gitignore
1. [Criando um novo layout](#new-layout)  
    1. Por que criar layouts inteiramente via código?
    1. Por que utilizar XIBs ao invés de Storyboards?
    1. Aproveitando o melhor dos dois mundos
1. [Dependency Management](#dependency-management)
    1. Cocoapods
    1. Carthage
    1. Git Submodule
    1. Vendors
    1. Criando libs
    1. Libs mais utilizadas
1. [Arquitetura de Software](#architecture)
    1. Model-View-Controller (MVC)
    1. Model-View-ViewModel (MVVM)
    1. View-Interactor-Presenter-Entity-Routing (VIPER)
    1. Ilhasoft's own architecture
1. [Coding Style](#coding-style)
    1. Idioma
    1. Lint
    1. Documentação
1. [Stores](#stores)
    1. RxSwift
1. [Assets](#assets)
1. [Versionamento](#versionamento)  
1. [Git Flow](#git-flow)  
1. [Segurança](#security)
    1. Data Storage
    1. Logging
    1. User Interface
1. [Diagnósticos](#diagnostics)
    1. Warnings do compilador
    1. Clang Static Analyzer
    1. Faux Pas
    1. Debugging
    1. Profiling
1. [Analytics](#analytics)
    1. Crashlytics
1. [Building](#building)
    1. Configurações
    1. Targets
    1. Schemes
1. [Deployment](#deployment)
    1. Signing
    1. Provisioning
1. [In-App Purchases (IAP)](#in-app-purchases-iap)
1. [Referência](#reference)
1. [License](#license)

## Getting Started

### Linguagens de Programação

Utilizamos sempre a versão mais recente do Swift. Recomendamos uma boa lida na [documentação oficial](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309) para entender as particularidades da linguagem. Em alguns casos, algum conhecimento em Objective-C pode vir a ser útil.

### App Framework

Recomendamos a utilização do [Apple Developer  Documentation](https://developer.apple.com/documentation/) sempre que necessário.

### Human Interface Guidelines

Se você não está acostumado ou vem de outra plataforma, tire um tempo para se familiarizar com o conteúdo do [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/). As guidelines possuem um bom resumo com as nomeclaturas dos componentes de UI nativos, dimensões dos ícones etc.

### IDE

Usamos o XCode como IDE para construir os aplicativos, pois é a IDE recomendada pela Apple e possui o melhor o suporte a `Objective-C` e `Swift`.

## Criando um novo projeto

### Versão mínima do iOS

É sempre útil que façamos uma avaliação prévia antes de escolher qual a versão mínima do iOS que vamos utilizar quando criarmos um novo aplicativo. Isso porque muitas vezes temos que prestar atenção em funcionalidades que surgem a cada versão e precisamos estar de olho. Regra geral: costumamos criar sempre projetos onde a versão mínima do iOS seja duas versões a menos que a atual.

Use essas ferramentas para colher informações para fazer a melhor escolha:

- [Apple’s world-wide iOS version penetration statistics](https://developer.apple.com/support/app-store/)
- [DavidSmith: iOS Version Stats](https://david-smith.org/iosversionstats/)

### Estrutura do Projeto

TO DO

### Localization
### Constants
### Ignores

## Criando um novo layout

### Por que criar layouts inteiramente via código?

### Por que utilizar XIBs ao invés de Storyboards?

### Aproveitando o melhor dos dois mundos

## Dependency Management

### Cocoapods
### Carthage
### Git Submodule
### Vendors
### Criando Libs
### Libs mais utilizadas

## Arquitetura de Software

### Model-View-Controller (MVC)
### Model-View-ViewModel (MVVM)
### View-Interactor-Presenter-Entity-Routing (VIPER)
### Ilhasoft's own architecture

## Coding Style

### Idioma
### Lint
### Documentação

## Stores

### RxSwift

## Assets
## Versionamento
## Git Flow
## Segurança

### Data Storage
### Logging
### User Interface

## Diagnósticos

### Warnings do Compilador
### Clang Static Analyzer
### Faux Pas
### Debugging
### Profiling

## Analytics

### Crashlytics

## Building

### Configurações
### Targets
### Schemes

## Deployment

### Signing
### Provisioning

## In-App Purchases (IAP)

## Referência

Este documento baseia seus tópicos aos abordados em [Good ideas for iOS development by Futurice developers](https://github.com/futurice/ios-good-practices), mas foi traduzido e adaptado para estar de acordo com as práticas utilizadas pelos desenvolvedores da Ilhasoft.

## License
