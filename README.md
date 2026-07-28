# Projeto Docker + Docker Compose + CI + CD

## Informações do aluno

**Aluno(a):** Luiza Eduarda L da C Mendes  
**Turma:** Noturno
**Data:** 28/07/2026

**Aplicação utilizada:** Docker Getting Started App (To-Do em Node.js)

---

# Sobre o projeto

Este projeto teve como objetivo aprender a utilizar Docker para empacotar uma aplicação, criar imagens, trabalhar com containers, volumes, redes e Docker Compose.

Também foi implementado um processo de CI usando GitHub Actions para testar automaticamente a aplicação e um processo de CD para publicar a imagem Docker no Docker Hub.

---

# 1. Como executar o projeto

Clone o repositório:

```bash
git clone https://github.com/dudalps435/meu-projeto-docker.git
```

Entre na pasta:

```bash
cd meu-projeto-docker
```

Crie o arquivo de ambiente:

```bash
cp .env.example .env
```

Suba a aplicação:

```bash
docker compose up -d --build
```

A aplicação estará disponível em:

```
http://localhost:3000
```

Para parar os containers:

```bash
docker compose down
```

Para remover containers e volumes:

```bash
docker compose down -v
```

---

# 2. Dockerfile Multi-stage

Foi criado um Dockerfile utilizando multi-stage build.

Foram utilizados dois estágios:

## Builder

Responsável por instalar as dependências e preparar a aplicação.

## Runtime

Responsável apenas por executar a aplicação com os arquivos necessários.

A imagem base utilizada foi:

```
node:20-alpine
```

A aplicação roda utilizando usuário não-root para melhorar a segurança.

## Tamanho da imagem final

```
A imagem final possui aproximadamente 286MB.
```

## Por que o multi-stage ajuda?

O multi-stage reduz o tamanho da imagem final porque separa a instalação das dependências da execução da aplicação. Assim são enviados apenas os arquivos necessários, deixando a imagem mais leve e segura.

---

# 3. Volumes e persistência

Foram realizados testes para verificar a diferença entre usar e não usar volumes.

Sem volume, ao remover o container os dados das tarefas são perdidos.

Com volume nomeado, os dados permanecem salvos mesmo criando um novo container.

## Volume utilizado

```
todo-db
```

## Caminho dentro do container

```
/etc/todos
```

## Diferença entre docker compose down e docker compose down -v

O `docker compose down` remove os containers, mas mantém os dados dos volumes. Já o `docker compose down -v` remove também os volumes e apaga os dados armazenados.

---

# 4. Rede Docker

Foi criada uma rede para permitir a comunicação entre a aplicação e o banco MySQL.

## Rede criada

```
todo-net
```

## Serviços conectados

- app
- mysql

O banco de dados não possui porta publicada para o host, pois somente a aplicação precisa acessar o serviço.

O aplicativo consegue encontrar o banco pelo nome `mysql` porque o Docker possui DNS interno, permitindo comunicação entre containers pelo nome do serviço.

---

# 5. Docker Compose

Foi criado o arquivo:

```
compose.yaml
```

Ele permite subir toda a aplicação com apenas um comando.

Serviços utilizados:

- app
- db

Recursos configurados:

- Rede nomeada
- Volume nomeado
- Variáveis de ambiente
- Healthcheck no banco
- Dependência do app aguardando o banco estar pronto

As informações sensíveis ficam no arquivo `.env`, que não é enviado para o GitHub.

O arquivo `.env.example` serve como modelo para configuração.

---

# 6. Integração Contínua (CI)

Foi criado o workflow:

```
.github/workflows/ci.yml
```

O CI é executado nos eventos:

- push
- pull_request

O pipeline realiza:

1. Download do código.
2. Criação do arquivo `.env`.
3. Validação do Docker Compose.
4. Build da imagem.
5. Inicialização dos containers.
6. Teste para verificar se a aplicação responde.
7. Criação de uma tarefa pela API.
8. Encerramento dos containers.

---

# 7. Quebra proposital do CI

Foi realizada uma alteração proposital para testar se o CI conseguiria identificar um erro.

## Como o CI reagiu:

O GitHub Actions identificou o problema durante a execução do teste e marcou o workflow como falho.

## Como foi corrigido:

A alteração incorreta foi desfeita e um novo commit foi enviado. Depois disso o pipeline voltou a funcionar.

## Link do Pull Request:

[LINK DO PR]

---

# 8. Dificuldades e aprendizados

Durante a atividade tive dificuldades principalmente na configuração do Docker Compose, volumes e GitHub Actions. Com os testes e correções consegui entender melhor como os containers funcionam, como eles se comunicam e como automatizar testes usando CI.

Também aprendi como publicar uma imagem Docker automaticamente usando CD.

---

# 9. Checklist

✅ Dockerfile multi-stage funcionando  
✅ .dockerignore criado  
✅ Container sem usuário root  
✅ Volume nomeado com persistência  
✅ Rede Docker configurada  
✅ Banco sem porta exposta  
✅ Docker Compose funcionando  
✅ .env protegido  
✅ .env.example criado  
✅ CI funcionando  
✅ Erro proposital documentado  
✅ CD publicado no Docker Hub  

---

# CD - Publicação no Docker Hub


**Usuário Docker Hub:**

```
dudalps
```

**Imagem publicada:**

```
[SEU USUARIO/meu-projeto-docker:latest
```

**Link da imagem:**

https://hub.docker.com/repository/docker/dudalps/meu-projeto-docker/general

---

## Workflow utilizado

Arquivo:

```
.github/workflows/cd.yml
```

O CD é executado quando ocorre:

```
push na branch main
```

O processo realiza:

1. Baixa o código.
2. Faz login no Docker Hub usando Secrets.
3. Constrói a imagem Docker.
4. Publica a imagem automaticamente.

---

# Respostas do CD

## 1. O que é o Docker Hub?

Na minha visão, o Docker Hub é um lugar onde podemos guardar e compartilhar imagens Docker pela internet. Assim outras pessoas conseguem baixar uma imagem pronta e executar uma aplicação sem precisar configurar tudo novamente.

---

## 2. Qual a diferença entre CI e CD?

O CI serve para testar automaticamente se o projeto continua funcionando quando faço alterações no código. O CD serve para pegar essa aplicação que passou pelos testes e publicar ela automaticamente em um lugar como o Docker Hub.

---

## 3. Por que usamos token e Secrets em vez de escrever usuário e senha no arquivo?

Porque deixar usuário e senha no código seria inseguro, já que qualquer pessoa com acesso ao repositório poderia ver essas informações. Os Secrets guardam esses dados de forma protegida e o token permite um acesso controlado.

---

## 4. O que significa a tag latest?

A tag `latest` significa que aquela imagem representa a versão mais recente publicada. Quando não informamos uma versão específica ao baixar uma imagem, normalmente essa é a versão utilizada.

---

# Conclusão

Com essa atividade foi possível entender o caminho completo de uma aplicação: desde a criação da imagem Docker, execução em containers, comunicação entre serviços, testes automáticos com CI e publicação automática utilizando CD.
