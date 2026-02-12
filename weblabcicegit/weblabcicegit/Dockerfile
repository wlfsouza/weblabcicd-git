# Estágio de Build
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

# Copia tudo para não ter erro de arquivo faltando
COPY . .

# Restaura dependências (Isso já funcionou no seu último teste)
RUN dotnet restore "weblabcicegit/weblabcicegit/weblabcicegit.csproj"

# Compila (Aqui é onde estava dando erro por causa da versão)
RUN dotnet build "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/build

# Publica
FROM build AS publish
RUN dotnet publish "weblabcicegit/weblabcicegit/weblabcicegit.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Estágio Final (Execução)
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "weblabcicegit.dll"]
