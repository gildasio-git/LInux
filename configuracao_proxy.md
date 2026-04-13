
# Manual de Configuração de Proxy no Linux para Docker e APT

## Objetivo

Documentar a configuração do proxy corporativo no ambiente Linux, garantindo funcionamento dos seguintes serviços:

- Docker
- APT (Ubuntu)
- Shell / variáveis de ambiente
- Ferramentas como `curl`, `wget`, `git`

---

## 1. Configuração do Proxy no Sistema Operacional

A configuração global do proxy foi centralizada no arquivo:

```bash
/etc/environment
```

### Editar arquivo

```bash
sudo nano /etc/environment
```

### Conteúdo

```bash
HTTP_PROXY="http://USUARIO:SENHA@IP_PROXY:PORTA"
HTTPS_PROXY="http://USUARIO:SENHA@IP_PROXY:PORTA"
NO_PROXY="localhost,127.0.0.1,::1"

http_proxy="http://USUARIO:SENHA@IP_PROXY:PORTA"
https_proxy="http://USUARIO:SENHA@IP_PROXY:PORTA"
no_proxy="localhost,127.0.0.1,::1"
```

### Aplicar alterações

```bash
source /etc/environment
```

### Validar

```bash
env | grep -i proxy
```

---

## 2. Ajuste do `.bashrc`

Foi identificado proxy duplicado no arquivo do usuário:

```bash
~/.bashrc
```

### Ação executada

As linhas foram **comentadas** para evitar duplicidade.

### Exemplo

```bash
# export HTTP_PROXY="http://..."
# export HTTPS_PROXY="http://..."
# export NO_PROXY="localhost,127.0.0.1,::1"
```

### Aplicar

```bash
source ~/.bashrc
```

---

## 3. Configuração do Docker

O Docker daemon foi configurado com proxy dedicado.

### Arquivo

```bash
/etc/docker/daemon.json
```

### Editar

```bash
sudo nano /etc/docker/daemon.json
```

### Conteúdo

```json
{
  "proxies": {
    "http-proxy": "http://USUARIO:SENHA@IP_PROXY:PORTA",
    "https-proxy": "http://USUARIO:SENHA@IP_PROXY:PORTA",
    "no-proxy": "localhost,127.0.0.1,::1"
  }
}
```

### Reiniciar serviço

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### Validar

```bash
docker info | grep -i proxy
```

### Teste

```bash
docker run hello-world
```

---

## 4. Configuração do APT

Inicialmente foi identificado conflito entre:

```bash
/etc/apt/apt.conf
```

e configurações via ambiente.

### Ação executada

- remoção das linhas antigas de proxy
- utilização do proxy via `/etc/environment`

### Teste

```bash
sudo apt clean
sudo apt update
```

---

## 5. Diagnóstico realizado

### Teste de conectividade com proxy

```bash
curl -v -x http://USUARIO:SENHA@IP_PROXY:PORTA https://registry-1.docker.io/v2/
```

### Resultado esperado

```text
HTTP/1.1 200 Connection established
HTTP/2 401
```

Esse comportamento confirma:

- autenticação do proxy ok
- túnel HTTPS ok
- acesso ao Docker Hub ok

---

## 6. Comandos úteis de diagnóstico

### Ver variáveis carregadas

```bash
env | grep -i proxy
```

### Localizar configurações antigas

```bash
grep -Ri "proxy" ~/.bashrc ~/.profile /etc/environment /etc/profile.d 2>/dev/null
```

### Validar proxy no Docker

```bash
docker info | grep -i proxy
```

### Validar proxy no APT

```bash
sudo apt-config dump | grep -i proxy
```

---

## 7. Boas práticas adotadas

- centralização no `/etc/environment`
- remoção de duplicidade no `.bashrc`
- Docker com configuração dedicada
- validação por `curl`
- validação por `apt update`
- documentação do ambiente

---

## 8. Conclusão

Ambiente Linux configurado com sucesso para uso de proxy corporativo.

### Serviços validados

- sistema operacional
- Docker
- APT
- Shell
- conectividade HTTPS

Status final:

```text
OK
```
