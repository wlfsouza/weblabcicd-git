# USAR SDK 8.0 (Mais seguro, compila projetos 6.0, 7.0 e 8.0)
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# 1. Copia TUDO da raiz do repo para dentro da imagem
# Isso evita o erro de "arquivo não encontrado"
COPY . .

# 2. Comando de diagnóstico (vai mostrar no log onde o arquivo está se falhar)
RUN find . -name "*.csproj"

# 3. Restaura e Compila apontando para o caminho completo
# Ajuste o caminho abaixo se o comando 'find' mostrar algo diferente
RUN dotnet restore "weblabcicegit/weblabcicegit/weblabcicegit.csproj"
RUN dotnet build "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/build

# Publica
FROM build AS publish
RUN dotnet publish "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Imagem Final (Runtime)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
