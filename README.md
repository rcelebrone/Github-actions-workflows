# Workflow Reutilizável de CI/CD para Projetos Node.js

Este repositório contém um workflow do GitHub Actions que automatiza o processo de Continuous Integration e Continuous Delivery (CI/CD) para projetos Node.js. Ele realiza as seguintes etapas:

* **Build:** Constrói sua aplicação Node.js.
* **Testes:** Executa seus testes unitários.
* **Geração de Artefato:** Cria um arquivo zip contendo os artefatos da sua aplicação.
* **Upload para S3:** Envia o artefato gerado para um bucket no Amazon S3.
* **Testes de Integração:** Executa testes de integração usando o Newman (CLI do Postman).
* **Geração de Changelog:** Gera um changelog com base nas alterações desde o último release.

## Como Usar este Workflow no Seu Projeto

Para integrar este workflow ao seu projeto Node.js, siga os passos abaixo:

**1. Crie o Diretório de Workflows:**

   Se o seu projeto ainda não possui, crie um diretório chamado `.github/workflows` na raiz do seu repositório.

**2. Crie um Arquivo de Workflow:**

   Dentro do diretório `.github/workflows`, crie um novo arquivo com um nome descritivo (por exemplo, `ci-cd.yml`).

**3. Adicione a Referência ao Workflow Reutilizável:**

   No arquivo que você criou (`ci-cd.yml`), adicione o seguinte conteúdo, substituindo as informações conforme necessário:

   ```yaml
   name: CI/CD

   on:
     push:
       branches: [ "main" ] # Altere para o nome do seu branch principal, se for diferente
     pull_request:
       branches: [ "main" ] # Altere para o nome do seu branch principal, se for diferente

   jobs:
     ci_cd:
       uses: [seu-nome-de-usuario]/[nome-do-repositorio-deste-workflow]/.github/workflows/node-ci-cd.yml@v1 # Substitua com o caminho correto e a tag/branch
       with:
         node_version: '18'             # Versão do Node.js para usar (padrão: lts/*)
         s3_bucket: 'seu-bucket-s3'      # **Obrigatório:** Nome do seu bucket S3
         aws_region: 'sua-regiao-aws'     # **Obrigatório:** Região da AWS do seu bucket
         newman_collection_path: './postman/collection.json' # Caminho para a sua coleção do Newman
         newman_environment_path: './postman/environment.json' # Caminho para o seu ambiente do Newman (opcional)
         build_command: 'npm run build-prod' # Comando para construir seu projeto (opcional: padrão é 'npm run build')
         test_command: 'npm run test:unit'   # Comando para executar seus testes unitários (opcional: padrão é 'npm test')
       secrets:
         AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}     # **Obrigatório:** Seu Access Key ID da AWS (configure nos segredos do GitHub)
         AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }} # **Obrigatório:** Seu Secret Access Key da AWS (configure nos segredos do GitHub)
