# ------------------------------------------------------------------
# ESTÁGIO 1: Build (Usando SDK do .NET 9)
# ------------------------------------------------------------------
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copia todos os arquivos da raiz do repositório
COPY . .

# Restaura as dependências
# (O caminho completo que já validamos que funciona)
RUN dotnet restore "weblabcicegit/weblabcicegit/weblabcicegit.csproj"

# Compila o projeto
RUN dotnet build "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/build

# ------------------------------------------------------------------
# ESTÁGIO 2: Publicação
# ------------------------------------------------------------------
FROM build AS publish
RUN dotnet publish "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/publish /p:UseAppHost=false

# ------------------------------------------------------------------
# ESTÁGIO 3: Final / Runtime (Usando Runtime do .NET 9)
# ------------------------------------------------------------------
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app

# Configura a porta para 8080 (Padrão do .NET 9)
ENV ASPNETCORE_HTTP_PORTS=8080
EXPOSE 8080

# Copia os arquivos publicados
COPY --from=publish /app/publish .

# Define o ponto de entrada
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
