# Testing

## Testing Dependency

```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-junit5</artifactId>
    <scope>test</scope>
</dependency>
```

Provide @QuarkusTest

## Change Test Port

```
quarkus.http.test-port=8083
quarkus.http.test-ssl-port=8446
quarkus.http.test-timeout=10s
```

## Mockito

### Dependency

```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-junit5-mockito</artifactId>
    <scope>test</scope>
</dependency>
```

### Sample Code

Service must be @ApplicationScoped

```
package io.github.jithinsethu.gettingstartedtest.country;

import io.github.jithinsethu.gettingstartedtest.Country;
import io.quarkus.test.junit.mockito.InjectMock;

import io.github.jithinsethu.gettingstartedtest.CountryServices;
import io.quarkus.test.junit.QuarkusTest;
import org.eclipse.microprofile.rest.client.inject.RestClient;
import org.hamcrest.Matchers;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import java.util.Collections;

import static io.restassured.RestAssured.given;

@QuarkusTest
public class MockCountryResourceTest {

    @InjectMock
    @RestClient
    CountryServices countriesService;

    @BeforeEach
    void setUp() {
        Mockito.when(countriesService.getByName("tvm")).thenReturn(Collections
                .singleton(new Country("India", "Tvm")));
    }

    @Test
    public void testTvm() {
        given()
                .when().get("/country/name/tvm")
                .then()
                .statusCode(200)
                .body("$.size()", Matchers.is(1),
                        "[0].name", Matchers.is("India"),
                        "[0].capital", Matchers.is("Tvm")
                );
    }

}

```

## Wiremock

### Dependencies

```
<dependency>
    <groupId>com.github.tomakehurst</groupId>
    <artifactId>wiremock-jre8</artifactId>
    <version>2.27.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-core</artifactId>
    <version>3.19.0</version>
    <scope>test</scope>
</dependency>
```

### WiremockTest

```
package io.github.jithinsethu.gettingstartedtest.country;

import io.github.jithinsethu.gettingstartedtest.CountryServices;
import io.quarkus.test.common.QuarkusTestResource;
import io.quarkus.test.junit.QuarkusTest;
import org.assertj.core.api.Assertions;
import org.eclipse.microprofile.rest.client.inject.RestClient;
import org.junit.jupiter.api.Test;

import javax.inject.Inject;

import static org.assertj.core.api.Assertions.*;

@QuarkusTest
@QuarkusTestResource(WiremockCountries.class)
public class CountryServicesTest {

    @Inject
    @RestClient
    CountryServices countryServices;

    @Test
    void testGR() {
        assertThat(countryServices.getByName("GR")).hasSizeGreaterThan(1).extracting("name").contains("Greece");
    }
}
```

### Wiremock Server

```
package io.github.jithinsethu.gettingstartedtest.country;

import com.github.tomakehurst.wiremock.WireMockServer;
import io.quarkus.test.common.QuarkusTestResourceLifecycleManager;

import java.util.Collections;
import java.util.Map;

import static com.github.tomakehurst.wiremock.client.WireMock.*;

public class WiremockCountries implements QuarkusTestResourceLifecycleManager {

    private WireMockServer wireMockServer;

    @Override
    public Map<String, String> start() {
        wireMockServer = new WireMockServer();

        wireMockServer.start();

        stubFor(get(urlEqualTo("/v2/name/GR"))
                .willReturn(aResponse()
                        .withHeader("Content-Type", "application/json")
                        .withBody(
                                "[{" +
                                        "\"name\": \"Ελλάδα\"," +
                                        "\"capital\": \"Αθήνα\"" +
                                        "}]"
                        )));

        stubFor(get(urlMatching(".*")).atPriority(10).willReturn(aResponse().proxiedFrom("https://restcountries.eu/rest"))); // replacing 

        return Collections.singletonMap("io.github.jithinsethu.gettingstartedtest.CountryServices/mp-rest/url", wireMockServer.baseUrl());
    }

    @Override
    public void stop() {
        if (wireMockServer != null) {
            wireMockServer.stop();
        }

    }
}
```