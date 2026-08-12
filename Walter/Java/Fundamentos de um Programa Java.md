#java #programming 

## O que é uma classe?

Uma classe é um modelo que define dados e comportamentos.

Exemplo:

```
public class Test {}
```

Aqui foi criada uma classe chamada `Test`.

Pense em uma classe como uma planta de uma casa. A planta descreve a casa, mas não é a casa em si.

---

## O que é o método `main`?

Quando você executa um programa [[Java]], a JVM procura um método específico:

```
public static void main(String[] args)
```

Ele é o ponto de entrada da aplicação.

Fluxo:

```
Executar programa        ↓JVM procura main()        ↓Executa o código dentro dele
```

Exemplo:

```
public static void main(String[] args) {    System.out.println("Olá");}
```

---

## O que é `System.out.println()`?

É uma chamada de método para imprimir texto no terminal.

Separando:

```
System.out.println("Olá");
```

- `System` → classe da biblioteca Java
- `out` → objeto que representa a saída padrão
- `println()` → método que escreve uma linha

Resultado:

```
Olá
```

---

## Teoria principal do Bloco 1

O Java é uma linguagem orientada a objetos.

Por isso, todo código executável precisa estar dentro de uma classe.

Mesmo o programa mais simples segue a estrutura:

```
Classe   ↓Método main   ↓Instruções
```

[[Teoria por tras do Basic Training]]