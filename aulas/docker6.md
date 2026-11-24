# Iniciando um container para executar uma aplicação Node

O objetivo é criar um container a partir de uma imagem Node, disponibilizar o acesso à aplicação e compartilhar os arquivos do projeto com o container.

```bash
docker run -d -p 8080:3000 -v "$(pwd):/var/www" -w "/var/www" node npm run start
```

### `docker run`

Cria e inicia um novo container a partir de uma imagem.

### `-d`

Significa *detached*.

Executa o container em segundo plano, liberando o terminal para continuar trabalhando.

### `-p 8080:3000`

Faz o mapeamento de portas, permitindo acessar a aplicação executada dentro do container por meio da máquina host.

**Sintaxe:**

```bash
-p PORTA_HOST:PORTA_CONTAINER
```

- `8080`: porta disponibilizada na máquina host.
- `3000`: porta utilizada pela aplicação dentro do container.

Nesse exemplo, podemos acessar a aplicação pelo navegador através de `http://localhost:8080`.

### `-v "$(pwd):/var/www"`

Cria um volume do tipo *bind mount*, compartilhando um diretório da máquina host com o container.

**Sintaxe:**

```bash
-v CAMINHO_HOST:CAMINHO_CONTAINER
```

- `$(pwd)`: retorna o caminho absoluto do diretório atual da máquina host.
- `/var/www`: diretório dentro do container onde os arquivos serão disponibilizados.

Isso permite que o container acesse os arquivos do projeto diretamente da máquina host. Alterações nos arquivos também ficam disponíveis dentro do container.

### `-w "/var/www"`

Define o diretório de trabalho (*working directory*) dentro do container.

Nesse caso, os comandos seguintes serão executados a partir de `/var/www`, sem precisar informar o caminho completo.

### `node`

Nome da imagem utilizada para criar o container. Nesse caso, utilizamos a imagem Node.js.

### `npm run start`

Comando executado dentro do container.

Executa o script `start` definido no `package.json` do projeto.

**Exemplo:**

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

---

### Resumo dos parâmetros

| Parâmetro | Função |
|---|---|
| `docker run` | Cria e inicia um container. |
| `-d` | Executa em segundo plano. |
| `-p` | Mapeia as portas do host e do container. |
| `-v` | Compartilha diretórios entre host e container. |
| `-w` | Define o diretório de trabalho. |
| `node` | Imagem utilizada. |
| `npm run start` | Comando executado dentro do container. |