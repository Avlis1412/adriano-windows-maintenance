# adriano-windows-maintenance
---
Sim, **você pode e deve colocar o código completo do script `.bat` logo abaixo do resumo das funcionalidades no seu `README.md`**\!

É uma prática excelente no GitHub. Ao fazer isso, as pessoas que visitarem o seu repositório poderão:

  * Ler o resumo para entender o que o script faz.
  * Ver o código imediatamente no mesmo arquivo para inspecioná-lo, copiá-lo ou entender sua lógica.

Lembre-se de usar os blocos de código Markdown (` ```batch ` antes do código e ` ``` ` depois do código) para que o script seja exibido com formatação correta e destaque no `README.md`.

Ficaria assim:

````markdown
# Script de Manutenção e Otimização do Windows (Batch)

Este script em Batch (.bat) é uma ferramenta automatizada projetada para executar uma série de tarefas essenciais de manutenção e otimização em sistemas operacionais Windows. Ele consolida diversas operações comuns de linha de comando em um único arquivo executável, facilitando a rotina de cuidado com o PC.

## Principais Funcionalidades:
* **Preparação do Ambiente:** Fecha processos comuns (como navegadores e clientes de torrent) que poderiam interferir nas operações, minimizando interrupções.
* **Redefinição de Rede:** Reseta as configurações de rede (Winsock e IP) e solicita novos endereços IP, útil para resolver problemas de conectividade.
* **Limpeza de Arquivos Temporários:** Remove arquivos desnecessários da pasta temporária do sistema (`%TEMP%`), liberando espaço em disco.
* **Verificação e Reparo do Sistema:**
    * Utiliza **DISM (Deployment Image Servicing and Management)** para verificar a saúde da imagem do Windows (`CheckHealth`, `ScanHealth`) e restaurar sua integridade (`RestoreHealth`), corrigindo possíveis arquivos de sistema corrompidos.
    * Realiza uma **limpeza avançada de componentes do sistema** com DISM (`StartComponentCleanup /ResetBase`), o que pode liberar gigabytes de espaço ao remover versões antigas de componentes do Windows.
    * Executa o **SFC (System File Checker)** para verificar a integridade de todos os arquivos protegidos do sistema e repará-los, caso estejam danificados.
* **Otimização de Armazenamento:** Inicia a **Limpeza de Disco (cleanmgr)**, que pode ser pré-configurada para incluir a remoção de arquivos do sistema (requer uma configuração manual inicial via `cleanmgr /sageset:1`).
* **Verificação de Segurança:** Roda a **Ferramenta de Remocão de Software Mal-Intencionado (MRT)** da Microsoft, oferecendo uma camada básica de verificação contra ameaças.

## Objetivo:

O script visa simplificar e agilizar a manutenção periódica do Windows, garantindo que o sistema se mantenha rápido, estável e livre de arquivos desnecessários ou corrompidos, com o mínimo de intervenção manual. É uma solução prática para usuários que desejam otimizar o desempenho do seu computador com comandos nativos do sistema.

## Requisitos:

* **Privilégios de Administrador:** O script deve ser executado como administrador para que todos os comandos sejam processados corretamente.
* **Configuração Manual do Cleanmgr (Uma Vez):** Para a limpeza de disco automática incluir arquivos do sistema, é necessário configurar previamente o `cleanmgr /sageset:1` de forma interativa.

## O Código do Script:

```batch
@echo off
rem Este script executa uma série de tarefas de manutenção e otimização do Windows.
rem Ele foi ajustado para ser executado uma única vez e exige privilegios de Administrador.

color 0B
echo ==========================================================
echo           INICIANDO MANUTENCAO DO SISTEMA WINDOWS
echo ==========================================================
echo.
echo ESTE SCRIPT DEVE SER EXECUTADO COMO ADMINISTRADOR!
echo Feche outros outros programas antes de continuar para evitar interrupcoes.
echo.
pause
echo.

color 0F
echo Fechando processos comuns que podem interferir (opcional - remova se nao usar):
rem Você pode adicionar ou remover programas aqui conforme suas necessidades.
taskkill /im utorrent.exe /f >nul 2>&1
taskkill /im firefox.exe /f >nul 2>&1
taskkill /im chrome.exe /f >nul 2>&1
echo Tentativa de fechamento de programas comuns concluida.
echo.

color 0E
echo ==========================================================
echo           RESET DE CONFIGURACOES DE REDE
echo ==========================================================
echo Redefinindo configuracoes de rede (Winsock e IP)...
netsh winsock reset
netsh int ip reset
echo.
echo Solicitando novo endereco IP...
ipconfig /release
ipconfig /renew
echo Configuracoes de rede redefinidas e renovadas.
echo.

color 0A
echo ==========================================================
echo           LIMPEZA DE ARQUIVOS TEMPORARIOS
echo ==========================================================
echo Apagando arquivos temporarios da pasta TEMP...
del /F /Q %Temp%\* >nul 2>&1
echo Arquivos TEMP apagados.
rem A pasta PREFETCH nao sera limpa, pois nao traz beneficios significativos.
echo.

color 09
echo ==========================================================
echo           VERIFICACAO E REPARO DA IMAGEM DO WINDOWS (DISM)
echo ==========================================================
echo Verificando a saude da imagem do Windows (CheckHealth)...
DISM /Online /Cleanup-Image /CheckHealth
echo.
echo Escaneando a saude da imagem do Windows (ScanHealth) - pode demorar...
DISM /Online /Cleanup-Image /ScanHealth
echo.
echo Restaurando a saude da imagem do Windows (RestoreHealth) - pode demorar e requer internet...
DISM /Online /Cleanup-Image /RestoreHealth
echo Verificacao e reparo da imagem do Windows concluidos.
echo.

color 0C
echo ==========================================================
echo           LIMPEZA AVANCADA DE COMPONENTES DO SISTEMA (DISM)
echo ==========================================================
echo Executando limpeza avancada de componentes do sistema - pode levar muito tempo!
rem Remove todas as versoes anteriores de cada componente que o armazem de componentes
rem do WinSXS nao necessita mais. Isso pode liberar gigabytes de espaco.
DISM.exe /online /Cleanup-Image /StartComponentCleanup /ResetBase
echo Limpeza avancada de componentes do sistema concluida.
echo.

color 06
echo ==========================================================
echo           VERIFICACAO DE INTEGRIDADE DE ARQUIVOS (SFC)
echo ==========================================================
echo Verificando a integridade dos arquivos do sistema com SFC - pode demorar...
sfc /scannow
echo Verificacao de arquivos do sistema concluida.
echo.

color 05
echo ==========================================================
echo           LIMPEZA DE DISCO (CLEANMGR)
echo ==========================================================
echo Iniciando Limpeza de Disco (com configuracoes salvas, incluindo arquivos do sistema)...
rem Para que esta limpeza inclua arquivos do sistema automaticamente,
rem voce DEVE primeiro configurar o "cleanmgr /sageset:1" manualmente.
rem Clique em "Limpar arquivos do sistema" na janela e marque as opcoes desejadas.
cleanmgr /sagerun:1
echo Limpeza de Disco iniciada. (Sera executada com as configuracoes pre-definidas no ID 1)
echo.

color 04
echo ==========================================================
echo           VERIFICACAO DE MALWARE (MRT)
echo ==========================================================
echo Executando a Ferramenta de Remocao de Software Mal-Intencionado (MRT) - pode demorar...
mrt /F:Y
echo Verificacao MRT concluida.
echo.

color 0B
echo ==========================================================
echo           TODAS AS TAREFAS DE MANUTENCAO CONCLUIDAS!
echo ==========================================================
echo O computador pode precisar ser reiniciado para que algumas alteracoes facam efeito.
echo.
pause
exit
````
