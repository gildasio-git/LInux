# 🔐 Criação de Cofre Criptografado com LUKS (Linux)

## 📌 Objetivo

Criar um arquivo que funcione como um **disco virtual criptografado**, permitindo armazenar dados com segurança, acessíveis apenas mediante senha.

---

# 🧱 1. Criar o arquivo do cofre (disco virtual)

```bash
dd if=/dev/zero of=cofre.img bs=1M count=500
```

📌 Explicação:

* Cria um arquivo de 500MB
* Ajuste o tamanho conforme necessário (`count`)

---

# 🔐 2. Criptografar o arquivo com LUKS

```bash
cryptsetup luksFormat cofre.img
```

⚠️ Aviso:

* Este comando **apaga completamente qualquer conteúdo existente**

Confirme digitando:

```
YES
```

---

# 🔑 3. Definir senha

O sistema solicitará:

```
Enter passphrase:
Verify passphrase:
```

📌 Recomendações:

* Use senha forte (letras, números e símbolos)
* Mínimo de 8 a 12 caracteres

Exemplo:

```
Rec@2026#Backup!
```

---

# 🔓 4. Abrir o cofre criptografado

```bash
cryptsetup open cofre.img cofre_seguro
```

📌 Isso criará o dispositivo:

```
/dev/mapper/cofre_seguro
```

---

# 💽 5. Criar sistema de arquivos

```bash
mkfs.ext4 /dev/mapper/cofre_seguro
```

📌 Agora o cofre está pronto para uso

---

# 📂 6. Montar o cofre

```bash
mount /dev/mapper/cofre_seguro /mnt
```

📌 O diretório `/mnt` será o ponto de acesso

---

# 📥 7. Utilizar o cofre

Exemplo de cópia de arquivo:

```bash
cp arquivo.txt /mnt/
```

---

# 🔒 8. Fechar o cofre (OBRIGATÓRIO)

```bash
umount /mnt
cryptsetup close cofre_seguro
```

📌 Garante que os dados fiquem protegidos

---

# 🔁 9. Acessar o cofre após reinicialização

Após reiniciar, o cofre estará fechado.

## Abrir:

```bash
cryptsetup open cofre.img cofre_seguro
```

## Montar:

```bash
mount /dev/mapper/cofre_seguro /mnt
```

---

# 🧠 Funcionamento

| Estado        | Acesso             |
| ------------- | ------------------ |
| Cofre fechado | ❌ Não acessível    |
| Cofre aberto  | ✔️ Acesso liberado |

---

# 🔐 Segurança

* Criptografia padrão LUKS (AES)
* Proteção contra acesso não autorizado
* Segurança dependente da senha

---

# ⚠️ IMPORTANTE

* ❗ Perda da senha = perda total dos dados
* ❗ Sempre desmontar antes de desligar
* ❗ Não compartilhar senha junto com o arquivo

---

# 📦 Estrutura final

```
cofre.img                 → arquivo criptografado
/dev/mapper/cofre_seguro → volume montado (temporário)
```

---

# 🏁 Conclusão

Você criou um:

✔️ Disco virtual
✔️ Criptografado com segurança forte
✔️ Portátil (pode ser movido/copiado)
✔️ Ideal para backup seguro

---

# 🚀 Possíveis melhorias

* Script automático de montagem
* Backup do header LUKS
* Integração com rotina de backup
* Uso com armazenamento externo
