# Boas Práticas no Desenvolvimento iOS

_Assim como software, este documento também deve ser mantido pelos desenvolvedores. Desta forma, poderemos ter a garantia de que ele estará sempre atualizado com as nossas necessidades e costumes diários. Convidamos todos a nos ajudarem nisto - abra uma issue ou mande um pull request._

## Motivação

Sabemos que pode ser difícil mergulhar de cabeça no mundo iOS. A estrada pode ser longa e tortuosa da construção até o lançamento dos aplicativos na AppStore. Mas não se desespere! Foi por isso que criamos este documento. Ele tem como objetivo ajudar aos iniciantes e servir como guia aos desbravadores que sempre pensam e procuram fazer as coisas da "maneira correta". Tudo que escrevemos aqui são sugestões de boas práticas, mas caso tenha uma boa razão para fazer as coisas de maneira diferente, te encorajamos a seguir em frente!

## Conteúdo

Se estiver procurando por algo específico, pode ir diretamente à seção.

1. [Getting Started](#getting-started)
  1. [Linguagens de Programação](#programming-languages)
  1. [Human Interface Guidelines](#human-interface-guidelines)
  1. [Cocoaland Framework](#native-framework)
  1. [Configurando o XCode](#xcode)
1. [Criando um novo projeto](#project-setup)
  1. [Versão mínima do iOS](#minimu-version-ios)
  1. [Estrutura do Projeto](#estrutura-do-projeto)
  1. [Localization](#localization)
  1. [Constants](#contants)
  1. [Ignores](#ignores)
1. [Criando um novo layout](#new-layout)  
  1. [Por que criar layouts inteiramente em código?](#code-layout)
  1. [Por que utilizar XIBs ao invés de Storyboards?](#xib-storyboard)
  1. [Aproveitando o melhor dos dois mundos](#o-melhor-dos-dois-muntos)
1. [Dependency Management](#dependency-management)
  1. [Cocoapods](#cocoapods)
  1. [Carthage](#carthage)
  1. [Git Submodule](#git-submodule)
  1. [Vendors](#vendors)
  1. [Criando libs](#criando-libs)
  1. [Libs mais utilizadas](#common-libraries)
1. [Arquitetura de Software](#architecture)
  1. [Model-View-Controller (MVC)](#mvc)
  1. [Model-View-ViewModel (MVVM)](#mvvm)
  1. [View-Interactor-Presenter-Entity-Routing (VIPER)](#viper)
  1. [Ilhasoft's own architecture](#ilhasoft-architecture)
1. [Coding Style](#coding-style)
  1. [Idioma Sugerido](#idiom)
  1. [Lint](#lint)
  1. [Documentação](#documentation)
1. [Stores](#stores)
  1. [RxSwift](#rx-swift)
1. [Assets](#assets)
1. [Versionamento](#versionamento)  
1. [Git Flow](#git-flow)  
1. [Segurança](#security)
  1. [Data Storage](#data-storage)
  1. [Logging](#logging)
  1. [User Interface](#user-interface)
1. [Diagnósticos](#diagnostics)
  1. [Warnings do compilador](#compiler-warnings)
  1. [Clang Static Analyzer](#clang-static-analyzer)
  1. [Faux Pas](#faux-pas)
  1. [Debugging](#debugging)
  1. [Profiling](#profiling)
1. [Analytics](#analytics)
  1. [Crashlytics](#crashlytics)
1. [Building](#building)
  1. [Configurações](#build-configurations)
  1. [Targets](#targets)
  1. [Schemes](#schemes)
1. [Deployment](#deployment)
  1. [Signing](#deployment)
  1. [Provisioning](#deployment)
1. [In-App Purchases (IAP)](#in-app-purchases-iap)
1. [Referência](#reference)
1. [License](#license)

## Getting Started

### Human Interface Guidelines

Se você não está acostumado ou vem de outra plataforma, tire um tempo para se familiarizar com o conteúdo do [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/). As guidelines possuem um bom resumo com as nomeclaturas dos componentes de UI nativos, dimensões dos ícones etc.

### Xcode

Usamos o XCode como IDE para construir os aplicativos, pois é a IDE recomendada pela Apple e possui o melhor o suporte a `Objective-C` e `Swift`.

### Estrutura do Projeto

Em geral, os projetos iOS da Ilhasoft seguem uma estrutura comum de devisão de pastas, no seguinte estilo:

ProjectName
- Classes
  - Controllers
  - Model
  - Util
  - Views

#### Por que não usar Storyboards?

TODO

## Libs mais utilizadas
## Arquitetura de Software
## Stores
## Assets
## Coding Style
## Segurança
## Diagnósticos
## Analytics
## Building
## Deployment
## In-App Purchases (IAP)
## Referência
## License

## Referência

Este documento baseia seus tópicos aos abordados em [Good ideas for iOS development by Futurice developers](https://github.com/futurice/ios-good-practices), mas foi traduzido e adaptado para estar de acordo com as práticas utilizadas pelos desenvolvedores da Ilhasoft.
