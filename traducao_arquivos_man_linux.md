# 📘 Tutorial Completo — Tradução de `man` e `--help` no Linux (On-the-fly)

## 🎯 Objetivo
Permitir a tradução de:
- Manuais (`man`)
- Saída de ajuda (`--help`)

Mesmo quando não existe tradução oficial no sistema (ex: rsync).

---

## 🔎 Cenário

Mesmo com o sistema em pt_BR.UTF-8, muitos comandos continuam em inglês porque:
- Não possuem tradução oficial
- Não existem arquivos em /usr/share/man/pt_BR/

Solução: tradução dinâmica em tempo real.

---

## ⚠️ Pré-requisitos

Instalar ferramenta de tradução:

```bash
sudo apt update
sudo apt install translate-shell
```

---

## 🛠️ Etapa 1 — Criar funções no Bash

Editar o arquivo:

```bash
nano ~/.bashrc
```

### Função para traduzir man

```bash
manpt() {
    if [ -z "$1" ]; then
        echo "Uso: manpt <comando>"
        return 1
    fi

    man "$1" | col -b | trans -b :pt
}
```

### Função para traduzir --help

```bash
helppt() {
    if [ -z "$1" ]; then
        echo "Uso: helppt <comando>"
        return 1
    fi

    "$@" --help 2>&1 | trans -b :pt
}
```

---

## 🛠️ Etapa 2 — Aplicar configurações

```bash
source ~/.bashrc
```

---

## ✅ Uso prático

### Traduzir manual

```bash
manpt rsync
manpt systemctl
```

### Traduzir help

```bash
helppt rsync
helppt ls
```

---

## ✔️ Validação

```bash
manpt ls | head
```

Se aparecer em português → OK

---

## 🚀 Otimização com cache

Criar diretório:

```bash
mkdir -p ~/.cache/man_pt
```

Função com cache:

```bash
manpt() {
    if [ -z "$1" ]; then
        echo "Uso: manpt <comando>"
        return 1
    fi

    CACHE="$HOME/.cache/man_pt/$1.txt"

    if [ -f "$CACHE" ]; then
        less "$CACHE"
    else
        man "$1" | col -b | trans -b :pt > "$CACHE"
        less "$CACHE"
    fi
}
```

---

## ⚠️ Boas práticas

- Tradução pode conter erros técnicos
- Use para estudo
- Em produção, prefira documentação original

---

## 📌 Conclusão

Sistema não tem tradução nativa para todos comandos.
Solução: tradução dinâmica via script.
