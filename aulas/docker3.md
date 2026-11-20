# Imagens e Containers

## Imagens (Images)

Uma imagem é um modelo utilizado para criar containers. Ela contém as instruções, dependências, bibliotecas e arquivos necessários para executar uma aplicação.

Quando tentamos executar uma imagem, o Docker procura localmente. Caso não encontre, tenta baixá-la de um registry, como o Docker Hub.

**Exemplo:**

```bash
docker run nginx
```

O comando `docker run`:
1. Procura a imagem localmente.
2. Se não encontrar, baixa do Docker Hub.
3. Cria um container a partir da imagem.
4. Inicia o container.

**Principais comandos:**

- Baixar uma imagem e criar/iniciar um container:
```bash
docker run nome-imagem
```

- Listar containers em execução:
```bash
docker ps
```

- Listar todos os containers:
```bash
docker ps -a
```

- Iniciar um container existente:
```bash
docker start id-container
```

- Parar um container:
```bash
docker stop id-container
```

- Remover um container:
```bash
docker rm id-container
```

- Remover todos os containers parados:
```bash
docker container prune
```

- Listar imagens locais:
```bash
docker images
```

- Remover uma imagem:
```bash
docker rmi nome-imagem
```

**Diferença importante:**
- Image: modelo utilizado para criar containers.
- Container: instância executável de uma imagem.

Uma mesma imagem pode ser utilizada para criar vários containers independentes.

---

## Sistema de camadas (Layered Filesystem)

Imagens Docker são construídas a partir de várias camadas (*layers*).

Cada camada representa uma parte da construção da imagem, como sistema base, dependências, bibliotecas e arquivos da aplicação.

**Características das camadas:**
- São somente leitura (*read-only*).
- Podem ser compartilhadas entre diferentes imagens.
- Podem ser reaproveitadas, evitando duplicação de dados e economizando espaço.

### Camada de escrita (Writable Layer)

Quando criamos um container, o Docker adiciona uma camada de escrita sobre as camadas da imagem.

Essa camada permite que o container realize alterações sem modificar a imagem original.

**Estrutura:**

```text
        CONTAINER
┌─────────────────────────┐
│ Camada de escrita       │ ← Writable Layer
├─────────────────────────┤
│ Camada 3                │
├─────────────────────────┤
│ Camada 2                │ ← Read-only
├─────────────────────────┤
│ Camada 1                │
└─────────────────────────┘
          IMAGEM
```

**Importante:**
- As camadas da imagem são somente leitura.
- Cada container possui sua própria camada de escrita.
- Alterações realizadas no container ficam armazenadas nessa camada.
- Um container não modifica a imagem original.
- Ao remover o container, as alterações armazenadas exclusivamente na camada de escrita são perdidas.

Para persistir dados mesmo após remover um container, podemos utilizar volumes Docker.