# Teste-de-APi---regres
import org.testng.annotations.Test;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class APITest {

    private static final String BASE_URI = "https://reqres.in";
    private static final String CREATE_USER_ENDPOINT = "/api/users";

    @Test
    public void testCreateUser() {
        // Configurar a URL base da API
        baseURI = BASE_URI;

        // Criação do objeto POJO para o payload do POST
        CreateUserRequest userRequest = new CreateUserRequest();
        userRequest.setName("John Doe");
        userRequest.setJob("Engineer");

        // Executar a solicitação POST e obter a resposta
        int statusCode = given()
            .contentType("application/json")
            .body(userRequest)
        .when()
            .post(CREATE_USER_ENDPOINT)
        .then()
            .log().all() // Exibir detalhes da resposta no log
            .statusCode(201) // Validar o status code esperado (201 - Created)
            .body("name", equalTo("John Doe")) // Validar o campo "name"
            .body("job", equalTo("Engineer")) // Validar o campo "job"
            .extract().statusCode(); // Extrair o status code da resposta

        // Validar o status code da resposta
        Assert.assertEquals(statusCode, 201);
    }
}
