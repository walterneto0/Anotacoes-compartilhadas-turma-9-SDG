#programming #java 

## O problema

Imagine escrever tudo do zero:

- números aleatórios
- datas
- arquivos
- listas
- conexões de rede

Seria inviável.

---

## Bibliotecas

[[Java]] já vem com milhares de classes prontas.

Exemplos:

```
StringDateRandomFileArrayList
```

Essas classes ficam organizadas em pacotes.

---

## O que é um pacote?

Um pacote é uma forma de agrupar classes relacionadas.

Exemplos:

```
java.util
```

Utilidades gerais.

```
java.io
```

Entrada e saída.

```
java.net
```

Rede.

```
java.lang
```

Classes fundamentais.

---

## O que é import?

Quando você quer usar uma classe de outro pacote:

```
import java.util.Random;
```

Você está dizendo:

> "Use a classe Random que está dentro do pacote java.util."

---

## Por que o compilador reclamou?

Sem import:

```
Random random = new Random();
```

O compilador vê:

```
Random? O que é isso?
```

Erro:

```
cannot find symbol
```

Após:

```
import java.util.Random;
```

O compilador sabe onde encontrar a classe.

---

## O caso especial do `java.lang`

As classes deste pacote são importadas automaticamente.

Por isso funciona:

```
String texto = "Olá";
```

Mesmo sem:

```
import java.lang.String;
```

---

## O que significa `new`?

Quando você escreve:

```
new Date()
```

Você está pedindo:

> Crie um novo objeto da classe Date.

Isso reserva memória e devolve uma instância da classe.

---

## Teoria principal do Bloco 4

As bibliotecas Java são organizadas hierarquicamente:

```
Pacote
    ↓
Classe
    ↓
Objeto
```

Exemplo:

```
java.util
    ↓
Random
    ↓
new Random()
```

O objetivo dos imports é permitir que o compilador encontre corretamente as classes que você deseja utilizar.

[[Teoria por tras do Basic Training]]