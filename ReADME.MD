# Projeto Raízes do Nordeste - API RESTful Lanchonete

Este repositório contém o back-end e a modelagem de dados automatizada para a rede de lanchonetes Raízes do Nordeste, desenvolvido como Projeto Multidisciplinar para o Centro Universitário Internacional UNINTER.

A aplicação adota uma arquitetura em camadas estruturada com Node.js, Prisma ORM e controle transacional de estoque por unidade.

## 🛠️ Requisitos e Dependências

* Node.js (Versão LTS estável)
* Gerenciador de pacotes NPM
* Banco de dados (SQLite/PostgreSQL configurado via Prisma)

## 🚀 Como Executar o Projeto Localmente

Siga o passo a passo abaixo no terminal do seu editor de código para inicializar a API do zero:

1. **Instalar as dependências do projeto:**
   ```bash
   npm install
   ```

2. **Configurar as Variáveis de Ambiente:**
   Crie um arquivo chamado `.env` na raiz do projeto baseado no `.env.example` e insira sua string de conexão e chave JWT:
   ```env
   DATABASE_URL="file:./dev.db"
   JWT_SECRET="sua_chave_secreta_aqui"
   ```

3. **Executar as Migrations do Banco de Dados:**
   Gere as tabelas estruturais de forma automática com o comando do Prisma:
   ```bash
   npx prisma migrate dev --name init
   ```

4. **Executar o Seed (Carga Inicial de Dados):**
   Popule as tabelas com os perfis de usuários de teste e insumos iniciais para validação das regras de negócio:
   ```bash
   npx prisma db seed
   ```

5. **Iniciar o Servidor da Aplicação:**
   ```bash
   npm run dev
   ```

O servidor iniciará localmente e a documentação interativa estará disponível em: `http://localhost:3000/api-docs`

## 📁 Organização e Evidências do Repositório
* `Lanchonete_Postman_Collection.json`: Arquivo na raiz contendo a suíte com os 11 cenários de teste automatizados (T01 a T11).
* `docs/DER.png`: Imagem contendo o Modelo Entidade-Relacionamento lógico do banco de dados relacional.
* `src/`: Código-fonte segmentado nas camadas de Domain, Application, Infrastructure e API.
