Desenvolva o script da automação seguindo as informações a seguir:
Documentação = https://reqres.in/
URI = https://reqres.in/api/
1) Validar o script de "CREATE" método "POST” cobertura de testes em RestAssured da API
2) Validar cobertura de Status Code, Campos obrigatórios e Contrato
3) Desenvolver com POJOs. 

<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>4.4.0</version>
</dependency>


implementation 'io.rest-assured:rest-assured:4.4.0'

public class CreateUserRequest {
    private String name;
    private String job;

    // Construtores, getters e setters

    // Exemplo de método para obter um objeto JSON a partir do POJO
    public JsonObject toJson() {
        JsonObject json = new JsonObject();
        json.addProperty("name", name);
        json.addProperty("job", job);
        return json;
    }
}

import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import org.testng.annotations.Test;

import static io.restassured.RestAssured.given;
import static org.testng.Assert.assertEquals;

public class CreateUserTest {

    @Test
    public void testCreateUser() {
        // Configuração da base URL
        RestAssured.baseURI = "https://reqres.in/api";

        // Criar um objeto CreateUserRequest com os dados desejados
        CreateUserRequest request = new CreateUserRequest();
        request.setName("John Doe");
        request.setJob("Engineer");

        // Enviar a requisição POST e receber a resposta
        Response response = given()
                .contentType(ContentType.JSON)
                .body(request.toJson().toString())
                .when()
                .post("/users");

        // Verificar o status code da resposta
        assertEquals(response.statusCode(), 201);

        // Verificar se os campos obrigatórios estão presentes no corpo da resposta
        String responseBody = response.getBody().asString();
        assertTrue(responseBody.contains("id"));
        assertTrue(responseBody.contains("name"));
        assertTrue(responseBody.contains("job"));
        assertTrue(responseBody.contains("createdAt"));

        // Verificar o contrato da resposta utilizando POJOs
        CreateUserResponse createUserResponse = response.getBody().as(CreateUserResponse.class);
        assertEquals(createUserResponse.getName(), "John Doe");
        assertEquals(createUserResponse.getJob(), "Engineer");
    }
}




      
