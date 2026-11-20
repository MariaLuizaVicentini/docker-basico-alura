# Docker

Plataforma de containerização que permite criar, executar e gerenciar aplicações em ambientes isolados chamados **containers**.

Auxilia no desenvolvimento, distribuição, deploy e execução de aplicações, garantindo maior consistência entre diferentes ambientes.

## Arquitetura

Podemos representar a arquitetura de uma aplicação utilizando Docker da seguinte maneira:

```text
APP 1          APP 2
  |              |
Container 1   Container 2
       \       /
      Docker Engine
            |
       Sistema Operacional
            |
         Hardware
```

Os containers compartilham o kernel do sistema operacional hospedeiro, mas possuem isolamento entre seus processos e recursos.

Diferentemente das máquinas virtuais, não precisam executar um sistema operacional completo para cada aplicação.

## Docker Engine

Principal componente responsável por construir, executar e gerenciar containers.

Responsabilidades:
- Criar e executar containers.
- Gerenciar imagens, volumes e redes.
- Intermediar as operações entre o Docker e o sistema operacional.
- Disponibilizar uma API para comunicação com o Docker.

## Docker Compose

Ferramenta utilizada para definir e gerenciar aplicações compostas por múltiplos containers.

Permite descrever os serviços, redes e volumes de uma aplicação em um arquivo `compose.yaml`.

Exemplo: uma aplicação que utiliza:
- Backend.
- Frontend.
- Banco de dados.

Com o Compose, conseguimos configurar e executar esses serviços em conjunto.

## Docker Swarm

Ferramenta nativa do Docker para orquestração de containers em múltiplas máquinas.

Permite:
- Gerenciar um conjunto de máquinas (cluster).
- Distribuir containers entre diferentes máquinas.
- Escalar serviços.
- Gerenciar a disponibilidade dos serviços.

## Docker Hub

Registro de imagens Docker (Container Registry).

Permite armazenar, compartilhar e distribuir imagens Docker.

Podemos:
- Baixar imagens públicas.
- Publicar nossas próprias imagens.
- Compartilhar imagens entre desenvolvedores.

Exemplo:

```bash
docker pull nginx
```

Baixa a imagem do Nginx do Docker Hub.

## Docker Machine

Ferramenta utilizada para provisionar e gerenciar máquinas com Docker Engine instalado.

Permitia criar hosts Docker em diferentes ambientes, como máquinas virtuais locais e provedores de nuvem.

**Observação:** é uma ferramenta legada e não faz parte dos fluxos modernos recomendados para utilização do Docker.