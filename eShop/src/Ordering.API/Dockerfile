# STAGE 1 - deps 
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS deps

WORKDIR /src

COPY Directory.Packages.props ./
COPY Directory.Build.props ./

COPY src/Ordering.API/Ordering.API.csproj src/Ordering.API/
COPY src/EventBus/EventBus.csproj src/EventBus/
COPY src/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/EventBusRabbitMQ/
COPY src/IntegrationEventLogEF/IntegrationEventLogEF.csproj src/IntegrationEventLogEF/
COPY src/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/EventBusRabbitMQ/
COPY src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj src/eShop.ServiceDefaults/
COPY src/Ordering.Domain/Ordering.Domain.csproj src/Ordering.Domain/
COPY src/Ordering.Infrastructure/Ordering.Infrastructure.csproj src/Ordering.Infrastructure/

RUN dotnet restore src/Ordering.API/Ordering.API.csproj


# STAGE 2 - build 
FROM deps AS builder

WORKDIR /src

COPY src/ src/

RUN dotnet publish src/Ordering.API/Ordering.API.csproj -c Release --no-restore -o /app/publish


# STAGE 3 - runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime


WORKDIR /app

ENV ASPNETCORE_URLS=http://+:5224

COPY --from=builder /app/publish .

EXPOSE 5243

ENTRYPOINT ["dotnet", "Ordering.API.dll"]