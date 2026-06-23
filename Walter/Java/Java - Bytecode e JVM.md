#programming #java]

## O problema da portabilidade

Imagine que você compilou um programa no Linux.

Como executar no Windows?

Cada sistema possui instruções diferentes.

---

## A solução criada pelo [[Java]]

Em vez de compilar para cada sistema:

```
Java
 ↓
Bytecode
```

Depois:

```
Bytecode
 ↓
JVM do Windows
```

ou

```
Bytecode
 ↓
JVM do Linux
```

ou

```
Bytecode
 ↓
JVM do macOS
```

---

## O que é a JVM?

JVM significa:

**Java Virtual Machine**

Ela é um programa que interpreta ou traduz bytecode para o sistema operacional.

Fluxo:

```
Código Java
      ↓
Compilador
      ↓
Bytecode
      ↓
JVM
      ↓
CPU
```

---

## Por que isso é importante?

Por causa da famosa frase:

> "Write Once, Run Anywhere"

Escreva uma vez e execute em qualquer lugar.

O mesmo `.class` pode rodar em vários sistemas.

---

## Teoria principal do Bloco 3

A JVM cria uma camada intermediária entre o Java e o sistema operacional.

```
Java
 ↓
JVM
 ↓
Sistema Operacional
```

Essa camada fornece portabilidade e segurança.

[[Teoria por tras do Basic Training]]