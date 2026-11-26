# Comunicação entre containers

Os containers Docker são ambientes isolados, mas podem precisar se comunicar para que uma aplicação funcione corretamente.

Um exemplo comum é uma aplicação dividida em três containers:

```text
                 REDE DOCKER
                      |
              ┌───────────────┐
              │ Load Balancer │
              │    / Proxy    │
              └───────┬───────┘
                      |
              ┌───────▼───────┐
              │  Aplicação    │
              │  (Backend)    │
              └───────┬───────┘
                      |
              ┌───────▼───────┐
              │    MySQL      │
              │   (Banco)     │
              └───────────────┘
```

Nesse cenário:

- **Container 1:** recebe e distribui as requisições (Load Balancer / Proxy).
- **Container 2:** executa o código-fonte da aplicação.
- **Container 3:** executa o banco de dados MySQL.

Para que esses containers funcionem em conjunto, precisamos estabelecer uma comunicação entre eles.

## 1. Volumes x Networks

É importante entender que volumes e networks possuem responsabilidades diferentes.

| Recurso | Responsabilidade |
|---|---|
| Volumes | Persistir e compartilhar dados entre containers. |
| Networks | Permitir a comunicação entre containers. |

Por exemplo:

- Podemos utilizar um volume para persistir os dados do MySQL, evitando que eles sejam perdidos quando o container for removido.
- Podemos utilizar uma network para permitir que a aplicação acesse o banco de dados.

**Neste estudo, vamos trabalhar com as networks.**

---

# 2. Redes padrão do Docker

Quando instalamos o Docker, ele disponibiliza algumas redes padrão.

Para visualizar as redes existentes:

```bash
docker network ls
```

Entre elas, temos:

| Rede | Descrição |
|---|---|
| bridge | Rede padrão utilizada por containers quando nenhuma outra é especificada. |
| host | Compartilha a rede do host com o container. |
| none | Desabilita a conectividade de rede do container. |

Por padrão, quando criamos um container sem especificar uma rede, ele é conectado à rede `bridge` padrão.

## 3. Criando um container e inspecionando sua rede

Vamos criar um container Ubuntu em modo interativo:

```bash
docker run -it ubuntu
```

**Entendendo os parâmetros:**

- `docker run`: cria e executa um container.
- `-i`: mantém a entrada interativa aberta.
- `-t`: disponibiliza um terminal.
- `ubuntu`: imagem utilizada.

O terminal ficará parecido com:

```bash
root@1307ff58fb19:/#
```

Agora, abra outro terminal no sistema operacional (fora do container) e execute:

```bash
docker ps
```

Identifique o container ativo e inspecione suas configurações:

```bash
docker inspect ID_CONTAINER
```

Dentro do resultado, procure por:

```json
"NetworkSettings": {
    "Networks": {
        "bridge": {
            "Gateway": "172.17.0.1",
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16
        }
    }
}
```

Algumas informações importantes:

- `Gateway`: endereço do gateway da rede.
- `IPAddress`: endereço IP atribuído ao container.
- `NetworkID`: identificador da rede à qual o container está conectado.

Nesse exemplo, o container recebeu o IP `172.17.0.2`.

## 4. Descobrindo o IP de dentro do container

Também podemos consultar o endereço IP diretamente pelo terminal do container:

```bash
hostname -i
```

Exemplo de saída:

```bash
172.17.0.2
```

Isso confirma que o container recebeu um endereço IP da rede Docker.

**Observação:** os endereços IP podem mudar quando os containers são recriados.

## 5. Testando a comunicação entre containers

Containers conectados à mesma rede podem se comunicar utilizando seus endereços IP.

Para testar essa comunicação, precisamos instalar o utilitário `ping` dentro do Ubuntu:

```bash
apt-get update && apt-get install -y iputils-ping
```

Agora podemos testar a comunicação:

```bash
ping 172.17.0.3
```

Nesse exemplo, estamos tentando alcançar outro container pelo IP.

Se houver resposta, significa que existe comunicação entre os containers.

> Importante: o ping depende de o container de destino estar acessível e permitir tráfego ICMP. Nem toda falha de ping significa necessariamente que a comunicação entre aplicações está indisponível.

---

# 6. Criando nossa própria rede Docker

Embora seja possível utilizar a rede padrão `bridge`, existe uma alternativa mais organizada: criar uma rede própria para nossos containers.

Uma vantagem importante é que, nas redes `bridge` criadas pelo usuário, o Docker disponibiliza resolução DNS automática.

Isso significa que podemos acessar outros containers pelo nome, sem precisar decorar seus endereços IP.

### 6.1 Criando uma rede

Execute:

```bash
docker network create --driver bridge minha-rede
```

**Entendendo o comando:**

- `docker network create`: cria uma nova rede.
- `--driver bridge`: define o tipo de rede.
- `minha-rede`: nome escolhido para a rede.

### 6.2 Verificando as redes disponíveis

```bash
docker network ls
```

Nossa rede `minha-rede` deverá aparecer na listagem.

### 6.3 Criando um container conectado à rede

Agora vamos criar um container Ubuntu conectado à nossa rede:

```bash
docker run -it --name meu-container-ubuntu --network minha-rede ubuntu
```

**Novos parâmetros:**

- `--name`: define um nome para o container.
- `--network`: determina em qual rede o container será conectado.

Podemos utilizar o `docker inspect` novamente para verificar se o container está conectado à rede correta.

### 6.4 Testando a comunicação pelo nome

Agora vamos criar um segundo container na mesma rede:

Abra outro terminal e execute:

```bash
docker run -it --name segundo-container --network minha-rede ubuntu
```

Instale o `ping` nos containers, caso ainda não tenha feito isso:

```bash
apt-get update && apt-get install -y iputils-ping
```

Dentro do primeiro container, execute:

```bash
ping segundo-container
```

O Docker conseguirá resolver o nome `segundo-container` para o IP correspondente.

Isso acontece porque as redes `bridge` criadas pelo usuário possuem resolução DNS automática.

**Principal vantagem:** não precisamos descobrir ou informar manualmente o endereço IP de cada container.

---

# 7. Resumo do aprendizado

| Conceito | O que faz |
|---|---|
| Network | Permite a comunicação entre containers. |
| Bridge | Driver de rede que conecta containers em uma rede virtual. |
| IPAddress | Endereço IP atribuído ao container. |
| Gateway | Endereço utilizado para alcançar outras redes. |
| Docker DNS | Permite localizar containers pelo nome em redes criadas pelo usuário. |
| Volume | Permite persistir ou compartilhar dados. |

### Comandos importantes

```bash
# Listar redes
docker network ls

# Criar uma rede
docker network create --driver bridge minha-rede

# Inspecionar um container
docker inspect ID_CONTAINER

# Criar container conectado a uma rede
docker run -it --name meu-container --network minha-rede ubuntu

# Conectar um container existente a uma rede
docker network connect minha-rede ID_CONTAINER

# Inspecionar uma rede
docker network inspect minha-rede

# Remover uma rede
docker network rm minha-rede
```

## Conclusão

A comunicação entre containers é fundamental para trabalhar com aplicações distribuídas em Docker.

Em projetos reais, podemos separar cada responsabilidade em um container e conectá-los por meio de uma rede própria.

Assim, nossa aplicação consegue acessar serviços como banco de dados, APIs e proxies utilizando os nomes dos containers, sem depender de endereços IP fixos.