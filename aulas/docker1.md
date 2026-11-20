## Problema de antigamente

Conforme as aplicações crescem, aumenta também a quantidade de dependências de serviços externos.

Antigamente, cada serviço geralmente precisava ser executado em uma máquina física própria, com seu próprio sistema operacional (SO).

Exemplos:
- Apache
- Tomcat
- MySQL
- MongoDB

Para que esses serviços se comunicassem entre si e com a aplicação principal, as máquinas precisavam estar conectadas por uma rede.

**Problemas desse modelo:**
- Alto custo com equipamentos físicos, energia e manutenção.
- Dificuldade de gerenciamento de várias máquinas.
- Deploys e configurações mais trabalhosos.
- Desperdício de recursos: os servidores precisavam ter capacidade para suportar momentos de pico, mas ficavam ociosos na maior parte do tempo.

## Para melhorar esse cenário: virtualização

Surgiram as máquinas virtuais (VMs).

A virtualização permite executar várias máquinas virtuais dentro de um único servidor físico, compartilhando seus recursos de hardware.

**Máquinas virtuais (VMs):**
- Cada VM possui recursos virtuais próprios, como CPU, memória RAM e armazenamento.
- Cada VM possui seu próprio sistema operacional.
- Funcionam como ambientes independentes e isolados.
- Podem executar sistemas operacionais diferentes na mesma máquina física.

Exemplo:

Um único servidor físico pode executar três VMs:
- VM 1: Linux + Apache.
- VM 2: Linux + MySQL.
- VM 3: Windows + outro serviço.

**Vantagens:**
- Melhor aproveitamento dos recursos físicos.
- Redução da quantidade de servidores físicos necessários.
- Menor custo com energia, espaço físico e manutenção.
- Maior flexibilidade para criar e gerenciar ambientes.

Com isso, a infraestrutura se tornou mais eficiente, reduzindo custos e desperdício de recursos.

---

# Containers

A virtualização resolveu muitos problemas, mas também trouxe algumas limitações:

- Cada VM possui seu próprio sistema operacional, consumindo memória RAM, armazenamento e outros recursos.
- A inicialização das VMs costuma ser mais lenta.
- Atualizações, manutenção e segurança se tornam mais trabalhosas, pois cada VM precisa ser administrada individualmente.

## O que são containers?

Um container é um ambiente isolado que empacota uma aplicação e suas dependências necessárias para execução.

Podemos imaginar um container como uma caixinha que contém:
- A aplicação.
- Bibliotecas.
- Dependências.
- Configurações necessárias para execução.

**Como funciona?**

Diferentemente das máquinas virtuais, os containers não precisam de um sistema operacional completo para cada aplicação.

Eles compartilham o kernel do sistema operacional hospedeiro, mas mantêm isolamento entre suas aplicações e seus processos.

Cada container pode ter seu próprio ambiente de execução e configurações de rede, por exemplo.

**Vantagens:**
- Menor consumo de recursos em comparação com VMs.
- Inicialização mais rápida.
- Facilidade para criar, remover e distribuir ambientes.
- Maior portabilidade entre diferentes ambientes.
- Isolamento entre aplicações e suas dependências.