# Demonstration of Spring Cloud Gateway with .NET Core

This is a _simple_ demonstration of using Spring Cloud Gateway to proxy requests to a rest API implemented in .NET Core.

### Prerequisites

- Java
- Maven
- [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download)
- curl (`brew install curl`)
- jq (`brew install jq`)

### Running

- Run the .NET Core application
  - In a terminal, navigate to the `webapi` folder
  - `dotnet watch run`
- Run Spring Cloud Gateway
  - In another terminal window, navigate to the `gateway` folder
  - `mvn spring-boot:run`

### Tests
- Use [Spring Cloud Gateway's Actuator API](https://cloud.spring.io/spring-cloud-gateway/multi/multi__actuator_api.html#_actuator_api) to view all available routes
  - `curl -s http://localhost:8080/actuator/gateway/routes | jq '.'`
- Demonstrate using SCG to proxy calls to httpbin.org
  - `curl -sv http://localhost:8080/hello`
- Use SCG to proxy requests to .NET Core
  - Verify the .NET Core API by calling it directly at /weatherforecast
    - `curl -s http://localhost:5000/weatherforecast | jq '.'`
    - Response should contain randomly generated weather data
  - Call SCG's /forecast endpoint
    - `curl -s http://localhost:8080/forecast | jq '.'`
    - Response should contain randomly generated weather data, just like calling .NET Core API directly
  - Compare the HTTP headers for the two requests
    - `curl -sv http://localhost:5000/weatherforecast`
    - `curl -sv http://localhost:8080/forecast`
      - The SCG request will contain an "X-Debug: Hello from Spring Cloud Gateway" header
- Demonstrate using SCG to perform weighted routing
  - Execute `curl -s http://localhost:8080/weighted | jq '.'` several times
  - Note that the response is a JSON object where "weighted-route" is "1" and other times it is "2"
  - Calls made to `http://localhost:8080/weighted` are sometimes forwarded by SCG to `http://echo.jsontest.com/weighted-route/1` and other times to `http://echo.jsontest.com/weighted-route/2`