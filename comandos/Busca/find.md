<h3> Find </h3>

>find <caminho> <opcoes>

* Exemplos:

`find /var/log` #traz apenas a lista de arquivos diretório log
`find /var/log -type f` #procura apenas arquivos de tipo comum
`find /var/log -type d` #procura por diretórios
`find /var/run -type p` #procura por arquivos de pipe
`find /var/run -type s` #procura arquivos de socket 
`find /var/fun -type l` #procura por arquivos de links simbólicos 
`find /var/log -type f -name *.gz`#procura por tipos de arquivos .gz
`find /var/log -type f -name b*`#procura arquivo que inicia com b
`find /var/log -type f -name auth*` #procurar arquivo auth+qualquer coisa
`find /var/log -type f -name auth.*`#procura arquivos auth.* + qualquer coisa
`find /var/log -type f -name .xz`#procura arquivos que termine com .xz
`find /var/log -type f -name *th*`#procura arquivos que contenha no meio do nome a palavra *th*
`find/root/ -type f -iname arq*`#procura nomes que comece por arq porém ignorando MAIUSU. MINUSC.
`find /etc/ -type f -perm 755` #pesquisa arquivos com permissão 755
`find / -type -size 48k` #pesquisa arquivos com 48k
`find / -type -size +48k` #pesquisa arquivos maiores que 48k
`find / -type -size -48k` #pesquisa arquivos menores que 48k
`find /etc -type f -atime -2`#pesquisa arquivos que foram acessados nos últimos 2 dias
`find /etc -type f -amin 20`#pesquisa arquivos que foram acessados nos últimos 20 minutos
`find /etc -type f -amin -20`#pesquisa arquivos que foram acessados a menos de 20 minutos
`find /etc -type f -amin +20`#pesquisa arquivos que foram acessados a mais que 20 minutos
`find /etc -type f -atime 1`#pesquisa arquivos que foram acessados a 1 dia
`find /etc -type f -atime +2`#pesquisa arquivos que foram acessados a mais que dois dias 
`find /etc -type f -cmin 5`#pesquisa arquivos que foram criados a 5 minutos
`find /etc -type f -cmin -5`#pesquisa arquivos que foram criados a menos do que 5 minutos
`find /etc -type f -ctime 20`#pesquisa arquivos que foram criados a um dia 
`find /etc -type f -ctime -20`#pesquisa arquivos que foram criados amenos que 20 dias
`find /var/log -type f -ctime +31`#pesquisa arquivos de logs com amis de 30 dias
`find /var/log -type f -mmin 20`#pesquisa arquivos modificados a 20 minutos 
`find /var/log -type f -mmin -20`#pesquisa arquivos modificados a menos de 20 minutos 
`find /var/log -type f -mtime +1 -mtime -3` #pesquisa arquivos modificados a mais de um dia e a menos do que 3 dias
`find /var/log  -type f -size +600k -size 6000k `#pesquisa arquivos com mais de 600k e menos que -6000k
`find /var/ -type -size +2M -size 3M` #pesquisa arquivos com mais de 2M e menos que 3M
`find /var/log -type f -newer arq1`#pesquisa arquivos modificados mais recentemente que o arq1
`find / -type  f -gid 42`#pesquisa arquivos cujo grupo desse arquivo tem o numero 42
`find / -type  f -group shadow 42`#pesquisa arquivos cujo grupo seja shadow
`find / -type `










 