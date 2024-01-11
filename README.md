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

    // Constructors, getters, and setters

    // Example method to convert the POJO to a JSON object
    public JsonObject toJson() {
        JsonObject json = new JsonObject();
        json.addProperty("name", name);
        json.addProperty("job", job);
        return json;
    }
}

public class CreateUserResponse {
    private String id;
    private String name;
    private String job;
    private String createdAt;

    // Constructors, getters, and setters
}

import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static io.restassured.RestAssured.given;
import static org.testng.Assert.assertEquals;
import static org.testng.Assert.assertTrue;

public class CreateUserTest {

    @BeforeClass
    public void setup() {
        RestAssured.baseURI = "https://reqres.in/api";
    }

    @Test
    public void testCreateUser() {
        // Create a CreateUserRequest object with the desired data
        CreateUserRequest request = new CreateUserRequest();
        request.setName("John Doe");
        request.setJob("Engineer");

        // Send the POST request and receive the response
        Response response = given()
                .contentType(ContentType.JSON)
                .body(request.toJson().toString())
                .when()
                .post("/users");

        // Verify the status code of the response
        assertEquals(response.statusCode(), 201);

        // Verify if the mandatory fields are present in the response body
        String responseBody = response.getBody().asString();
        assertTrue(responseBody.contains("id"));
        assertTrue(responseBody.contains("name"));
        assertTrue(responseBody.contains("job"));
        assertTrue(responseBody.contains("createdAt"));

        // Verify the response contract using POJOs
        CreateUserResponse createUserResponse = response.getBody().as(CreateUserResponse.class);
        assertEquals(createUserResponse.getName(), "John Doe");
        assertEquals(createUserResponse.getJob(), "Engineer");
    }
}





      
