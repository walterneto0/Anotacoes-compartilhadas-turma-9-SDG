#programming #liferay 

A partir dos dois tutoriais do seu repositório, o fluxo de deploy de um projeto **REST Builder** pode ser entendido como uma extensão do fluxo normal de deploy de módulos OSGi no Liferay.

O ponto principal é: **REST Builder gera módulos OSGi, mas apenas `api` e `impl` precisam ser instalados no Liferay para disponibilizar a API REST.** O módulo `client` é usado por aplicações Java clientes e o `test` contém testes.

### 1. Estrutura do projeto

O projeto criado pelo REST Builder tem, essencialmente:

```text
liferay-r3b2/
├── headless-r3b2-api/
├── headless-r3b2-client/
├── headless-r3b2-impl/
├── headless-r3b2-test/
├── gradle.properties
├── settings.gradle
└── gradlew
```

O tutorial de REST Builder explica que `rest-config.yaml` e `rest-openapi.yaml` são os arquivos principais usados pelo gerador. O `buildREST` transforma essas definições em código Java.

---

## 2. Configurar o Liferay Workspace

Antes de trabalhar com o REST Builder, o projeto precisa estar configurado como um **Liferay Workspace**.

O tutorial de OSGi mostra que isso é feito pelo `settings.gradle`, aplicando:

```gradle
buildscript {
    dependencies {
        classpath group: "com.liferay",
            name: "com.liferay.gradle.plugins.workspace",
            version: "latest.release"
    }

    repositories {
        mavenLocal()

        maven {
            url "https://repository-cdn.liferay.com/nexus/content/groups/public"
        }
    }
}

apply plugin: "com.liferay.workspace"
```

E o `gradle.properties` define a versão do Liferay utilizada pelo projeto, por exemplo:

```properties
liferay.workspace.product=portal-7.4-ga132
```

Isso é importante porque o Gradle precisa saber contra qual versão da plataforma os módulos serão compilados.

---

# 3. Gerar o código do REST Builder

No projeto REST Builder, existem dois arquivos importantes:

```text
headless-r3b2-impl/
├── rest-config.yaml
└── rest-openapi.yaml
```

### `rest-config.yaml`

Contém informações básicas sobre a API.

### `rest-openapi.yaml`

Define a API propriamente dita:

- endpoints;
    
- métodos HTTP;
    
- schemas;
    
- parâmetros;
    
- respostas;
    
- recursos como `Foo` e `Goo`.
    

O tutorial utiliza:

```bash
code headless-r3b2-impl/rest-config.yaml
```

e:

```bash
code headless-r3b2-impl/rest-openapi.yaml
```

Depois disso, o código pode ser gerado com:

```bash
./gradlew headless-r3b2-impl:buildREST
```

O `buildREST` gera classes como:

```text
Foo.java
Goo.java

FooResource.java
GooResource.java

BaseFooResourceImpl.java
BaseGooResourceImpl.java

FooResourceFactoryImpl.java
GooResourceFactoryImpl.java

HeadlessR3B2Application.java
```

entre outras.

---

# 4. Entender o que é gerado

Há uma distinção importante entre os arquivos gerados.

Por exemplo:

```text
FooResource.java
FooResourceImpl.java
```

`FooResource.java` possui `@generated` e **não deve ser editado manualmente**. Ele é regenerado quando:

```bash
./gradlew headless-r3b2-impl:buildREST
```

é executado.

Já:

```text
FooResourceImpl.java
```

não possui `@generated`. Ele é criado pelo REST Builder somente se ainda não existir e é justamente o arquivo onde você implementa a lógica da API.

O tutorial compara isso ao `*ServiceImpl.java` do Service Builder.

Portanto, a ideia é:

```text
rest-openapi.yaml
        │
        ▼
    buildREST
        │
        ├── FooResource.java       ← gerado
        │
        ├── BaseFooResourceImpl.java ← gerado
        │
        └── FooResourceImpl.java   ← implementação do desenvolvedor
```

---

# 5. Compilar o projeto

Depois de implementar os recursos, compile:

```bash
./gradlew headless-r3b2-impl:classes
```

Se o projeto estiver correto, o Java será compilado.

Para gerar novamente os arquivos do REST Builder:

```bash
./gradlew headless-r3b2-impl:buildREST
```

Uma sequência típica é:

```bash
./gradlew headless-r3b2-impl:buildREST
./gradlew headless-r3b2-impl:classes
```

O tutorial também demonstra que apagar `FooResourceImpl.java` e `GooResourceImpl.java` fará a compilação falhar, pois essas classes são necessárias para a implementação dos recursos.

---

# 6. Gerar os JARs

Depois de compilar, você pode gerar os artefatos:

```bash
./gradlew headless-r3b2-api:jar
```

e:

```bash
./gradlew headless-r3b2-impl:jar
```

Eles aparecerão aproximadamente em:

```text
headless-r3b2-api/build/libs/
headless-r3b2-impl/build/libs/
```

O `client` também pode gerar seu próprio JAR:

```bash
./gradlew headless-r3b2-client:jar
```

Mas **não é necessário instalar esse JAR no Liferay**. Ele é destinado aos programas Java que consumirão sua API. O próprio tutorial separa a seção de `Client Resource` do processo de deploy da API.

---

# 7. Subir o Liferay

No tutorial de OSGi, o Liferay é executado em Docker:

```bash
docker run \
    --name ephesians-liferay \
    --rm \
    -it \
    -p 8080:8080 \
    liferay/portal:7.4.3.132-ga132
```

O nome:

```text
ephesians-liferay
```

é importante porque será utilizado posteriormente pelo Gradle para saber em qual container instalar os módulos.

Você pode verificar se o container está executando:

```bash
docker ps
```

---

# 8. Primeiro método: copiar manualmente os JARs

O método mais básico demonstrado pelo tutorial de OSGi é:

```bash
docker cp \
    headless-r3b2-api/build/libs/seu-arquivo.jar \
    ephesians-liferay:/opt/liferay/osgi/modules
```

e depois fazer o mesmo com o `impl`.

O diretório dentro do container é:

```text
/opt/liferay/osgi/modules
```

O tutorial demonstra justamente essa operação com um módulo OSGi e depois verifica o JAR usando:

```bash
docker exec -it ephesians-liferay \
    /bin/ls /opt/liferay/osgi/modules
```

O Liferay detecta o JAR e instala/inicia o bundle OSGi.

Para REST Builder, entretanto, há uma maneira melhor.

---

# 9. Método recomendado pelo tutorial: `deploy.docker.container.id`

O próprio Liferay Workspace possui uma tarefa `deploy`.

Para o REST Builder, o tutorial usa:

```bash
./gradlew headless-r3b2-api:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay
```

Depois:

```bash
./gradlew headless-r3b2-impl:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay
```

Esse é o fluxo principal que você deve memorizar.

Em forma resumida:

```text
                 REST Builder
                      │
                      ▼
             rest-openapi.yaml
                      │
                      ▼
               buildREST
                      │
                      ▼
                  Java code
                      │
                      ▼
                   classes
                      │
                      ▼
                     JAR
                      │
                      ▼
              Gradle deploy
                      │
                      ▼
             Docker container
                      │
                      ▼
          /opt/liferay/osgi/modules
                      │
                      ▼
              Liferay / OSGi
                      │
                      ▼
               REST API ativa
```

O tutorial explicitamente manda fazer o deploy de `api` e `impl` e **não fazer deploy de `client` e `test`**.

---

# 10. Por que `api` e `impl`?

Imagine:

```text
headless-r3b2-api
        │
        └── contratos/interfaces/DTOs da API

headless-r3b2-impl
        │
        └── implementação da API
```

O `api` fornece os contratos necessários.

O `impl` contém a implementação que efetivamente será registrada no OSGi e exposta como REST.

Por isso:

```bash
./gradlew headless-r3b2-api:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay

./gradlew headless-r3b2-impl:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay
```

Já:

```text
headless-r3b2-client
```

é para quem **consome** a API.

E:

```text
headless-r3b2-test
```

é para testes.

---

# 11. Verificar se o deploy funcionou

Depois do deploy, abra:

```text
http://localhost:8080/o/api
```

Entre em:

```text
REST Applications
```

e procure:

```text
headless-r3b2/v1.0
```

O tutorial observa que, antes do deploy dos módulos, essa aplicação não aparece. Depois do deploy de `api` e `impl`, ela deve aparecer.

A partir daí você pode utilizar a interface para testar:

```text
Foo
Goo
```

e seus respectivos endpoints.

---

# 12. Testar com `curl`

Por exemplo, se seu recurso tiver:

```text
GET /foo/{fooId}
```

você poderá fazer:

```bash
curl \
    -u "test@liferay.com:test" \
    http://localhost:8080/o/headless-r3b2/v1.0/foo/1
```

Para POST, você enviaria o JSON correspondente ao schema definido no `rest-openapi.yaml`.

O ponto importante é que a URL da API é determinada pela configuração do REST Builder. Portanto, não é correto simplesmente assumir que toda API terá o mesmo endpoint: você deve conferir o `basePath`/configuração e os paths definidos no OpenAPI.

---

# 13. Fluxo completo que você deve decorar

Para o seu projeto `liferay-r3b2`, o procedimento fica:

### 1. Subir o Liferay

```bash
docker run \
    --name ephesians-liferay \
    --rm \
    -it \
    -p 8080:8080 \
    liferay/portal:7.4.3.132-ga132
```

### 2. Alterar a definição da API

```bash
code headless-r3b2-impl/rest-openapi.yaml
```

### 3. Gerar o código

```bash
./gradlew headless-r3b2-impl:buildREST
```

### 4. Implementar os recursos

```text
FooResourceImpl.java
GooResourceImpl.java
```

### 5. Compilar

```bash
./gradlew headless-r3b2-impl:classes
```

### 6. Deploy do API

```bash
./gradlew headless-r3b2-api:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay
```

### 7. Deploy do implementation

```bash
./gradlew headless-r3b2-impl:deploy \
    -Ddeploy.docker.container.id=ephesians-liferay
```

### 8. Não fazer deploy

```text
headless-r3b2-client
headless-r3b2-test
```

### 9. Verificar no Liferay

```text
http://localhost:8080/o/api
        ↓
REST Applications
        ↓
headless-r3b2/v1.0
```

### 10. Testar

```text
/o/headless-r3b2/v1.0/...
```

usando:

```text
Swagger/API Explorer
curl
Postman
```

Esse é essencialmente o fluxo de **REST Builder → Gradle → JAR → OSGi → Liferay → REST API** descrito pelos dois tutoriais.

### Uma observação importante sobre o seu ambiente

Os tutoriais do repositório são antigos em alguns pontos — por exemplo, o tutorial de OSGi usa `portal-7.4-ga90` como exemplo de Workspace e o container `7.4.3.132-ga132`. Portanto, **não copie cegamente todas as versões** para um projeto Liferay mais novo. A lógica do deploy continua sendo a mesma, mas `gradle.properties`, Java, Gradle e as dependências do Workspace precisam corresponder à versão do Liferay que você está executando.

Isso também explica por que, no seu projeto `liferay-r3b2`, erros de resolução como `release.portal.api` ou incompatibilidades entre `portal-7.4-ga132` e uma configuração `dxp-2026.q1.9-lts` não são resolvidos simplesmente repetindo o comando `deploy`: primeiro o projeto precisa estar corretamente configurado e compilando.