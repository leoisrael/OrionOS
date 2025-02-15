# Projeto: OrionOS - Um Sistema Operacional Minimalista Baseado em NFS

## Introdução
O **OrionOS** é um conceito de sistema operacional **diskless** (sem disco) que carrega seu ambiente diretamente via rede, utilizando **PXE boot e NFS** como tecnologias principais. Ele permite que máquinas na rede operem sem armazenamento local, oferecendo um ambiente resiliente, modular e escalável. O OrionOS se propõe a redefinir a arquitetura de sistemas operacionais distribuídos, permitindo que qualquer nó na rede possa ser rapidamente provisionado e integrado ao cluster sem necessidade de configuração manual.

Diferentemente de distribuições tradicionais, **o OrionOS está sendo desenvolvido do zero, utilizando a metodologia Linux From Scratch (LFS)**, garantindo um controle absoluto sobre cada componente do sistema, permitindo otimizações profundas e um design arquitetural altamente especializado para ambientes diskless e distribuídos.

## Objetivos do Sistema
O OrionOS foi concebido com as seguintes finalidades:
- **Failover e Resiliência:** Permitir que múltiplos servidores rodem a mesma aplicação sem depender de armazenamento local, garantindo continuidade de serviços em caso de falhas.
- **Infraestrutura Escalável:** Novos servidores podem ser adicionados instantaneamente sem necessidade de instalação manual ou pré-configuração.
- **Gerenciamento Unificado:** Um único ponto de controle para todas as instâncias do sistema operacional, facilitando atualizações e manutenção.
- **Flexibilidade Arquitetural:** Permite a criação de clusters de computação distribuída, balanceamento de carga automatizado e distribuição de workloads dinâmicos.
- **Simplicidade e Controle Total:** Como estamos desenvolvendo do zero, podemos evitar dependências desnecessárias e criar um sistema enxuto e eficiente.

## Construção Baseada em Linux From Scratch (LFS)
O OrionOS será **desenvolvido do zero**, sem depender de distribuições prontas como Debian ou CentOS. Utilizaremos **Linux From Scratch (LFS)** como base para construir um sistema totalmente modular, adaptado às necessidades específicas de um ambiente NFS diskless.

### 1. Construção do Sistema Base

- **Compilação do Kernel:** O kernel do OrionOS será compilado manualmente a partir do código-fonte, incluindo apenas os módulos necessários para operar via PXE/NFS.
- **Sistema de Init:** O OrionOS utilizará um init system leve, possivelmente baseado em **runit** ou **OpenRC**, eliminando dependências pesadas como systemd.
- **Gerenciamento de Pacotes:** Como o sistema será mantido em NFS, um gerenciador de pacotes minimalista será implementado para facilitar atualizações remotas e controle de versões.
- **Libc e Compilador:** Será utilizado **musl libc** ou **glibc otimizado**, além de um toolchain próprio baseado no **GCC** para garantir total compatibilidade com nosso kernel customizado.

### 2. Otimizações para Boot PXE
- **Bootloader PXE:** O OrionOS carregará via **syslinux/iPXE**, permitindo a inicialização sem disco rígido.
- **Montagem Inicial via Initrd:** Um initrd mínimo será carregado via PXE para montar o NFS root.
- **Otimizações para Redução de Latência:** O boot será otimizado para minimizar tempo de carregamento, priorizando caching de arquivos críticos e paralelização de inicialização.

### 3. Personalização do Filesystem
Como todo o OrionOS rodará via NFS, o gerenciamento do filesystem deve ser altamente otimizado:
- **OverlayFS para suporte a gravação temporária**
- **Cache agressivo de leitura para reduzir acessos desnecessários ao servidor**
- **Sincronização inteligente entre os clientes para minimizar impacto na rede**
- **Implementação de um sistema de logs centralizado via NFS para análise e depuração global**

## Arquitetura do OrionOS

### 1. Camada do Kernel
O OrionOS terá um kernel customizado **compilado especificamente para execução via NFS**, eliminando código desnecessário e implementando otimizações para comunicação em rede e gerenciamento de arquivos remotos.

- **Implementação do suporte a NFS no Kernel:** O OrionOS não apenas suporta NFS no espaço de usuário, mas integra diretamente chamadas de sistema otimizadas para comunicação eficiente com servidores NFS.
- **Gerenciamento de Memória e Caching:** Algoritmos avançados de cache são utilizados para reduzir a dependência de acessos diretos ao servidor NFS, utilizando técnicas como **lazy loading e speculative fetching** para prever quais blocos serão acessados com base no comportamento dos processos.
- **Módulos para Tolerância a Falhas:** O kernel do OrionOS é capaz de monitorar falhas no NFS e alternar dinamicamente para um nó secundário, sem interrupção perceptível dos processos em execução.

### 2. Servidor Central
O OrionOS possui um **servidor central**, que é responsável por armazenar o sistema operacional e disponibilizá-lo para os clientes via **NFS** e **TFTP/PXE**. Esse servidor pode ser redundante para evitar pontos únicos de falha e pode armazenar múltiplas versões do sistema, permitindo boot condicional baseado em políticas de rede.

### 3. Nós de Processamento (Clientes Diskless)
Os nós clientes do OrionOS não possuem disco rígido e carregam todo o sistema via **NFS**. Eles podem ser **servidores de aplicação, workers de cluster, máquinas de failover ou qualquer outro tipo de nó distribuído**. O kernel do OrionOS é otimizado para esse modelo de operação, minimizando acessos à rede sempre que possível.

### 4. Gerenciamento de Failover e Balanceamento
A arquitetura do OrionOS permite o uso de múltiplas abordagens para garantir **alta disponibilidade e recuperação automática**:
- **Keepalived + VRRP**
- **Orquestração via Kubernetes/Docker Swarm**
- **Replicação e Armazenamento Distribuído**
- **Failover Automático Baseado em Latência de Rede**


