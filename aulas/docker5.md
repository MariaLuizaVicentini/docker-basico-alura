# Relembrando containers

Um container é uma instância criada a partir de uma imagem.

A imagem possui camadas de leitura (*read-only*), enquanto o container adiciona uma camada de escrita (*read-write*).

Isso significa que:

- A imagem original não é modificada quando trabalhamos com o container.
- As alterações feitas durante a execução ficam na camada de escrita do container.
- Quando removemos o container, sua camada de escrita também é removida. Consequentemente, os dados armazenados somente nela são perdidos.

**Problema:** como manter os dados mesmo quando um container é removido?

É comum derrubar e criar containers novamente. Por isso, precisamos de uma maneira de armazenar dados independentemente do ciclo de vida do container.

É aí que entram os **volumes**.

---

# Volumes

Volumes são mecanismos de persistência de dados do Docker.

Eles permitem armazenar dados fora da camada de escrita do container, mantendo essas informações mesmo quando o container é removido.

### Como funcionam?

- Um volume é armazenado no Docker Host (máquina onde o Docker está instalado).
- O Docker gerencia o local onde os dados do volume ficam armazenados.
- O container acessa esse armazenamento por meio de uma montagem (*mount*).
- As alterações feitas pelo container são gravadas no volume, e não na camada de escrita do container.

**Importante:** o volume e o container são independentes. Por isso, remover um container não significa remover o volume ou os dados armazenados nele.

### Visualizando a ideia

```text
DOCKER HOST
│
├── Container
│   ├── Camadas da imagem (leitura)
│   └── Camada de escrita
│
└── Volume
    └── Dados persistentes
```

O container acessa os dados do volume por meio de uma montagem.

Assim, podemos remover e recriar containers sem perder os dados que precisam ser preservados.


---
## Criando um container com volume

Podemos criar um container e definir um volume utilizando a opção `-v` do comando `docker run`.

```bash
docker run -v "/var/www" ubuntu
```

Nesse exemplo:
- `docker run`: cria e inicia um container.
- `-v /var/www`: define um volume montado no diretório `/var/www` do container.
- `ubuntu`: imagem utilizada para criar o container.

**Importante:** quando informamos apenas um caminho em `-v`, sem especificar um caminho no Docker Host, o Docker cria e gerencia um volume anônimo.

### Inspecionando o volume

Para visualizar informações do container, utilizamos:

```bash
docker inspect ID_DO_CONTAINER
```

No resultado, procure pela propriedade `Mounts`.

Ela apresenta informações sobre as montagens realizadas no container, incluindo:

- `Type`: tipo de montagem (`volume` ou `bind`).
- `Source`: caminho onde os dados ficam armazenados no Docker Host.
- `Destination`: diretório dentro do container onde o volume está montado.

**Atenção:** `Source` representa o caminho no Docker Host, enquanto `Destination` representa o caminho acessível dentro do container.

### Acessando o container pelo terminal

Para iniciar um container em modo interativo, utilizamos:

```bash
docker run -it ubuntu
```

- `-i` (*interactive*): mantém a entrada do terminal aberta.
- `-t` (*tty*): disponibiliza um terminal para interação.

Podemos combinar a criação do container com a montagem do volume:

```bash
docker run -it -v /var/www ubuntu
```

Assim, entramos no terminal do container e podemos executar comandos e manipular arquivos dentro dele.