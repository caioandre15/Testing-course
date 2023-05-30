# Curso de testes com Xunit do site desenvolvedor.IO  
O objetivo deste curso é entender e se aprofundar em testes unitários e de integração, para melhor aplicá-los.  

## Padrão de teste AAA - Arrange, Act e Assert

A seção Organizar (Arrange) de um método de teste de unidade inicializa os objetos e define o valor dos dados que são passados para o método sendo testado.  

A seção Agir (Act) invoca o método sendo testado com os parâmetros organizados.  

A seção Declarar (Assert) verifica se a ação do método em teste se comporta conforme o esperado. Para o .NET, os métodos na classe Assert geralmente são usados para verificação.  

## Nomenclaturas de teste  
Para a criação de testes são utilizados alguns padrões para a identificação de cada teste. Segue abaixo os templates:  

#### Padrão 01  
ObjetoEmTeste_MetodoComportamentoEmTeste_ComportamentoEsperado  

Ex:  
Pedido_AdicionarPedidoItem_DeveIncrementarUnidadesSeItemJaExistente  
Estoque_RetirarItem_DeveEnviarEmailSeAbaixoDe10Unidades  

#### Padrão 02  
MetodoEmTeste_EstadoEmTeste_ComportamentoEsperado

Ex:  
AdicionarPedidoItem_ItemExistenteCarrinho_DeveIncrementarUnidadesDoItem  
RetirarItemEstoque_EstoqueAbaixoDe10Unidades_DeveEnviarEmailDeAviso  

#### O que é Mock?

Objetos mock, objetos simulados ou simplesmente mock (do inglês mock object) são objetos que simulam o comportamento de objetos reais de forma controlada. São normalmente criados para testar o comportamento de outros objetos. Em outras palavras, os objetos mock são objetos “falsos” que simulam o comportamento de uma classe ou objeto “real” para que possamos focar o teste na unidade a ser testada.  
Uma vantagem do Mock é que o objeto simulado pode ser criado dinamicamente através de um framework de Mock e poupando o desenvolvedor ter que criar uma classe física para simular aquele objeto. Uma classe física que simula o objeto costuma ser chamada de Fake, mas na teoria é um Mock, a diferença é que foi criada manualmente.  

Framework de Mock que será utilizado: Moq  
Instalação do framework  
````
PM> Install-Package Moq  
````  
Teste com apenas uma entrada [Fact]  
Teste com multiplas entradas [Theory]  

### Asserts

**Equal** compara se dois valores são iguais (Pode ser uma váriavel primitiva ou não, por exemplo uma lista) o primeiro parametro é o valor esperado e o segundo o valor atual.  
Ex:  
````
        [Fact]
        public void Calculadora_Somar_RetornarValorSoma()
        {
            // Arrange
            var calculadora = new Calculadora();

            // Act
            var resultado = calculadora.Somar(2, 2);

            // Assert
            Assert.Equal(4, resultado);
        }
````

**NotEqual** compara se dois valores não são iguais (Pode ser uma váriavel primitiva ou não, por exemplo uma lista) o primeiro parametro é o valor esperado e o segundo o valor atual.  
Ex:  
````
        [Fact]
        public void Calculadora_Somar_NaoDeveSerIgual()
        {
            // Arrange
            var calculadora = new Calculadora();

            // Act
            var result = calculadora.Somar(1.13, 2.23);

            // Assert
            Assert.NotEqual(3.3, result, precision: 1);
        }
````

**Contains** Verifica se um valor existe dentro da coleção ou variável. o primeiro parametro é o trecho ou valor esperado e o segundo o valor com o trecho completo ou lista.  
Ex:  
````
        [Fact]
        public void StringsTools_UnirNomes_DeveConterTrecho()
        {
            // Arrange
            var sut = new StringsTools();

            // Act
            var nomeCompleto = sut.Unir("Eduardo", "Pires");

            // Assert
            Assert.Contains("ardo", nomeCompleto);
        }
````

**DoesNotContain** Verifica se um valor não existe dentro da coleção ou variável. o primeiro parametro é o trecho ou valor esperado e o segundo o valor com o trecho completo ou lista.  
Ex:  
````
        [Fact]
        public void Funcionario_Habilidades_JuniorNaoDevePossuirHabilidadeAvancada()
        {
            // Arrange & Act
            var funcionario = FuncionarioFactory.Criar("Eduardo", 1000);

            // Assert
            Assert.DoesNotContain("Microservices", funcionario.Habilidades);
        }
````

**StartsWith** Compara se a string começa conforme o valor passado:
Ex:  
````
        [Fact]
        public void StringsTools_UnirNomes_DeveComecarCom()
        {
            //Arrange
            var sut = new StringsTools();

            // Act
            var nomeCompleto = sut.Unir("Eduardo", "Pires");

            // Assert
            Assert.StartsWith("Edu", nomeCompleto);
        }
````

**EndsWith** Compara se a string termina conforme o valor passado:
Ex:  
````
        [Fact]
        public void StringsTools_UnirNomes_DeveAcabarCom()
        {
            //Arrange
            var sut = new StringsTools();

            // Act
            var nomeCompleto = sut.Unir("Eduardo", "Pires");

            // Assert
            Assert.EndsWith("res", nomeCompleto);
        }
````

**Matches** Valida um texto por uma expressão regular:
Ex:  
````
        [Fact]
        public void StringsTools_UnirNomes_ValidarExpressaoRegular()
        {
            //Arrange
            var sut = new StringsTools();

            // Act
            var nomeCompleto = sut.Unir("Eduardo", "Pires");

            // Assert
            Assert.Matches("[A-Z]{1}[a-z]+ [A-Z]{1}[a-z]+", nomeCompleto);
        }
````

**True** e **False** Valida se uma expressão é verdadeira e ou falsa, respectivamente:
Ex:  
````
        [Fact]
        public void Funcionario_apelido_NaoDeveterApelido()
        {
            // Arrange & Act
            var funcionario = new Funcionario("Eduardo", 1000);

            // Assert
            Assert.Null(funcionario.Apelido);

            // Assert Bool
            Assert.True(string.IsNullOrEmpty(funcionario.Apelido));
            Assert.False(funcionario.Apelido?.Length > 0);
        }
````

**InRange** e **NotRange** Valida se um valor está dentro de um range de valores, e que não está respectivamente:
Ex:  
````
        [Theory]
        [InlineData(700)]
        [InlineData(1500)]
        [InlineData(2000)]
        public void Funcionario_Salario_FaixasSalariaisDevemRespeitarNivelProfissional(double salario)
        {
            // Arrange & Act
            var funcionario = new Funcionario("Eduardo", salario);

            // Assert
            if (funcionario.NivelProfissional == NivelProfissional.Junior)
                Assert.InRange(funcionario.Salario, 500, 1999);

            if (funcionario.NivelProfissional == NivelProfissional.Pleno)
                Assert.InRange(funcionario.Salario, 2000, 7999);

            if (funcionario.NivelProfissional == NivelProfissional.Senior)
                Assert.InRange(funcionario.Salario, 8000, double.MaxValue);

            Assert.NotInRange(funcionario.Salario, 0, 499);
        }
````

**IsType** Valida o tipo de uma variável é igual ao tipo informado:
Ex:  
````
        [Fact]
        public void FuncionarioFactory_Criar_DeveRetornarTipoFuncionario()
        {
            // Arrange & Act
            var funcionario = FuncionarioFactory.Criar("Eduardo", 10000);

            // Assert
            Assert.IsType<Funcionario>(funcionario);
        }
````

**IsAssignableFrom** Valida uma variável é vem de um tipo ou é derivada do tipo informado:
Ex:  
````
        [Fact]
        public void FuncionarioFactory_Criar_DeveRetornarTipoDerivadoPessoa()
        {
            // Arrange & Act
            var funcionario = FuncionarioFactory.Criar("Eduardo", 10000);

            // Assert
            Assert.IsAssignableFrom<Pessoa>(funcionario);
        }
````

**All** Valida todos os valores de uma lista através de uma expressão. Se passa primeiro a lista e depois a expressão validando, como se estivesse validando apenas um objeto:  
Ex:  
````
        [Fact]
        public void Funcionario_Habilidades_NaoDevePossuirHabilidadesVazias()
        {
            // Arrange & Act
            var funcionario = FuncionarioFactory.Criar("Eduardo", 10000);

            // Assert
            Assert.All(funcionario.Habilidades, habilidade => Assert.False(string.IsNullOrWhiteSpace(habilidade)));
        }
````

*Throws* Valida se uma exception foi lançada:  
Ex1:  
````
        [Fact]
        public void Calculadora_Dividir_DeveRetornarErroDivisaoPorZero()
        {
            // Arrange
            var calculadora = new Calculadora();

            // Act & Assert
            Assert.Throws<DivideByZeroException>(() => calculadora.Dividir(10, 0));
        }
````
Ex2:  
````
[Fact]
        public void Funcionario_Salario_DeveRetornarErroSalarioInferiorPermitido()
        {
            // Arrange & Act & Assert
            var exception =
                Assert.Throws<Exception>(() => FuncionarioFactory.Criar("Eduardo", 250));

            Assert.Equal("Salario inferior ao permitido", exception.Message);
        }
````

### Traits
Serve para organizar os testes criando-se categorias para os testes. É importante utilizar em conjunto com o DisplayName para melhorar a visualização:  
Modelo:  
````
[Fact(DisplayName = "Nome do teste")]
[Trait("Categoria", "Valor da Categoria"]
````
Ex:  

````
[Fact(DisplayName = "Novo Cliente Válido")]  
[Trait("Categoria", "Cliente Trait Testes"]
````
### Fixture  
É uma implementação fixa que permitir o reuso de classes, objetos, banco de dados para uma mesma classe de teste sem se preocupar com o estado, pois framework se encarrega disso. Com ele, deixamos o Arrange dos testes limpo e organizado.  [Fixture Doc](https://xunit.net/docs/shared-context)

A classe de teste é criada a cada teste executado, ou seja, se uma classe/objeto/banco foi instanciado irá perder a referência. Sendo recriada a cada execução de cada teste.

Já utilizando o Fixture a classe/objeto/banco é criado e ele fica disponivel a todos os testes da mesma classe que estiverem rodando naquela coleção. 
Pois, só perde a referência depois que todos os testes terminam.

Como implementar classe Fixture:

1) Criar uma classe ex: ClienteTestsFixture  
2) Deve implementar o IDisposable (Por essa classe ser reaproveitavel entre as outras classes de testes)  
3) Criar métodos que serão utilizados em vários testes.  
4) Criar uma classe de coleção ex: ClienteCollection que implementa a interface ICollectionFixture<> passando a classe ClienteTestsFixture como tipo ICollectionFixture<ClienteTestsFixture> (Dentro do mesmo arquivo da classe)  
5) Implementar uma definição de coleção: CollectionDefinition(nameof(ClienteCollection))   
6) Implementar nas classes testes a classe ClienteTestsFixture atráves de injeção de dependência via construtor.  
7) Depois implementação no Arrange, chamando os métodos.  
8) Implentar IClassFixture<> em cada classe de teste passando a classe Fixture como tipo IClassFixture<ClienteTestsFixture>.  

Ex. de classe Fixture:  
````
    [CollectionDefinition("ClienteCollection")]
    public class ClienteCollection : ICollectionFixture<ClienteTestsFixture>
    {}
    public class ClienteTestsFixture : IDisposable
    {
        public Cliente GerarClienteValido(){}
        public Cliente GerarClienteInvalido(){}
        public void Dispose(){}
    }    
````
Ex: de classe de teste:  
````
[CollectionDefinition(nameof(ClienteCollection))]
    public class ClienteTesteValido : IClassFixture<ClienteTestsFixture>
    {
        private readonly ClienteTestsFixture _clienteTestsFixture;

        public ClienteTesteValido(ClienteTestsFixture clienteTestsFixture)
        {
            _clienteTestsFixture = clienteTestsFixture;
        }

        [Fact(DisplayName = "Novo Cliente Válido")]
        [Trait("Categoria", "Cliente Trait Testes")]
        public void Cliente_NovoCliente_DeveEstarValido()
        {
            // Arrange
            var cliente = _clienteTestsFixture.GerarClienteValido();

            // Act
            var result = cliente.EhValido();

            // Assert
            Assert.True(result);
            Assert.Equal(0, cliente.ValidationResult.Errors.Count);
        }
    }
````
### Ordenação de Testes  
Tem o objetivo de impor uma ordem de execução nos testes, que normalmente são executados aleatoriamente. Faz sentido utilizar esta ordenação para testes de integração, ex: 
"Cliente é criado no banco de dados e em seguida é realizado o login do mesmo". Já para testes unitários não faz sentido.  

Para realizar a ordenação é necessário criar uma classe chamada **PriorityOrderer** e outra **TestPriorityAttribute**. Que seguem abaixo.  
````
[AttributeUsage(AttributeTargets.Method)]
    public class TestPriorityAttribute : Attribute
    {
        public TestPriorityAttribute(int priority)
        {
            Priority = priority;
        }

        public int Priority { get; }
    }

    public class PriorityOrderer : ITestCaseOrderer
    {
        public IEnumerable<TTestCase> OrderTestCases<TTestCase>(IEnumerable<TTestCase> testCases) where TTestCase : ITestCase
        {
            var sortedMethods = new SortedDictionary<int, List<TTestCase>>();

            foreach (var testCase in testCases)
            {
                var priority = 0;

                foreach (var attr in testCase.TestMethod.Method.GetCustomAttributes((typeof(TestPriorityAttribute).AssemblyQualifiedName)))
                    priority = attr.GetNamedArgument<int>("Priority");

                GetOrCreate(sortedMethods, priority).Add(testCase);
            }

            foreach (var list in sortedMethods.Keys.Select(priority => sortedMethods[priority]))
            {
                list.Sort((x, y) => StringComparer.OrdinalIgnoreCase.Compare(x.TestMethod.Method.Name, y.TestMethod.Method.Name));
                foreach (var testCase in list)
                    yield return testCase;
            }
        }

        private static TValue GetOrCreate<TKey, TValue>(IDictionary<TKey, TValue> dictionary, TKey key) where TValue : new()
        {
            if (dictionary.TryGetValue(key, out var result)) return result;

            result = new TValue();
            dictionary[key] = result;

            return result;
        }
    }    
````
Depois basta adicionar o atributo TesteCase order passando namespace da classe Priority e o namespace da classe de teste. Segue exemplo abaixo.  
        
````
[TestCaseOrderer("Features.Tests.PriorityOrderer", "Features.Tests")]
    public class OrdemTestes
    {
        public static bool Teste1Chamado;

        [Fact(DisplayName = "Teste 04"), TestPriority(3)]
        [Trait("Categoria", "Ordenacao Testes")]
        public void Teste04()
        {}        
````
*Geração de dados aleatórios humanizados*  

Porque gerar dados aleátorios?  
Em um cenário real em produção existirão cenários diversos de cadastro e ao realizar o teste de unidade para apenas um cenário
estamos realizando um teste probre que pode estar deixando algum cenário despercebido. Por isso utilizamos a geração de dados aleatórios, para garantir que 
realmente seu cadastro está valido perante a N cenários.  

Para isso temos o framework [**Bogus**](https://github.com/bchavez/Bogus) que é um gerador de dados Fake.
Para instalá-lo:  
````
Install-Package Bogus
````
Utilização sem construtor:  
````
var genero = new Faker().PickRandom<Name.Gender>();
var email = new Faker().Internet.Email("eduardo", "pires", "gmail");
var clienteFaker = new Faker<Cliente>();
clienteFaker.RuleFor(c => c.Nome, (f, c) => f.Name.FirstName());
````

Com construtor na classe:          
````
var cliente = new Faker<Cliente>("pt_BR")
                .CustomInstantiator(f => new Cliente(
                    Guid.NewGuid(),
                    f.Name.FirstName(genero),
                    f.Name.LastName(genero),
                    f.Date.Past(80, DateTime.Now.AddYears(-18)),
                    "",
                    true,
                    DateTime.Now))
                .RuleFor(c => c.Email, (f, c) =>
                   f.Internet.Email(c.Nome.ToLower(), c.Sobrenome.ToLower()));
````        
### Trabalhando com Mock  

Ao tentar realizar o teste de um classe de serviço, percebemos que para instanciar a classe que provisiona os dados fixos e aleatórios não temos problemas (Fixture), mas
quando tentamos instanciar a classe de serviço percebemos que é solicitado instanciar as classes solicitas em seus parâmetros. Como queremos testar apenas a unidade
, ou seja, um método em especifico, se faz necessário o uso do MOQ que é uma classe que simula estas intancias das classes solicitadas. Para que não seja necessário uma conexão com o banco de dados, por exemplo.  

Utilizaremos o framework MOQ. Para instalá-lo selecionar o projeto de teste no Packge Manager e executar o comando:  
````
install-package  MOQ
````

Para instanciar a classe Mock podemos instanciar um objeto genérico, ou de um tipo específico, passando a interface, ou seja o contrato daquela classe, conforme abaixo.  
Ex:  
````
// Arrange
var cliente = _clienteTestsFixtureBogus.GerarClienteValido();        
var clienteRepo = new Mock<IClienteRepository>();
var mediatr = new Mock<IMediator>();
```` 

Agora estas váriaveis são do tipo Mock, para que sejam passadas diretamente como parâmetros na classe de serviço, ou seja, passar o tipo esperado, é necessário inferir o .object em cada uma delas.  
Ex:  
````
var clienteService = new ClienteService(clienteRepo.object, mediatr.object)
````         

Para invocar o método basta utilizar a classe de serviço chamando o método desejado e passando a váriavel com os dados criados aleatóriamente.  
````
// Act
clienteService.Adicionar(cliente);
````

Para testar se o método foi executado com sucesso, utilizamos o recurso Verify do Mock para validar se os métodos chamados por ele foram chamado uma vez. (no caso deve ser chamado apenas uma vez pois se trata de um cadastro e uma publicação de mensagem).

````
// Assert
clienteRepo.Verify(c => c.Adicionar(cliente), Times.Once());
````

No caso acima de cadastro de cliente, para que o teste ocorra corretamente, devemos passar o mesmo objeto criado no Arrange.

````
// Assert
mediatr.Verify(m => m.Publish(It.IsAny<INotification>(), CancellationToken.None), Times.Once); 
````

No caso acima de publicação de mensagem, o método **Publish()** necessita que seja passado um objeto **Notification**, e para que não seja necessário passar especificamente o objeto Notification instanciando um novo mock, podemos passar um objeto genérico **(It.IsAny<INotification>())** que implementa a classe INotification.
Como o **Publish()** instancia um cancelation token padrão, também é necessário passar o CancellationToken.None como parâmetro.  

Em ambas verificações é utilizado o método Verify validando a expressão e quantas vezes o método foi chamado. **Verify(expression, times)**.    

### Teste com método que possuí retorno (Setup)  

No caso de um teste de um método que possúi retorno de dados ao realizar o mock precisamos gerar os dados falsos e configurar o método atráves do atribruto da funcionalidade Setup() para que os valores sejam retornados. 
**Setup** Devemos pensar que Setup significa que eu vou ensinar o método a fazer algo conforme eu quero.  
Ex:           
````        
clienteRepo.Setup(c => c.ObterClientesVariados())
     .Returns(_clienteTestsBogus.ObterClientesVariados())
````

Para que os dados falsos sejam gerados em larga escala existe a funcionalidade **Generate** do **Bogus** que nos ajuda muito com isso.  
Ex:
````        
return clientes.Generate(quantidade);        
````        

### AutoMock
Automock é uma feature que facilita a criação dos mocks. Com ele não é necessário mais saber quais são dependências do construtor que um classe possuí para ser instanciada, a responsabilidade desta ação que pode ser trabalhosa é delegada para o AutoMock.

Comando para instalar o automock:  
````
install-package MOQ.automock
````

Como utilizar o AutoMock:  

1) Remover os mocks do teste  
2) Instanciar o AutoMocker  
````
var mocker = new AutoMocker();
````
3) Criar a instancia do objeto utilizando o método **CreateInstance<CLASSE CONCRETA>()** passando a classe concreta e não a interface.  
````
var clienteService = mocker.CreateInstance<ClienteService>();
````
4) Para acessar o método Verify no Assert, precisamos utilizar o método **GetMock<INTERFACE>()** passando a interface. Pois, o mock precisa ser exposto para conseguir acessar este método.  
````
mocker.GetMock<IClienteService>().Verify()
````
Funciona da mesma forma para utilizar o setup():  
````
mocker.GetMock<IClienteRepository>().Setup(c => c.ObterTodos())
                .Returns(_clienteTestsFixtureBogus.ObterClientesVariados());
````


Como refatorar seu uso do AutoMocker utilizando uma Fixture e otimizando o construtor, para não inicializar o AutoMock em cada teste:  

1) Criar um Fixture adicionando duas propriedade públicas que serviram para qualquer teste que implemente esta Fixture  
````
public ClienteService ClienteService;
public AutoMocker Mocker;
````
2) Criar um método que irá trabalhar estas instancias e devolve-las.  
````
public ClienteService ObterClienteService()
{
	Mocker = new AutoMocker();
	ClienteService = Mocker.CreateInstance<ClienteService>();
	return ClienteService;
}
````
3) Alterar a Coleção da Fixture da Classe Teste.  
4) Remover instancia do AutoMocker no Arrange e substituir por esta chamada ao método ObterClienteService():  
````
var clienteService = _clienteTestsAutoMockerFixture.ObterClienteService();
````
5) No Assert em verify alterar adionar o .Mocker para referênciar corretamente:  
````
_clienteTestsAutoMockerFixture.Mocker.GetMock<IClienteRepository>().Verify();
````
6) Refatorar construtor adionando a criação do cliente service no mesmo:  
````
private readonly ClienteService _clienteService;
public ClienteServiceAutoMockFixtureTests(ClienteTestsAutoMockerFixture clienteTestsFixtureAutoMocker)
        {
            _clienteTestsAutoMockerFixture = clienteTestsFixtureAutoMocker;
            _clienteService = _clienteTestsAutoMockerFixture.ObterClienteService();
        }
````

### Fluent Assertions  

Instalação do pacote: 
````
install-package FluentAssertions
````

A ideia e substituir os Asserts tradicionais deixando mais clara a escrita.  

Cliente Válido - Assert True:  
````
// Assert Tradicional
Assert.True(result);
// Assert FluentValidations
result.Should().BeTrue();
````
Cliente Válido - Assert Equal:  
````
// Assert Tradicional
Assert.Equal(0, cliente.ValodationResult.Errors.Count);
// Assert FluentValidations
cliente.ValidationResult.Errors.Should().HaveCount(0);
````

Cliente inválido - Assert False:  
````
// Assert Tradicional
Assert.False(result);
// Assert FluentValidations
result.Should().BeFalse();
````
Cliente inválido - Assert NotEqual:  
````
// Assert Tradicional
Assert.NotEqual(0, cliente.ValidationResult.Errors.Count);
// Assert FluentValidations
cliente.ValidationResult.Errors.Should().HaveCountGreaterOrEqualTo(15, "Deve possuir erros de validação");
````
Neste caso foi utlizada a cláusula **Because** para retornar uma mensagem, quando não atender a validação.  

Cliente Service - Assert True:  
````
// Assert Tradicional
Assert.True(clientes.Any());
// Assert FluentValidations
clientes.Should().HaveCountGreaterOrEqualTo(1).And.OnlyHaveUniqueItems();
````
Neste caso foi utlizada a cláusula **And** para testar também se são clientes unicos.

Cliente Service - Assert False:  
````
// Assert Tradicional
Assert.False(clientes.Count(c => !c.Ativo) > 0);
// Assert FluentValidations
clientes.Should().NotContain(c => !c.Ativo);
````	
        
### Skip - Serve para pular um teste específico.  
Ex:  
````
[Fact(DisplayName = "Novo Cliente 2.0", Skip = "Nova versão 2.0 quebrando")]
````
 

        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
