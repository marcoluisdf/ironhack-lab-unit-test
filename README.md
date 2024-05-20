# ironhack-lab-unit-test

# Scenario 1: User Authentication Tests

## Original Test Case (Pseudocode)

``` java
TEST UserAuthentication
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong credentials")
END TEST

```



## Issues
- El nombre de la prueba es poco descriptivo
- Multiples afirmaciones en una prueba


## Solution

- Se debera refactorizar el codigo dividiendolo en 2 pruebas separadas

- Se le asignara un nombre mas descriptivo del escenario a validar

``` java
public class UserAuthentication {
    public boolean authenticate(String username, String password) {
        return "validUser".equals(username) && "validPass".equals(password);
    }
}

public class UserAuthenticationTest {
    @Test
    public void testAuthenticateWithValidCredentials() {
        UserAuthentication auth = new UserAuthentication();
        assertTrue("Should succeed with correct credentials", auth.authenticate("validUser", "validPass"));
    }

    @Test
    public void testAuthenticateWithInvalidCredentials() {
        UserAuthentication auth = new UserAuthentication();
        assertFalse("Should fail with wrong credentials", auth.authenticate("validUser", "wrongPass"));
    }
}

```

# Scenario 2: Data Processing Functions

## Issues

``` java

TEST DataProcessing
  DATA data = fetchData()
  TRY
    processData(data)
    ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
  END TRY
END TEST

```

- Nombre de la prueba poco claro
- Se detecta que no existe una preparacion y limpieza de los insumos en la prueba, para el caso del metodo fetchData()
- Se esta manejando en el mismo unit test, un escenario y una excepcion


## Solution

``` java
public class DataProcessingTest {

    @Test
    public void testProcessDataSuccessfully() {
        DATA data = fetchData();
        processData(data);
        cleanData();
        assertTrue(data.processedSuccessfully, "Data should be processed successfully");
    }

    @Test
    public void testProcessDataErrorHandling() {
        DATA data = fetchErrorData();
        
        try {
            processData(data);
            throw new RuntimeException("Expected error was not thrown");
        } catch (Exception e) {
            cleanData();
            assertEquals("Data processing error", e.getMessage(), "Should handle processing errors");

        }
    }

    private DATA fetchData() {
        .....
    }

    private DATA fetchErrorData() {
        .....
    }

    private void cleanData(){
        ....
    }

    private void processData(DATA data) throws Exception {
        if (!data.isValid()) {
            throw new Exception("Data processing error");
        }
        data.setProcessedSuccessfully(true);
    }
}
```

- Se divide la pruebas 2 diferentes
- Se asignan nombres mas descriptivos a las pruebas
- Se agrega control para la preparacion y limpieza de los datos


# Scenario 3: UI Responsiveness





multiples afirmaciones (registro en bd, envio de correo electronico)
Falta de separacion de responsabilidades
