# EstanteWeb

Acervo de livros da disciplina Programação para Web 3.

## Como rodar

**1. Subir o banco de dados.** O MySQL roda em um contêiner Docker:

```sh
docker run --name estanteweb-mysql \
  -e MYSQL_ROOT_PASSWORD=estante \
  -e MYSQL_DATABASE=estanteweb \
  -p 3306:3306 -d mysql:8
```

Se o contêiner já existir de outra aula, basta iniciá-lo:

```sh
docker start estanteweb-mysql
```

**2. Rodar a aplicação.**

```sh
mvn spring-boot:run
```

O Flyway cria as tabelas e insere os dados iniciais na primeira execução. A aplicação
sobe em `http://localhost:8080`.

**3. Entrar.** O acervo é público, mas cadastrar, alterar e remover exigem login:

- E-mail: `marta@estanteweb.com`
- Senha: `estante123`

## Acessando o banco pelo terminal

```sh
docker exec -it estanteweb-mysql mysql -uroot -pestante estanteweb
```

## Recriando o banco do zero

Se os dados ficarem inconsistentes, apague o banco e deixe o Flyway refazer tudo:

```sh
docker exec -it estanteweb-mysql mysql -uroot -pestante \
  -e "DROP DATABASE estanteweb; CREATE DATABASE estanteweb;"
```

Depois reinicie a aplicação.

## Rotas

| Caminho | Método | Papel | Acesso |
| ---------------------- | ------ | ---------------- | -------- |
| `/livros/html` | GET | lista o acervo | público |
| `/livros/{id}` | GET | exibe um livro | público |
| `/livros/novo` | GET | formulário vazio | restrito |
| `/livros` | POST | cria | restrito |
| `/livros/{id}/editar` | GET | formulário cheio | restrito |
| `/livros/{id}` | POST | altera | restrito |
| `/livros/{id}/excluir` | GET | pede confirmação | restrito |
| `/livros/{id}/excluir` | POST | remove | restrito |
| `/sugestoes` | GET | lista sugestões | público |
| `/sugestoes/nova` | GET | formulário | público |
| `/sugestoes` | POST | grava sugestão | público |
| `/login` | GET | formulário | público |
| `/login` | POST | autentica | público |
| `/logout` | POST | encerra a sessão | público |
