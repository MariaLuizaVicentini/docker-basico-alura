# Docker - Container que executa um site estático

## 1. Executando um container

Ao executar um container a partir de uma imagem, o Docker:

1. Verifica se a imagem existe localmente.
2. Caso não exista, tenta baixá-la do Docker Hub.
3. Cria o container a partir da imagem.
4. Executa o processo principal definido pela imagem.

**Executando o site estático:**

```bash
docker run dockersamples/static-site
```

Nesse caso, o container é executado em primeiro plano, mantendo o terminal associado ao processo.

### Executando em segundo plano

Utilizamos a opção `-d` (*detached*) para executar o container em segundo plano, liberando o terminal para outros comandos.

```bash
docker run -d dockersamples/static-site
```

- `-d`: executa o container em segundo plano.
- O Docker retorna o ID do container.
- O processo continua sendo executado independentemente do terminal atual.

## 2. Parando um container

Para interromper a execução de um container:

```bash
docker stop ID_DO_CONTAINER
```

Por padrão, o Docker aguarda um período para que o processo finalize corretamente.

**Definindo o tempo de espera:**

```bash
docker stop -t 0 ID_DO_CONTAINER
```

- `-t`: define o tempo de espera (*timeout*).
- `0`: não aguarda o período padrão antes de forçar a interrupção.

Exemplo:

```bash
docker stop -t 0 b9a5e6339e02
```

---

# 3. Portas e acesso ao site estático

Quando executamos um container que contém um servidor web, a aplicação pode ficar escutando em uma porta interna.

No exemplo do site estático:

- `80`: HTTP.
- `443`: HTTPS.

Podemos verificar as portas expostas utilizando:

```bash
docker ps
```

Exemplo de saída:

```text
CONTAINER ID   IMAGE                       STATUS          PORTS
b9a5e6339e02   dockersamples/static-site   Up 12 seconds   80/tcp, 443/tcp
```

**Importante:** as portas `80` e `443` estão expostas pelo container, mas ainda não possuem necessariamente um mapeamento para portas do computador (host).

Para acessar a aplicação pelo navegador, precisamos mapear essas portas.

## 4. Mapeamento automático de portas (-P)

Utilizamos `-P` (P maiúsculo) para mapear automaticamente as portas expostas pelo container para portas disponíveis no host.

```bash
docker run -d -P dockersamples/static-site
```

O Docker escolhe portas aleatórias disponíveis no computador.

Exemplo:

| Porta do container | Porta do host |
|---|---|
| 80/tcp | 32768 |
| 443/tcp | 32769 |

Isso significa que a aplicação continua escutando na porta `80` dentro do container, mas podemos acessá-la pela porta `32768` do computador.

### Descobrindo as portas mapeadas

Utilizamos:

```bash
docker port ID_DO_CONTAINER
```

Exemplo:

```bash
docker port b9a5e6339e02
```

Saída:

```text
80/tcp -> 0.0.0.0:32768
80/tcp -> [::]:32768
443/tcp -> 0.0.0.0:32769
443/tcp -> [::]:32769
```

**Entendendo a saída:**

- `80/tcp`: porta interna do container.
- `32768`: porta do host.
- `0.0.0.0`: endereço IPv4 que aceita conexões em todas as interfaces de rede.
- `[::]`: endereço IPv6.

Para acessar o site pelo navegador:

```text
http://localhost:32768
```

Utilizamos `http://` porque estamos acessando a porta HTTP (80).

---

## 5. Nomeando um container

Por padrão, o Docker gera nomes aleatórios para os containers.

Podemos definir um nome personalizado utilizando `--name`.

```bash
docker run -d -P --name meu-site dockersamples/static-site
```

Agora podemos identificar o container pelo nome:

```bash
docker ps
```

Também podemos utilizar o nome para executar outros comandos:

```bash
docker stop meu-site
```

Isso facilita a administração dos containers, pois não precisamos memorizar ou copiar seus IDs.

---

## 6. Mapeamento manual de portas (-p)

Além do `-P`, existe a opção `-p` (p minúsculo).

A diferença é que, com `-p`, definimos manualmente qual porta do host será associada à porta do container.

```bash
docker run -d -p 8080:80 dockersamples/static-site
```

A estrutura é:

```text
-p PORTA_DO_HOST:PORTA_DO_CONTAINER
```

Nesse exemplo:

- `8080`: porta do computador (host).
- `80`: porta interna do container.

Para acessar o site:

```text
http://localhost:8080
```

### Diferença entre -P e -p

| Opção | Funcionamento |
|---|---|
| `-P` | Mapeia automaticamente para portas disponíveis no host. |
| `-p` | Permite escolher manualmente a porta do host. |

---

## 7. Resumo dos comandos

| Comando | Finalidade |
|---|---|
| `docker run imagem` | Cria e executa um container. |
| `docker run -d imagem` | Executa em segundo plano. |
| `docker run -P imagem` | Mapeia portas automaticamente. |
| `docker run -p 8080:80 imagem` | Mapeia manualmente as portas. |
| `docker ps` | Lista containers em execução. |
| `docker ps -a` | Lista todos os containers. |
| `docker port ID` | Mostra as portas mapeadas. |
| `docker stop ID` | Interrompe um container. |
| `docker stop -t 0 ID` | Interrompe sem aguardar o timeout padrão. |
| `docker run --name nome imagem` | Define um nome para o container. |

## 8. Conceitos importantes

- **Imagem:** contém os arquivos e as instruções necessários para criar um container.
- **Container:** instância executável criada a partir de uma imagem.
- **Porta interna:** porta em que a aplicação escuta dentro do container.
- **Porta do host:** porta do computador utilizada para acessar a aplicação.
- **Mapeamento de portas:** associação entre uma porta do host e uma porta do container.
- **Detached (`-d`):** execução em segundo plano, sem manter o terminal associado ao processo.