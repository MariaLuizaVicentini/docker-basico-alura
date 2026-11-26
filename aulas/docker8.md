# Publicando uma imagem no Docker Hub

O **Docker Hub** é um repositório público ou privado onde podemos armazenar e compartilhar imagens Docker.

Para publicar uma imagem local no Docker Hub, precisamos realizar três etapas:

1. Autenticar no Docker Hub.
2. Criar uma tag associada ao repositório remoto.
3. Enviar a imagem para o Docker Hub.

## 1. Autenticação no Docker Hub

Primeiro, precisamos autenticar o Docker CLI na nossa conta do Docker Hub.

```bash
docker login
```

O comando solicitará as credenciais da conta. Após a autenticação, será possível enviar imagens para os repositórios aos quais temos acesso.

## 2. Criando uma tag para a imagem

Antes de enviar a imagem, precisamos associá-la ao nome do repositório no Docker Hub.

```bash
docker tag volume-exemplo:latest vicentinimaluzeradev/volume-exemplo:latest
```

**Entendendo o comando:**

- `docker tag`: cria uma nova referência (tag) para uma imagem existente.
- `volume-exemplo:latest`: nome e tag da imagem local.
- `vicentinimaluzeradev/volume-exemplo:latest`: nome do usuário, repositório e tag que serão utilizados no Docker Hub.

**Padrão de nomenclatura:**

```text
usuario/nome-do-repositorio:tag
```

> Importante: o comando `docker tag` não cria uma nova imagem. Ele apenas cria uma referência para a imagem existente, permitindo identificá-la com o nome do repositório remoto.

## 3. Enviando a imagem para o Docker Hub

Com a imagem identificada corretamente, podemos enviá-la:

```bash
docker push vicentinimaluzeradev/volume-exemplo:latest
```

O Docker fará o upload das camadas da imagem para o repositório correspondente no Docker Hub.

Após a conclusão, a imagem estará disponível no repositório da sua conta.

## Resumo do fluxo

```bash
# 1. Autenticar no Docker Hub
docker login

# 2. Criar uma tag para o repositório remoto
docker tag volume-exemplo:latest vicentinimaluzeradev/volume-exemplo:latest

# 3. Enviar a imagem
docker push vicentinimaluzeradev/volume-exemplo:latest
```

## Consultando as imagens locais

Para verificar as imagens disponíveis na máquina:

```bash
docker images
```

Isso permite conferir se a imagem existe localmente e identificar seu nome e tag antes de iniciar o processo de publicação.