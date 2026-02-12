# Imagem base
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

# Imagem de build
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# 1. Copia o csproj usando o caminho COMPLETO desde a raiz do git
# O formato é: COPY ["caminho/do/arquivo", "caminho/destino/"]
COPY ["weblabcicegit/weblabcicegit/weblabcicegit.csproj", "weblabcicegit/weblabcicegit/"]

# 2. Restaura as dependências apontando para esse arquivo
RUN dotnet restore "weblabcicegit/weblabcicegit/weblabcicegit.csproj"

# 3. Copia todo o código do repositório
COPY . .

# 4. Muda a pasta de trabalho para onde está o projeto para fazer o build
WORKDIR "/src/weblabcicegit/weblabcicegit"
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
