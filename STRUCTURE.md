# Estrutura de Diretórios do OrionOS

Este documento descreve a organização do repositório do **OrionOS** e a função de cada diretório e arquivo principal. Essa estrutura foi projetada para garantir modularidade, escalabilidade e facilidade de manutenção do sistema.

## 📂 Diretórios Principais

```
/orionos
│── 📁 docs         -> Documentação geral do OrionOS
│── 📁 init         -> Scripts de inicialização (runit/OpenRC/systemd)
│── 📁 kernel       -> Código-fonte do kernel e patches
│── 📁 nfs_root     -> Estrutura base do sistema de arquivos para NFS
│── 📁 scripts      -> Ferramentas auxiliares para automação e configuração
│── 📁 toolchain    -> Ambiente de compilação do LFS (GCC, binutils, glibc, etc.)
│── 📁 packages     -> Scripts de compilação e instalação de pacotes essenciais
│── 📁 config       -> Arquivos de configuração globais (fstab, resolv.conf, network)
│── 📁 logs         -> Armazena logs do kernel, NFS e inicialização do OrionOS
│── 📁 tests        -> Scripts e ferramentas para testar estabilidade e performance
│── 📄 LICENSE      -> Arquivo de licença do OrionOS
│── 📄 README.md    -> Descrição geral do projeto
│── 📄 SECURITY.md  -> Política de segurança e suporte a vulnerabilidades
│── 📄 STRUCTURE.md -> Explicação da estrutura do projeto e função de cada diretório
```

## 📂 **Explicação dos Diretórios**

### `docs/`
Contém toda a documentação relacionada ao OrionOS, incluindo guias de instalação, configuração e desenvolvimento.

### `init/`
Scripts responsáveis pelo **processo de inicialização do OrionOS**, seja com **runit, OpenRC ou systemd**. Inclui configurações de serviços essenciais e scripts de boot.

### `kernel/`
Código-fonte do **kernel do OrionOS**, incluindo patches personalizados para otimizar o desempenho em um ambiente NFS diskless. Contém:
- **Configurações personalizadas do kernel**
- **Patches específicos para compatibilidade com o OrionOS**
- **Drivers e otimizações para PXE/NFS**

### `nfs_root/`
Contém a **estrutura base do sistema de arquivos** que será montada via NFS nos clientes. Inclui:
- `/bin`, `/sbin`, `/etc`, `/usr`, `/var` e outras pastas fundamentais para o funcionamento do sistema.

### `scripts/`
Scripts utilitários para **automatizar tarefas**, como:
- Instalação e configuração do OrionOS
- Gerenciamento de pacotes e atualizações
- Automação de inicialização e provisionamento

### `toolchain/`
Ambiente de compilação **LFS (Linux From Scratch)**, contendo:
- **GCC, binutils, glibc, musl libc** e outras ferramentas essenciais para compilar o sistema do zero.
- **Compiladores e utilitários necessários para a construção do kernel e userland.**

### `packages/`
Scripts para **compilação e instalação de pacotes essenciais**, incluindo:
- `coreutils, busybox, bash, util-linux, openssh, network utilities` e outros.

### `config/`
Armazena **arquivos de configuração críticos do sistema**, incluindo:
- `fstab` (configuração de montagem do sistema de arquivos)
- `resolv.conf` (configuração de DNS)
- `hosts` (resolução de nomes local)
- `network/interfaces` (configuração de rede estática ou dinâmica)
- `sysctl.conf` (ajustes de parâmetros do kernel)

### `logs/`
Diretório para armazenar logs gerados pelo sistema, como:
- Logs de inicialização
- Logs de rede/NFS
- Logs do kernel
- Registros de erro do OrionOS

### `tests/`
Scripts e ferramentas de **validação e testes de estabilidade do OrionOS**. Inclui:
- **Testes de compatibilidade do sistema**
- **Validações de segurança**
- **Testes de performance e benchmarks**

## 📄 **Arquivos na Raiz do Repositório**

### `LICENSE`
Define a licença do OrionOS. Atualmente, o sistema é distribuído sob **Licença MIT**.

### `README.md`
Descrição geral do OrionOS, instruções básicas de instalação e diretrizes para contribuidores.

### `SECURITY.md`
Política de segurança, suporte a vulnerabilidades e ciclo de suporte.

### `STRUCTURE.md`
**Este documento**, que explica a estrutura de diretórios e a função de cada componente.