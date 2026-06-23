#programming #java 

## O problema

O computador não entende [[Java]].

Ele entende apenas instruções de máquina.

Exemplo:

```
System.out.println("Olá");
```

Isso faz sentido para humanos, mas não para o processador.

---

## O que é compilação?

Compilar significa traduzir o código-fonte para bytecode.

Comando:

```
javac Test.java
```

Resultado:

```
Test.java      ↓javac      ↓Test.class
```

---

## O que é o arquivo `.class`?

É o resultado da compilação.

Ele contém bytecode.

Não é mais código Java legível:

```
cafe babe 0000 ...
```

É uma representação intermediária.

---

## O que é executar?

Depois da compilação:

```
java Test
```

A JVM carrega o arquivo `.class` e executa as instruções.

Fluxo:

```
Test.java    ↓javac    ↓Test.class    ↓java    ↓Execução
```

---

## Por que recompilar?

Suponha:

```
System.out.println("Hello");
```

Você altera para:

```
System.out.println("Hello Bob");
```

Mas não recompila.

A JVM continuará executando o bytecode antigo.

Por isso:

```
Alterou o .java        ↓Precisa recompilar        ↓Novo .class
```

---

## Erros de compilação

Exemplo:

```
System.out.println("Olá")
```

Faltou:

```
;
```

O compilador detecta isso antes do programa executar.

Isso é chamado de erro de compilação.

---

## Teoria principal do Bloco 2

Java é uma linguagem compilada.

Mas diferente de C ou C++, ela não compila diretamente para código de máquina.

Ela compila para bytecode.


[[Teoria por tras do Basic Training]]