# Project Setup


## Mvn Powershell

```
mvn io.quarkus:quarkus-maven-plugin:1.13.0.Final:create "-DprojectGroupId=org.acme" "-DprojectArtifactId=getting-started" "-DclassName=org.acme.getting.started.GreetingResource" "-Dpath=/hello"
```

mvn io.quarkus:quarkus-maven-plugin:1.13.0.Final:create "-DprojectGroupId=io.github.jithinsethu" "-DprojectArtifactId=hibernatevalidation" "-DclassName=io.github.jithinsethu.hibernatevalidation.GreetingResource" "-Dpath=/hello"

mvn io.quarkus:quarkus-maven-plugin:1.13.0.Final:create "-DprojectGroupId=io.github.jithinsethu" "-DprojectArtifactId=hibernate-reactive" "-DclassName=io.github.jithinsethu.hibernate.reactive.GreetingResource" "-Dpath=/hello"

mvn io.quarkus:quarkus-maven-plugin:1.13.0.Final:create "-DprojectGroupId=io.github.jithinsethu.superhero" "-DprojectArtifactId=restfights" "-DclassName=io.github.jithinsethu.superhero.restfights.GreetingResource" "-Dpath=/api/fights"


mvn io.quarkus:quarkus-maven-plugin:1.13.2.Final:create "-DprojectGroupId=io.github.jithinsethu.keycloak" "-DprojectArtifactId=keycloakgettingstarted" "-DclassName=io.github.jithinsethu.keycloak.keycloakgettingstarted.GreetingResource" "-Dpath=/api/employee"


    -DprojectArtifactId=security-keycloak-authorization-quickstart \
    -Dextensions="oidc,keycloak-authorization,resteasy-jackson" \
    -DnoExamples
cd security-keycloak-authorization-quickstart


## Add Extension 

```
mvn quarkus:add-extension -Dextensions="rest-client,rest-client-jackson"

mvn quarkus:add-extension -Dextensions="hibernate-validator"

mvn quarkus:add-extension -Dextensions="quarkus-hibernate-orm, quarkus-jdbc-postgresql"

mvn quarkus:add-extension -Dextensions="quarkus-hibernate-reactive-panache, quarkus-hibernate-reactive, quarkus-reactive-pg-client, quarkus-resteasy-reactive, quarkus-resteasy-reactive-jackson"

mvn quarkus:add-extension -Dextensions="oidc,keycloak-authorization"

mvn quarkus:add-extension -Dextensions="quarkus-resteasy-reactive, quarkus-resteasy-reactive-jackson"




```

## Reference

* Georgios (geoand)