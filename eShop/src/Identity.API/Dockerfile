# STAGE 1 - deps 
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS deps

WORKDIR /src

COPY Directory.Packages.props ./
COPY Directory.Build.props ./

COPY src/Identity.API/Identity.API.csproj src/Identity.API/
COPY src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj src/eShop.ServiceDefaults/

RUN dotnet restore src/Identity.API/Identity.API.csproj


# STAGE 2 - build 
FROM deps AS builder

WORKDIR /src

COPY src/ src/

RUN dotnet publish src/Identity.API/Identity.API.csproj -c Release --no-restore -o /app/publish


# STAGE 3 - runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime


WORKDIR /app

ENV ASPNETCORE_URLS=http://+:5243

COPY --from=builder /app/publish .

EXPOSE 5243

ENTRYPOINT ["dotnet", "Identity.API.dll"]