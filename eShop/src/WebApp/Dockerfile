# STAGE 1 - RESTORE DEPENDENCIES
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS deps

WORKDIR /src

COPY Directory.Packages.props ./

COPY Directory.Build.props ./

COPY src/EventBus/EventBus.csproj src/EventBus/
COPY src/WebApp/WebApp.csproj src/WebApp/
COPY src/WebAppComponents/WebAppComponents.csproj src/WebAppComponents/
COPY src/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/EventBusRabbitMQ/
COPY src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj src/eShop.ServiceDefaults/

RUN dotnet restore src/WebApp/WebApp.csproj

# STAGE 2 - buld source code 
FROM deps AS builder

WORKDIR /src

COPY src/ src/

RUN dotnet publish src/WebApp/WebApp.csproj -c Release --no-restore -o /app/publish

# STAGE 3 - runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime

WORKDIR /app

ENV ASPNETCORE_URLS=http://+:5045

COPY --from=builder /app/publish .

EXPOSE 5045

ENTRYPOINT ["dotnet", "WebApp.dll"]