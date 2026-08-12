#java #programming 

<iframe src="https://github.com/walterneto0/liferay-basic-training/blob/master/docs/java" width="100%" height="600"></iframe>

---

Resumo da Progressão

```
1. Estrutura de um programa Java
            ↓
2. Compilação e execução
            ↓
3. Bytecode e JVM
            ↓
4. Imports e bibliotecas padrão
            ↓
5. Objetos e instanciação
            ↓
6. Bibliotecas externas (JAR)
            ↓
7. Packages
            ↓
8. Orientação a Objetos
            ↓
9. Memória e passagem de parâmetros
            ↓
10. Gradle e gerenciamento de projetos
```

Essa sequência forma uma base sólida para avançar para temas mais usados em projetos Java corporativos, como Maven/Gradle avançado, Servlets, Spring e desenvolvimento de módulos Liferay.

---

## Bloco 1 - 4

<div style="display:flex; gap:10px; overflow-x:auto;">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/1.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/2.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/3.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/4.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/5.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/6.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/7.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/8.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/9.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/10.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/11.jpg" width="500">
  <img src="Slides/Teoria por tras do Basic Traning - Blocos 1 - 4/12.jpg" width="500">
</div>

### Bloco 1 — [[Fundamentos de um Programa Java]]

Objetivo: entender a estrutura mínima de uma aplicação [[Java]].

### Conceitos

- Arquivo `.java`
- Classe (`class`)
- Método `main`
- `System.out.println`

### Exemplo

```
public class Test {    public static void main(String[] args) {        System.out.println("Hello, World!");    }}
```

### Resultado esperado

Entender como um programa Java começa e qual é o ponto de entrada da aplicação.

---

### Bloco 2 — [[Compilação e Execução Java]]

Objetivo: entender a diferença entre escrever código e executá-lo.

### Conceitos

- Código-fonte (`.java`)
- Compilação (`javac`)
- Arquivo compilado (`.class`)
- Execução (`java`)
- Erros de compilação

### Fluxo

```
Test.java    ↓ javacTest.class    ↓ javaPrograma executando
```

### Resultado esperado

Compreender que Java não executa diretamente o arquivo `.java`.

---

### Bloco 3 — [[Java - Bytecode e JVM]]

Objetivo: entender o que acontece após a compilação.

### Conceitos

- Bytecode
- Arquivo `.class`
- JVM (Java Virtual Machine)

### Fluxo

```
Código Java    ↓Compilador    ↓Bytecode (.class)    ↓JVM    ↓Sistema Operacional
```

### Resultado esperado

Saber por que Java é multiplataforma.

---

### Bloco 4 — [[Imports e Bibliotecas Nativas Java]]

Objetivo: utilizar classes já existentes.

### Conceitos

- Import
- Pacotes
- Biblioteca padrão Java

### Classes utilizadas

```
import java.util.Random;import java.util.Date;
```

### Conceitos adicionais

```
java.lang
```

Pacote carregado automaticamente.

Exemplos:

```
StringSystemMathStrictMath
```

### Resultado esperado

Entender de onde vêm as classes utilizadas no código.

---


## Bloco 5 - 10

### Bloco 5 — Objetos e Instanciação

Objetivo: criar objetos.

### Conceitos

- Classe
- Objeto
- Instância
- Operador `new`

### Exemplos

```
Random random = new Random();
```

```
Date date = new Date();
```

### Resultado esperado

Entender a diferença entre uma classe e um objeto criado a partir dela.

---

### Bloco 6 — Bibliotecas Externas

Objetivo: usar código criado por terceiros.

### Conceitos

- Arquivos JAR
- Dependências
- Classpath

### Exemplo

```
import org.apache.commons.math3.util.ArithmeticUtils;
```

### Problemas abordados

```
ClassNotFoundException
```

```
NoClassDefFoundError
```

### Resultado esperado

Entender como Java localiza bibliotecas externas.

---

### Bloco 7 — Packages

Objetivo: organizar projetos maiores.

### Conceitos

```
package com.acme.able;
```

### Estrutura

```
com└── acme    ├── able    ├── baker    └── charlie
```

### Execução

```
java com.acme.able.Test
```

### Resultado esperado

Entender a relação entre:

```
Diretórios↔Packages↔Classes
```

---

### Bloco 8 — Orientação a Objetos

Objetivo: modelar comportamentos usando classes.

## 8.1 Classes e Herança

### Conceitos

```
class Apple extends Fruit
```

```
class Lemon extends Fruit
```

### Resultado

Reutilização de código.

---

## 8.2 Classes Abstratas

### Conceito

```
abstract class Fruit
```

### Método abstrato

```
public abstract String getColor();
```

### Resultado

Definir contratos para classes filhas.

---

## 8.3 Polimorfismo

### Exemplo

```
Fruit fruit = new Apple();
```

### Resultado

Trabalhar com diferentes tipos usando a mesma interface.

---

## 8.4 Override

### Exemplo

```
public boolean isSweet() {    return false;}
```

### Resultado

Modificar comportamento herdado.

---

## 8.5 Overload

### Exemplo

```
getColor()
```

```
getColor(boolean ripe)
```

### Resultado

Mesmo método com parâmetros diferentes.

---

### Bloco 9 — Como Java Passa Parâmetros

Objetivo: compreender o modelo de memória.

### Conceitos

## Tipos primitivos

```
int value = 10;
```

Mudanças não afetam a variável original.

---

## Objetos

```
AtomicInteger
```

Mudanças internas podem ser observadas.

---

## Arrays

```
int[] values
```

Também permitem alterações no conteúdo.

---

### Conceito principal

Java é:

```
Pass-by-Value
```

Mesmo quando trabalha com objetos.

### Resultado esperado

Evitar erros comuns relacionados à memória e parâmetros.

---

### Bloco 10 — Automação com Gradle

Objetivo: deixar de compilar manualmente.

### Problema

Antes:

```
javacjavaclasspathjar
```

Tudo manual.

### Solução

```
Gradle
```

### Conceitos

- Build
- Dependências
- Automação
- Gradle Wrapper (`gradlew`)

### Resultado esperado

Preparação para projetos profissionais e para o desenvolvimento com Liferay.

---
