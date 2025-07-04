🧑‍💻 Filho(a), se você quer aprender Linux de verdade...
…então o Arch Linux é um baita laboratório. Não é o mais fácil, mas é o que mais te ensina.

Você vai entender:

Como o sistema opera por trás dos cliques.

Como montar disco, rede, usuário, permissão.

E como deixar o PC leve e rápido do seu jeito.

Mas você não vai ver tela bonita de instalação. Vai ser linha por linha. E isso é bom — porque vai te dar autonomia e raciocínio lógico.

🧭 Sites que você DEVE visitar
1. Arch Wiki
📚 É a Bíblia do Arch. Tudo está lá. Bem escrito. E atualizado.

Começa por aqui: https://wiki.archlinux.org/title/Installation_guide

Leia em inglês, mesmo que dê trabalho. Aprende mais assim.

2. DistroTube (YouTube)
Um canal ótimo de um cara que vive em terminal.

Procure: “DistroTube Arch Install” ou “Manual Tiling WM Linux”

3. Luke Smith (YouTube)
Ele é radical, mas ensina bem o Arch e ferramentas Unix-like.

4. r/archlinux (Reddit)
Perguntas, soluções e experiências de usuários do mundo todo.

5. Linux Journey
Curso online, interativo, bem explicadinho. Perfeito pra ir de iniciante a intermediário.

⚙️ O que eu te ensinaria antes de instalar
Esses são os blocos fundamentais. Você precisa aprender na ordem:

Como funciona uma partição e o que é EFI
→ pra você saber onde o sistema vai morar e como ele vai dar boot.

Como montar partições e formatar com mkfs e mount
→ aprender que o Linux “monta” coisas em diretórios.

Como configurar rede via terminal (iwctl, ip, dhcpcd)

Como instalar pacotes com pacstrap e gerar fstab

Como usar o arch-chroot pra configurar o sistema

Como criar usuário, configurar sudo, hostname, locale

Como instalar um bootloader (grub, systemd-boot)

💬 Dica de pai: Não decore. Entenda.
Eu te diria: “não copia o comando só porque alguém falou que funciona. Entenda o que ele faz”.
Porque se um comando falhar, só quem entende resolve. E isso te dá poder.

🚧 E o mais importante: você vai errar.
E tudo bem. Faz parte.

Formatar errado, não dar boot, ficar sem internet... tudo isso é parte do aprendizado.
É assim que você vira alguém que entende de verdade.

🧠 Se quiser começar leve...
Faz primeiro no VirtualBox ou no QEMU (sem medo de quebrar).

E depois, se quiser, bota num notebook velho.



----------------------------------------------------------------------

1. Ao iniciar a .ISO do Arch Linux no VirtualBox tive um problema

`kernel driver not installed (rc=-1908)`

`VirtualBox can't operate in VMX root mode. Please disable the KVM kernel extension,recompile your kernel and reboot (VERR_VMX_IN_VMX_ROOT_MODE).  Result Code: NS_ERROR_FAILURE (0x80004005) Component: ConsoleWrap Interface: IConsole {6ac83d89-6ee7-4e33-8ae6-b257b2e81be8}`



<span style="color:red">**PROBLEMA:**</span> 

**KVM** ( *VM do próprio kernel linux* ). Se o **VirtualBox** tentar utilizar um recurso no hardware e o KVM estiver ativado dá erro, pois ambos não pode usar o mesmo recurso ao mesmo tempo



<span style="color:green">**SOLUÇÃO:**</span> 

```bash
sudo rmmod kvm_intel kvm 
```

`rmmod`: Remove módulo do Linux Kernel



Configurando layout do keyboard

```bash
localectl list-keymaps
```

`localectl`: Controla o sistema de locales e keyboard layouts

`list-keymaps`: Lista keyboard mapping disponíveis



Carregando o kayboard do Brasil com abnt2

```bash
loadkeys br-abnt2
```



Configurando Font pois estava muito pequena
localizado em `/usr/share/kbd/consolefonts/`

```bash
setfont ter-c24b
```



Checando qual a arquitetura

```bash
cat /sys/firmware/efi/fw_platform_size
```

`esse path acima só existe pelo fato que o sistema foi iniciado em modo UEFI`

`UEFI: Firmware; Suporta GPT; Usa partição EFI; Pode utilizar Secure Boot`



Checando a hora e data do sistema

```ba
timedatectl
```

`NTP`: *Network Time Protocol* (true ou false) - Mantém o relógio do sistema sempre sincronizado de acordo com a internet

**America/Sao_Paulo**: É a TimeZone do Brasil que atende Brasília, SP, RJ, MG.



### Etapa muito importante que cobre o particionamento de disco

---

**⚠️ CUIDADO AO RODAR `fdisk` e `mkfs`, pois eles apagam dados.**



Identifica os discos e como estão organizados

```bash
lsblk
```

Identifica os discos de forma detalhada

```bash
fdisk -l
```

<span style="color:#0077bb">**São requeridas sempre um diretório root e se o boot for modo UEFI tem que ter um EFI SYSTEM PARTITION (ESP)**</span> 

**UEFI:** Sucessor da Bios.
	Software que inicializa o computador antes do sistema operacional.
	UEFI busca arquivo de boot dento da ESP ( Que deve estar em formato FAT32)

**ESP:** Partição ***obrigatória*** para boot em modo UEFI
	Deve estar formatada em FAT32
	Montada geralmente em `/boot` ou `/boot/efi`
	É onde ficam os **arquivos do carregador de boot**

	- grubx64.efi (caso utilize o GRUB) (Arquivo binário executável)
	- bootx64.efi (carregador genérico) (Arquivo binário executável)
	- loader.efi (caso use systemd-boot) (Arquivo binário executável)



Seleciona o disco para modificação

```bash
fdisk /dev/sda
```



**GPT ou MBR**

**GPT: ** Se o computador utiliza modo UEFI ( MODO UEFI TEM QUE EXISTE ESTA PASTA `/sys/firmware/efi`ou `cat /sys/firmware/efi/fw_platform_size`).
Se queremos utilizar mais de 4 partições primárias
Se queremos tecnologia moderna, com mais segurança e robustez
**Recomendado para sistemas modernos**



**MBR:** Se o sistema só suporta BIOS (modo legado)
Máxima compatibilidade com sistemas antigos (ex: dual boot com windows legado)

**Evite, se possível. Funciona, mas é uma tecnologia antiga(década de 80).**



Selecionando o disco e particionando com **fdisk**. Pode-se fazer a formatação

```bash
mkfs.ext4 /dev/particao
mkswap /dev/particao
mkfs.fat -F 32 /dev/particao
```



Após tudo formatado é necessário montar as partições

```bash
mount /dev/root_particao /mnt
mount --mkdir /dev/efi_particao /mnt/boot
swapon /dev/swap_particao
```



## INSTALAÇÃO

Após ter configurado as partições podemos fazer a instalação de algumas coisas

***Firmware, Kernel, etc.***



**Selecionando Mirrors**

1. Espelhos podem ser acessador em `/etc/pacman.d/mirrorlist`
2. `reflector`: Código em Python que pode buscar os últimos mirrors, fitrando-os quanto a sua última atualização, ordem e velocidade
   - Instala o `reflector`: `pacman -S reflector`
   - veja https://man.archlinux.org/man/reflector.1#EXAMPLES. ( **mostra como deixar os espelhos mais otimizados** )



<span style="color: red">**PROBLEMA:**</span>

Ao tentar instalar com o Pacman tive o problema 

`warning: Public keyring not found: have you run 'pacman-key --init'?`



<span style="color:green">**SOLUÇÃO:**</span>

`pacman-key --init`: Serve para inicializar o sistema de chaves do Pacman.

***As chaves servem para verificar a autenticidade dos pacotes***

`pacman-key --populate archlinux`: Atualiza as chaves públicas oficiais do *archlinux* ao keyring do Pacman

`pacman-key --refresh-key`: Atualiza as chaves existentes no sistema



---

#### Instalação do Kernel e Firmware

***Por que instalar o kernel?***

O Kernel é a ponte entre o Hardware e os Programas, pois ele gerencia tudo.

- Processador
- Memória RAM
- Dispositivos de entrada e saída
- Comunicação entre o Software e o Hardware

Sem ele é impossível fazer a Distro funcionar

**MAIS UTILIZADO:** É o KERNEL `linux` pois é o recomendado e o padrão com atualizações recentes.



***Por que instalar o Firmware?***

Contém microcódigos e instruções específicas para o funcionamento de componentes como

- Placa de vídeo
- Wi-Fi e Bluetooth
- Processador (microcódigo)
- Controladores USB
- Drivers de armazenamento

Sem o Firmware pode haver falhas, incompatibilidade, ou seja, pode haver problemas so sistema operacional.

**MAIS UTILIZADO:** Depende se o seu processador é INTEL ou AMD. Mas o pacote é `linux-firmware`.



```bash
pacstrap -K /mnt base linux linux-firmware [...outras ferramentas...]
```



`pacstrap -K /mnt`: Usado para instalar **pacotes dentro de um ambiente montado**, no caso, `/mnt`. O `-K` serve para não modificar as chaves públicas previamente configuradas.

`base`: Base mínima para funcionar um computador: `filesystem`, `ferramentas de linha de comando`. Ou seja, ferramentas necessárias para gerenciar arquivos, usuários e processos.

`linux`: *Kernel* padrão



### Configurando o Sistema

---

Gerando um arquivo `fstab`

O `fstab` mostra para o sistema como os arquivos serão montados automaticamente na hora da inicialização. 

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

`-U`: Define para UUID



Após esta configuração, podemos inicializar o novo sistema.

```ba
arch-chroot /mnt
```



Par que o sistema saiba qual fuso horário utilizar, iremos mostrar para ele da seguinte forma

```bas
ln -sf /usr/share/zoneinfo/Região/Cidade /etc/localtime
```

`ln`: Faz links entre arquivos



Configuração da hora

```bash
timedatectl set-timezone America/Sao_Paulo #Sincronização de timezone

hwclock --systohc #Sincroniza para o horário de hardware
hwclock --show #Mostra se foi sincronizado ou não
```



### Localização

Para configurar a locale

1. Edite o arquivo `/etc/locale.gen` descomentando a locale desejada `en_US.UTF-8 UTF-8` ou `pt_BR.UTF-8 UTF-8`.

2. Depois `locale-gen` para gerar as locales

3. Depois de gerar os arquivos binários com `locale-gen`vamos definir qual locale o sistema vai usar por padrão

4. ```bash
   echo LANG=en_US.UTF-8 >> /etc/locale.conf
   echo LANG=pt_BR.UTF-8 >> /etc/locale.conf 
   ```

5. Para configurar o layout do teclado podemos configurar como mostrado abaixo

6. ```bas
   echo KEYMAP=br-abnt2 >> /etc/vconsole.conf
   ```

7. Criar um hostname

8. ```bash
   echo hostname >> /etc/hostname 
   ```

9. ```bash
   mkinitcpio -P 
   
   #Gera imagem de inicialização do kernel
   
   # Quando o kernel precisa de um sistema de arquivos temporários(em RAM)
   # Para monstar arquivos; Carregar módulos; fazer um boot real do sistema



### Configurar e instalar um bootloader

**Vamos configurar o gruub**

https://wiki.archlinux.org/title/GRUB



Primeiro vamos instalar

```bash
pacman -S grub efibootmgr
```

Depois vamos instalar o grub como boot loader

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

`--efi-directory=/boot`: Tem que colocar onde o EFI está, no boot é onde é criado o efi.



<span style="color:red">**PROBLEMA:**</span>

Descrição: Instalei o GRUB mas não analisei se o GRUB estava configurado de forma correta onde ele identifica quem é o root e quem é o boot.

Ao inicializar o grub não deixou o sistema iniciar e o grub iniciou com a CLI própria.



<span style="color:green">**SOLUÇÃO:**</span>

```bash
ls

output: (proc) (hd0) (hd0,gp3)(hd0,gp2)(hd0,gp1)(cd0)
A saída pode variar dependendo das partições e quantos HDD ou SSD presente

set root=(hd0,gp1) #local onde encontra-se o /boot
linux /vmlinuz-linux root=/dev/sda2 #local onde está a partição raiz para iniciar o kernel
initrd /initramfs-linux.img
boot
```



<span style="color:brown">**MOTIVO DO ERRO:**</span>

Foi instalado o GRUB mas não foi gerado um arquivo de configuração do GRUB fazendo com que o GRUB não identifique os arquivos necessários para ler o /boot e /root



<span style="color:green">**SOLUÇÃO:**</span>

```bash
grub-mkconfig -o /boot/grub/grub.cfg #Gerar o arquivo de configuração do GRUB
```

