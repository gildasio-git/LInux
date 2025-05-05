
Claro! Aqui está o conteúdo do tutorial em formato Markdown (.md). Você pode copiar e colar em um arquivo chamado, por exemplo, tutorial_cntlm.md.

# 🛠️ Tutorial: Instalando e Configurando o CNTLM no Linux

## ✅ O que é o CNTLM

CNTLM (NTLM Authorization Proxy Server) é um proxy local que autentica sua máquina contra um proxy corporativo NTLM.

### ✨ Vantagens do uso do CNTLM

- Permite uso do `apt`, `wget`, `curl`, etc., atrás de proxy autenticado.
- Suporte a NTLMv1, NTLMv2 e NTLM2 Session.
- Não expõe sua senha diretamente nos comandos ou arquivos de configuração.

---

## 🧱 Pré-requisitos

- Distribuição Linux (Debian, Ubuntu, RHEL, Fedora, etc.)
- Acesso root ou via `sudo`
- Dados do proxy:
  - Endereço e porta (ex: `10.20.0.4:3128`)
  - Usuário
  - Domínio
  - Senha

---

## 1. 📦 Instalação

### Debian/Ubuntu:

```bash
sudo apt update
sudo apt install cntlm

RHEL/Fedora/CentOS:

sudo dnf install cntlm

2. 🧾 Configuração do CNTLM

Arquivo de configuração: /etc/cntlm.conf
Faça backup:

sudo cp /etc/cntlm.conf /etc/cntlm.conf.bkp

Edite:

sudo nano /etc/cntlm.conf

Exemplo de configuração inicial:

Username        SEU_USUARIO
Domain          SEU_DOMINIO
Password        SUA_SENHA
Proxy           10.20.0.4:3128
NoProxy         localhost, 127.0.0.1
Listen          3128

3. 🔐 Gerar hash da senha

Execute:

cntlm -H

Você verá algo assim:

PassLM          xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
PassNT          xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
PassNTLMv2      EED5997335109771088F091C7A5CD769

Use apenas a linha do PassNTLMv2 no arquivo /etc/cntlm.conf e remova a senha em texto plano.
Exemplo seguro:

Username        44448
Domain          mpro.gov
PassNTLMv2      EED5997335109771088F091C7A5CD769

Proxy           10.20.0.4:3128
NoProxy         localhost, 127.0.0.1
Listen          3128

4. 🚀 Iniciar o serviço

sudo systemctl restart cntlm
sudo systemctl enable cntlm

Verifique:

sudo systemctl status cntlm

5. 🌐 Configurar variáveis de ambiente
Bash ou Zsh (~/.bashrc, ~/.zshrc):

export http_proxy="http://127.0.0.1:3128"
export https_proxy="http://127.0.0.1:3128"
export ftp_proxy="http://127.0.0.1:3128"
export no_proxy="localhost,127.0.0.1"

Aplicar:

source ~/.bashrc

6. 🧪 Testar conexão

curl -I https://www.google.com

Você deve ver HTTP/1.1 200 OK.
7. ⚙️ Configurar proxy para o APT (opcional)

Crie o arquivo:

sudo nano /etc/apt/apt.conf.d/95proxies

Adicione:

Acquire::http::Proxy "http://127.0.0.1:3128";
Acquire::https::Proxy "http://127.0.0.1:3128";

8. 🧠 Dicas finais

    Para testar o modo de autenticação:

cntlm -M www.google.com

Para logs detalhados:

journalctl -u cntlm



**ENDODANDO A SENHA PROXY**

* APT
* *

* PARA SCRIPS
export http_proxy="http://44448:%3A.%23dti%40mpro@10.20.0.4:3128"
export https_proxy="http://44448:%3A.%23dti%40mpro@10.20.0.4:3128"
´
* ENCODING
** : → %3A

** . → %2E

** # → %23

** @ → %40
