# Boas Práticas no Desenvolvimento iOS

_Assim como software, este documento também deve ser mantido pelos desenvolvedores. Desta forma, poderemos ter a garantia de que ele estará sempre atualizado com as nossas necessidades e costumes diários. Convidamos todos a nos ajudarem nisto - abra uma issue ou mande um pull request._

## Motivação

Sabemos que pode ser difícil mergulhar de cabeça no mundo iOS. A estrada pode ser longa e tortuosa da construção até o lançamento dos aplicativos na AppStore. Mas não se desespere! Foi por isso que criamos este documento. Ele tem como objetivo ajudar aos iniciantes e servir como guia aos desbravadores que sempre pensam e procuram fazer as coisas da "maneira correta". Tudo que escrevemos aqui são sugestões de boas práticas, mas caso tenha uma boa razão para fazer as coisas de maneira diferente, te encorajamos a seguir em frente!

## Conteúdo

Se estiver procurando por algo específico, pode ir diretamente à seção.

1. [Getting Started](#getting-started)
1. [Criando um novo projeto](#criando-um-novo-projeto)
1. [Criando um novo layout](#criando-um-novo-layout)  
1. [Dependency Management](#dependency-management)
1. [Coding Style](#coding-style)
1. [Stores](#stores)
1. [Segurança](#segurança)
1. [Diagnósticos](#diagnósticos)
1. [Analytics](#analytics)
1. [Building](#building)
1. [Deployment](#deployment)
1. [In-App Purchases (IAP)](#in-app-purchases-iap)
1. [Referência](#referência)
1. [License](#license)

## Getting Started

#### Linguagens de Programação

Utilizamos sempre a versão mais recente do Swift. Recomendamos uma boa lida na [documentação oficial](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309) para entender as particularidades da linguagem. Em alguns casos, algum conhecimento em Objective-C pode vir a ser útil.

#### Human Interface Guidelines

Se você não está acostumado ou vem de outra plataforma, tire um tempo para se familiarizar com o conteúdo das [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/). Estas guidelines possuem uma bom explanação dos componentes UI nativos e suas nomeclaturas, dimensões dos ícones etc.

#### App Framework

Recomendamos a utilização do [Apple Developer  Documentation](https://developer.apple.com/documentation/) sempre que necessário.

#### IDE

Usamos o Xcode como IDE para construir os aplicativos, pois é a IDE recomendada pela Apple e possui o melhor suporte a `Objective-C` e `Swift`.

## Criando um novo projeto

#### Versão mínima do iOS

É sempre útil que façamos uma avaliação prévia antes de escolher qual a versão mínima do iOS que vamos utilizar ao criarmos um novo aplicativo. Isso porque muitas vezes temos que prestar atenção em funcionalidades que surgem a cada versão e precisamos estar de olho. Regra geral: costumamos criar sempre projetos onde a versão mínima do iOS seja duas versões a menos que a atual.

Use essas ferramentas para colher informações para fazer a melhor escolha:

- [Apple’s world-wide iOS version penetration statistics](https://developer.apple.com/support/app-store/)
- [DavidSmith: iOS Version Stats](https://david-smith.org/iosversionstats/)

#### Localization

Mantenha todas as strings em arquivos de localization desde o início do projeto. Essa prática não é somente boa para traduções, mas torna mais fácil a busca por textos visíveis ao usuário.

#### Constantes

Mantenha o escopo das constantes o menor possível. Por exemplo, caso você precise das constantes somente dentro de uma determinada classe, as constantes devem estar definidias dentro desta classe. Àquelas constantes cujos escopos devem ser globais devem ser mantidas em um só lugar. Em `Swift`, você deve usar enums definidos em um arquivo chamado `Constants.swift` para agrupar, armazenar e acessar as constantes globais de uma maneira prática e elegante:

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

#### Ignores

Um bom passo a ser tomado quando colocamos um projeto em um controle de versão é usarmos um `.gitignore` apropriado. Desta forma, arquivos não desejados (user settings, arquivos temporários, etc) nunca poluirão o repositório. Por sorte, o Github nos proporciona arquivos `.gitignore` por default para tanto `Swift` quanto `Objective-C`. Caso deseje um `.gitignore` mais elaborado, indicamos a ferramenta [gitignore.io](https://www.gitignore.io/).

## Criando um novo layout

##### Por que criar layouts inteiramente via código?

* Os storyboards são mais propensos a gerarem conflitos devido a estrutura complexa do seu XML. Isto faz com que os merges sejam mais difíceis do que se criássemos a view totalmente via código.
* É mais fácil estruturar e reusar views que sejam feitas via código, mantendo assim o seu código `DRY (Don't Repeat Yourself)`.
* Toda informação está em um só lugar. No Interface Builder você deve buscar em todos os inspectors afim de achar o que você estiver procurando.
* Os storyboard introduzem uma certa acoplação entre seu código e a UI que pode levar a diversos erros. Por exemplo: quando um IBOutlet ou IBAction não é configurada corretamente. Estes erros não são detectados pelo compilador.

##### Por que utilizar XIBs ao invés de Storyboards?

* Como a estrutura do XML dos XIBs é menos complexa do que a estrutura XML de storyboards, a possibilidade de que hajam conflitos de merge é diminuída.
* A possibilidade de criar o XIB de componentes independentes aumenta a reusabilidade, o que por consequência mantém o princípio DRY.
* A possibilidade de pré-visualizar os componentes e ter uma visão mais próxima de como ele irá se comportar em um dispositivo.

##### Aproveitando o melhor dos dois mundos

Você também pode utilizar o modo híbrido: comece criando um rascunho da tela no XIB, isso faz com que pequenas mudanças de layout se tornem mais fáceis e rápidas. Neste processo, você também pode chamar os designers para participarem da criação. Assim que o UI amadurecer, você pode alternar para uma abordagem mais voltada ao código afim de introduzir a lógica do negócio.

## Dependency Management

Aqui temos algumas possíveis ferramentas caso você esteja planejando incluir bibliotecas de terceiros em seu projeto. Elas estão ordenadas de acordo com a prioridade de uso em nossos projetos. Geralmente seguimos a seguinte prioridade: `Cocoapods` > `Carthage` > `Vendors`.

#### Cocoapods

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

#### Carthage

Vemos no [Carthage](https://github.com/Carthage/Carthage) a segunda melhor alternativa ao se adicionar bibliotecas de terceiros em seu projeto. Sua abordagem consiste em compilar as dependências em arquivos `framework` e não os adiciona "magicamente" ao projeto. Isto reduz significamente o tempo de compilação do projeto, visto que as bibliotecas já estão compiladas a priori.

No Carthage não há repositório centralizado para as bibliotecas e isto significa que qualquer biblioteca que possa ser compilada em um arquivo `framework` suporta o Carthage __out of the box__.

Para iniciar, siga a [instruções na documentação oficial](https://github.com/Carthage/Carthage#installing-carthage) do Carthage.

#### Vendors

Caso o componente que você deseja incluir no projeto não esteja disponível no `Cocoapods` e nem no `Carthage`, te aconselhamos a adicioná-lo ao código base do projeto em um diretório chamado `Vendors`.

:warning: IMPORTANTE :warning:

- Lembre de ler, avaliar e adicionar a licença de software (caso haja) aos arquivos do componente que estiverem presentes em seu projeto, dando os devidos créditos ao autor e incluindo uma `url` fonte para facilitar futuras buscas.
- Como não existirá um `Dependency Manager` vinculado ao componente, fique atento! Será responsabilidade da sua equipe manter esta parte de código. Aconselhamos a não incluir algo que não consigamos compreender.
- __SUGESTÃO__: Caso seja feita alguma mudança/melhoria ao código, lembre-se de compartilhá-la junto ao projeto de origem. Apreciamos a contribuição em projetos `open-source`.

#### Libs mais utilizadas

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
- [CleanroomLogger](https://github.com/emaloney/CleanroomLogger)

## Coding Style

#### Idioma

Preferimos sempre utilizar o idioma __inglês__ ao criarmos nossos projetos.

#### Lint

Procuramos seguir sempre boas convenções de código. Para tanto, utilizamos o [SwiftLint](https://github.com/realm/SwiftLint) como ferramenta a nos auxiliar nesta missão. O `SwiftLint` age como um agente inspecionando o nosso código em busca de `bad smells` e nos alertando que algo não está tão legal com warnings providenciais.

Para instalar o `SwiftLint`, siga as instruções em sua [documentação](https://github.com/realm/SwiftLint#installation).

Recomendamos também uma boa lida nas [Code Conventions de Ray Wenderlich](https://github.com/raywenderlich/swift-style-guide).

#### Documentação

Pregamos o bom senso com relação a documentação de código. Procuramos prevenir a necessidade de documentações extensas ao criarmos códigos legíveis. Entretanto, fique à vontade Caso haja a necessidade de explicar alguma decisão tomada. Pense sempre que seus amigos ficarão mais felizes por não precisarem perder tempo tentando entender o código que foi feito por você.

Consideramos que o autores do [NSHipster](http://nshipster.com/swift-documentation/) fizeram um bom trabalho ao definir como formatar a documentação. Recomendamos uma lida no material deles sempre que necessário. :wink:

## Stores

#### ReactiveCocoa

Na camada mais baixa de todo aplicativo geralmente os modelos são mantidos de alguma maneira, seja em um banco de dados local ou em um servidor remoto. Esta camada também é útil para abstrair atividades relacionadas com a disposição de objetos do modelo.

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

Mesmo em tempos onde confiamos nossos dados mais particulares aos nossos dispositivos portáteis, a segurança nos aplicativos continua sendo algo frequentemente negligenciado. Tente achar um bom trade-off dada a natureza de seus dados seguindo somente algumas regras que listaremos a seguir. Um bom material para iniciar é a [iOS Security Guide criada pela própria Apple](https://www.apple.com/business/docs/iOS_Security_Guide.pdf).

#### Data Storage

Caso seu aplicativo precise manter no dispositivo dados sensitivos, como nome de usuário e senha, auth token ou dados pessoais do usuário, você precisa mantê-los em um local que não possa ser acessado de fora do app. Nunca use `UserDefaults`, outros arquivos `plist` no disco ou `Core Data` para este fim porque eles não podem ser encriptados! Na maioria dos casos, o `iOS Keychain` será seu melhor amigo.

Enquanto estiver mantendo arquivos e/ou senhas, esteja seguro de determinar o nível de proteção correto (escolha-o conservadoramente). Se você precisa ter acesso enquanto o dispositivo estiver bloqueado, use `accessible after first unlock`. Em outros casos, você geralmente deve só liberar o acesso quando o dispositivo tenha sido desbloqueado.

:warning: Só mantenha dados sensitivos quando realmente necessário! :warning:

#### Networking

Mantenha encriptado com TLS todo o tráfego HTTP do aplicativo ao servidor remoto. Para evitar ataques `man-in-the-middle` que interceptem seus dados encriptados, você pode configurar um [pinning certificate](https://possiblemobile.com/2013/03/ssl-pinning-for-increased-app-security/). Bibliotecas populares como [AFNetworking](https://github.com/AFNetworking/AFNetworking) ou [Alamofire](https://github.com/Alamofire/Alamofire) já o suportam `out-of-box`.

#### Logging

Tome um cuidado extra ao configurar apropriadamente níveis de log (dica: utilize a lib `CleanroomLogger`) antes de lançar seu app. Builds em produção não devem nunca dar log em senhas, tokens de API ou similares, porque são dados sensitivos e podem vazar ao público facilmente. Por outro lado, dar log no controle de fluxo pode ajudar a descobrir problemas que seus usuários estão experenciando.

#### User Interface

Quando estiver usando um `UITextField` para inserção de senha, lembre de configurar sua propriedade `secureTextEntry` para `true` para evitar que a senha esteja visível. Você também deve disabilitar o corretor automático e limpar o campo sempre que apropriado, como quando seu app entra em background.

Quando isto acontecer, é uma boa prática limpar o `Pastboard` para evitar que o password seja descoberto de alguma maneira. Como o iOS tira screenshots para dispôr no `App Switcher`, esteja seguro de limpar qualquer dado sensitivo no UI _antes_ de retornar do `applicationDidEnterBackground`.

## Diagnósticos

#### Warnings do Compilador

Habilite todos os warnings do compilador e os trate como se fossem erros - [vai valer a pena](https://speakerdeck.com/hasseg/the-compiler-is-your-friend).

Para tratar warnings como erros em Swift, adicione `-warnings-as-errors` às configurações de compilação.

#### Clang Static Analyzer

O compilador CLang (utilizado pelo Xcode) tem um analisador estático que analisa o fluxo de dados e de controle do seu código e detecta inúmeros erros que o compilador não é capaz.

Você pode executar manualmente o analisador a partir do item de menu no Xcode: _Product -> Analyze_.

O analisador consegue trabalha em dois modos diferentes: `swallow` e `deep`. O modo `deep` é executado mais demoradamente que o modo `swallow` por conta de sua varredura profunda.

Recomendações:

- Habilite todos os checks do analisador.
- Habilite o `Analize during 'Build'` na configuração de compilação de release para que o analisador seja executado automaticamente após a compilação de releases.
- Configure o campo `Mode of Analysis for 'Analysis'` da configuração de compilação para `shallow`.
- Configure o campo `Mode of Analysis for 'Build'` da configuração de compilação para `deep`.

#### Debugging

Quando seu app falha o Xcode não abre o debugger por padrão. Para que isto aconteça, adicione um breakpoint de exceção (clique no "+" no fundo da Navigator de breakpoints do Xcode) para interromper a execução sempre que uma exceção for lançada. Isto irá capturar qualquer exceção, até mesmo as que já são tratadas.

#### Profiling

O Xcode vem com uma suite de análise de perfomance chamada `Instruments`. Ela contém uma infinidade de ferramenta para análise de uso de memória, cpu, rede, gráficos e muito mais. É de uma complexidade só! Mas um de seus casos mais simples é buscar `memory leaks` utilizando o instrumento `Allocations`: aperte o botão `Record` e filtre o sumário em alguma string útil, como o prefixo do nome das classes do seu app. A contagem da coluna `Persistence` te diz quantas instâncias de cada objeto você tem. Qualquer classe a que mostre o aumento indiscriminado na contagem indica `memory leaking`.  

## Analytics

Incluir algum framework de analytics em seu app é extremamente recomendado já que ele te permite ter insights sobre como as pessoas realmente o utilizam. Aquela feature realmente agrega valor? O botão X é ruim de clicar? Para responder essas perguntas, você pode enviar eventos, timings e outras informações mensuráveis para um serviço que as agrega e oferece uma visualização simples.

Exemplos:

- [Google Tag Manager](https://www.google.com/analytics/tag-manager/)
- [Google Analytics](https://www.google.com/analytics/)
- [Firebase Analytics](https://firebase.google.com/docs/analytics/?hl=pt-br)

#### Crashlytics

Da mesma maneira, também é interessante que se acompanhe os erros que os usuários do seu app podem estar experimentando. Para tanto, é recomendado o uso de alguma ferramenta de rastreio de erros como o [Crashlytics by Fabric](https://get.fabric.io/).

## Building

#### Configurações
#### Targets
#### Schemes

## Deployment

#### Signing
#### Provisioning

## In-App Purchases (IAP)

Quando validar recibos de compras dentro do aplicativo, lembre de garantir os seguintes itens:

- Autenticidade: O recibo vem da Apple.
- Integridade: O recibo não foi adulterado.
- O app coincide: A identificação do pacote do app no recibo coincide com o identificador do pacote da aplicação.
- O produto coincide: A ID do produto no recibo corresponda ao seu identificador de produto esperado.
- Novo recibo: Você não recebeu o mesmo ID de recibo antes.

Sempre que possível, configure seu `IAP (In-App Purchase)` de forma a guardar o conteúdo para venda no lado do servidor e somente entregue ao seu cliente em troca de um recibo válido onde todos os itens abaixos tenham sido verificados. Este tipo de configuração frustra mecanismos de pirataria comuns e - já que a validação é feita no lado do servidor - permite que você use o serviço de validação HTTP de recibos da Apple ao invés de fazê-lo a mão.

Para mais informações a respeito deste tópico, leia a seguinte matéria: [Futurice blog: Validating in-app purchases in your iOS app](https://futurice.com/blog/validating-in-app-purchases-in-your-ios-app).

## Referência

Este documento baseia seus tópicos aos abordados em [Good ideas for iOS development by Futurice developers](https://github.com/futurice/ios-good-practices), mas foi traduzido e adaptado para estar de acordo com as práticas utilizadas pelos desenvolvedores da [Ilhasoft](https://www.google.com.br/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjv1raw3vHZAhWHQpAKHRU7CLEQFggpMAA&url=http%3A%2F%2Filhasoft.com.br%2F&usg=AOvVaw3yklQnYJro06a14t-tC3lF).

## License

[Futurice](https://futurice.com/) • Creative Commons Attribution 4.0 International (CC BY 4.0)
