# 📘 Tutorial Completo — Tradução de `man` e `--help` no Linux (On-the-fly)

## 🎯 Objetivo
Permitir a tradução de:
- Manuais (`man`)
- Saída de ajuda (`--help`)

👉 Mesmo quando **não existe tradução oficial no sistema** (ex: `rsync`)

---

## 🔎 Cenário

Mesmo com o sistema em `pt_BR.UTF-8`, muitos comandos continuam em inglês porque:

- Não possuem tradução oficial
- Não existem arquivos em `/usr/share/man/pt_BR/`

✔ Solução: **tradução dinâmica (tempo real)**

---

## ⚠️ Pré-requisitos

Instalar ferramenta de tradução:

```bash
sudo apt update
sudo apt install translate-shell
