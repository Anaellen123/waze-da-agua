# 💧 Waze da Água

Plataforma colaborativa para monitoramento de ocorrências relacionadas ao abastecimento de água, permitindo o registro e a visualização georreferenciada de problemas reportados pelos usuários.

---

## 📌 Tecnologias do projeto

A arquitetura prevista utiliza:

### Front-end Web
- React
- TypeScript
- Next.js
- Docker

### Back-end
- Node.js
- TypeScript
- Prisma ORM
- Redis

### Banco de dados
- PostgreSQL
- PostGIS

### Infraestrutura
- Docker
- Docker Compose
- Nginx
- Linux / WSL2

### Mobile
- React Native

---

# 🏗️ Arquitetura

Cada serviço da aplicação será executado separadamente.

```text
                        ┌─────────────────┐
                        │   React Native  │
                        │     Mobile      │
                        └────────┬────────┘
                                 │
                                 │ API
                                 ▼
┌───────────────┐        ┌─────────────────┐
│ Next.js/React │ ─────► │      Nginx      │
│      Web      │        └────────┬────────┘
└───────────────┘                 │
                                  ▼
                         ┌─────────────────┐
                         │ Node.js + TS    │
                         │      API        │
                         └───────┬─────────┘
                                 │
                    ┌────────────┴────────────┐
                    ▼                         ▼
             ┌─────────────┐           ┌───────────┐
             │ PostgreSQL  │           │   Redis   │
             │  + PostGIS  │           │           │
             └─────────────┘           └───────────┘
```

---

# 📂 Estrutura inicial

```text
waze-da-agua/
│
├── backend/
│
├── frontend/
│
├── nginx/
│
├── .env
├── .gitignore
├── docker-compose.yml
└── README.md
```

> As pastas `backend`, `frontend` e `nginx` serão preenchidas conforme o desenvolvimento dos respectivos serviços.

---

# 💻 Configuração do ambiente

## Windows

Para desenvolver no Windows, utilizamos:

- WSL2
- Ubuntu
- Docker Desktop
- Git
- Visual Studio Code

O desenvolvimento deve ser feito preferencialmente dentro do ambiente Linux fornecido pelo WSL2.

---

# 1. Instalar o WSL2

Abra o PowerShell como administrador e execute:

```powershell
wsl --install
```

Depois da instalação, reinicie o computador caso seja solicitado.

Para verificar:

```powershell
wsl --status
```

Também é possível listar as distribuições instaladas:

```powershell
wsl -l -v
```

O Ubuntu deve estar utilizando a versão `2` do WSL.

Exemplo:

```text
NAME       STATE      VERSION
Ubuntu     Running    2
```

---

# 2. Instalar o Docker Desktop

Instale o Docker Desktop para Windows.

Durante a configuração, certifique-se de utilizar o backend baseado em WSL2.

No Docker Desktop, verifique também a integração com a distribuição Ubuntu.

Depois, dentro do Ubuntu/WSL, execute:

```bash
docker --version
```

e:

```bash
docker compose version
```

Para verificar se o Docker Engine está funcionando:

```bash
docker info
```

---

# 3. Instalar o Git no Ubuntu

Dentro do WSL/Ubuntu:

```bash
sudo apt update
sudo apt install git -y
```

Verifique:

```bash
git --version
```

Configure sua identidade:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@exemplo.com"
```

Use preferencialmente o e-mail associado à sua conta do GitHub ou o endereço `noreply` disponibilizado pelo GitHub.

---

# 4. Clonar o projeto

Entre na pasta onde deseja armazenar seus projetos.

Por exemplo:

```bash
cd ~
```

Clone o repositório:

```bash
git clone https://github.com/Anaellen123/waze-da-agua.git
```

Entre no projeto:

```bash
cd waze-da-agua
```

---

# 5. Abrir no Visual Studio Code

Dentro da pasta do projeto:

```bash
code .
```

Certifique-se de que o VS Code esteja trabalhando conectado ao ambiente WSL/Ubuntu.

O terminal utilizado para desenvolvimento deve apresentar um caminho semelhante a:

```text
usuario@computador:~/waze-da-agua$
```

---

# 🔐 6. Configurar variáveis de ambiente

O arquivo `.env` contém configurações locais e informações sensíveis.

Por segurança:

> ⚠️ O arquivo `.env` NÃO deve ser enviado para o GitHub.

Ele está incluído no `.gitignore`.

Crie um arquivo `.env` na raiz do projeto:

```bash
touch .env
```

Adicione:

```env
POSTGRES_DB=waze_agua
POSTGRES_USER=waze_user
POSTGRES_PASSWORD=COLOQUE_SUA_SENHA_AQUI
POSTGRES_PORT=5432
```

Cada desenvolvedor pode configurar sua senha local.

Nunca coloque senhas reais diretamente no `README.md`.

---

# 🐘 7. PostgreSQL + PostGIS

O banco de dados é executado através do Docker.

Utilizamos PostgreSQL com a extensão PostGIS para permitir operações geográficas e geoespaciais.

Isso será importante para recursos como:

- localização das ocorrências;
- latitude e longitude;
- busca de ocorrências próximas;
- áreas afetadas;
- mapas;
- consultas por distância.

O serviço é definido no `docker-compose.yml`.

---

# ⚡ 8. Redis

Também utilizamos Redis em um container separado.

O Redis poderá ser utilizado para:

- cache;
- sessões;
- rate limiting;
- filas;
- armazenamento temporário.

---

# 🐳 9. Iniciar os containers

Na raiz do projeto:

```bash
docker compose up -d
```

Verifique:

```bash
docker compose ps
```

Os serviços devem aparecer como ativos.

Atualmente:

```text
waze_agua_postgres
waze_agua_redis
```

---

# 🧪 10. Testar PostgreSQL

Entre no PostgreSQL:

```bash
docker exec -it waze_agua_postgres psql -U waze_user -d waze_agua
```

Dentro do PostgreSQL:

```sql
SELECT version();
```

Para verificar o PostGIS:

```sql
SELECT PostGIS_Version();
```

Para sair:

```text
\q
```

---

# 🧪 11. Testar Redis

Execute:

```bash
docker exec waze_agua_redis redis-cli ping
```

O resultado esperado é:

```text
PONG
```

---

# 🛑 12. Parar os containers

Para parar:

```bash
docker compose down
```

Para iniciar novamente:

```bash
docker compose up -d
```

Os dados do PostgreSQL e Redis são armazenados em volumes Docker e não devem ser apagados simplesmente ao executar `docker compose down`.

---

# ⚠️ Cuidado com volumes

Evite executar:

```bash
docker compose down -v
```

sem saber exatamente o que está fazendo.

A opção:

```text
-v
```

remove os volumes associados e pode apagar os dados locais do banco.

---

# 🌿 Git e branches

Antes de começar a trabalhar, atualize sua branch:

```bash
git pull
```

O projeto utiliza a branch:

```text
main
```

A estratégia de branches de desenvolvimento será definida pela equipe.

Evite desenvolver diretamente na `main` caso a equipe esteja utilizando branches individuais ou branches de funcionalidades.

---

# 🔄 Fluxo básico do Git

Verificar alterações:

```bash
git status
```

Adicionar alterações:

```bash
git add .
```

Criar commit:

```bash
git commit -m "descrição da alteração"
```

Enviar:

```bash
git push
```

Atualizar:

```bash
git pull
```

---

# 🔑 Autenticação no GitHub

O GitHub não aceita a senha comum da conta para operações Git via HTTPS.

Uma opção é utilizar o GitHub CLI.

Instalação no Ubuntu:

```bash
sudo apt update
sudo apt install gh -y
```

Autenticação:

```bash
gh auth login
```

Selecione:

```text
GitHub.com
HTTPS
Login with a web browser
```

Caso o WSL não consiga abrir automaticamente o navegador, abra manualmente no navegador do Windows o endereço exibido pelo GitHub CLI e informe o código fornecido pelo terminal.

Nunca compartilhe códigos de autenticação, tokens ou senhas.

Para verificar a autenticação:

```bash
gh auth status
```

---

# 🔎 Comandos úteis

Ver containers:

```bash
docker compose ps
```

Ver logs:

```bash
docker compose logs
```

Acompanhar logs:

```bash
docker compose logs -f
```

Ver apenas PostgreSQL:

```bash
docker compose logs postgres
```

Ver apenas Redis:

```bash
docker compose logs redis
```

Reiniciar os serviços:

```bash
docker compose restart
```

---

# 🚀 Primeira configuração resumida

Depois que WSL2, Docker, Git e VS Code estiverem instalados:

```bash
git clone https://github.com/Anaellen123/waze-da-agua.git

cd waze-da-agua

cp .env.example .env

docker compose up -d

docker compose ps
```

> O comando `cp .env.example .env` funcionará depois que o `.env.example` estiver disponível no repositório.

---

# 🔒 Segurança

Nunca enviar para o GitHub:

```text
.env
senhas
tokens
chaves privadas
credenciais de banco
credenciais de APIs
```

Antes de realizar um commit:

```bash
git status
```

Confira sempre quais arquivos serão enviados.

---

# 📍 Estado atual do ambiente

Até o momento, o ambiente possui:

- [x] WSL2 / Ubuntu
- [x] Docker
- [x] Docker Compose
- [x] PostgreSQL 17
- [x] PostGIS 3.5
- [x] Redis
- [x] Git
- [x] GitHub
- [ ] Backend Node.js + TypeScript
- [ ] Prisma
- [ ] Front-end React + TypeScript + Next.js
- [ ] Nginx
- [ ] Aplicativo React Native

Este README deverá ser atualizado conforme novos serviços forem adicionados ao projeto.
