# Construindo nossas próprias imagens

Uma imagem Docker funciona como uma **receita de bolo**: ela reúne as instruções necessárias para preparar um ambiente que poderá ser utilizado para criar containers.

Para construir nossas próprias imagens, utilizamos um arquivo de texto chamado **Dockerfile**.

Por padrão, seu nome é:

```text
Dockerfile
```

Cada instrução dentro dele define uma etapa da construção da imagem.

## Principais instruções

### FROM

Define a imagem base que será utilizada para construir nossa própria imagem.

```dockerfile
FROM node:22
```

- `node`: imagem base do Node.js.
- `22`: versão do Node.js utilizada.

É recomendável especificar uma versão em vez de utilizar `latest`, para ter mais previsibilidade sobre o ambiente utilizado.

### MAINTAINER

Identificava o responsável pela manutenção da imagem.

```dockerfile
MAINTAINER Malu Vicentini
```

**Observação:** essa instrução está obsoleta. Atualmente, recomenda-se utilizar `LABEL` para adicionar metadados à imagem.

```dockerfile
LABEL maintainer="Malu Vicentini"
```

### COPY

Copia arquivos e diretórios da máquina host para dentro da imagem.

```dockerfile
COPY . /var/www
```

- `.`: representa o diretório de origem (contexto de construção).
- `/var/www`: diretório de destino dentro da imagem.

Nesse exemplo, copiamos os arquivos do projeto para `/var/www`.

### WORKDIR

Define o diretório de trabalho dentro da imagem.

```dockerfile
WORKDIR /var/www
```

A partir dessa instrução, os comandos seguintes serão executados nesse diretório, sem precisar informar o caminho completo.

### RUN

Executa comandos durante a construção da imagem.

É utilizado para instalar dependências, configurar o ambiente e executar outras tarefas necessárias para preparar a imagem.

```dockerfile
RUN npm install
```

Nesse exemplo, instalamos as dependências definidas no `package.json`.

**Importante:** o `RUN` é executado durante o processo de construção da imagem, e não quando o container é iniciado.

### ENTRYPOINT

Define o comando principal que será executado quando um container for iniciado a partir da imagem.

```dockerfile
ENTRYPOINT ["npm", "start"]
```

Diferentemente do `RUN`, o `ENTRYPOINT` não é executado durante a construção da imagem, mas sim na inicialização do container.

### EXPOSE

Documenta a porta em que a aplicação espera receber conexões dentro do container.

```dockerfile
EXPOSE 3000
```

**Atenção:** o `EXPOSE` não publica a porta automaticamente para a máquina host. Para disponibilizar o acesso, utilizamos o mapeamento de portas com `-p` no `docker run`.

Exemplo:

```bash
docker run -p 8080:3000 minha-imagem
```

Nesse caso, acessamos a aplicação pela porta `8080` da máquina host, que será direcionada para a porta `3000` do container.

---

## Exemplo de Dockerfile

Juntando as instruções que estudamos:

```dockerfile
FROM node:22

LABEL maintainer="Malu Vicentini"

WORKDIR /var/www

COPY . .

RUN npm install

EXPOSE 3000

ENTRYPOINT ["npm", "start"]
```

## Construindo a imagem

Depois de criar o Dockerfile, precisamos construir a imagem utilizando:

```bash
docker build -t minha-imagem .
```

- `docker build`: inicia o processo de construção da imagem.
- `-t`: define um nome (tag) para a imagem.
- `minha-imagem`: nome escolhido para a imagem.
- `.`: indica o contexto de construção, ou seja, o diretório atual.

## Executando um container a partir da imagem

Depois de construir a imagem, podemos criar e iniciar um container:

```bash
docker run -d -p 8080:3000 minha-imagem
```

Assim, temos uma imagem própria, construída a partir das instruções definidas no Dockerfile, que pode ser utilizada para criar containers.