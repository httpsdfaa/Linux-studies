## Anotações Relacionado ao Linux

1. Leia antes de atualizar
   Sempre consultar o https://archlinux.org/news/

   Neste site anuncia mudanças importantes

   

2. Faça backup de arquivos de configuração importantes

   ```bash
   /etc/pacman.conf
   /etc/mkinicpio.conf
   /etc/fstab
   /etc/locale.gen
   /etc/hostname
   
   # Caso altera algo faça o backup
   
   sudo cp /etc/pacman.conf ~/backups/pacman.conf.bkp
   ```



3. **EVITE USAR O SISTEMA COMO ROOT**. Utilizar somente em necessidade
4. **Atualize regularmente, mas com atenção. Não atualizar meses depois ( pode ser que quebre )**



# Hierarquia de Pastas em Linux

https://www.youtube.com/watch?v=90UseHX4-ns



`/ - root`: Diretório raiz do sistema

`/bin - Binaries`: Diretório que contém programas essenciais do sistema. Usado por **usuários comuns** e pelo **sistema** durante a inicialização.



`/sbin - System Binaries`: Ferramentas administrativas. Administrador root



`/usr/bin - Binaries do usuário`: *Linkado* de `/bin`. Comandos não essenciais ou adicionais. A maioria dos programas vem para cá.

`/boot`: Arquivos necessários para a inicialização do sistema, ou seja, o boot onde encontra-se o bootloader ( grub, etc...)

`/dev - devices`: Diretório onde contém arquivos que representam dispositivos físico e virtuais do computador. **Tudo em Unix/Linux é tratado como arquivos**

`/etc - et cetera`: Armazena todos os **arquivos de configurações** do sistema. É como dizer ao Linux como ele deve funcionar. Usuários, redes, permissões, inicialização, serviços, etc.

`/home`: Contém diretórios de todos os usuários do sistema.

`/lib - Library ( /lib32 ou /lib64 )`: Bibliotecas básicas + módulos do kernel, indispensáveis no boot. Os `/lib32 ou /lib64` depende da arquitetura que o computador tá usando.

`/mnt` - Mount: Local onde podemos **montar partição manualmente** ou dispositivos que não tem uma configuração de montagem automática.

`/opt - Optional`: **Programas Opcionais** que não fazem parte do sistema base ( ou seja, que não usam PACMAN )



`/proc - Process`: Sistema virtual que expõe informações do kernel, processos e hardware. Monitora e pode configurar o sistema em tempo real, sem reiniciar. É virtual e não ocupa espaço em disco.



`/sys - System`: Sistema de arquivos virtuais. Criado para expor infomações e interfaces do kernel para o espaço do usuário.



`/run`: Usado para armazenar informações de estado em tempo de execução. Diretório volátil, criado em tempo de boot. Sumirá após reiniciar. Substituiu diretórios antigos como `/var/run`, `/var/lock`, etc.



`/srv - Service`: Usado para armazenar dados específicos de serviços oferecidos pelo sistema. Dados de servidores web, FTP, GIT, etc.



`/tmp - Temporary`: Diretório para arquivos temporários. Usado por apps para armazenar dados que só precisam por período curto.



`/usr - User System Resources`: Contém a maior parte dos programas, bibliotecas, documentações e arquivos de suporte do sistema.



`/var - Variable`: Variáveis. É onde ficam arquivos que mudam com frequência durante o funcionamento do sistema.



---

# FERRAMENTAS QUE ACHO ÚTIL

Utilizando **ZSH** por ser mais poderoso e customizável

```bash
$ sudo pacman -S zsh

$ chsh -s $(which zsh) # muda para a shell zsh

# só reiniciar que as alterações são feitas
```

`/etc/bin/zsh`: Path
