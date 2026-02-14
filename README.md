Lab para simular a implantação de um site simples no Azure APP usando o GitHub e Git Action

## Configuração do GitHub Actions

O arquivo de configuração do GitHub Actions está localizado em:
`.github/workflows/azure-container-webapp.yml`

Este arquivo contém o workflow que:
- Constrói uma imagem Docker da aplicação
- Faz push da imagem para o GitHub Container Registry (ghcr.io)
- Realiza o deploy automático para o Azure Web App
