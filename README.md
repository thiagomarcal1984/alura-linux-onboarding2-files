# Filtrando o conteúdo
Copiando o arquivo `/etc/services` para testar filtragem de conteúdo de arquivos:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ cp /etc/services .
thiago@thiago-pc:~/labs/filtrando_conteudo$ ls
services
```
O comando `grep` pesquisa padrões dentro de arquivos. A sintaxe básica:
```
grep string_pesquisada arquivo_pesquisado
```
Pesquisando a string `http` no arquivo `services`
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep http services
# Updated from https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml .
http            80/tcp          www             # WorldWideWeb HTTP
https           443/tcp                         # http protocol over TLS/SSL
https           443/udp                         # HTTP/3
http-alt        8080/tcp        webcache        # WWW caching service
```

Para ignorar a caixa em que o termo pesquisado está escrito, use o parâmetro `-i (--ignore-case, ignorar caixa alta ou baixa)`. Note que a última linha do comando abaixo não estava presente no comando sem o parâmetro `-i`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep -i http services
# Updated from https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml .
http            80/tcp          www             # WorldWideWeb HTTP
https           443/tcp                         # http protocol over TLS/SSL
https           443/udp                         # HTTP/3
http-alt        8080/tcp        webcache        # WWW caching service
hkp             11371/tcp                       # OpenPGP HTTP Keyserver
```

Copiando o arquivo `/etc/passwd` para o diretório atual e pesquisando no conteúdo de todos os arquivos o texto `thiago`. Note que antes de cada linha há o nome do arquivo pesquisado (no caso, `passwd`):
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ cp /etc/passwd .
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep thiago *
passwd:thiago:x:1000:1000:Thiago:/home/thiago:/bin/bash
thiago@thiago-pc:~/labs/filtrando_conteudo$
```

Pesquisando pelo número `1000` em todos os arquivos do diretório (note que cada linha é prefixada com o nome do arquivo que retornou o item pesquisado):
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep 1000 *
passwd:thiago:x:1000:1000:Thiago:/home/thiago:/bin/bash
services:webmin         10000/tcp
thiago@thiago-pc:~/labs/filtrando_conteudo$ ls
passwd  services
```

O parâmetro `-l` (minúsculo) no `grep` retorna apenas o nome do arquivo que `contém` o valor procurado. Note que o texto `thiago` está presente no arquivo `passwd`, mas não no arquivo `services`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep -l thiago *
passwd
```

O parâmetro `-L` (maiúsculo) no `grep` retorna apenas o nome do arquivo que `NÃO contém` o valor procurado. Note que o texto `thiago` está presente no arquivo `passwd`, mas não no arquivo `services`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep -L thiago *
services
```

# Utilizando a recursividade no Grep
Copiando o arquivo `services` para a pasta `teste` com o nome `services2`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ mkdir teste
thiago@thiago-pc:~/labs/filtrando_conteudo$ cp -p services teste/services2
thiago@thiago-pc:~/labs/filtrando_conteudo$ ls teste
services2
```
Pesquisando por `HTTP` no arquivo `services`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep HTTP services
http            80/tcp          www             # WorldWideWeb HTTP
https           443/udp                         # HTTP/3
hkp             11371/tcp                       # OpenPGP HTTP Keyserver
```

Pesquisando por `HTTP` em todos os arquivos (note que o conteúdo da subpasta `teste` não é pesquisado):
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep HTTP *
services:http           80/tcp          www             # WorldWideWeb HTTP
services:https          443/udp                         # HTTP/3
services:hkp            11371/tcp                       # OpenPGP HTTP Keyserver
grep: teste: Is a directory
```

Pesquisando por `HTTP` `recursivamente` em todos os arquivos (note que o nome do arquivo que contém o resultado da pesquisa aparece no início de cada linha do resultado):
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep -r HTTP *
services:http           80/tcp          www             # WorldWideWeb HTTP
services:https          443/udp                         # HTTP/3
services:hkp            11371/tcp                       # OpenPGP HTTP Keyserver
teste/services2:http            80/tcp          www             # WorldWideWeb HTTP
teste/services2:https           443/udp                         # HTTP/3
teste/services2:hkp             11371/tcp                       # OpenPGP HTTP Keyserver
```

Exibindo os nomes dos arquivos (`-l`) que contenham o texto `HTTP` e usando a recursividade (`-r`):
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ grep -rl HTTP *
services
teste/services2
```

# Formatando a saída da tela
## O comando `cat`
O comando `cat` exibe o conteúdo resultante da concatenação dos arquivos informados:
``` 
thiago@thiago-pc:~/labs/filtrando_conteudo$ echo Conteudo1 > arq1
thiago@thiago-pc:~/labs/filtrando_conteudo$ echo Conteudo2 > arq2
thiago@thiago-pc:~/labs/filtrando_conteudo$ echo Conteudo3 > arq3
thiago@thiago-pc:~/labs/filtrando_conteudo$ cat arq1 arq2 arq3
Conteudo1
Conteudo2
Conteudo3
``` 

## O comando `more`
O comando `more` permite exibir de forma paginada o conteúdo de um arquivo grande.

Dentro do `more` podemos rolar a tela para baixo com as teclas `espaço` ou `Page Down`, ou rolar para cima com a tecla `n` ou `Page Up`.

Para sair do comando, tecle `q` ou vá até o fim do arquivo visualizado.

## O comando `less`
O comando `less` faz a mesma coisa que o `more`. A diferença é que as setas para baixo e para cima no `more` navega por uma página inteira, enquanto no `less` navega linha a linha. 

O `less` costuma ser melhor que o `more` para ler conteúdo.

## Os comandos `head` e `tail`
Esses comandos exibem o início e o fim do arquivo, respectivamente. Por padrão, cada comando exibe 10 linhas do arquivo.

Informando o parâmetro `-n` podemos mudar o número de linhas exibidas pelos comandos `head` e `tail`:
```
thiago@thiago-pc:~/labs/filtrando_conteudo$ head -n 3 passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin

thiago@thiago-pc:~/labs/filtrando_conteudo$ tail -n 3 passwd
usbmux:x:113:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
thiago:x:1000:1000:Thiago:/home/thiago:/bin/bash
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
```

# Procurando arquivos no sistema
Para procurar arquivos pelo nome, use o comando `find <path_de_origem> -name <nome_procurado>`:
```
thiago@thiago-pc:/$ find / -name *.conf
```
Às vezes é necessário usar o `sudo` para buscar o conteúdo de certos diretórios que tem acesso restrito:
```
thiago@thiago-pc:/$ sudo find / -name *.conf
```

O parâmetro `-maxdepth <n>` limita o nível de profundidade para buscar arquivos recursivamente com o `find`. Quanto maior o `maxdepth`, maior o número de resultados porque entra mais profundamente nos subdiretórios:
```
thiago@thiago-pc:/$ find /etc -maxdepth 1 -name *.conf
/etc/ca-certificates.conf
/etc/ld.so.conf
/etc/sudo_logsrvd.conf
/etc/usb_modeswitch.conf
/etc/host.conf
/etc/nsswitch.conf
/etc/adduser.conf
/etc/fuse.conf
/etc/sudo.conf
/etc/debconf.conf
/etc/nftables.conf
/etc/logrotate.conf
/etc/overlayroot.conf
/etc/multipath.conf
/etc/gai.conf
/etc/hdparm.conf
/etc/e2scrub.conf
/etc/deluser.conf
/etc/resolv.conf
/etc/pam.conf
/etc/libaudit.conf
/etc/mke2fs.conf
/etc/xattr.conf
/etc/sysctl.conf
/etc/rsyslog.conf
/etc/ucf.conf
```
# Mais recursos do find - amin, atime, iname
Criação do arquivo `novo-teste` e exibição da data de modificação dos arquivos:
```
thiago@thiago-pc:~/labs/glob$ touch novo-teste
thiago@thiago-pc:~/labs/glob$ ll
total 8
drwxrwxr-x 2 thiago thiago 4096 abr 21 15:53 ./
drwxrwxr-x 6 thiago thiago 4096 abr 21 14:28 ../
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:09 arq
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:06 arq1
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:46 Arq1
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:06 arq10
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:06 arq100
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:06 arq2
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:46 Arq2
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:06 arq3
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:38 arq5
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:16 arq78
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:38 arq9
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:16 arq90
-rw-rw-r-- 1 thiago thiago    0 abr 21 15:53 novo-teste
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:07 tmp1
-rw-rw-r-- 1 thiago thiago    0 abr 21 12:07 tmp2
```

## O parâmetro `-d` do comando `touch`
O comando `touch -d "formato_de_data"` modifica a data de modificação do arquivo:
```
thiago@thiago-pc:~/labs$ mkdir thiago
thiago@thiago-pc:~/labs$ cd thiago/

thiago@thiago-pc:~/labs/thiago$ touch -d "2023-01-15 08:15" arq-2023-01-15
thiago@thiago-pc:~/labs/thiago$ touch -d "2023-02-15 16:30" arq-2023-02-15
thiago@thiago-pc:~/labs/thiago$ touch -d "2023-03-15 19:45" arq-2023-03-15

thiago@thiago-pc:~/labs/thiago$ ls -la
total 8
drwxrwxr-x 2 thiago thiago 4096 abr 21 17:09 .
drwxrwxr-x 7 thiago thiago 4096 abr 21 17:07 ..
-rw-rw-r-- 1 thiago thiago    0 jan 15 08:15 arq-2023-01-15
-rw-rw-r-- 1 thiago thiago    0 fev 15 16:30 arq-2023-02-15
-rw-rw-r-- 1 thiago thiago    0 mar 15 19:45 arq-2023-03-15
```

## O parâmetro `-amin` (access minute) do comando `find`
O parâmetro `-amin` filtra os arquivos cuja tempo de modificação esteja entre agora e o número de *minutos* informado.

> Nota: o parâmetro deve ser ***negativo***. Se for positivo, significa que a data de modificação do arquivo é futura.

Listando o conteúdo da pasta para saber a data de modificação dos arquivos:
```
thiago@thiago-pc:~/labs/thiago$ ls -la
total 8
drwxrwxr-x 2 thiago thiago 4096 abr 21 18:51 .
drwxrwxr-x 7 thiago thiago 4096 abr 21 17:07 ..
-rw-rw-r-- 1 thiago thiago    0 abr 21 14:00 14h
-rw-rw-r-- 1 thiago thiago    0 abr 21 18:00 18h
```

Obtendo a hora atual para comparar os horários:
```
thiago@thiago-pc:~/labs/thiago$ date
sex 21 abr 2023 18:51:23 UTC
```

Obtendos os arquivo acessados a 2 horas atrás ou menos (120 minutos):
```
thiago@thiago-pc:~/labs/thiago$ find . -amin -120
.
./18h
```

Obtendos os arquivo acessados a 5 horas atrás ou menos (600 minutos):
```
thiago@thiago-pc:~/labs/thiago$ find . -amin -600
.
./14h
./18h
thiago@thiago-pc:~/labs/thiago$
```

## Mostrando a data e a hora
Use o comando `date`, sem parâmetros:
```
thiago@thiago-pc:~/labs/thiago$ date
sex 21 abr 2023 17:44:47 UTC
```

## O parâmetro `-atime` (access time) do comando `find`
O parâmetro `-atime` filtra os arquivos cuja data de modificação esteja entre agora e o número de *dias* informado.
> Nota: o parâmetro deve ser ***negativo***. Se for positivo, significa que a data de modificação do arquivo é futura.

Listando o conteúdo da pasta `/labs/thiago`:
```
thiago@thiago-pc:~/labs/thiago$ ls -la
total 8
drwxrwxr-x 2 thiago thiago 4096 abr 21 17:09 .
drwxrwxr-x 7 thiago thiago 4096 abr 21 17:07 ..
-rw-rw-r-- 1 thiago thiago    0 jan 15 08:15 arq-2023-01-15
-rw-rw-r-- 1 thiago thiago    0 fev 15 16:30 arq-2023-02-15
-rw-rw-r-- 1 thiago thiago    0 mar 15 19:45 arq-2023-03-15
```

Pesquisando os arquivos acessado há no máximo 30 dias (apenas o diretório corrente aparece):
```
thiago@thiago-pc:~/labs/thiago$ find . -atime -30
.
```

Modificando para 60 dias (a data de hoje é 21/04/2023):
```
thiago@thiago-pc:~/labs/thiago$ find . -atime -60
.
./arq-2023-03-15
```

E agora para 90 dias (a data de hoje é 21/04/2023):
```
thiago@thiago-pc:~/labs/thiago$ find . -atime -90
.
./arq-2023-02-15
./arq-2023-03-15
```

## O parâmetro `-size` do comando `find`
O parâmetro `-size` filtra todos os arquivos cujo tamanho correspondam ao critério fornecido como parâmetro.

Buscando arquivos com mais de 300MB (note o valor `+300M`, representa qualquer arquivo cujo tamanho seja maior que 300MB):

> É importante prefixar o valor com mais ou menos (`+` ou `-`, senão a pesquisa vai retornar arquivos com o tamanho exato) e sufixar com a medida (k minúsculo para kibibytes, M para mebibytes, G para gibibytes etc.).

```
thiago@thiago-pc:~/labs$ sudo find / -size +300M
/swap.img
/proc/kcore
find: ‘/proc/3029/task/3029/fd/6’: No such file or directory
find: ‘/proc/3029/task/3029/fdinfo/6’: No such file or directory
find: ‘/proc/3029/fd/5’: No such file or directory
find: ‘/proc/3029/fdinfo/5’: No such file or directory

thiago@thiago-pc:~/labs$ ls -la /swap.img
-rw------- 1 root root 2147483648 abr 20 16:20 /swap.img
```

O arquivo `/proc/kcore` tem mais de 2GB (o número 140737471590400 no resultado é o número de bytes):
```
thiago@thiago-pc:~/labs$ sudo find / -size +2G
/proc/kcore
find: ‘/proc/3046/task/3046/fd/6’: No such file or directory
find: ‘/proc/3046/task/3046/fdinfo/6’: No such file or directory
find: ‘/proc/3046/fd/5’: No such file or directory
find: ‘/proc/3046/fdinfo/5’: No such file or directory
thiago@thiago-pc:~/labs$ ls -la /proc/kcore
-r-------- 1 root root 140737471590400 abr 20 18:31 /proc/kcore
```

## Listar tamanho de arquivos em formato legível para humanos
Você pode exibir o tamanho dos arquivo em formato ***legível para humanos*** com o parâmetro `-h` (o `/proc/kcore` tem 128TB!):
```
thiago@thiago-pc:~/labs$ ls -lh /proc/kcore
-r-------- 1 root root 128T abr 20 18:31 /proc/kcore
```
O arquivo `/swap.img` tem 2GB:
```
thiago@thiago-pc:~/labs$ ls -lh /swap.img
-rw------- 1 root root 2,0G abr 20 16:20 /swap.img
```

## Ignorando a caixa/caso no comando find com `-iname`
O parâmetro `-iname` no `find` faz com que a busca seja case insensitive:
```
thiago@thiago-pc:~/labs$ mkdir thiago
thiago@thiago-pc:~/labs$ mkdir thiago/Ab
thiago@thiago-pc:~/labs$ mkdir thiago/ab

thiago@thiago-pc:~/labs$ find thiago/ -iname "aB*"
thiago/Ab
thiago/ab

thiago@thiago-pc:~/labs$ find thiago/ -iname a*
thiago@thiago-pc:~/labs$
```
> Repare nas aspas no parâmetro `-iname`: sem elas, você não pode usar as curingas.

# Prefixos dos parâmetros `min` e `time`
| Prefixo| min (minutos) | time (dias) |
| ---| ---| ---|
| `a` = access, se refere à ultima leitura, visualização (ex. more, less) | `amin` | `atime` |
| `m` = modify, se refere à ultima modificação (ex. vi) | `mmin` | `mtime` |
| `m` = change, se refere à ultima modificação dos meta dados (ex. chmod, chown) | `cmin` | `ctime` |

Lembrando que o valor que aparece em seguida ao parâmetro deve ser um inteiro com ou sem o prefixo +/-:
1. `+n` para maior ou igual a n;
2. `-n` para menor ou igual a n;
3. `n` para exatamente igual a n;

# Redirecionando a saída padrão para um arquivo
Exibindo todas as linhas contendo `ssh` no arquivo services (saída padrão para o console):
```
thiago@thiago-pc:~/labs$ cd redirecionamento/
thiago@thiago-pc:~/labs/redirecionamento$ cp /etc/services .
thiago@thiago-pc:~/labs/redirecionamento$ grep ssh services
ssh             22/tcp                          # SSH Remote Login Protocol
```

Redirecionando a saída do programa `grep` para o arquivo `listagem.txt` com o operador `>`:
```
thiago@thiago-pc:~/labs/redirecionamento$ grep ssh services > listagem.txt
thiago@thiago-pc:~/labs/redirecionamento$ tail listagem.txt
ssh             22/tcp                          # SSH Remote Login Protocol
```

Se você repetir o comando, o arquivo listagem será sobrescrito (o operador `>` limpa o arquivo antes de gravar nele):
```
thiago@thiago-pc:~/labs/redirecionamento$ grep 3389 services > listagem.txt
thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
ms-wbt-server   3389/tcp
```
Para evitar a sobrescrita, use o operador `>>` ao invés de `>`:
```
thiago@thiago-pc:~/labs/redirecionamento$ grep ssh services > listagem.txt
thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
ssh             22/tcp                          # SSH Remote Login Protocol
thiago@thiago-pc:~/labs/redirecionamento$ grep 3389 services >> listagem.txt
thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
ssh             22/tcp                          # SSH Remote Login Protocol
ms-wbt-server   3389/tcp
```

O operador pipe (`|`) redireciona a saída para ***outro comando*** (e não para outro arquivo, como no caso dos operadores `>` e `>>`).

Exemplo: buscando o usuário `thiago` nas 10 últimas linhas do arquivo `/etc/passwd`:
```
thiago@thiago-pc:~/labs/redirecionamento$ tail /etc/passwd | grep thiago
thiago:x:1000:1000:Thiago:/home/thiago:/bin/bash
```
Com um bom conhecimento dos operadores, podemos escrever um comando complexo em uma única linha.

Exemplo: buscando o usuário `thiago` nas 10 últimas linhas do arquivo `/etc/passwd` e jogando o resultado para o arquivo `listagem_usuarios.txt`:
```
thiago@thiago-pc:~/labs/redirecionamento$ tail /etc/passwd | grep thiago > listagem_usuarios.txt

thiago@thiago-pc:~/labs/redirecionamento$ cat listagem_usuarios.txt
thiago:x:1000:1000:Thiago:/home/thiago:/bin/bash
```

# Combinando comando com o pipe
O comando `sort` ordena o conteúdo de uma determinada entrada/arquivos:
```
thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
ssh             22/tcp                          # SSH Remote Login Protocol
ms-wbt-server   3389/tcp

thiago@thiago-pc:~/labs/redirecionamento$ sort listagem.txt
ms-wbt-server   3389/tcp
ssh             22/tcp                          # SSH Remote Login Protocol

thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt | sort
ms-wbt-server   3389/tcp
ssh             22/tcp                          # SSH Remote Login Protocol
```

> O comando a seguir falha quando usamos o mesmo arquivo como entrada e saída (perceba o arquivo vazio como resultado):
> ```
> thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt | sort > listagem.txt
> thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
> thiago@thiago-pc:~/labs/redirecionamento$
> ```
> Para evitar isso, evite que o arquivos de entrada e saída sejam os mesmos:
> ```
> thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt | sort > listagem_ordenada.txt
>
> thiago@thiago-pc:~/labs/redirecionamento$ cat listagem_ordenada.txt
> ms-wbt-server   3389/tcp
> ssh             22/tcp                          # SSH Remote Login Protocol
>
> thiago@thiago-pc:~/labs/redirecionamento$ cat listagem.txt
> ssh             22/tcp                          # SSH Remote Login Protocol
> ms-wbt-server   3389/tcp
> ```

## Exemplo com o arquivo `/var/log/syslog`
Obtendo os 5 últimos registros do arquivo:
```
thiago@thiago-pc:~/labs/redirecionamento$ tail -n 5 /var/log/syslog
Apr 22 14:39:37 thiago-pc systemd[1]: systemd-tmpfiles-clean.service: Deactivated successfully.
Apr 22 14:39:37 thiago-pc systemd[1]: Finished Cleanup of Temporary Directories.
Apr 22 15:03:36 thiago-pc systemd[1]: Starting Ubuntu Advantage Timer for running repeated jobs...
Apr 22 15:03:37 thiago-pc systemd[1]: ua-timer.service: Deactivated successfully.
Apr 22 15:03:37 thiago-pc systemd[1]: Finished Ubuntu Advantage Timer for running repeated jobs.
```

Filtrando apenas os logs com o texto `finished`:
```
thiago@thiago-pc:~/labs/redirecionamento$ tail -n 5 /var/log/syslog | grep -i finished
Apr 22 14:39:37 thiago-pc systemd[1]: Finished Cleanup of Temporary Directories.
Apr 22 15:03:37 thiago-pc systemd[1]: Finished Ubuntu Advantage Timer for running repeated jobs.
```

Jogando o resultado para o arquivo `log_finished.txt`:
```
thiago@thiago-pc:~/labs/redirecionamento$ tail -n 5 /var/log/syslog | grep -i finished > log_finished.txt
thiago@thiago-pc:~/labs/redirecionamento$ cat log_finished.txt
Apr 22 14:39:37 thiago-pc systemd[1]: Finished Cleanup of Temporary Directories.
Apr 22 15:03:37 thiago-pc systemd[1]: Finished Ubuntu Advantage Timer for running repeated jobs.
```
# Filtrando as informações com o cut
O comando `wc` conta linhas, palavras e caracteres (no exemplo, 1004 linhas, 11652 palavras e 94253 caracteres):
```
thiago@thiago-pc:/var/log$ thiago@thiago-pc:/var/log$ cat syslog | grep systemd | wc
grep: (standard input): binary file matches
   1004   11652   94253
```

Visualize pausadamente os resultados usando o pipe seguido de `less`:
```
thiago@thiago-pc:/var/log$ cat syslog | grep systemd | less
```

## Usando o comando `cut`
O comando `cut` corta as linhas de um arquivo a partir de um delimitador (parâmetro `-d`) e permite a seleção de um ou mais campos (parâmetro `-f`)

Criando o arquivo de log para o comando `cut`:
```
thiago@thiago-pc:/var/log$ cat syslog | grep systemd > ~/labs/extraindo_conteudos/logs
grep: (standard input): binary file matches
thiago@thiago-pc:/var/log$ cd ~/labs/extraindo_conteudos/
```

Obtendo o conteúdo da primeira coluna (`-f1`)do resultado do `cut` usando o espaço como delimitador (`-d " "`):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f1
Apr
Apr
Apr
Apr
Apr
Apr
Apr
Apr
Apr
Apr
```

Obtendo a data dos 5 últimos logs com o comando `cut`, selecionando a primeira e a segunda colunas (`-f1,2`):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f1,2
Apr 21
Apr 21
Apr 21
Apr 21
Apr 21
```

Obtendo a data e a hora dos 5 últimos logs com o comando `cut`, selecionando as 3 primeiras colunas (repare que o parâmetro `-f` utiliza um range `1-3`, `-f1-3`):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f1-3
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
```

O mesmo resultado é obtido quando omitimos número 1 no parâmetro `-f-3`, ou seja, filtra do início até a terceira coluna):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f-3
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
Apr 21 18:42:31
```

Obtendo os 5 últimos logs separados por espaço a partir da sexta coluna em diante (repare no parâmetro `-f6-`, ou seja, filtra da sexta coluna até a última; repare também que o delimitador espaço continua aparecendo nas colunas):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f6-
dm-0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/dm-0' failed with exit code 1.
sda3: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda3' failed with exit code 1.
Created slice Slice /system/lvm2-pvscan.
Starting LVM event activation on device 8:3...
sr0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sr0' failed with exit code 1.
```

Excluindo as colunas 4 e 5 do resultado usando `cut` (repare no parâmetro `-f1-3,6-`, ou seja, filtra as 3 primeiras colunas e da coluna seis até a última):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs | cut -d " " -f-3,6-
Apr 21 18:42:31 dm-0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/dm-0' failed with exit code 1.
Apr 21 18:42:31 sda3: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda3' failed with exit code 1.
Apr 21 18:42:31 Created slice Slice /system/lvm2-pvscan.
Apr 21 18:42:31 Starting LVM event activation on device 8:3...
Apr 21 18:42:31 sr0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sr0' failed with exit code 1.
```

Compare com o conteúdo original (ele contém o nome da máquina e o programa que gerou o log):
```
thiago@thiago-pc:~/labs/extraindo_conteudos$ tail -n 5 logs
Apr 21 18:42:31 thiago-pc systemd-udevd[447]: dm-0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/dm-0' failed with exit code 1.
Apr 21 18:42:31 thiago-pc systemd-udevd[449]: sda3: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda3' failed with exit code 1.
Apr 21 18:42:31 thiago-pc systemd[1]: Created slice Slice /system/lvm2-pvscan.
Apr 21 18:42:31 thiago-pc systemd[1]: Starting LVM event activation on device 8:3...
Apr 21 18:42:31 thiago-pc systemd-udevd[448]: sr0: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sr0' failed with exit code 1.
```
# Regex com o Grep #1
Instale o pacote wamerican no Ubuntu (esse pacote contém um dicionário de palavras em inglês):
```
sudo apt install wamerican
```

Depois, tente localizar todos os arquivos no diretório `/usr` que contenham `american`:
```
thiago@thiago-pc:~$ sudo find /usr/ -name *american*
/usr/share/man/man5/american-english.5.gz
/usr/share/doc/wamerican
/usr/share/doc/wamerican/wamerican.scowl-word-lists-used
/usr/share/dict/american-english
```

Copie o arquivo `american-english` para o diretório `expressoes_regulares`:
```
thiago@thiago-pc:~/labs$ mkdir expressoes_regulares
thiago@thiago-pc:~/labs$ cd expressoes_regulares/

thiago@thiago-pc:~/labs/expressoes_regulares$ cp /usr/share/dict/american-english .

thiago@thiago-pc:~/labs/expressoes_regulares$ ls
american-english
```

Visualizando o número de linhas do arquivo `american-english` com `wc -l`:
```
wc american-english -l
```

Ou:
```
cat american-english | wc -l
```

Acrescentando o parâmetro `-E` (maiúsculo) ao comando `grep` podemos fazer pesquisas utilizando expressões regulares.

Exemplo: pesquisando palavras com o sufixo `computers` (excluindo a palavra `computers`):
```
thiago@thiago-pc:~/labs/expressoes_regulares$ grep -E ".+computers" american-english
microcomputers
minicomputers
supercomputers
```

# Regex com o Grep #2

Busca case-insensitive (`-i`) com expressões regulares (`-E`):
```
thiago@thiago-pc:~/labs/expressoes_regulares$ grep -iE "^computer$" american-english
computer
Computer
COMPUTER
```

## O comando `egrep`
O comando `egrep` é o mesmo que o comando `grep -E`: ele pega uma expressão regular e faz sua busca num arquivo:
```
thiago@thiago-pc:~/labs/expressoes_regulares$ egrep -i "^computer$" american-english
computer
Computer
COMPUTER
```

Pesquisando por `smartphones` ou `computers` usando `egrep`:
```
thiago@thiago-pc:~/labs/expressoes_regulares$ egrep -i "^smartphones$|^computers$" american-english
computers
smartphones
```

Uma versão alternativa do comando anterior:
```
egrep -i "^(smartphones|computers)$" american-english
```
