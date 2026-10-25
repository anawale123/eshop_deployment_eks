# STAGE 1 - RESTORE DEPENDENCIES
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS deps

WORKDIR /src

COPY Directory.Packages.props ./

COPY Directory.Build.props ./

COPY src/EventBus/EventBus.csproj src/EventBus/
COPY src/Catalog.API/Catalog.API.csproj src/Catalog.API/
COPY src/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/EventBusRabbitMQ/
COPY src/IntegrationEventLogEF/IntegrationEventLogEF.csproj src/IntegrationEventLogEF/
COPY src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj src/eShop.ServiceDefaults/

RUN dotnet restore src/Catalog.API/Catalog.API.csproj

# STAGE 2 - buld source code 
FROM deps AS builder

WORKDIR /src

COPY src/ src/

RUN dotnet publish src/Catalog.API/Catalog.API.csproj -c Release --no-restore -o /app/publish

# STAGE 3 - runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime

WORKDIR /app

ENV ASPNETCORE_URLS=http://+:8080

COPY --from=builder /app/publish .

EXPOSE 8080

ENTRYPOINT ["dotnet", "Catalog.API.dll"]