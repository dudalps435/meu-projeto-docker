# Atividade Docker + CI + CD

**Aluno(a):** [Seu nome completo]  
**Turma:** [Sua turma]  
**Data:** [Data da entrega]

**Aplicação utilizada:** docker/getting-started-app (To-Do em Node.js)

---

# 1. Como executar este projeto

Clone o repositório:

```bash
git clone [URL do seu repositório]
cd [nome da pasta]
```

Copie o arquivo de exemplo das variáveis de ambiente:

```bash
cp .env.example .env
```

Suba os containers:

```bash
docker compose up -d --build
```

Acesse a aplicação:

```
http://localhost:3000
```

Para parar os containers:

```bash
docker compose down
```

Para parar e apagar os volumes:

```bash
docker compose down -v
```

---

# 2. Imagem e Dockerfile Multi-stage

**Estágios utilizados**

- Builder: instala as dependências e prepara a aplicação.
- Runtime: gera uma imagem menor contendo apenas o necessário para executar o projeto.

**Imagem base**

```
node:20-alpine
```

**Usuário de execução**

```
node (não-root)
```

**Tamanho final da imagem**

[Informe o tamanho obtido com `docker images`.]

### Por que o multi-stage ajuda?

O Dockerfile multi-stage reduz o tamanho da imagem final, pois copia apenas os arquivos necessários para execução da aplicação. Isso torna o download mais rápido e melhora a segurança.

---

## Print 1

*Build da imagem e resultado do comando `docker images`.*

(Coloque a imagem aqui)

---

## Print 2

*Aplicação funcionando com tarefas cadastradas.*

(Coloque a imagem aqui)

---

# 3. Volumes e Persistência

**Volume utilizado**

```
[nome do volume]
```

**Montado em**

```
[caminho dentro do container]
```

---

## Print 3

*Sem volume: dados perdidos após recriar o container.*

(Coloque a imagem aqui)

---

## Print 4

*Com volume: dados preservados.*

(Coloque a imagem aqui)

---

### Diferença entre `docker compose down` e `docker compose down -v`

O comando `docker compose down` remove apenas os containers e a rede. Já `docker compose down -v` também remove os volumes, apagando os dados persistidos.

---

# 4. Rede

**Rede criada**

```
[nome da rede]
```

**Serviços conectados**

- app
- db

### A porta do banco está exposta ao host?

Não. O banco fica acessível apenas pela rede interna do Docker, aumentando a segurança da aplicação.

### Por que o aplicativo consegue acessar o banco usando o nome do serviço?

Porque o Docker Compose cria uma rede interna com DNS próprio, permitindo que os containers se comuniquem usando o nome dos serviços.

---

## Print 5

*Resultado do comando `docker network inspect`.*

(Coloque a imagem aqui)

---

## Print 6

*Resultado do comando `SELECT * FROM todo_items;`.*

(Coloque a imagem aqui)

---

# 5. Docker Compose

**Serviços**

- app
- db

**Rede**

```
[nome da rede]
```

**Volume**

```
[nome do volume]
```

**Healthcheck**

```
db
```

**depends_on**

```
condition: service_healthy
```

As variáveis sensíveis são carregadas pelo arquivo `.env`, que não é enviado ao GitHub. Apenas o `.env.example` é versionado.

---

## Print 7

*Resultado do comando `docker compose ps`.*

(Coloque a imagem aqui)

---

# 6. Integração Contínua (CI)

**Workflow**

```
.github/workflows/ci.yml
```

**Disparado em**

- push
- pull_request

### O pipeline realiza as seguintes etapas:

1. Valida o arquivo Docker Compose.
2. Constrói a imagem Docker.
3. Inicia os containers.
4. Aguarda a aplicação responder e realiza um teste criando uma tarefa via API.
5. Finaliza os containers.

---

## Print 8

*Workflow do GitHub Actions executado com sucesso.*

(Coloque a imagem aqui)

---

# 7. Quebra proposital do CI

### O que foi alterado?

Foi realizada uma alteração proposital no projeto para provocar uma falha no pipeline de CI.

### Erro apresentado

```
[Cole aqui a mensagem principal do erro.]
```

### Como o CI reagiu?

O workflow interrompeu a execução no passo em que encontrou o erro, impedindo que as próximas etapas fossem executadas.

### Como foi corrigido?

A alteração incorreta foi desfeita e um novo commit foi enviado ao repositório, fazendo o pipeline voltar a executar com sucesso.

### Link do Pull Request

```
[Cole o link]
```

---

## Print 9

*Execução do workflow em vermelho mostrando o erro.*

(Coloque a imagem aqui)

---

# 8. Dificuldades e aprendizados

Durante a atividade, a principal dificuldade foi configurar corretamente o Docker Compose e os workflows do GitHub Actions. Também foi necessário entender como funcionam os volumes, redes e variáveis de ambiente. Após resolver os problemas, ficou mais claro como o Docker facilita a criação de ambientes padronizados e como o CI automatiza testes sempre que o código é atualizado.

---

# 9. Checklist

- [x] Dockerfile multi-stage funcionando
- [x] .dockerignore presente
- [x] Container executando sem usuário root
- [x] Volume nomeado com persistência
- [x] Rede nomeada
- [x] Banco não exposto ao host
- [x] Docker Compose funcionando
- [x] `.env` no `.gitignore`
- [x] `.env.example` versionado
- [x] CI funcionando
- [x] PR com CI vermelho documentado
- [x] Todos os prints adicionados

---

# CD – Publicação no Docker Hub

**Aluno(a):** [Seu nome]

**Turma:** [Sua turma]

**Usuário do Docker Hub:** [Seu usuário]

**Imagem publicada:**

```
[seu-usuario]/projeto-devops:latest
```

**Link da imagem no Docker Hub**

[Cole aqui o link.]

**Dispara quando**

```
push na branch main
```

**Workflow**

```
.github/workflows/cd.yml
```

---

## Print 1

*Token criado no Docker Hub.*

(Coloque a imagem aqui)

---

## Print 2

*Secrets cadastrados no GitHub (`DOCKERHUB_USERNAME` e `DOCKERHUB_TOKEN`).*

(Coloque a imagem aqui)

---

## Print 3

*Workflow de CD executado com sucesso.*

(Coloque a imagem aqui)

---

## Print 4

*Imagem publicada no Docker Hub.*

(Coloque a imagem aqui)

---

## Print 5

*Resultado do comando `docker pull`.*

(Coloque a imagem aqui)

---

# Respostas

### 1. O que é o Docker Hub?

O Docker Hub é um serviço online utilizado para armazenar, compartilhar e distribuir imagens Docker. Ele permite publicar imagens para que possam ser baixadas e executadas em qualquer computador que tenha Docker instalado.

---

### 2. Qual a diferença entre CI e CD?

O CI (Integração Contínua) verifica automaticamente se o projeto está funcionando corretamente sempre que há alterações no código. Já o CD (Entrega Contínua) publica automaticamente a imagem Docker no Docker Hub após o processo de CI ser concluído com sucesso.

---

### 3. Por que usamos token e Secrets em vez de escrever usuário e senha no `cd.yml`?

Porque usuário e senha são informações sensíveis. Os Secrets do GitHub armazenam esses dados de forma segura, enquanto o token permite um acesso controlado ao Docker Hub sem expor a senha da conta no repositório.

---

### 4. O que significa a tag `latest`?

A tag `latest` representa a versão mais recente da imagem publicada. Quando nenhuma versão específica é informada no comando `docker pull`, normalmente essa é a versão utilizada pelo Docker.
