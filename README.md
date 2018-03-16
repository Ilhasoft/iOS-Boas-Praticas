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
1. [Coding Style](#coding-style)
    1. Idioma
    1. Lint
    1. Documentação
1. [Stores](#stores)
    1. RxSwift
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

Usamos o XCode como IDE para construir os aplicativos, pois é a IDE recomendada pela Apple e possui o melhor suporte a `Objective-C` e `Swift`.

## Criando um novo projeto

### Versão mínima do iOS

É sempre útil que façamos uma avaliação prévia antes de escolher qual a versão mínima do iOS que vamos utilizar quando criarmos um novo aplicativo. Isso porque muitas vezes temos que prestar atenção em funcionalidades que surgem a cada versão e precisamos estar de olho. Regra geral: costumamos criar sempre projetos onde a versão mínima do iOS seja duas versões a menos que a atual.

Use essas ferramentas para colher informações para fazer a melhor escolha:

- [Apple’s world-wide iOS version penetration statistics](https://developer.apple.com/support/app-store/)
- [DavidSmith: iOS Version Stats](https://david-smith.org/iosversionstats/)

### Estrutura do Projeto

TO DO

### Localization

Mantenha todas as strings em arquivos de localization desde o início do projeto. Essa prática não é somente boa para traduções, mas torna mais fácil a busca por textos visíveis ao usuário.

### Constants

Mantenha o escopo das constantes o menor possível. Por exemplo, caso você precise das constantes somente dentro de uma determinada classe, as constantes devem estar definidias dentro desta classe. Àquelas constantes cujos escopos devem ser globais devem ser mantidas em um só lugar. Em swift, você deve usar enums definidos em um arquivo chamado 'Constants.swift' para agrupar, armazenar e acessar as constantes globais de uma maneira elegante:

```swift

enum Config {
    static let baseURL = NSURL(string: "http://www.example.org/")!
    static let splineReticulatorName = "foobar"
}

enum Color {
    static let primaryColor = UIColor(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
    static let secondaryColor = UIColor.lightGray

    // A visual way to define colours within code files is to use #colorLiteral
    // This syntax will present you with colour picker component right on the code line
    static let tertiaryColor = #colorLiteral(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
}

```

### Ignores

Um bom passo a ser tomado quando colocamos um projeto em um controle de verssão é usarmos um `.gitignore` decente. Desta forma, arquivos não desejados (user settings, arquivos temporários, etc) nunca poluirão o repositório. Por sorte, o `Github` nos proporciona arquivos `.gitignore` por default para tanto `swift` quanto `Objective-C`. Caso deseje um `.gitignore` mais elaborado, indicamos a ferramenta [gitignore.io](https://www.gitignore.io/).

## Criando um novo layout

### Por que criar layouts inteiramente via código?

* Os storyboards são mais propensos a gerarem conflitos devido a estrutura complexa do seu XML. Isto faz com que os merges sejam mais difíceis do que se criássemos a view totalmente via código.
* É mais fácil estruturar e reusar views que sejam feitas via código, mantendo assim o seu código DRY (Don't Repeat Yourself).
* Toda informação está em um só lugar. No Interface Builder você deve buscar em todos os inspectors afim de achar o que você estiver procurando.
* Os storyboard introduzem uma certa acoplação entre seu código e a UI que pode levar a diversos erros. Por exemplo: quando um IBOutlet ou IBAction não é configurada corretamente. Estes erros não são detectados pelo compilador.

### Por que utilizar XIBs ao invés de Storyboards?

* Como a estrutura do XML dos XIBs é menos complexa do que a estrutura XML de storyboards, a possibilidade de que hajam conflitos de merge é diminuída.
* A possibilidade de criar o XIB de componentes independentes aumenta a reusabilidade, o que por consequência mantém o princípio DRY.
* A possibilidade de pré-visualizar os componentes e ter uma visão mais próxima de como ele irá se comportar em um dispositivo.

### Aproveitando o melhor dos dois mundos

Você também pode utilizar o modo híbrido: comece criando um rascunho da tela no XIB, isso faz com que pequenas mudanças de layout se tornem mais fáceis e rápidas. Neste processo, você também pode chamar os designers para participarem da criação. Assim que o UI amadurecer, você pode alternar para uma abordagem mais voltada ao código afim de introduzir a lógica do negócio.

## Dependency Management

Aqui temos algumas possíveis ferramentas caso você esteja planejando incluir bibliotecas de terceiros em seu projeto. Elas estão ordenadas de acordo com a prioridade de uso em nossos projetos. Geralmente seguimos a seguinte prioridade: `Cocoapods` > `Carthage` > `Vendors`.

### Cocoapods

Devido a sua simplicidade, o [Cocoapods](https://cocoapods.org/) oferece uma integração bastante fácil e rápida. Instale-o com o seguinte comando:

    sudo gem install cocoapods

Para iniciar, acesse o diretório de seu projeto via terminal e digite:

    pod init

Este comando criará o arquivo `Podfile` que será responsável por agrupar todas as bibliotecas que serão utilizadas no projeto em um só lugar. Depois que você adicionar as bibliotecas no `Podfile`, execute no terminal o seguinte comando:

    pod install

Este comando incluirá as bibliotecas como parte do workspace de seu projeto. Geralmente é recomendado [dar commit nas dependências instaladas do seu repositório](https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html) ao invés de que cada desenvolvedor execute `pod install` depois de um novo `checkout`.

Perceba:

- A partir de agora você terá que utilizar o arquivo `.xcworkspace` ao invés de `.xcproject` ou seu projeto não compilará.
- O comando `pod update` atualizará todos as dependências em suas últimas versões permitidas pelo `Podfile`. Você pode usar alguns [operadores](http://guides.cocoapods.org/syntax/podfile.html#pod) para especificar qual versão utilizar.

### Carthage
### Vendors
### Criando Libs
### Libs mais utilizadas

## Coding Style

### Idioma
### Lint
### Documentação

## Stores

### RxSwift
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
