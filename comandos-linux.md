### Comandos Gerais

`echo Hello World` Saída no terminal "Hello World"

`whoami` Mostra username que está logado
`who`  Mostra todos os usuários que estão logados

`whatis [cat | ls | pwd ]`  IMPORTANTE, POIS MOSTRA O QUE UM COMANDO FAZ

### Interagindo com o filesystem

`pwd` Mostra qual diretório estamos trabalhando

`ls` Lista arquivos ou diretórios
`ls -l`informações mais detalhadas
`ls -a` Mostra arquivos ocultos
`ls -la` EXECUTA AS DUAS FUNÇÕES ACIMA
`ls -R` Mostra a pasta + o conteúdo na mesma linha
`ls -t` Classifica por hora de modificação, mais recente primeiro

`cd` Altera diretório
`cd ~` Volta para o diretório inicial
`cd -` Volta para o diretório onde estávamos

`touch nome-arquivo` Cria um arquivo .txt vazio

`file banana.jpg` Ele mostra que tipo de arquivo é. Se é Texto, uma Imagem, um diretório, etc. 

`cat` concatenate ( Usado para exibir texto de algum arquivo )
Serve também para combintar leitura de arquivos
![[Pasted image 20250519181513.png]]

`head /var/log/arquivo`  Mostra o ínicio de um arquivo longo
`head -n 2 /var/log/arquivo`  -n 2 é o número de linhas

`tail /var/log/arquivo`  Faz o mesmo que o `head` porém o inverso.
`tail -f /var/log/arquivo`  Utilizando o sinalizador `-f` se o arquivo for sendo atualizado ele mostrará de forma automática.



<span style="color:red">**IMPORTANTE**</span>

`less texto1`  Usado para exibir texto que tenha muitas palavras
*q - Sair*
*g - Move para o início*
*G - Move para o final*
*/Search - Pesquisa no texto em busca de palavras*
*h - Menu ajuda*

`ls | tee texto.txt`  Adiciona a saída do comando `ls`em um arquivo texto.txt

`pwd` mostra qual diretório estamos trabalhando

`find` Encontrar arquivo no sistema. Assim não é necessário utilizar `cd` ou `ls` sempre que precisar de algo que já se sabe.
![[Pasted image 20250519182517.png]]

`wc -l access.log` output  número linhas / número palavras / número bytes 
Conta o número de entradas em um arquivo, no caso do exemplo, um arquivo de log.
l - linha
w - palavras
c - número de bytes

`nl /arquivo`  Enumera as linhas de um arquivo

**IMPORTANTE**
`grep "81.143.211.90" acess.log`
Encontra o IP no arquivo de log acima, ou qualquer entrada em qualquer arquivo
i - não diferencia maíscula de minúscula

`env | grep -i usuário`  Procura por palavras de "usuário" que está em `env` 

`python3 -m zipfile -e <a ser extraido> /destino/...`
EXTRAIR ARQUIVOS
-e = extract
-m = módulo

### Uso de operador

`echo` Linux `>` arquivoCriado --> O arquivoCriado será criando tendo Linux  ( *CUIDADO, POIS SOBRESCREVE* )

`echo` "Linux 2" `>>` arquivoCriado --> Diferente do outro, não sobrescreve e sim somente adiciona ao arquivo


## TRATANDO TEXTO

`env`  Mostra as variáveis de ambiente.
`echo $HOME`  Acessamos a variável de ambiente

## Gerenciamento de Usuário

`sudo useradd bob`  Adicionando usuário
`sudo userdel bob`  Removendo usuário



`sudo useradd -m -G wheel -s /bin/bash bob`: 
`-m`: Cria um diretório home para o usuário.
`-G wheel`: Adiciona ao grupo de superusuário
`-s /bin/bash`: Qual sheell o usuário utilizará



`userdel -r bob`
`-r`: Exclui diretório home do usuário.



`usermod -aG wheel bob` Adicionar bob no grupo wheel
`-a`: append ao grupo existente
`-G wheel` : Adiciona ao grupo wheel



*Quem faz parte do grupo **wheel** e o `/etc/sudoers` estiver configurado para permitir isso, todos que estão no grupo podem executar comandos com `sudo`.*




## PERMISSÕES

`chmod u+x meu-arquivo`  Adiciona permissão de executável para usuário no arquivo "meu-arquivo".

`chmod u-x meu-arquivo`  Retira a permissão de executável para o usuário no arquivo "meu-arquivo".

`chmod ug+w`  Libera permissão de ESCRITA para usuário e grupo

`sudo chown baleias meu-arquivo`   Altera permissão de um arquivo de usuário x para usuário baleias
`sudo chgrp baleias meu-arquivo`   Altera permissão de um arquivo do grupo x para o grupo baleias



## PROCESSOS

Os processos são gerenciados pelo o *kernel* e possuí um **ID(PID)**
**PPID** É O PAI DE UM PROCESSO, OU SEJA, QUEM CHAMOU O PROCESSO **PID**

`ps`: Mostra processos que estão em execução

---

**PID	TTY	STAT	TIME	CMD**

41230	pts/4	Ss	00:00:00	bash

51224	pts/4	R+	00:00:00	ps

---



`ps aux`: Mostra processos mais detalhados

`a`: Mostra processos em execução e ainda de outros usuários

`u`: Mostra detalhes sobre o processo

`x`: Lista todos os processos que não têm TTY

---

- `PID`: ID do processo
- `TTY`: Terminal de controle associado ao processo
- `STAT`: Código de status do processo
- `TIME`: Tempo total de uso da CPU
- `CMD`: Nome do executável/comando

---

`top`: Mostra processos em tempo real. **ÚTIL**



**TTY**: TELA DE LOGIN SEPARADA AO DO AMBIENTE GRÁFICO.
PODEM SER USADOS PARA SEPARAÇÃO DE TAREFAS, RECUPERAÇÃO DE ERROS, USO SOMENTE DA CLI.

**É FUNÇÃO DO KERNEL GARANTIR QUE OS PROCESSOS RECEBAM A QUANTIDADE NECESSÁRIA DE RECURSOS, DEPENDENDO DE SUAS DEMANDAS**



---

### Sinais

Um sinal é uma notificação a um processo de que algo aconteceu.



`SIGHUP` ou `HUP` ou 1: Desligar

`SIGINT` ou `INT` ou 2: Interrupção

`SIGKILL` ou `KILL` ou 9: Matar

`SIGSEGV` ou `SEGV` ou 11: Falha de segmentação

`SIGTERM` ou `TERM` ou 15: Rescisão de Software

`SIGSTOP` ou `STOP` ou 19: Parar 



```bash
kill -l #Ver todos os sinais

kill PID #Mata um processo. Por padrão envia um sinal TERM. Mata com alguma limpeza antes
kill -9 PID #Mata um process. No caso com SIGKILL. Mata sem nenhuma limpeza antes
```

---

### Nice

Significa que o processo tem um número para determinar sua prioridade

Quanto maior o *Nice* = Menor a prioridade

Quanto menor o *Nice* = Maior a prioridade

*Nice* = 0 -- Padrão

---

### Estados do Processo

Na coluna **STAT**, existem diversos valores. Isso significa o estado de um processo

`R`: Em execução ou executável, está apenas esperando a CPU Processá-lo

`S`: Suspensão interrompível, aguardando a conclusão de um evento, como uma entrada do terminal

`D`: Sono ininterrupto, processos que não podem ser eliminados ou interrompidos com um sinal, para fazê-lo desaparecer, faz-se necessário reiniciar ou corrigir o problema.

`Z`: Zumbi

`T`: Parado / Suspenso



---

## IMPORTANTE

Tudo no Linux é um arquivo, até mesmo os processo. As informações de um processos são armazenadas em um sistema de arquivos especial

```bash
$ ls /proc
$ cat /proc/[PID]/status # Aqui poderão ver mais detalhes de um processo
```



---

### Controle de Tarefas

Caso queiramos deixar uma tarefa em segundo plano para que assim possamos utilizar a shell. Podemos controlar isso.

Para colocar no segundo plano, basta usar **&**

EX.:

```bash
$ sleep 1000 & # Coloca no segundo plano
$ sleep 1001 & # Coloca no segundo plano
$ top & # Coloca no segundo plano
$ ps aux & # coloca no segundo plano

$ jobs # Trabalhos que estão no segundo plano
```



`fg`: Retoma no foreground ( Principal ) | `fg %1`: Retoma um Foreground específicio

`Ctrl-Z`: Stopped

`bg`: Retoma em background

`kill %1`: Elimina trabalhos no segundo plano



---

## PACOTES

Pacotes podem ser encontrados no seguinte path. Mas depende da distribuição que está sendo utilizada

```bash
$ /etc/pacman.d/mirrorlist
$ /etc/apt/sources.list

...

# Ou seja, depende muito da distribuição
```



Arquivos compactados `.zip` e `.rar`



**COMPACTANDO/DESCOMPACTANDO ARQUIVOS COM GZIP (.gz)** 

<span style="color:red">***o gzip não adiciona vários arquivos em um único arquivo***</span>

```bash
$ gzip meu_arquivo # Compacta
$ gunzip meu_arquivo # Descompacta
```



**CRIANDO ARQUIVOS COM .tar**

Empacotando

```bash
$ tar cvf meu_tar_arquivo.tar arquivo1 arquivo2 ... arquivoN
```

`c`: Criar

`v`: Diga o programa para ser detalhado para deixar-nos ver o que está fazendo

`f`: Se estiver criado um arquivo.tar, o nome deve ser criado depois dessa opção.



Desempacotando

```bash
$ tar xvf tar_arquivo.tar
```

`x`: Extrair



**.tar**: Empacota

**.gz**: Comprime

Logo posso ter **file.tar.gz**

Neste caso é só Descompactar o **gz** e depois Desempacotar com o **tar**. OU SEJA, DE FORA PARA DENTRO.



---

## DEVICES

Estão guardados no diretório `/dev/`

`/dev/null`: Existe para que não encha o `/dev` de arquivo igual era antigamente na criação de algum dispositivo já que em Linux dispositivos é tratado como arquivo.



```bash
$ ls -l /dev
 brw-rw---- 1 disco raiz 8, 0 20 de dezembro 20:13 sda
 crw-rw-rw- 1 raiz raiz 1, 3 20 de dezembro 20:13 null
 srw-rw-rw- 1 raiz raiz 0 20 de dezembro 20:13 log
 prw-r--r-- 1 raiz raiz 0 20 de dezembro 20:13 dados
```

c - Personagem -- Transferem dados, mas um caractere por vez

b - bloco -- Transferem dados, mas em grandes blocos de tamanho fixo

p - tubo

s - soquete



`ls` lista diretório e arquivos. `lspci`: Lista dispositivos PCI

`lsusb`: Lista dispositivos USB

`lsscsi`: scsi = sd (sda): lista dispositivos scsi



```bash
# Copiando null para o pendrive (/dev/sdb). Acaba "formatando"
dd if=/dev/zero of=/dev/sdb bs=1M status=progress


```





## SSH

```bash
$ scp /caminho/arquivo/arq.txt usuario@ip_usuario:/path/destino

# Enviando arquivo remotamente por meio do SSH
```



## TCPDUMP

Escuta e analisa o que se passa na rede



```bash
$ tcpdump -i <interface_rede> tcp port 80

# Escutando a rede TCP e na porta 80
```





## 🔧 **Comandos básicos DO VI**

### ✅ **1. Entrar no modo de inserção**

- `i` → Insere **antes** do cursor
- `a` → Insere **após** o cursor
- `o` → Abre nova linha **abaixo**

➡️ Depois de digitar, **pressione `ESC` para sair do modo de inserção.**

------

### 💾 **2. Salvar e sair (pressione `ESC` primeiro)**

- `:w` → Salvar
- `:q` → Sair
- `:wq` ou `ZZ` → Salvar e sair
- `:q!` → Sair **sem salvar**

------

### 🗑️ **3. Deletar**

- `dd` → Deletar linha inteira
- `x` → Deletar caractere
- `dw` → Deletar palavra

------

### 📋 **4. Copiar e colar**

- `yy` → Copiar linha
- `p` → Colar depois do cursor
- `P` → Colar antes do cursor

------

### 🔍 **5. Buscar texto**

- `/palavra` → Busca para frente
- `?palavra` → Busca para trás
- `n` → Próxima ocorrência
- `N` → Anterior

------

### 🎯 **6. Navegar**

- `h` → Esquerda
- `l` → Direita
- `j` → Baixo
- `k` → Cima
- `gg` → Início do arquivo
- `G` → Fim do arquivo
- `:n` → Ir para linha `n` (ex: `:42`)
