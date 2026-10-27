# Calculadora Lógica em Haskell

Primeiro projeto de **Programação Funcional e em Lógica** (L.EIC, 2026/27, FCUP/FEUP).

O objetivo é implementar uma *calculadora lógica*: um programa que lê proposições lógicas da entrada padrão, uma por linha, e classifica cada uma como:

| Classificação   | Significado                                                        |
|-----------------|--------------------------------------------------------------------|
| `TAUTOLOGY`     | a proposição é verdadeira para todas as atribuições de variáveis  |
| `CONTRADICTION` | a proposição é falsa para todas as atribuições de variáveis       |
| `CONTINGENT`    | a proposição pode ser verdadeira ou falsa, conforme as variáveis  |

O projeto parte da calculadora aritmética das aulas teóricas (baseada no capítulo 8 de *Programming in Haskell*, de Graham Hutton).

## Estrutura do projeto

| Ficheiro        | Descrição                                                                   |
|-----------------|-----------------------------------------------------------------------------|
| `Parsing.hs`    | Biblioteca de *parsing* funcional fornecida nas aulas (usar **sem alterações**) |
| `Calculator.hs` | Calculadora aritmética de exemplo (`+`, `*` e parênteses), ponto de partida   |
| `project1.pdf`  | Enunciado do projeto                                                        |

## Sintaxe das proposições

As proposições são representadas pelo tipo:

```haskell
data Prop = Const Bool        -- constantes (T / F)
          | Var Char          -- variáveis
          | And Prop Prop     -- conjunção
          | Or Prop Prop      -- disjunção
          | Not Prop          -- negação
```

| Símbolo   | Significado                          |
|-----------|--------------------------------------|
| `T` / `F` | constantes verdadeiro / falso        |
| `a`–`z`   | variáveis (uma letra minúscula)      |
| `!`       | negação                              |
| `&`       | conjunção                            |
| `\|`      | disjunção                            |
| `( )`     | agrupamento                          |

A conjunção tem maior precedência do que a disjunção, e ambas associam à esquerda (por exemplo, `x|y&z` lê-se `x|(y&z)`).

### Gramática

```
prop     ::= term propCont
propCont ::= '|' term propCont | ε
term     ::= factor termCont
termCont ::= '&' factor termCont | ε
factor   ::= '!' factor | name | constant | '(' prop ')'
constant ::= 'T' | 'F'
```

## Tarefas

1. **Parser:** escrever um parser para `Prop`, seguindo o modelo do parser de expressões aritméticas.
2. **Classificação:** escrever o programa completo, com uma função auxiliar `classify :: Prop -> String` que usa a tabela de verdade da proposição para a classificar.

## Compilar e executar

É necessário ter o [GHC](https://www.haskell.org/ghcup/) instalado.

```bash
# compilar
ghc -o calculadora Calculator.hs

# executar, lendo as proposições de um ficheiro
./calculadora < input.txt

# ou sem compilar
runghc Calculator.hs < input.txt
```

## Exemplo

Entrada:

```
T
a&F
a
a|!a
!p&!!p
p&T|p
p|!p&q
p&!p&p
!(p&q)|p
```

Saída esperada:

```
TAUTOLOGY
CONTRADICTION
CONTINGENT
TAUTOLOGY
CONTRADICTION
CONTINGENT
CONTINGENT
CONTRADICTION
TAUTOLOGY
```

## Entrega

- Submissão individual no servidor **Codex** até **sexta-feira, 13 de novembro de 2026, às 23:00**.
- O Codex testa com o exemplo acima e com casos adicionais, por isso convém criar testes próprios.

## Créditos

- Enunciado e código base: Pedro Vasconcelos (DCC/FCUP) e Jácome Cunha (DEI/FEUP).
- Biblioteca de parsing baseada em *Programming in Haskell*, de Graham Hutton.
