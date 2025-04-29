1. Instalação do tgpt

Primeiro, você precisa instalar o tgpt (uma ferramenta de linha de comando para interagir com o GPT, como o ChatGPT). Para instalá-lo, você pode seguir as instruções do repositório oficial ou usar um dos seguintes métodos.
Instalando o tgpt via go (Go)

Caso você tenha o Go instalado:

# Baixe e instale o tgpt
go install github.com/cheusov/tgpt@latest

Instalando o tgpt via pacote pré-compilado (Linux)

Ou, se preferir instalar diretamente no Linux, você pode baixar o binário:

    Baixe o binário da versão mais recente de https://github.com/cheusov/tgpt/releases.

    Extraia e mova o binário para /usr/local/bin ou outro diretório do PATH.

tar -xzvf tgpt-<version>-linux-x86_64.tar.gz
sudo mv tgpt /usr/local/bin/

2. Erro Recebido

O erro que você encontrou foi:

parse "http://44448:.": invalid port ":." after host

Esse erro ocorreu porque a configuração do proxy estava malformada. Ou seja, o http_proxy estava configurado incorretamente, provavelmente sem o formato correto de host e porta.
3. Correção do Erro - Configuração do Proxy

Agora vamos corrigir esse erro configurando o proxy corretamente, incluindo o tratamento da senha (caso ela tenha caracteres especiais como #, que é o seu caso).
Passo 3.1: URL Encoding

Vamos fazer o URL encoding da senha que contém caracteres especiais.

Senha:

minha.senha#segura

A senha codificada seria:

minha.senha%23segura

    # → %23

Passo 3.2: Configuração das variáveis de ambiente

Agora, você precisa configurar as variáveis de proxy no terminal. Com a senha já codificada, defina as variáveis de ambiente:

export http_proxy="http://usuario:minha.senha%23segura@proxy.empresa.com:8080"
export https_proxy="http://usuario:minha.senha%23segura@proxy.empresa.com:8080"

Certifique-se de substituir usuario, proxy.empresa.com e 8080 pelos dados reais do seu proxy.
4. Verificando as Variáveis de Proxy

Para garantir que as variáveis de ambiente foram configuradas corretamente, execute:

env | grep -i proxy

Você deve ver algo assim:

http_proxy=http://usuario:minha.senha%23segura@proxy.empresa.com:8080
https_proxy=http://usuario:minha.senha%23segura@proxy.empresa.com:8080

5. Testando a Conexão com o Proxy

Testar com um comando como curl para garantir que a configuração de proxy esteja funcionando corretamente:

curl -I https://www.google.com

Se a configuração de proxy estiver correta, você verá uma resposta de cabeçalho HTTP, como:

HTTP/2 200

6. Tornando a Configuração Permanente

Para não ter que reconfigurar o proxy a cada vez que abrir o terminal, você pode tornar as variáveis de proxy permanentes.
Passo 6.1: Editando o arquivo de configuração do shell

Se você estiver usando bash:

nano ~/.bashrc

Ou, se estiver usando zsh:

nano ~/.zshrc

Adicione as variáveis de proxy no final do arquivo:

# Proxy corporativo
export http_proxy="http://usuario:minha.senha%23segura@proxy.empresa.com:8080"
export https_proxy="http://usuario:minha.senha%23segura@proxy.empresa.com:8080"

Passo 6.2: Aplicando a configuração

Salve o arquivo (CTRL + O, depois ENTER para confirmar) e saia (CTRL + X).

Depois, recarregue o arquivo de configuração:

source ~/.bashrc    # ou ~/.zshrc

7. Testando o tgpt

Agora que as variáveis de proxy estão configuradas corretamente e de forma permanente, tente rodar o tgpt:

tgpt --help

Se tudo estiver certo, o comando tgpt deverá funcionar sem erros.
