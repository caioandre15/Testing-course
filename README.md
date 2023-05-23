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





