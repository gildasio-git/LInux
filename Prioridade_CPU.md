# Tutorial — Controlando o Consumo de CPU e Disco do OCRmyPDF no Linux

## Introdução

Ferramentas de OCR como o OCRmyPDF podem consumir muitos recursos do sistema durante o processamento de arquivos PDF.

Isso ocorre porque o OCR normalmente utiliza:

- múltiplos núcleos da CPU;
- grande quantidade de memória RAM;
- leitura e escrita intensa em disco;
- processos paralelos;
- geração de imagens temporárias.

Em máquinas mais simples ou durante processamento de arquivos grandes, isso pode causar:

- travamentos;
- congelamentos;
- uso excessivo de swap;
- lentidão geral do sistema;
- abortamento do processo OCR.

Este tutorial mostra como executar o OCR de forma controlada e profissional utilizando recursos nativos do Linux.

---

# Objetivo

Executar o OCR utilizando:

- menor prioridade de CPU;
- menor prioridade de disco;
- limitação de núcleos utilizados;
- controle de paralelismo;
- otimização do PDF final.

---

# Comando utilizado

```bash
ionice -c3 nice -n 19 \
taskset -c 0,1 \
ocrmypdf -j 2 \
--optimize 2 \
entrada.pdf saida.pdf
```

---

# Entendendo o comando passo a passo

---

# 1. ionice -c3

```bash
ionice -c3
```

## O que é?

O `ionice` controla a prioridade de acesso ao disco (I/O).

O OCR realiza muitas operações de:

- leitura;
- escrita;
- criação de arquivos temporários.

Sem controle de I/O, o sistema pode ficar lento ou congelar.

## O que significa `-c3`?

O parâmetro:

```bash
-c3
```

define a classe `idle`.

Isso significa:

> O processo só utilizará o disco quando ele estiver livre.

## Benefícios

Com isso:

- o sistema permanece responsivo;
- outros programas continuam funcionando normalmente;
- reduz travamentos durante OCR.

---

# 2. nice -n 19

```bash
nice -n 19
```

## O que é?

O `nice` controla a prioridade de CPU de um processo.

## Escala de prioridade

O Linux trabalha com prioridades entre:

```text
-20 = prioridade máxima
  0 = prioridade padrão
 19 = prioridade mínima
```

## O que significa `-n 19`?

Você está dizendo ao Linux:

> Execute este processo apenas quando houver CPU disponível.

## Benefícios

Isso evita:

- congelamento da interface gráfica;
- lentidão extrema do sistema;
- consumo agressivo da CPU.

---

# 3. taskset -c 0,1

```bash
taskset -c 0,1
```

## O que é?

O `taskset` define afinidade de CPU.

Ele limita em quais núcleos o processo poderá executar.

## O que significa `-c 0,1`?

Você está informando:

> Utilize apenas os núcleos 0 e 1 da CPU.

## Exemplo prático

Em um processador com 8 threads:

Sem controle:

```text
CPU0 ████████
CPU1 ████████
CPU2 ████████
CPU3 ████████
CPU4 ████████
CPU5 ████████
CPU6 ████████
CPU7 ████████
```

Com `taskset`:

```text
CPU0 ████████
CPU1 ████████
CPU2
CPU3
CPU4
CPU5
CPU6
CPU7
```

## Benefícios

- evita uso total do processador;
- mantém o sistema utilizável;
- ideal para desktops e servidores.

---

# 4. ocrmypdf -j 2

```bash
ocrmypdf -j 2
```

## O que é?

O parâmetro `-j` define quantos processos paralelos serão utilizados.

## O que significa `-j 2`?

Você está limitando o OCR para:

```text
2 workers simultâneos
```

## Sem limitação

O OCR pode tentar utilizar todos os núcleos disponíveis.

Isso normalmente causa:

- CPU em 100%;
- aquecimento excessivo;
- travamentos.

## Benefícios

- menor uso de CPU;
- maior estabilidade;
- menor consumo de memória RAM.

---

# 5. --optimize 2

```bash
--optimize 2
```

## O que faz?

Controla a compressão e otimização do PDF final.

## Níveis disponíveis

| Nível | Descrição |
|---|---|
| 0 | sem otimização |
| 1 | leve |
| 2 | média |
| 3 | agressiva |

## Benefícios

Reduz:

- tamanho final do PDF;
- uso de disco;
- cache em memória.

---

# Fluxo completo do processamento

O Linux interpreta o comando da seguinte forma:

```text
1. Execute OCR
2. Utilize baixa prioridade de CPU
3. Utilize baixa prioridade de disco
4. Use apenas CPU 0 e CPU 1
5. Execute apenas 2 workers
6. Comprima o resultado final
```

---

# Monitoramento do processo

## Monitorar CPU

Instale:

```bash
sudo apt install htop
```

Execute:

```bash
htop
```

## Monitorar memória RAM

```bash
free -h
```

## Monitorar uso de disco

Instale:

```bash
sudo apt install iotop
```

Execute:

```bash
iotop
```

---

# Verificando encerramento por falta de memória

Caso o OCR seja encerrado inesperadamente:

```bash
dmesg | grep -i oom
```

Se aparecer:

```text
Killed process
```

significa que faltou memória RAM.

---

# Versão mais profissional com controle de memória

```bash
systemd-run --scope \
-p MemoryMax=2G \
ionice -c3 nice -n 19 \
taskset -c 0,1 \
ocrmypdf -j 2 entrada.pdf saida.pdf
```

## O que esse comando adicional faz?

### MemoryMax=2G

Limita o processo para usar no máximo:

```text
2 Gigabytes de RAM
```

## Benefícios

Evita:

- estouro de memória;
- swap excessivo;
- congelamentos severos;
- atuação do OOM Killer.

---

# Recomendações práticas

## Máquina simples (4 GB RAM)

```bash
-j 1
taskset -c 0
```

## Máquina intermediária (8 GB RAM)

```bash
-j 2
taskset -c 0,1
```

## Máquina mais forte (16 GB+)

```bash
-j 4
taskset -c 0,1,2,3
```

---

# Conclusão

Utilizando corretamente:

- ionice
- nice
- taskset
- ocrmypdf -j

é possível transformar um processo pesado de OCR em uma tarefa controlada e estável, evitando:

- travamentos;
- congelamentos;
- consumo excessivo da máquina.

Essa abordagem é amplamente utilizada em:

- servidores Linux;
- ambientes corporativos;
- automações;
- processamento em lote;
- digitalização documental profissional.
