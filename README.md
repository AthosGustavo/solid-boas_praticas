# solid-boas_praticas

### CRIANDO MÉTODOS
 - listarAbrigos(requisição get)
 - cadastrarAbrigos()
 - listarPetsDoAbrigo(requisicao get)
 - importarPetsDoAbrigo

### REMOVENDO DUPLICAÇÃO DE CÓDIGO:GET

Dentro de todas as funções que fazem requisições GET, existe uma duplicação de código e essa duplicação pode ser resolvida através de uma função que irá encapsular esse código e pode ser reutilizada em métodos GET.

#### Código duplicado
```java
HttpRequest request = HttpRequest.newBuilder()
  .uri(URI.create(uri))
  .method("GET", HttpRequest.BodyPublishers.noBody())
  .build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
```

#### Função inteira
```java
private void listarAbrigo() {
    HttpClient client = HttpClient.newHttpClient();
    String uri = "http://localhost:8080/abrigos";
    HttpRequest request = HttpRequest.newBuilder()
      .uri(URI.create(uri))
      .method("GET", HttpRequest.BodyPublishers.noBody())
      .build();

    HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
    String responseBody = response.body();
    JsonArray jsonArray = JsonParser.parseString(responseBody).getAsJsonArray();

    System.out.println("Abrigos cadastrados:");
    for (JsonElement element : jsonArray) {
        JsonObject jsonObject = element.getAsJsonObject();
        long id = jsonObject.get("id").getAsLong();
        String nome = jsonObject.get("nome").getAsString();
        System.out.println(id +" - " +nome);
    }
}

```
#### Método dispararRequisicaoGet criado
```java
private static HTTPResponse dispararRequisicaoGet(HTTPClient client, String uri) throws IOException, InterruptedException {
    HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create(uri))
    .method("GET", HttpRequest.BodyPublishers.noBody())
    .build();
  return client.send(request, HttpResponse.BodyHandlers.ofString());
}

```
#### Método listarAbrigo com o método dispararRequisicaoGet incorporado
```java
private void listarAbrigo() {
    HttpClient client = HttpClient.newHttpClient();
    String uri = "http://localhost:8080/abrigos";

    HTTPResponse<String> response = dispararRequisicaoGet(client, uri);
    String responseBody = response.body();
    JsonArray jsonArray = JsonParser.parseString(responseBody).getAsJsonArray();

    System.out.println("Abrigos cadastrados:");
    for (JsonElement element : jsonArray) {
        JsonObject jsonObject = element.getAsJsonObject();
        long id = jsonObject.get("id").getAsLong();
        String nome = jsonObject.get("nome").getAsString();
        System.out.println(id +" - " +nome);
    }
}

```

### REMOVENDO DUPLICAÇÃO DE CÓDIGO:POST

Assim como nos métodos get, nas funções que fazem requisições post,existem duplicações de código e isso precisa ser abstraído.

```java

```


listarAbrigo
cadastrarAbrigo
listarPets
cadastrarServices

- String[] Args
 - INSTANCIA ClientHttpConfiguration 

- PASTA Service
 - CLASSE AbrigoService(client)
   - construct{client}
  - METODO listarAbrigoService()
  - METODO cadastrarAbrigoService()

 - CLASSE PetsService(client)
   - construct{client}
  - METODO listarPetsService()
  - METODO cadastrarPetsService()

- PASTA Client
 - CLASSE ClientHttpConfiguration
   - METODO disparaRequisicaoGet()
     - INSTANCIA HTTPClient
   - METODO disparaRequisicaoPost()
     - INSTANCIA HTTPClient
 













