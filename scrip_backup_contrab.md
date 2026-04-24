# 📦 Manual Completo — Backup Docker + Agendamento (Produção)
# obs. editar o crontab do root (sudo crontab -e)
# linha do crontab "0 3 */15 * * /opt/scripts/backup_docker.sh >> /var/log/backup_cron.log 2>&1"
---

# 🧾 1. Script Definitivo de Backup

```bash
#!/bin/bash

# ==========================
# CONFIGURAÇÕES
# ==========================

# Data/hora para nome do arquivo
DATA=$(date +"%Y-%m-%d_%H-%M-%S")

# Diretório onde os backups serão armazenados
DESTINO="/backup_local/docker"

# Nome do arquivo final
ARQUIVO="$DESTINO/backup_docker_$DATA.tar.gz"

# Arquivo de log
LOG="/var/log/backup_docker.log"

# Detecta automaticamente o diretório do Docker
DOCKER_DIR=$(docker info 2>/dev/null | grep "Docker Root Dir" | awk '{print $4}')

# Caso não consiga detectar, usa padrão
if [ -z "$DOCKER_DIR" ]; then
    DOCKER_DIR="/var/lib/docker"
fi

# Lista de volumes a serem copiados
VOLUMES=(
    "postgres-db-volume"
    "htfapps-volume"
    "docker_letsencrypt"
    "alfresco-solr-volume"
    "alfresco-content-data-volume"
)

# Diretório com arquivos de instalação/configuração
PASTA_INSTALACAO="/opt/docker_apps"

# ==========================
# INÍCIO DO LOG
# ==========================
echo "===================================" >> "$LOG"
echo "Início: $DATA" >> "$LOG"

# ==========================
# VALIDAÇÕES
# ==========================

# Cria diretório de destino se não existir
mkdir -p "$DESTINO"

# Verifica se diretório do Docker existe
if [ ! -d "$DOCKER_DIR" ]; then
    echo "Erro: diretório Docker não encontrado: $DOCKER_DIR" >> "$LOG"
    exit 1
fi

# ==========================
# PARAR DOCKER
# ==========================
echo "Parando Docker..." >> "$LOG"
systemctl stop docker

# Verifica se o comando executou com sucesso
if [ $? -ne 0 ]; then
    echo "Erro ao parar Docker" >> "$LOG"
    exit 1
fi

# ==========================
# MONTAR LISTA DE BACKUP
# ==========================
LISTA_BACKUP=""

# Percorre lista de volumes
for VOLUME in "${VOLUMES[@]}"; do

    CAMINHO="$DOCKER_DIR/volumes/$VOLUME/_data"

    # Verifica se o volume existe
    if [ -d "$CAMINHO" ]; then
        LISTA_BACKUP="$LISTA_BACKUP $CAMINHO"
    else
        echo "Aviso: volume não encontrado -> $VOLUME" >> "$LOG"
    fi
done

# Adiciona pasta de instalação ao backup
if [ -d "$PASTA_INSTALACAO" ]; then
    LISTA_BACKUP="$LISTA_BACKUP $PASTA_INSTALACAO"
else
    echo "Aviso: pasta de instalação não encontrada" >> "$LOG"
fi

# ==========================
# BACKUP
# ==========================
echo "Executando backup..." >> "$LOG"

tar -czpf "$ARQUIVO" $LISTA_BACKUP

# Verifica erro no backup
if [ $? -ne 0 ]; then
    echo "Erro durante backup!" >> "$LOG"
    systemctl start docker
    exit 1
fi

# ==========================
# VALIDAÇÃO DO BACKUP
# ==========================
echo "Validando integridade..." >> "$LOG"

tar -tzf "$ARQUIVO" > /dev/null

if [ $? -ne 0 ]; then
    echo "Backup corrompido!" >> "$LOG"
else
    echo "Backup válido." >> "$LOG"
fi

# ==========================
# SUBIR DOCKER
# ==========================
echo "Iniciando Docker..." >> "$LOG"
systemctl start docker

# ==========================
# LIMPEZA DE BACKUPS ANTIGOS
# ==========================
find "$DESTINO" -type f -name "*.tar.gz" -mtime +7 -delete

# ==========================
# FINALIZAÇÃO
# ==========================
echo "Finalizado: $(date)" >> "$LOG"
echo "===================================" >> "$LOG"
```

---

# 🧠 2. Explicação dos principais comandos

### 🔹 DATA

Gera timestamp para evitar sobrescrita:

```
date +"%Y-%m-%d_%H-%M-%S"
```

---

### 🔹 docker info

Descobre automaticamente onde o Docker armazena os dados.

---

### 🔹 systemctl stop docker

Para todos os containers → garante consistência.

---

### 🔹 for VOLUME in

Percorre lista de volumes e valida existência.

---

### 🔹 tar -czpf

Cria backup compactado:

* c → criar
* z → gzip
* p → mantém permissões
* f → arquivo

---

### 🔹 tar -tzf

Valida o backup sem extrair.

---

### 🔹 find -mtime +7

Remove backups com mais de 7 dias.

---

# 🕒 3. Agendamento com CRONTAB

---

## 🔧 1. Dar permissão ao script

```bash
chmod +x /opt/scripts/backup_docker.sh
```

---

## 🔧 2. Editar crontab

```bash
crontab -e
```

---

## 🕑 3. Exemplo de agendamento

### ▶️ Executar todo dia às 02:00

```bash
0 2 * * * /opt/scripts/backup_docker.sh >> /var/log/backup_cron.log 2>&1
```

---

## 📌 Explicação do cron

```
┌──────── minuto (0 - 59)
│ ┌────── hora (0 - 23)
│ │ ┌──── dia do mês
│ │ │ ┌── mês
│ │ │ │ ┌─ dia da semana
│ │ │ │ │
0 2 * * *
```

---

# 🔄 4. Procedimento de validação

Após executar:

```bash
ls -lh /backup_local/docker
```

Verifique:

* Arquivo gerado
* Tamanho coerente

---

## Teste de integridade

```bash
tar -tzf backup.tar.gz
```

---

# 🧩 5. Estratégia recomendada

### Diário

✔ Script de backup

---

### Semanal

✔ Imagem completa (snapshot via boot)

---

### Mensal

✔ Teste de restore

---

# ⚠️ 6. Boas práticas

* Executar fora do horário comercial
* Monitorar espaço em disco
* Testar restauração regularmente
* Armazenar backup fora do servidor

---

# 💬 Conclusão

Este processo garante:

✔ Backup consistente
✔ Recuperação rápida
✔ Segurança de dados
✔ Estrutura profissional

---

# 🚀 Próximo nível (opcional)

* Integração com nuvem (rclone)
* Alertas automáticos
* Backup incremental
* Snapshot via LVM

---
