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
