<h3>EXPRESSÕES REGULARES</h3>

>Instale o pacote **wamerican** para praticar
 `apt install wamerican`

<h3>PRATICANDO</h3>

`cat american-english | grep computer`   - Aqui ele filgra apenas palavras computer, porém não é o suficiente porque precisanos melhorar, ou seja extrair apenas essa palavra da lista e para isso podemos usar uma explressão passando um parâmetro `-E` que diz que estamos passando uma expressão regular para que o SHELL possa entender e devolver o que precisamos.

`cat american-english | grep -E "^computer"`

`^` | Indica início de linha, a exemplo acima pesquisa todas as palavras que iniciam com computer

`cat american-english | grep -E "computer$"`

`$` | Indica final palavra, no caso agora a exemplo da pesquisa acima, estamos pesquisando palavra que terminam com a palavra computer.

`cat american-english | grep -E "^computer$"` | Veja que agora estamos juntando as duas formas, pesquisando palavras que iniciam e terminam com a palavra computer.

`grep -i computer` | Ignora letras maiusculas e minuscula

`cat american-english | grep -iE "^computer$"` | Realiza mesma busca da palavra computer, porém agora ignora palavra maiusulas e minusculas, para isso utilizamos o parêmtro -i - ignora maisculas e minusculas e -E que infomra que estamos passando uma expressão regular.

>o EGREP é equivalente ao utilizamos o parãetro -E, é como se fosse um atalho para o "grep -E", também podemos utilizar a consulta sem aspas, que também vai funcionar.


<h3>UTILIZANDO CONDICIONAL EM EXPRESSÕES REGULARES</h3>

`cat american-english | grep  "computer|smartphone"` | Realiza pesquisa dentro do texto, pelos termos computers ou smartphone.

`cat american-english | grep -iE "^smartphone$|^computer$"` | Reinando a busca acima, agora temos apenas os termos que desejamos.


<h3>MAIS EXPRESSÕES REGULARES COM EGREP</h3>

`egrep "^.oot" american-english` | pesquisa termo onde o inicio pode váriar em qualquer caracteres seguidos das strings oot.

`.` | Pode ser qualquer coisa, ou seja pode variar.
 
`egrep "^.oot$" american-english` | Pesquisa termo onde a palavra pode iniciar com qualquer caracteres  "." e terminar com t. 

`egrep "^.oot..$" american-english` | Pesquisa temron onde o inicio pode variar em qualquer caractere, seguidos das strings "oot.." podendo ter mais dois caracteres ".."

`egrep "^[fhio]oot..$" american-english` | Pesquisa temros que inicia com os caracteres "[fhio]" seguidos de "oot.." podendo ter mais dois caracteres "..". 

>EXMPLO DE EXPRESSÕES REGULARES "^, $, |, ., []"


