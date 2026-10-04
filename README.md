# Checkpoint 5 — Bug Hunt PetFiap

> Copie este arquivo para a raiz do seu repositório com o nome **README.md**
> e preencha todas as seções.

## Identificação

**Grupo:** ___

| Integrante | RM | Turma |
|---|---|---|
| | | |
| | | |
| | | |
| | | |

| Campo | |
|---|---|
| **Total de bugs corrigidos** | 12 / 12 |
| **Total de ajustes de Clean Code** | 2 / 6 |
| **Total de testes novos escritos** | 4 / 6 |
| **Suíte final (Run As → JUnit Test)** | 24 testes, 0 falhas |

---

## Parte 1 — Bugs encontrados

> Uma linha por bug, na ordem em que você os encontrou. Use a numeração dos seus
> commits (`fix: bug01 ...`). Preencha TODAS as colunas — metade da nota está aqui.

| # | Sintoma observado (o que fiz/vi) | Causa raiz (arquivo e linha aproximada) | Correção aplicada | Conceito da disciplina |
|---|---|---|---|---|
| bug01 | GeradorProtocoloTest falhava em deveManterUmaUnicaInstancia e deveGerarProtocolosSequenciais. Chamadas a getInstancia() retornavam instâncias diferentes e números não sequenciais. | GeradorProtocolo.java (~linha 19): o método getInstancia() retornava `new GeradorProtocolo()` diretamente sem atribuir à variável estática `instancia`. | Atribuída a nova instância criada à variável de classe `instancia` antes do retorno (`instancia = new GeradorProtocolo();`). | Padrão de Projeto Singleton (Aula 14) e variáveis/atributos de classe estáticos. |
| bug02 | AtendimentoBuilderTest falhava em deveMontarAtendimentoCompleto: getPetNome() retornava null em vez de "Rex". | AtendimentoBuilder.java (~linha 24): o método comPet fazia `petNome = petNome;` (autoatribuição do parâmetro), deixando o atributo do objeto sem valor. | Alterado para `this.petNome = petNome;`, atribuindo corretamente o valor recebido ao atributo da instância. | Escopo de identificadores, sombreamento de variáveis (shadowing) e referência `this` em POO. |
| bug03 | AtendimentoBuilderTest falhava em deveRecusarMontagemSemNomeDoPet: nenhuma exceção era lançada ao montar sem nome do pet. | AtendimentoBuilder.java (~linha 42): o método construir() delegava a criação sem validar se `petNome` era nulo ou vazio. | Adicionada validação de `petNome == null || petNome.isBlank()` lançando `IllegalArgumentException` no método construir(). | Padrão Builder (Aula 14) e garantia de invariantes: o objeto só nasce em estado válido. |
| bug04 | AtendimentoBuilderTest falhava em deveRecusarMontagemSemPorte: nenhuma exceção era lançada ao montar sem porte do pet. | AtendimentoBuilder.java (~linha 45): o método construir() não validava se `petPorte` era nulo ou vazio. | Adicionada validação de `petPorte == null || petPorte.isBlank()` lançando `IllegalArgumentException` no método construir(). | Padrão Builder (Aula 14), integridade de dados e validação de invariantes em POO. |
| bug05 | AtendimentoFactoryTest falhava em deveCriarTosaQuandoTipoForTosa: esperava instância de Tosa, mas recebia Banho. | AtendimentoFactory.java (~linha 17): no switch do factory, o case "TOSA" retornava `new Banho` por engano. | Corrigido o case "TOSA" para instanciar e retornar `new Tosa(...)`. | Padrão de Projeto Factory (Aula 14) e polimorfismo na criação de subclasses concretas. |
| bug06 | AtendimentoFactoryTest falhava em devePreencherOsDadosDoPetNaConsulta: os atributos do pet vinham null. | ConsultaVeterinaria.java (~linha 17): o construtor com parâmetros chamava `super();` vazio, sem repassar os dados para a superclasse. | Alterado para `super(protocolo, petNome, petPorte, tutorNome, dataHora);`, inicializando os campos da classe abstrata. | Herança (Aula 07), chamada a construtor da superclasse via `super(...)` e inicialização de atributos herdados. |
| bug07 | AgendaServiceTest falhava em deveRecusarAgendamentoComHorarioJaOcupado lançando NullPointerException. | AgendaService.java (~linha 23): comparava `a.getDataHora() == novo.getDataHora()` e `a.getPetNome() == novo.getPetNome()` usando operador de identidade `==`. | Substituído `==` por `.equals()` nas comparações de String e LocalDateTime. | Comparação lógica (`.equals()`) vs comparação de referência (`==`) em tipos de objetos (Aula 07). |
| bug08 | AgendaServiceTest falhava em deveLancarExcecaoQuandoAtendimentoNaoExiste: esperava AtendimentoNaoEncontradoException, mas nenhuma exceção era lançada. | AgendaService.java (~linha 37): o método buscarPorId envolvia o orElseThrow em um bloco try-catch genérico (catch (Exception e)) capturando a exceção e retornando null. | Removido o bloco try-catch genérico de buscarPorId, permitindo a propagação da exceção AtendimentoNaoEncontradoException quando o registro não for encontrado. | Tratamento de Exceções (Aula 11), antipadrão de engolir exceções (exception swallowing) com catch genérico e propagação de RuntimeException. |
| bug09 | BanhoTest falhava em deveCustar60ReaisParaPortePequeno: esperava 60.0 mas recebia 100.0. | Banho.java (~linha 27): os valores de retorno para porte PEQUENO (100.0) e GRANDE (60.0) estavam invertidos em calcularPreco(). | Invertidos os retornos de calcularPreco() em Banho.java, retornando 60.0 para PEQUENO e 100.0 para GRANDE conforme o contrato da aplicação. | Polimorfismo e implementação de regras de negócio em subclasses concretas (Aula 07). |
| bug10 | TosaTest falhava em deveDurar60Minutos: esperava 60 mas recebia 30 minutos. | Tosa.java (~linha 40): o método getDuracaoMinutos(String porte) continha parâmetro indevido, gerando sobrecarga (overload) em vez de sobrescrita (override) de getDuracaoMinutos() de Atendimento. | Removido o parâmetro String porte e adicionada a anotação @Override ao método getDuracaoMinutos() em Tosa.java para sobrescrever o método da superclasse. | Herança e Polimorfismo: Sobrescrita (Override) vs Sobrecarga (Overload) de métodos e anotação @Override (Aula 07). |
| bug11 | AgendaServiceTest falhava em deveRecusarCancelamentoDeAtendimentoConcluido: esperava StatusInvalidoException, mas nenhuma exceção era lançada. | Atendimento.java (~linha 63): o método cancelar() alterava o status para "CANCELADO" diretamente sem verificar se o atendimento estava no status "AGENDADO". | Adicionada validação if (!"AGENDADO".equals(status)) lançando StatusInvalidoException no método cancelar(). | Encapsulamento, integridade de estado e validação de transição de estados em POO (Aula 07 e 11). |
| bug12 | AgendaServiceTest falhava em deveRecusarAgendamentoComDataHoraNoPassado: esperava IllegalArgumentException, mas o agendamento prosseguia e consultava o banco. | AgendaService.java (~linha 20): o método agendar() não validava se a data e hora do atendimento estavam no passado antes de consultar o repositório. | Adicionada validação de dataHora no passado lançando IllegalArgumentException antes de acessar o repositório. | Integridade de dados, validação de regras de negócio em services e validação de invariantes temporais (Aula 11 e 15). |

## Parte 2 — Ajustes de Clean Code

| # | Onde estava | Qual princípio/boas práticas era violado | O que eu mudei |
|---|---|---|---|
| clean01 | AtendimentoFactory.java (~linha 14) | Nomes Significativos / Legibilidade de Parâmetros (Clean Code Cap. 2): parâmetros nomeados com letras únicas e abreviações crípticas (p, t, n, po, tu, d). | Renomeados os parâmetros do método criar() para nomes descritivos e autoexplicativos: protocolo, tipo, petNome, petPorte, tutorNome e dataHora. |
| clean02 | AtendimentoController.java (~linha 110) | Código Morto / YAGNI (Clean Code Cap. 17): método privado calcularDescontoFidelidade() nunca invocado com comentários especulativos sobre funcionalidades futuras. | Removido o método morto calcularDescontoFidelidade() e os comentários especulativos que poluíam a classe controller. |
| clean03 | | | |
| clean04 | | | |
| clean05 | | | |
| clean06 | | | |

## Parte 3 — Testes novos (regras que estavam sem cobertura)

> Uma linha por teste novo (`test: ...`). "Regra coberta" é o comportamento do
> contrato (seção 3 do enunciado) que o teste protege. Em "Resultado", diga se o
> teste ficou vermelho ao ser escrito (revelou bug — qual?) ou verde de cara
> (regra já estava correta).

| # | Teste escrito (classe.método) | Regra coberta | Resultado ao escrever (vermelho/verde) |
|---|---|---|---|
| teste01 | BanhoTest.deveCustar60ReaisParaPortePequeno | Preço do banho para porte pequeno (R$ 60,00) | Vermelho (revelou bug09) |
| teste02 | TosaTest.deveDurar60Minutos | Duração da tosa (60 minutos) | Vermelho (revelou bug10) |
| teste03 | AgendaServiceTest.deveRecusarCancelamentoDeAtendimentoConcluido | Cancelamento de atendimento já concluído recusado com StatusInvalidoException | Vermelho (revelou bug11) |
| teste04 | AgendaServiceTest.deveRecusarAgendamentoComDataHoraNoPassado | Agendamento com data/hora no passado recusado com IllegalArgumentException | Vermelho (revelou bug12) |
| teste05 | | | |
| teste06 | | | |

---

## Parte 4 — Perguntas de reflexão

> Responda com suas palavras, 5 a 10 linhas cada, **usando o código real do
> projeto como exemplo**. Respostas genéricas de tutorial não pontuam.

### 1. A suíte como contrato (Aula 15)
O projeto chegou com 20 testes, 9 vermelhos. Descreva como você usou as
mensagens de falha (ex.: `expected: <Rex> but was: <null>`) para caçar os bugs.
O que a suíte de testes tem de melhor do que testar tudo na mão com curl?

### 2. Mock e injeção de dependência (Aulas 13 a 15)
No `AgendaServiceTest`, o `@Mock` cria um `AtendimentoRepository` falso e o
`@InjectMocks` o injeta no service. Explique a relação disso com o `@Autowired`
que o Spring faz em produção — quem "injeta" em cada mundo, e por que o teste
consegue rodar sem banco e sem subir o Spring?

### 3. `==` vs `.equals()` (Aula 7)
Um dos bugs fazia o agendamento duplicado passar pela verificação de conflito.
Explique por que `==` entre Strings e `LocalDateTime` falhou aqui, por que ele
"funciona por sorte" com literais como `"Rex"`, e o que a sua correção mudou.

### 4. Sobrescrita vs sobrecarga (Aula 7)
Um dos bugs compilava sem nenhum erro: um método parecia sobrescrever
`getDuracaoMinutos`, mas na verdade criava uma assinatura nova. Explique a
diferença entre override e overload nesse caso e por que a anotação `@Override`
teria impedido o bug.

### 5. Singleton manual vs bean do Spring (Aula 14)
O `GeradorProtocolo` é um Singleton escrito à mão e causou um dos bugs.
Explique o que ele garante, qual foi o bug, e por que o `AgendaService`
(`@Service`) não corre o mesmo risco no container do Spring.

### 6. Cobertura de testes: onde parar? (Aula 15)
Dos 6 testes novos que você escreveu, alguns ficaram vermelhos (revelaram
bugs) e outros verdes de cara (regras já corretas). Vale a pena manter os que
ficaram verdes? Em um projeto real com prazo, o que você priorizaria testar:
caminho feliz, caminhos de erro, ou 100% de cobertura? Justifique.

---

## Parte 5 — Espaço livre (opcional)

Alguma dificuldade, dúvida ou comentário sobre o checkpoint?

```

```