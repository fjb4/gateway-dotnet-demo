# Demonstration of Spring Cloud Gateway with .NET Core

This is a _simple_ demonstration of using Spring Cloud Gateway to proxy requests to a rest API implemented in .NET Core.

### Prerequisites

- Java
- Maven
- [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download)

### Running

- Run the .NET Core application
  - In a terminal, navigate to the `webapi` folder
  - `dotnet watch run`
- Run Spring Cloud Gateway
  - In another terminal window, navigate to the `gateway` folder
  - `mvn spring-boot:run`

### Tests
- Verify the .NET Core API by executing `curl -v http://localhost:5000/weatherforecast`
  - Should return weather JSON
- Verify that Spring Cloud Gateway is proxying calls to the .NET Core API by executing `curl -v http://localhost:8080/forecast`
  - Should return the same weather JSON
  - Should return an additional "X-Debug" HTTP header indicating the call was handled by Spring Cloud Gateway
