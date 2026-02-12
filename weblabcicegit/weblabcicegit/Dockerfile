# Usamos SDK 8.0 pois ele consegue compilar projetos 6.0, 7.0 e 8.0 sem erros
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# 1. Copia TUDO (Isso elimina dúvidas sobre contexto)
COPY . .

# 2. DIAGNÓSTICO: Lista os arquivos para termos certeza da estrutura no log
RUN echo "=== ESTRUTURA DE PASTAS ===" && ls -R && echo "========================="

# 3. Restaura usando o caminho relativo que vimos na imagem
# Ajuste aqui se o log mostrar algo diferente
RUN dotnet restore "weblabcicegit/weblabcicegit/weblabcicegit.csproj"

# 4. Build
RUN dotnet build "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/build

# Publica
FROM build AS publish
RUN dotnet publish "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
