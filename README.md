# Curso de testes com Xunit do site desenvolvedor.IO  
O objetivo deste curso é entender e se aprofundar em testes unitários e de integração, para melhor aplicá-los.  

## Padrão de teste AAA - Arrange, Act e Assert

A seção Organizar (Arrange) de um método de teste de unidade inicializa os objetos e define o valor dos dados que são passados para o método sendo testado.  

A seção Agir (Act) invoca o método sendo testado com os parâmetros organizados.  

A seção Declarar (Assert) verifica se a ação do método em teste se comporta conforme o esperado. Para o .NET, os métodos na classe Assert geralmente são usados para verificação.  

## Nomenclaturas de teste  
Para a criação de testes são utilizados alguns padrões para a identificação de cada teste. Segue abaixo os templates:  

MetodoEmTeste_EstadoEmTeste_ComportamentoEsperado

Ex:  
AdicionarPedidoItem_ItemExistenteCarrinho_DeveIncrementarUnidadesDoItem  
RetirarItemEstoque_EstoqueAbaixoDe10Unidades_DeveEnviarEmailDeAviso  



