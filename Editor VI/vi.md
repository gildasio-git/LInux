<h3>USO PADRÃO DO VI/vim - EDIÇÃO</h3>

`INSERT` | Inserir/Editar arquivo 
`ESQ+:q!` | Sai sem alterar nada 
`ESQ+x` |Sai e salva as modificações
`R` | Raliza o replace de um caractere
`ESQ:nomearquivo` | Salva o arquivo com outro nome 


<h3>MANIPULAÇÃO DE TEXTOS</h3>

`DD` | copia linha para memória 
`ESQ+numero_linhas_para_recordar+DD` | Rercorta números de linhas desejadas.
`ESQ:40` | Vai para linha 40 do arquivo 
`ESQ:gg` | Vai para o inicio do arquivo
`ESQ:G` | Vai para o final do arquivo 
`ESQ:/` | Localiz string dentro do arquivo 
`u` | Undo Desfaz o último comando 
`p` | Copia linha ABAIXO
`P` | Copia linha ACIMA
`ESQ+numero_copiar+yy`| Copia 4 linhas 
`:w` | Salva as alterações no arquivo 
`:s/ssh/SSH` | Pequisa uma string e realiza a substituição, porém apenas a primeita entrada
`:s/SSH/ssh/g` | Substitui todas as entradas maicusulas SSH para minusculas ssh, porém apenas na mesma linha 
`:%s/domain/teste/g` | Substitui em todo o arquivo.


