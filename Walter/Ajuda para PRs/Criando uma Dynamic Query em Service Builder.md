#programming #liferay 

criando uma `DynamicQuery` e dizendo **para qual entidade essa query será feita**.

```
DynamicQuery query =
    DynamicQueryFactoryUtil.forClass(
        H7G5Entry.class,
        getClass().getClassLoader()
    );
```

### 1. `DynamicQuery query`

Aqui você está apenas declarando uma variável:

```
DynamicQuery query
```

Pense nela como:

> "Uma variável que vai representar minha consulta ao banco."

Neste momento, ainda não estamos dizendo **qual condição** será pesquisada.

---

### 2. `DynamicQueryFactoryUtil`

É uma classe utilitária do Liferay responsável por **criar uma `DynamicQuery`**.

```
DynamicQueryFactoryUtil.forClass(...)
```

O nome `forClass` dá uma boa pista:

> "Crie uma Dynamic Query para esta classe."

---

### 3. `H7G5Entry.class`

Aqui está a parte mais importante:

```
H7G5Entry.class
```

Você está dizendo:

> "Minha consulta será sobre a entidade `H7G5Entry`."

Não é:

```
new H7G5Entry()
```

É:

```
H7G5Entry.class
```

porque você está passando a **classe**, e não uma instância de `H7G5Entry`.

Conceitualmente:

```
DynamicQuery
     │
     ▼
H7G5Entry
     │
     ▼
OHQIWTSFHL_H7G5Entry
```

Ou seja, a query será construída para trabalhar com os objetos `H7G5Entry` e sua representação persistida.

---

### 4. `getClass().getClassLoader()`

Essa parte é mais técnica.

```
getClass().getClassLoader()
```

obtém o **ClassLoader da classe que está executando o código**.

No seu caso, você estará dentro de algo como:

```
H7G5EntryLocalServiceImpl
```

Então o Liferay usa o ClassLoader daquele módulo para localizar corretamente a classe `H7G5Entry` dentro do ambiente OSGi.

Você pode pensar simplesmente:

```
H7G5Entry.class
       +
ClassLoader
       ↓
Liferay consegue identificar corretamente
a entidade no módulo
```

Não é algo que você normalmente precisa modificar.

---

# Onde colocar isso no seu projeto?

**No `H7G5EntryLocalServiceImpl.java`**, dentro de um método que você está criando para realizar uma consulta dinâmica.

Por exemplo:

```
h7g5-service/
└── src/
    └── main/
        └── java/
            └── com/
                └── liferay/
                    └── h7g5/
                        └── service/
                            └── impl/
                                └── H7G5EntryLocalServiceImpl.java
```

Seu arquivo ficaria aproximadamente:

```
package com.liferay.h7g5.service.impl;

import com.liferay.h7g5.model.H7G5Entry;
import com.liferay.h7g5.service.base.H7G5EntryLocalServiceBaseImpl;

import com.liferay.portal.kernel.dao.orm.DynamicQuery;
import com.liferay.portal.kernel.dao.orm.DynamicQueryFactoryUtil;

import java.util.List;

public class H7G5EntryLocalServiceImpl
    extends H7G5EntryLocalServiceBaseImpl {

    public List<H7G5Entry> getEntriesByName(String name) {

        DynamicQuery query =
            DynamicQueryFactoryUtil.forClass(
                H7G5Entry.class,
                getClass().getClassLoader()
            );

        // Aqui você adicionaria as condições da consulta

        return dynamicQuery(query);
    }
}
```

Depois você adicionaria, por exemplo:

```
query.add(
    RestrictionsFactoryUtil.eq("name", name)
);
```

O método completo ficaria:

```
public List<H7G5Entry> getEntriesByName(String name) {

    DynamicQuery query =
        DynamicQueryFactoryUtil.forClass(
            H7G5Entry.class,
            getClass().getClassLoader()
        );

    query.add(
        RestrictionsFactoryUtil.eq("name", name)
    );

    return dynamicQuery(query);
}
```

O raciocínio é:

```
H7G5EntryLocalServiceImpl
        │
        │ cria
        ▼
DynamicQuery
        │
        │ "quero consultar H7G5Entry"
        ▼
H7G5Entry.class
        │
        │ adiciona condições
        ▼
Restrictions
        │
        │ executa
        ▼
dynamicQuery(query)
        │
        ▼
Persistence / Hibernate
        │
        ▼
Banco de dados
```

### Por que no `LocalServiceImpl`?

Porque no seu projeto é o **Local Service que representa a camada onde você implementa operações sobre `H7G5Entry`**.

Você já tem:

```
h7g5EntryPersistence.findByName(name);
```

para consultas que o Service Builder consegue gerar através de `finder`.

Quando você precisa construir uma consulta mais complexa, você pode criar uma `DynamicQuery` no:

```
H7G5EntryLocalServiceImpl.java
```

e executá-la através de:

```
dynamicQuery(query)
```

**Não coloque essa linha no `service.xml`.** O `service.xml` descreve entidades, colunas, finders etc.; a Dynamic Query é código Java escrito na implementação do serviço.