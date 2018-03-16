# Boas Práticas no Desenvolvimento iOS

_Assim como software, este documento também deve ser mantido pelos desenvolvedores. Desta forma, poderemos ter a garantia de que ele estará sempre atualizado com as nossas necessidades e costumes diários. Convidamos todos a nos ajudarem nisto - abra uma issue ou mande um pull request._

## Motivação

Sabemos que pode ser difícil mergulhar de cabeça no mundo iOS. A estrada pode ser longa e tortuosa da construção até o lançamento dos aplicativos na AppStore. Mas não se desespere! Foi por isso que criamos este documento. Ele tem como objetivo ajudar aos iniciantes e servir como guia aos desbravadores que sempre pensam e procuram fazer as coisas da "maneira correta". Tudo que escrevemos aqui são sugestões de boas práticas, mas caso tenha uma boa razão para fazer as coisas de maneira diferente, te encorajamos a seguir em frente!

## Conteúdo

Se estiver procurando por algo específico, pode ir diretamente à seção.

1. [Getting Started](#getting-started)
1. [Criando um novo projeto](#project-setup)
1. [Criando um novo layout](#new-layout)  
1. [Dependency Management](#dependency-management)
1. [Coding Style](#coding-style)
1. [Stores](#stores)
1. [Segurança](#security)
1. [Diagnósticos](#diagnostics)
1. [Analytics](#analytics)
1. [Building](#building)
1. [Deployment](#deployment)
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

Mantenha o escopo das constantes o menor possível. Por exemplo, caso você precise das constantes somente dentro de uma determinada classe, as constantes devem estar definidias dentro desta classe. Àquelas constantes cujos escopos devem ser globais devem ser mantidas em um só lugar. Em swift, você deve usar enums definidos em um arquivo chamado `Constants.swift` para agrupar, armazenar e acessar as constantes globais de uma maneira elegante:

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

Um bom passo a ser tomado quando colocamos um projeto em um controle de versão é usarmos um `.gitignore` apropriado. Desta forma, arquivos não desejados (user settings, arquivos temporários, etc) nunca poluirão o repositório. Por sorte, o `Github` nos proporciona arquivos `.gitignore` por default para tanto `swift` quanto `Objective-C`. Caso deseje um `.gitignore` mais elaborado, indicamos a ferramenta [gitignore.io](https://www.gitignore.io/).

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
- O comando `pod update` atualizará todos as dependências em suas últimas versões permitidas pelo `Podfile`. Você pode usar alguns [operadores](http://guides.cocoapods.org/syntax/podfile.html#pod) para especificar quais versões das bibliotecas seu projeto deverá utilizar.

### Carthage

Vemos no [Carthage](https://github.com/Carthage/Carthage) como a segunda melhor alternativa ao se adicionar bibliotecas de terceiros em seu projeto. Sua abordagem consiste em compilar as dependências em arquivos de framework e não os adiciona "magicamente" ao projeto. Isto reduz significamente o tempo de compilação do projeto, visto que as bibliotecas já estão compiladas a priori.

No Carthage não há repositório centralizado para as biblioteca e isto significa que qualquer biblioteca que possa ser compilada em um arquivo framework suporta o Carthage __out of the box__.

Para iniciar, siga a [instruções na documentação oficial](https://github.com/Carthage/Carthage#installing-carthage) do Carthage.

### Vendors

Caso o componente de terceiro que você deseja incluir no projeto não esteja disponível no `Cocoapods` e nem no `Carthage`, te aconselhamos a adicioná-lo ao código base do projeto em um diretório chamado `Vendors`.

:warning: IMPORTANTE :warning:

- Lembre de verificar a disponibilidade e adicionar (caso haja) a licensa de software aos componentes de terceiros presentes em seu projeto, dando os devidos créditos ao autor e incluindo uma `url` como fonte para fácil identificação.
- Como não existirá um `Dependency Manager` vinculado ao componente, esteja atento, pois será responsabilidade da sua equipe manter esta parte de código. Aconselhamos a não incluir algo que não consigamos compreender.
- __SUGESTÃO__: Caso seja feita alguma mudança/melhoria ao código, lembre-se de compartilhá-la junto ao projeto de origem. Apreciamos a contribuição em projetos `open-source`.

### Libs mais utilizadas

- [Alamofire](https://github.com/Alamofire/Alamofire)
- [Object Mapper](https://github.com/Hearst-DD/ObjectMapper)
- [TPKeyboardAvoiding](https://github.com/michaeltyson/TPKeyboardAvoiding)
- [ISOnDemandTableView](https://github.com/Ilhasoft/ISOnDemandTableView)
- [SwipeCellKit](https://github.com/SwipeCellKit/SwipeCellKit)
- [Spring](https://github.com/MengTo/Spring)
- [Parse SDK](https://github.com/parse-community/Parse-SDK-iOS-OSX)
- [Firebase SDK](https://github.com/firebase/firebase-ios-sdk)
- [Realm](https://github.com/realm/realm-cocoa)
- [SDWebImage](https://github.com/rs/SDWebImage)
- [KingFisher](https://github.com/onevcat/Kingfisher)
- [RxSwift](https://github.com/ReactiveX/RxSwift)
- [GrowingTextView](https://github.com/KennethTsang/GrowingTextView)

## Coding Style

### Idioma

Preferimos sempre utilizar o idioma __inglês__ em ao criarmos nossos projetos.

### Lint

Procuramos seguir sempre boas convenções de código. Para tanto, utilizamos o [SwiftLint](https://github.com/realm/SwiftLint) como ferramenta a nos auxiliar nesta missão. O `SwiftLint` age como um agente inspecionando o nosso código em busca de `bad smells` e nos alertando com warnings providenciais mostrando que algo não está tão legal assim.


Para instalar o `SwiftLint`, siga as instruções em sua [documentação](https://github.com/realm/SwiftLint#installation).

Recomendamos também uma boa lida nas [Code Conventions de Ray Wenderlich](https://github.com/raywenderlich/swift-style-guide).

### Documentação

Pregamos o bom senso com relação a documentação de código. Procuramos prevenir a necessidade de documentações extensas ao criarmos códigos legíveis. Caso haja a necessidade de explicar alguma decisão tomada, fique à vontade. Pense sempre que seus amigos ficarão mais felizes por não precisarem perder tempo tentando entender o código que foi feito por você.

Consideramos que o autores do [NSHipster](http://nshipster.com/swift-documentation/) fizeram um bom trabalho ao definir como formatar a documentação. Recomendamos uma lida no material deles sempre que necessário. :wink:

## Stores

### ReactiveCocoa

Na camada mais baixa de todo aplicativo geralmente os modelos estão sendo mantidos de alguma maneira, seja em disco, em um banco de dados local ou em um servidor remoto. Esta camada também é útil para abstrair atividades relacionadas com a disposição de objetos do modelo, como o caching.

Geralmente quando desejamos lançar uma requisição ao backend ou deserializar um grande arquivo em disco, fazemos isto de maneira assíncrona. Sua API deve refletir este requisito, caso contrário a execução de seu app seria interrompida enquanto não houvesse resposta das requisições a API.

Se você está usando [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa), `SignalProducer` é uma escolha natural de tipo de retorno. Por exemplo, se quiséssemos requisitar as gigs de um artista, faríamos como o seguinte código em Swift e ReactiveSwift:

```swift

func fetchGigs(for artist: Artist) -> SignalProducer<[Gig], Error> {
    // ...
}

```

Veja que o SignalProducer é somente uma forma de receber uma lista de gigs. Somente quando iniciado pelo `subscriber` ele irá realmente fazer a requisição dos gigs. Dar `unsubscribe` antes que os dados tenham sido recebidos cancelará a requisição.

:warning: Se você não deseja utilizar `signals`, `futures` ou mecanismos similares para representar seus dados futuros, você pode sempre utilizar blocos de `callback`. Mas tenha em mente que o encadeamento ou aninhamento destes blocos pode rapidamente se torna difícil de manter - condição a qual chamamos de [`callback hell`](http://callbackhell.com/). :warning:

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

Este documento baseia seus tópicos aos abordados em [Good ideas for iOS development by Futurice developers](https://github.com/futurice/ios-good-practices), mas foi traduzido e adaptado para estar de acordo com as práticas utilizadas pelos desenvolvedores da [Ilhasoft](https://www.google.com.br/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjv1raw3vHZAhWHQpAKHRU7CLEQFggpMAA&url=http%3A%2F%2Filhasoft.com.br%2F&usg=AOvVaw3yklQnYJro06a14t-tC3lF).

## License

[Futurice](https://futurice.com/) • Creative Commons Attribution 4.0 International (CC BY 4.0)
