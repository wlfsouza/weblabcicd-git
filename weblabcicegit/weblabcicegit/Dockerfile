# Imagem base
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
# Garante que a app escute na porta 8080
ENV ASPNETCORE_URLS=http://+:8080

# Imagem de build
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Como vamos mudar o contexto no GitHub Action, podemos copiar direto
COPY ["weblabcicegit.csproj", "."]
RUN dotnet restore "./weblabcicegit.csproj"

# Copia todo o resto
COPY . .
WORKDIR "/src"
RUN dotnet build "weblabcicegit.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publica
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "weblabcicegit.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
