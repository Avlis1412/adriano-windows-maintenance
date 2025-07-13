# adriano-windows-maintenance
---

### **Resumo do Script Batch de Manutenção e Otimização do Windows**

Este script em Batch (.bat) é uma ferramenta automatizada projetada para executar uma série de tarefas essenciais de manutenção e otimização em sistemas operacionais Windows. Ele consolida diversas operações comuns de linha de comando em um único arquivo executável, facilitando a rotina de cuidado com o PC.

**Principais Funcionalidades:**

* **Preparação do Ambiente:** Fecha processos comuns (como navegadores e clientes de torrent) que poderiam interferir nas operações, minimizando interrupções.
* **Redefinição de Rede:** Reseta as configurações de rede (Winsock e IP) e solicita novos endereços IP, útil para resolver problemas de conectividade.
* **Limpeza de Arquivos Temporários:** Remove arquivos desnecessários da pasta temporária do sistema (`%TEMP%`), liberando espaço em disco.
* **Verificação e Reparo do Sistema:**
    * Utiliza **DISM (Deployment Image Servicing and Management)** para verificar a saúde da imagem do Windows (`CheckHealth`, `ScanHealth`) e restaurar sua integridade (`RestoreHealth`), corrigindo possíveis arquivos de sistema corrompidos.
    * Realiza uma **limpeza avançada de componentes do sistema** com DISM (`StartComponentCleanup /ResetBase`), o que pode liberar gigabytes de espaço ao remover versões antigas de componentes do Windows.
    * Executa o **SFC (System File Checker)** para verificar a integridade de todos os arquivos protegidos do sistema e repará-los, caso estejam danificados.
* **Otimização de Armazenamento:** Inicia a **Limpeza de Disco (cleanmgr)**, que pode ser pré-configurada para incluir a remoção de arquivos do sistema (requer uma configuração manual inicial via `cleanmgr /sageset:1`).
* **Verificação de Segurança:** Roda a **Ferramenta de Remocão de Software Mal-Intencionado (MRT)** da Microsoft, oferecendo uma camada básica de verificação contra ameaças.

**Objetivo:**

O script visa simplificar e agilizar a manutenção periódica do Windows, garantindo que o sistema se mantenha rápido, estável e livre de arquivos desnecessários ou corrompidos, com o mínimo de intervenção manual. É uma solução prática para usuários que desejam otimizar o desempenho do seu computador com comandos nativos do sistema.

**Requisitos:**

* **Privilégios de Administrador:** O script deve ser executado como administrador para que todos os comandos sejam processados corretamente.
* **Configuração Manual do Cleanmgr (Uma Vez):** Para a limpeza de disco automática incluir arquivos do sistema, é necessário configurar previamente o `cleanmgr /sageset:1` de forma interativa.

---
