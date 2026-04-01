# Example Web Service

A simple ASP.NET Core Web API that stores a list of words in memory with:
- GET `/api/Words` – get all words
- POST `/api/Words?value=some-word` – add a new word
- Swagger UI available at `/swagger`

## Prerequisites

- .NET 8 SDK
- Docker and Docker Compose (for containerized deployment)

## Setup

Install .NET SDK:
```bash
sudo apt install -y dotnet-sdk-8.0
```

## Build

```bash
dotnet publish ExampleWebService/ExampleWebService.csproj -c Release -r linux-x64 --self-contained false
```

## Run

HTTP:
```bash
dotnet ExampleWebService/bin/Release/net8.0/linux-x64/publish/ExampleWebService.dll --urls "http://0.0.0.0:5000"
```

HTTPS:
```bash
dotnet ExampleWebService/bin/Release/net8.0/linux-x64/publish/ExampleWebService.dll --urls "https://0.0.0.0:5001"
```

> For HTTPS, ensure the development certificate is trusted:
> ```bash
> dotnet dev-certs https --trust
> ```

## Access

- API: `http://localhost:5000/api/Words`
- Swagger UI: `http://localhost:5000/swagger`

## Swagger & API Versioning

The Swagger UI will display the Git commit hash of the build if the `DEPLOY_REF` environment variable is set when the application is started. This is useful for identifying the version of the code running.

Example:
```bash
export DEPLOY_REF=$(git rev-parse --short HEAD)
dotnet ExampleWebService/bin/Release/net8.0/linux-x64/publish/ExampleWebService.dll --urls "http://0.0.0.0:5000"
```

## Running Tests

To run the tests, execute the following command:

```bash
dotnet test --logger:junit
```

## Docker Compose

Example of providing the PostgreSQL connection string for C# application in Docker Compose:

```yaml
environment:
  - ConnectionStrings__Postgres=Host=db;User Id=postgres;Password=123;Database=postgres;Port=5432;Pooling=false
```

Or

```yaml
environment:
  - "ConnectionStrings:Postgres=Host=db;User Id=postgres;Password=123;Database=postgres;Port=5432;Pooling=false"
```