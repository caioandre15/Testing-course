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



