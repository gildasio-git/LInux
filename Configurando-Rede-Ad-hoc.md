
🪟 Como criar uma rede ad-hoc no Windows (ex: Windows 10/11)

    ⚠️ Observação: A criação de redes ad-hoc foi removida da interface gráfica em versões mais recentes do Windows, mas ainda pode ser feita via terminal (Prompt de Comando).

🔧 Passos:

    Abrir o Prompt de Comando como administrador:

        Pressione Win + X > clique em Prompt de Comando (Admin) ou Windows Terminal (Admin).

    Verificar se sua placa Wi-Fi suporta modo hosted network:

netsh wlan show drivers

    Procure por: Hosted network supported: Yes

Criar a rede ad-hoc:

netsh wlan set hostednetwork mode=allow ssid=MinhaRedeAdHoc key=minhasenha123

    ssid: Nome da rede

    key: Senha de 8 a 63 caracteres

Iniciar a rede:

netsh wlan start hostednetwork

Conectar outros dispositivos: Eles poderão ver a rede Wi-Fi com o nome que você escolheu e se conectar usando a senha.


🐧 Como criar uma rede ad-hoc no Linux (ex: Ubuntu)

Você pode usar comandos no terminal ou ferramentas gráficas como o NetworkManager. Abaixo, um exemplo com terminal (modo mais direto).
🔧 Passos no terminal:

    Listar interfaces de rede disponíveis:

iwconfig

Exemplo: wlan0

Colocar a interface em modo ad-hoc:

sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode ad-hoc
sudo ifconfig wlan0 up

Criar a rede ad-hoc:

sudo iwconfig wlan0 essid "MinhaRedeAdHoc"
sudo iwconfig wlan0 channel 6

Atribuir IP manualmente (exemplo):

    sudo ifconfig wlan0 192.168.1.1 netmask 255.255.255.0

    Outros dispositivos Linux também podem se conectar com os mesmos passos, usando IPs da mesma sub-rede.


🔗 O que é uma rede mesh ad-hoc?

    Uma rede mesh é uma extensão mais robusta da rede ad-hoc: cada nó (dispositivo) não só se comunica diretamente com outros, mas também encaminha dados, criando rotas dinâmicas e resilientes.

Se um dispositivo sai da rede, os dados "procuram" outro caminho — como água fluindo por canos alternativos.
📡 Exemplo prático: 3 dispositivos Linux (Raspberry Pi ou notebooks)
Objetivo:

Você quer conectar 3 dispositivos via Wi-Fi sem roteador e permitir que eles se comuniquem mesmo que nem todos estejam ao alcance direto uns dos outros.
🛠️ Ferramenta recomendada: B.A.T.M.A.N. (Better Approach To Mobile Adhoc Networking)

    Um protocolo de roteamento em malha (mesh) para redes ad-hoc.

🐧 Passo a passo básico no Linux (Ubuntu ou Raspbian)
1. Instalar o B.A.T.M.A.N.

sudo apt update
sudo apt install batctl

2. Colocar a interface Wi-Fi em modo ad-hoc

sudo ip link set wlan0 down
sudo iwconfig wlan0 mode ad-hoc
sudo iwconfig wlan0 essid "MinhaMesh"
sudo iwconfig wlan0 channel 6
sudo ip link set wlan0 up

3. Ativar o B.A.T.M.A.N. na interface

sudo modprobe batman-adv
sudo batctl if add wlan0
sudo ip link set wlan0 up
sudo ip link set bat0 up

4. Atribuir IP para cada nó (dispositivo)

sudo ip addr add 10.0.0.1/24 dev bat0   # No dispositivo 1
sudo ip addr add 10.0.0.2/24 dev bat0   # No dispositivo 2
sudo ip addr add 10.0.0.3/24 dev bat0   # No dispositivo 3

5. Testar conectividade entre nós

No terminal de qualquer um dos dispositivos:

ping 10.0.0.2

Mesmo que o dispositivo 10.0.0.2 esteja fora do alcance direto, se 10.0.0.1 puder alcançá-lo através de 10.0.0.3, o ping deve funcionar. Isso mostra que a rede mesh está funcionando.
📂 Como enviar arquivos na rede mesh

Você pode usar qualquer ferramenta comum de rede, por exemplo:

    scp (via SSH):

    scp arquivo.txt usuario@10.0.0.2:/home/usuario/

    rsync, netcat, ou montar pastas com NFS ou Samba.

📌 Vantagens da rede mesh ad-hoc

    Nenhum ponto central: resiliente a falhas.

    Alta escalabilidade: quanto mais dispositivos, mais forte a rede.

    Ideal para locais remotos ou sem infraestrutura (campos, emergências, etc.).


