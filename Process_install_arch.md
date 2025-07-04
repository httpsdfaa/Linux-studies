O grande objetivo desse tutorial é fazer a instalação do Arch Linux em modo CLI com Dual boot, compartilhando o mesmo HDD com o Windows.

**Lembrando que estou mostrando com eu instalei o Arch Linux na minha máquina**

**Configuração do Setup**

- Processador i5
- Memória RAM com 16GB
- Placa de vídeo VRAM 4GB
- Informações da Bios
       Vendor: Insyde Corp. 
       Version: V2.08 
       Release Date: 09/23/2022 
       Address: 0xE0000 
       Runtime Size: 128 kB 
       ROM Size: 0 MB 
       Characteristics: 
           PCI is supported 
           BIOS is upgradeable 
           BIOS shadowing is allowed 
           Boot from CD is supported 
           Selectable boot is supported 
           EDD is supported 
           Japanese floppy for NEC 9800 1.2 MB is supported (int 13h) 
           Japanese floppy for Toshiba 1.2 MB is supported (int 13h) 
           5.25"/360 kB floppy services are supported (int 13h) 
           5.25"/1.2 MB floppy services are supported (int 13h) 
           3.5"/720 kB floppy services are supported (int 13h) 
           3.5"/2.88 MB floppy services are supported (int 13h) 
           8042 keyboard services are supported (int 9h) 
           CGA/mono video services are supported (int 10h) 
           ACPI is supported 
           USB legacy is supported 
           BIOS boot specification is supported 
           Targeted content distribution is supported 
           UEFI is supported 
       BIOS Revision: 2.8 
       Firmware Revision: 2.5

### Processo de instalação do Arch Linux no hardware do computador

Antes de iniciarmos, é importante destacar que este tutorial foi baseado no [Guia de Instalação oficial do Arch Linux](https://wiki.archlinux.org/title/Installation_guide).

> Caso algum passo não esteja completamente detalhado aqui, recomenda-se consultar o guia oficial para informações mais aprofundadas ou atualizadas.

### 💾 Criando o Pen Drive Bootável no Windows (x64)

1. **Baixe a imagem .ISO do Arch Linux**
    Acesse o site oficial: https://archlinux.org/download
    Recomenda-se usar a opção **BitTorrent** para garantir velocidade e integridade do download.

2. **Use o Rufus para criar o pendrive bootável**
    Baixe o Rufus no site oficial: https://rufus.ie/en

   **Configurações recomendadas no Rufus:**

   - **Esquema de partição**: `MBR` (caso seu sistema suporte UEFI com CSM).
      Em alguns setups mais modernos, será necessário usar `GPT`.
   - **Sistema de destino** (*Target System*): `BIOS or UEFI`
   - Selecione a imagem `.iso` baixada e clique em **Start**

   > 💡 **Nota:** Verifique as configurações da sua BIOS antes de iniciar a instalação.

3. **Inicialize o computador via pendrive**
    Após criar o pendrive bootável:

   - Insira o pendrive no computador
   - Acesse a BIOS/UEFI pressionando a tecla adequada (geralmente `F2`, `DEL`, `F10` ou `F12`).
   - Coloque o pendrive como prioridade na ordem de boot.

   > ⚠️ *As teclas de acesso à BIOS podem variar de acordo com a placa-mãe. No meu caso, uso `F2` ou `F12`.*
   >
   > Caso o modo seja UEFI, lembre-se de desativar o Secure Boot.



### 🛠️ Pré-instalação

1. **Configuração do layout do teclado**

   ```bash
   localectl list-keymaps
   ```

   > `localectl`: Controla os locais (locales) e layout do teclado do sistema
   >
   > `list-keymaps`: Lista todos os layouts de teclado disponíveis

   ```bash
   loadkeys br-abnt2
   ```

   > `loadkeys`: Carrega o layout de teclado escolhida para o console
   >
   > `br-abnt2`: Layout brasileiro ABNT2 (padrão com Ç)

2. **Configuração de uma fonte específica**

   ```bash
   setfont ter-c24b
   ```

   > `setfont`: Define a fonte do console para melhor legibilidade
   >
   > As fontes disponíveis ficam em: `/usr/share/kbd/consolefonts/`

3. **Verificando qual arquitetura do computador (32 ou 64 bits)** 

   ```bash
   cat /sys/firmware/efi/fw_platform_size
   ```

   >Se retornar 64, o sistema foi iniciado em UEFI de 64 bits
   >
   >Se retornar 32, o sistema foi iniciado em UEFI de 32 bits
   >
   >Se não retornar nenhum valor ou o arquivo não existir, o sistema provavelmente foi iniciado em modo Legacy (BIOS) ou  UEFI com CSM

4. **Conectando à Internet**

   Para configurar uma conexão de rede no ambiente live, siga as etapas abaixo:

   - Certifique-se que sua [interface de rede](https://wiki.archlinux.org/title/Interface_de_rede) esteja listada e ativada, por exemplo, com [ip-link(8)](https://man.archlinux.org/man/ip-link.8):
     ```bash
     ip link
     ```

   - Para rede sem fio e WWAN, certifique-se que a placa de rede não esteja bloqueada com [rfkill](https://wiki.archlinux.org/title/Rfkill_(Português)).

   - Conecte-se à rede:

     - Ethernet — conecte o cabo.
     - Wi-Fi — autentique-se à rede sem fio usando [iwctl](https://wiki.archlinux.org/title/Iwctl_(Português)).
     - Modem de internet móvel — conecte-se à rede móvel com o utilitário [mmcli](https://wiki.archlinux.org/title/Mmcli).

   - A conexão pode ser verificada com [ping](https://wiki.archlinux.org/title/Ping_(Português)):
     ```bash
     ping archlinux.org
     ```

