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
| **Total de bugs corrigidos** | 4 / 12 |
| **Total de ajustes de Clean Code** | 0 / 6 |
| **Total de testes novos escritos** | 0 / 6 |
| **Suíte final (Run As → JUnit Test)** | 20 testes, 4 falhas |

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
| bug05 | | | | |
| bug06 | | | | |
| bug07 | | | | |
| bug08 | | | | |
| bug09 | | | | |
| bug10 | | | | |
| bug11 | | | | |
| bug12 | | | | |

## Parte 2 — Ajustes de Clean Code

| # | Onde estava | Qual princípio/boas práticas era violado | O que eu mudei |
|---|---|---|---|
| clean01 | | | |
| clean02 | | | |
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
| teste01 | | | |
| teste02 | | | |
| teste03 | | | |
| teste04 | | | |
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
