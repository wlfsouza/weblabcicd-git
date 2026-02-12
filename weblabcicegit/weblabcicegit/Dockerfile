# Imagem base
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

# Imagem de build
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# COPIA SIMPLES: Como vamos rodar de dentro da pasta, só precisamos do nome do arquivo
COPY ["weblabcicegit.csproj", "./"]
RUN dotnet restore "./weblabcicegit.csproj"

# Copia todo o resto da pasta atual
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
