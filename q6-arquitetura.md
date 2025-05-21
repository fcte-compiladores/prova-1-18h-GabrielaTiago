De modo abstrato, o compilador é um programa que converte código de uma
linguagem para outra. Como se fosse uma função do tipo `compilador(str) -> str`.
No caso de compiladores que emitem código de máquina ou bytecode, seria mais
preciso dizer `compilador(str) -> bytes`, mas a idéia básica é a mesma.

De forma geral o processo é dividido em etapas como abaixo

```python
def compilador(x1: str) -> str | bytes:
    x2 = lex(x1)        # análise léxica
    x3 = parse(x2)      # análise sintática
    x4 = analysis(x3)   # análise semântica
    x5 = optimize(x4)   # otimização
    x6 = codegen(x5)    # geração de código
    return x6
```

Defina brevemente o que cada uma dessas etapas realizam e marque quais seriam os
tipos de entrada e saída de cada uma dessas funções. Explique de forma clara o
que eles representam. Você pode usar exemplos de linguagens e/ou compiladores
conhecidos para ilustrar sua resposta. Salve sua resposta nesse arquivo.

# Etapas do Compilador

## lex(str) -> List[Token]

A análise léxica quebra o código fonte em tokens (unidades básicas da linguagem). Pega uma string bruta e identifica palavras-chave, identificadores, operadores, literais, etc.

**Exemplo:** `int x = 42;` vira `[Token(KEYWORD, "int"), Token(ID, "x"), Token(ASSIGN, "="), Token(NUMBER, "42"), Token(SEMICOLON, ";")]`

## parse(List[Token]) -> AST

A análise sintática constrói uma árvore sintática abstrata (AST) a partir dos tokens, verificando se a sequência segue a gramática da linguagem.

**Exemplo:** Os tokens acima viram uma AST representando uma declaração de variável com inicialização. Tipo uma árvore onde o nó raiz é "declaração", filhos são "tipo" (int), "nome" (x), "valor" (42).

## analysis(AST) -> AST anotada

A análise semântica verifica se o programa faz sentido logicamente. Checa tipos, escopo de variáveis, se funções existem, etc. Geralmente anota a AST com informações extras.

**Exemplo:** Verifica se `x + "hello"` é válido (provavelmente não é), se variáveis foram declaradas antes do uso, se tipos batem em atribuições.

## optimize(AST anotada) -> AST otimizada

Aplica otimizações para melhorar performance ou reduzir tamanho do código. Pode eliminar código morto, propagar constantes, fazer loop unrolling, etc.

**Exemplo:** `int x = 2 * 3;` vira `int x = 6;` (constant folding). Ou remove `if (false) { ... }` completamente.

## codegen(AST otimizada) -> str | bytes

Gera o código final na linguagem alvo (assembly, bytecode, JavaScript, etc.). Traduz as construções da AST para instruções específicas da plataforma.

**Exemplo:** A declaração `int x = 42;` pode virar assembly como `mov eax, 42` ou bytecode da JVM. No caso do TypeScript->JavaScript, seria basicamente remover anotações de tipo.