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
