
# Manual de Solução — Erro Docker com Proxy Authentication Required

## Problema

Ao executar o comando:

```bash
docker run <nome_da_imagem>
```

era retornado o erro:

```text
docker: Error response from daemon:
Get "https://registry-1.docker.io/v2/":
Proxy Authentication Required
```

---

## Sintoma

O sistema operacional possuía conectividade com a rede e acesso à internet, porém o Docker não conseguia realizar pull das imagens do Docker Hub.

Erro apresentado:

```text
Proxy Authentication Required
```

---

## Causa Raiz

O host Linux possuía acesso ao proxy corporativo, porém o **Docker daemon não estava herdando a configuração do proxy**.

Em outras palavras:

- o sistema operacional navegava normalmente
- o `curl` acessava a internet
- o serviço Docker não utilizava as credenciais do proxy

---

## Etapa 1 — Validação da conectividade do proxy

Foi realizado teste direto com `curl`:

```bash
curl -v -x http://USUARIO:SENHA@10.20.0.4:3128 https://registry-1.docker.io/v2/
```

### Resultado esperado

```text
HTTP/1.1 200 Connection established
```

e posteriormente:

```json
{"errors":[{"code":"UNAUTHORIZED","message":"authentication required"}]}
```

### Interpretação

Esse resultado confirma que:

- proxy autenticou corretamente
- túnel HTTPS foi criado
- certificado SSL válido
- acesso ao Docker Hub funcional

---

## Etapa 2 — Diagnóstico do Docker daemon

Verificação do uso de proxy pelo Docker:

```bash
docker info | grep -i proxy
```

Foi constatado que o Docker **não estava exibindo variáveis de proxy**, indicando ausência de configuração efetiva.

---

## Etapa 3 — Configuração do proxy no Docker

Arquivo criado:

```bash
sudo nano /etc/docker/daemon.json
```

Conteúdo aplicado:

```json
{
  "proxies": {
    "http-proxy": "http://USUARIO:SENHA@10.20.0.4:3128",
    "https-proxy": "http://USUARIO:SENHA@10.20.0.4:3128",
    "no-proxy": "localhost,127.0.0.1,::1"
  }
}
```

---

## Etapa 4 — Reinicialização do serviço

Após salvar o arquivo, o serviço foi reiniciado:

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

## Etapa 5 — Validação da solução

Executado:

```bash
docker info | grep -i proxy
```

Resultado esperado:

```text
HTTP Proxy: http://USUARIO:SENHA@10.20.0.4:3128
HTTPS Proxy: http://USUARIO:SENHA@10.20.0.4:3128
```

---

## Etapa 6 — Teste final

Teste de download e execução:

```bash
docker run hello-world
```

ou

```bash
docker pull ubuntu
```

Resultado: **sucesso**.

---

## Solução Final

A solução consistiu em **configurar explicitamente o proxy no daemon do Docker** através do arquivo:

```text
/etc/docker/daemon.json
```

---

## Recomendações de Segurança

Aplicar permissões restritas ao arquivo:

```bash
sudo chmod 600 /etc/docker/daemon.json
sudo chown root:root /etc/docker/daemon.json
```

---

## Observações Técnicas

O código HTTP:

```text
401 UNAUTHORIZED
```

retornado pelo `curl` **não é erro do proxy**.

Ele indica apenas que o Docker Hub exige token de autenticação para acesso ao endpoint `/v2/`, comportamento normal.

O erro real estava no daemon não utilizando o proxy configurado.

---

## Conclusão

Problema resolvido com sucesso através de:

- validação da conectividade
- diagnóstico do serviço Docker
- configuração do `daemon.json`
- reinício do daemon
- teste funcional final
