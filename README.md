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
