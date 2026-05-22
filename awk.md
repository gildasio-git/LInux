# Guia Completo e Didático de AWK no Linux

# Introdução

O AWK é uma poderosa linguagem de processamento de texto presente em praticamente todos os sistemas Linux/Unix.

Seu principal objetivo é:

- Ler arquivos linha por linha
- Separar colunas automaticamente
- Filtrar informações
- Gerar relatórios
- Automatizar análises
- Manipular logs
- Trabalhar com CSV
- Processar dados estruturados

O nome AWK deriva dos sobrenomes de seus criadores:

- Alfred Aho
- Peter Weinberger
- Brian Kernighan

---

# Estrutura Básica do AWK

Sintaxe:

```bash
awk 'padrão { ação }' arquivo
```

Explicação detalhada:

| Elemento | Função |
|---|---|
| awk | Executa o interpretador AWK |
| padrão | Condição ou filtro |
| ação | O que será executado |
| arquivo | Arquivo processado |

---

# Primeiro Exemplo

Arquivo:

```text
Joao 25 TI
Maria 30 RH
Carlos 40 Financeiro
```

Comando:

```bash
awk '{ print $1 }' funcionarios.txt
```

Explicação:

| Parte | Significado |
|---|---|
| print | Imprime dados |
| $1 | Primeira coluna |

Resultado:

```text
Joao
Maria
Carlos
```

---

# Entendendo as Colunas

| Variável | Significado |
|---|---|
| $0 | Linha inteira |
| $1 | Primeira coluna |
| $2 | Segunda coluna |
| $3 | Terceira coluna |
| NF | Quantidade total de colunas |
| NR | Número da linha |

---

# Exibindo Múltiplas Colunas

```bash
awk '{ print $1, $3 }' funcionarios.txt
```

Explicação:

- `$1` → primeira coluna
- `$3` → terceira coluna
- A vírgula separa os campos exibidos

Resultado:

```text
Joao TI
Maria RH
Carlos Financeiro
```

---

# Exibindo a Última Coluna

```bash
awk '{ print $NF }' arquivo.txt
```

Explicação:

| Elemento | Significado |
|---|---|
| NF | Número total de campos |
| $NF | Última coluna |

Muito útil quando o número de colunas varia.

---

# Trabalhando com CSV

Arquivo CSV:

```text
Joao,25,TI
Maria,30,RH
Carlos,40,Financeiro
```

Comando:

```bash
awk -F ',' '{ print $1, $2 }' funcionarios.csv
```

Explicação:

| Parte | Significado |
|---|---|
| -F ',' | Define vírgula como separador |
| $1 | Nome |
| $2 | Idade |

Resultado:

```text
Joao 25
Maria 30
Carlos 40
```

---

# Filtrando Dados

## Mostrar apenas idade maior que 30

```bash
awk '$2 > 30 { print $1 }' funcionarios.txt
```

Explicação:

| Parte | Significado |
|---|---|
| $2 > 30 | Filtra segunda coluna maior que 30 |
| print $1 | Exibe nome |

Resultado:

```text
Carlos
```

---

# Operadores de Comparação

| Operador | Significado |
|---|---|
| == | Igual |
| != | Diferente |
| > | Maior |
| < | Menor |
| >= | Maior ou igual |
| <= | Menor ou igual |

---

# BEGIN e END

## BEGIN

Executa antes de processar o arquivo.

```bash
awk 'BEGIN { print "Inicio" } { print $1 }' arquivo.txt
```

Explicação:

- BEGIN executa antes da leitura
- Pode ser usado para:
  - Cabeçalhos
  - Inicialização
  - Variáveis

---

## END

Executa após terminar o arquivo.

```bash
awk 'END { print "Fim do processamento" }' arquivo.txt
```

---

# Somatórios

Arquivo:

```text
ProdutoA 100
ProdutoB 200
ProdutoC 300
```

Comando:

```bash
awk '{ soma += $2 } END { print soma }' vendas.txt
```

Explicação:

| Parte | Significado |
|---|---|
| soma += $2 | Soma valores da segunda coluna |
| END | Executa ao final |
| print soma | Mostra total |

Resultado:

```text
600
```

---

# Média de Valores

```bash
awk '{ soma += $2; total++ } END { print soma/total }' vendas.txt
```

Explicação:

| Parte | Significado |
|---|---|
| soma += $2 | Soma valores |
| total++ | Conta linhas |
| soma/total | Calcula média |

---

# printf e Formatação

```bash
awk '{ printf "Nome: %-10s Idade: %d\n", $1, $2 }' funcionarios.txt
```

Explicação:

| Elemento | Função |
|---|---|
| printf | Impressão formatada |
| %-10s | Texto alinhado |
| %d | Número inteiro |
| \n | Nova linha |

---

# Expressões Regulares

## Buscar palavra

```bash
awk '/erro/' sistema.log
```

Explicação:

- Exibe linhas contendo "erro"

---

## Ignorar linhas

```bash
awk '!/INFO/' sistema.log
```

Explicação:

| Símbolo | Significado |
|---|---|
| ! | Negação |

Exibe tudo exceto linhas contendo INFO.

---

# Trabalhando com Logs

## Extrair IPs de Apache/Nginx

```bash
awk '{ print $1 }' access.log
```

Explicação:

- Em logs web normalmente:
  - Primeira coluna = IP do cliente

---

# Contar acessos por IP

```bash
awk '{ ips[$1]++ } END { for (ip in ips) print ip, ips[ip] }' access.log
```

Explicação detalhada:

| Parte | Significado |
|---|---|
| ips[$1]++ | Incrementa contador do IP |
| END | Executa ao final |
| for | Percorre array |
| print | Exibe resultado |

Resultado:

```text
192.168.0.10 50
192.168.0.20 100
```

---

# Top 10 IPs

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head
```

Explicação:

| Comando | Função |
|---|---|
| awk | Extrai IPs |
| sort | Ordena |
| uniq -c | Conta ocorrências |
| sort -nr | Ordena numericamente reverso |
| head | Mostra top 10 |

---

# Administração Linux

# Auditoria de Usuários

```bash
awk -F ':' '{ print $1, $7 }' /etc/passwd
```

Explicação:

| Parte | Significado |
|---|---|
| -F ':' | Define ':' como separador |
| $1 | Nome do usuário |
| $7 | Shell do usuário |

---

# Usuários UID maior que 1000

```bash
awk -F ':' '$3 >= 1000 { print $1 }' /etc/passwd
```

Explicação:

- Usuários comuns geralmente possuem UID acima de 1000.

---

# Monitoramento Linux

## Disco acima de 80%

```bash
df -h | awk '$5+0 > 80 { print $1, $5 }'
```

Explicação:

| Parte | Significado |
|---|---|
| df -h | Mostra discos |
| $5+0 | Remove '%' para cálculo |
| > 80 | Filtra acima de 80% |

---

# Consumo de Memória

```bash
free -m | awk '/Mem:/ { print $3 " MB" }'
```

Explicação:

| Parte | Significado |
|---|---|
| free -m | Memória em MB |
| /Mem:/ | Filtra linha memória |
| $3 | Memória usada |

---

# CPU

```bash
top -bn1 | awk '/Cpu/ { print $2 }'
```

Explicação:

- Captura uso de CPU automaticamente.

---

# Processos Pesados

```bash
ps aux | awk '$4 > 10 { print $11, $4 "%" }'
```

Explicação:

| Campo | Significado |
|---|---|
| $4 | Uso de memória |
| $11 | Nome do processo |

---

# Arrays no AWK

```bash
awk '{ nomes[NR] = $1 } END { for(i in nomes) print nomes[i] }'
```

Explicação:

| Parte | Significado |
|---|---|
| nomes[NR] | Array indexado |
| NR | Número da linha |
| for | Percorre array |

---

# Contador de Palavras

```bash
awk '{ palavras[$1]++ } END { for(p in palavras) print p, palavras[p] }'
```

---

# Funções

```bash
awk '
function dobro(x) {
    return x * 2
}
{
    print dobro($2)
}' arquivo.txt
```

Explicação:

| Parte | Significado |
|---|---|
| function | Declara função |
| return | Retorna valor |
| dobro($2) | Executa função |

---

# IF e ELSE

```bash
awk '{
if ($2 > 100)
    print "ALTO"
else
    print "BAIXO"
}'
```

Explicação:

- Permite decisões condicionais.

---

# FOR

```bash
awk '{
for(i=1;i<=NF;i++)
    print $i
}'
```

Explicação:

| Parte | Significado |
|---|---|
| for | Loop |
| i<=NF | Percorre colunas |

---

# Remover Duplicados

```bash
awk '!seen[$0]++' arquivo.txt
```

Explicação:

| Parte | Significado |
|---|---|
| seen[$0] | Array de controle |
| ! | Exibe apenas primeira ocorrência |

---

# Substituição de Texto

```bash
awk '{ gsub("erro","falha"); print }' log.txt
```

Explicação:

| Função | Objetivo |
|---|---|
| gsub | Substitui texto globalmente |

---

# Conversão de Texto

## Maiúsculo

```bash
awk '{ print toupper($0) }'
```

## Minúsculo

```bash
awk '{ print tolower($0) }'
```

---

# Trabalhando com Strings

## Dividir Emails

```bash
awk '{ split($1, a, "@"); print a[1] }'
```

Explicação:

| Parte | Significado |
|---|---|
| split | Divide string |
| a[1] | Parte antes do @ |

---

# Comparando Arquivos

```bash
awk 'NR==FNR { a[$1]; next } !($1 in a)' arq1.txt arq2.txt
```

Explicação:

- Mostra linhas presentes no segundo arquivo e ausentes no primeiro.

---

# Relatórios

## Relatório de Disco

```bash
df -h | awk 'NR>1 {
print "Particao:", $1, "Uso:", $5
}'
```

---

# Script Completo AWK

Arquivo:

```awk
BEGIN {
    FS=","
    print "Relatorio de Funcionarios"
}

NR > 1 {

    soma += $4
    total++

    if ($4 > 5000)
        print $1, "Salario Alto"
}

END {
    print "Media:", soma/total
}
```

Execução:

```bash
awk -f relatorio.awk funcionarios.csv
```

Explicação:

| Parte | Função |
|---|---|
| BEGIN | Inicialização |
| FS | Separador |
| soma | Soma salários |
| if | Condição |
| END | Finalização |

---

# Casos Reais

## Detectar brute force SSH

```bash
awk '/Failed password/ { print $11 }' /var/log/auth.log
```

Objetivo:

- Identificar tentativas de invasão

---

## Descobrir serviços escutando portas

```bash
ss -tulnp | awk 'NR>1 { print $1, $5 }'
```

---

# Diferença entre grep, sed e awk

| Ferramenta | Objetivo |
|---|---|
| grep | Buscar linhas |
| sed | Editar texto |
| awk | Processar colunas e lógica |

---

# Performance do AWK

O AWK é extremamente eficiente para:

- Logs gigantes
- CSV
- Auditoria
- Relatórios
- Pipelines
- Monitoramento

---

# Boas Práticas

- Definir corretamente separadores
- Utilizar scripts .awk para projetos maiores
- Validar expressões regulares
- Comentar scripts
- Utilizar pipelines simples
- Testar comandos antes de produção

---

# Conclusão

O AWK é uma ferramenta extremamente poderosa para automação e administração Linux.

Dominar AWK permite:

- Processar milhões de linhas rapidamente
- Gerar relatórios
- Automatizar auditorias
- Monitorar sistemas
- Manipular CSV
- Analisar logs
- Criar ferramentas administrativas

É uma habilidade fundamental para profissionais Linux avançados.

---

# Referências

## Manual

```bash
man awk
```

## GNU AWK

```bash
info gawk
```

## Ver versão

```bash
awk --version
```
