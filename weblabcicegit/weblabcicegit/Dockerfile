# Imagem base para execução
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Imagem para build
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copia o arquivo .csproj (mesma pasta do Dockerfile)
COPY ["weblabcicegit.csproj", "./"]
RUN dotnet restore "./weblabcicegit.csproj"

# Copia todo o código
COPY . .
WORKDIR "/src"
RUN dotnet build "weblabcicegit.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publica o projeto
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "weblabcicegit.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Imagem final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
