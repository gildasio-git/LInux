<h3>PROBLEMAS QUE PODEM OCORRER DURANTE INSTALAÇÃO DEBAIN</h3>

**MANUAL DEBIAN**
* www.debian.org/releases/stable/amd64/index.pt.html

* BOOT
>Indicar o dispositivo que ira iniciar o sistema. ou UEFI ou BIOS., a partir do debian BUSTER não precisa desabilitar o "SECURITY BOOT". comanado **DD - Despejo de disco**,as imagens ja credenciam os dois tipos de sistema, UEFI / BIOS., pode ainda calcular o rash da imagem baixada para checar se a imgaem baixada esta ok.

* REDE
>Alguns noteboks precisam de firmware não livres, e como isso pode se deparar com tela reportando com a referẽncia dessa necessidade de firmware não livre, pode baixar de outro computador e apontar a mídia contendo o firmware não livre. Quando tem mais de uma placa o sistema ira detectar e pedir para escolher qual placa deseja usar, rede que não atribui automaticamente ip via DHCP, pode checar primeramente problema físico, cabo por exemplo. Porém saiba o endereço de seguimento do ip pode configurar manualmente, mais ainda pode continuar a instalação, falta da rede não impede a instalação do sistema.

* PARTICIONAMENTO
>Ler atentamente a msg para não apagar disco inteiro, pode separar com atencedẽncia uma partição para instalar ou fazer manualmente os particonamento. Uma observação os sistemas microsoft utilizam um sistema chamaqdo FAST BOOT ,que faz parecer que a iniciação do sistema é mais rápido, porém o que ele faz é uma hibernação., digo isso porque aqui pode ter problema ao participar o disco e durante a instalação do LINux ele reclamar que o modo hibernação do windows, isso em caso de DUAL boot,  esta ligado, e ai precisa desabilitar esse recurso no windows.

* GRUB
>Observer tipo de boot UEFI/GPT.

* INTERRUPÇÃO
>Interrução energia, pode ocorrer. com isso precisara realizar o boot e recomeçar o processo de instalação sem problemas.


<h3>MODELO MENTAL DO BOOT</h3>

* BIOS (BASIC INPUT OUTPUT SYSTEM)
>Sistema que funciona desde a década de 80 e computadores até uns 10 12 anos atrás ainda funcionava com esse mesmo esquema extremamente limitado sem praticamente nenhum recurso muito alimentar e o processo de boot então ele tinha que passar por uma quantidade bastante grande de etapas até que se conseguisse iteração com o sistema operacional.

* UEFI->GPT,DOS,FAT,PE,secure-boot
De uns anos para cá a maior parte das máquinas já possuem o que substitui o bios que é uefi ou efe tem diversas vantagens sobre a bios e eu vou citar algumas delas aqui em qual f conhece ela tem inteligência faz parte do software embutido.
conhecimento o entendimento de como funciona o sistema de particionamento gpt, também conhece o funcionamento do particionamento DOS, conhece o sistema de arquivos fat a bios não conhece qualquer sistema de ar que é ficou esse também o formato de arquivos. Tem recursos programas ali dentro para poder implementar o **secure boot** security é um recurso que faz com que a bios só carregue aquilo que foi assinado por certificados que estão gravados na máquina fazendo com que o sistema não boot qualquer coisa, é um mecanismo bastante interessante muita gente achou que isso era apenas para impedir que os gnu-linux funcionar sem o jogador mais no fim das contas isso acaba servindo de um recurso bastante interessante de configuração e de segurança para corporações, e para uso em desktops.


* Coreboot

* Libreboot

<h3>PARTICIONAMENTO</h3>

* dos/MBR -> 4 partições, 2TB
* GPT - Guid Partition table
* GUID - Globally Unique Identifiers
* 128 partições, 9ZB -> 1ZB 1 bilhão de Tb.

* MBR (512) bytes.
 * stage 1 
  * Primeiros 64 bytes (contém a tablea de partições DOS)
  * Pŕoximos 446 bytes (contém o bootloader) programa que da início ao carregamento do sistema operacional, os bootloaders mais moderno ainda usam um stagio após esse para somente depois começarem as partições.

 * stage 1,5
   

* GPT 
 * Possui Tabela GPT primária e secundária parao caso de algum problema ocorrer.

ESTRUTURA| 
|--------|
Protective MBR |
Primary GPT HEADER |
Entry1 Entry2 Entry 3 Entry4 |
Partition 1 |
Partition 2 |
Remaining Partitions |
Entry1 Entry2 Entry3 Entry4 |
Eentrries 5-18|
Secundary GPT Header |

* BOOTLOADER

GLUB | (Mais de um estágio)
|----|----------------
Estágios|
grub4.efi|
módulos| para poder carregar sistema de aruqivos, módulos de vídeo, criptografia, usar volumes lógicos 
/boot|

>Quando utilizamos a UEFI não precisa de vários estágios porque a UEFI é capaz de entender o sistema de arquivos, portando é capaz de pegar um glub todo, não sendo estágio por estágio e que constuma estar em um arquivo que no caso do **grub** se chama grub64.efi, e esse grub64 também contém os vários módulos necessários, como módulos de rede, sistema de arquivos etc.

* KERNEL
>O Kernel dentro da nossa estrutura de diretório, ele é um arquivo, apenas um arquivo **/vmlinuz/boot/vmlinuz-5.6.13-gnu5-> 15 MB**  esse **z** porquqe esse arquivo  [FHC](https://refspecs.linuxfoundation.org/fhs.shtml)
  * Wwol - Cadas programa de um jeito!
  * Unix like - "Cada coisa no lugar definido"fica compactado dentro da nossa estrutura de diretórios, localizaod dentro do /boot. Esse não tem tamanho fisico depedendo das complicações gerais, a menos que venha pdronizado da equipe do debian. mais nada impede de você compilar seu próprio kernel.

>Módulos | nem tudo que ta no código do kernel esta nesse arquivo do kernel, esta separado, em módulos, o que da um tamanho de 300 MB em média. O que acontece todos os dispositivos que o kernel tem suporte e que na compilação foram determinados para serem compilados, alguns são includos no próprio kernel que chamamos de **módulos BUITIN ou embutidos** e outros tantos foram separados. Então quando vocẽ instala um pacote de kernel, você vai instalar além do arquivo que é o kernel que é esse supramencionado acima, como também um conjunto enorme de módulos queq totalizam ai em torno de 300MB. E justamente por essa formulação modular voce leve o hd que instalou em uma maquina e levar para outra ele vai funcionar embora os hardwares sejam diferentes, lógico que desde que a arquitetura seja a mesma, porque o kernel em tempo de execução, quando ta rodando conversando com os barramentos descobre que hardware e módulo é necessário.


* /INITRD.IMG 
  * /boot/initrd.img-5.6.13-gnu

>É uma imagem de disco, ou melhor é uma imgem de disco, se olharmos dentro ele terá uma porção do  kernel, módulos que não estão no arquivo do kernel mais que são necessários para a sua maquina conseguir ter acesso ao dispositivo principal onde eta o sistema, ou seja seu HD, ele vai incluir esses módulos que são necessário para dar boot ao seu sistema. Quando você instala uma nova versão de kernel, um novo **INITRD.IMG** é montado com as coisas fundamentais que a sua máquina precisa ter acesso a raiz de iretorios e montar o \ , ou seja ter acesso a esturuta de diretório, possui um tamanho 8 a 20 MB porque também tem como a gente interferir como ele é criado. A exemplo posso criar um initrd.img em minha maquina, leva ro hd para outra e não funcionar, dar boot até que refaça o arquivo com os módulos necessários, o nome do arquivo também apresenta o nome da versão ao kernel, cada kernel tem um INITRD.IMG.



![Alt text](image.png)
>Estrutura do BOOT - inicialização


![Alt text](image-1.png)
>BIOS - Identificando os dispositivos

![Alt text](image-2.png)
>Menu GLUB 

![Alt text](image-3.png)
>Carregando o INITRD E KERNEL - papel do BOOTLOADER.

![Alt text](image-4.png)
>SCRIPT INIT DENTRO DO INITRD.IMG - CARREGANDO TODOS OS SERVIÇOS NECESSÁRIO PAR FUNCIONAR.

![Alt text](image-5.png)
>LOGIN

![Alt text](image-6.png)
>Caso tenha instalado um ambiente gráfico será chamado uma tela que chamamos sde gestor de login gráfico.

![Alt text](image-7.png)
>Ambiente DESKTOP, exemplo MATE, DE - Desktop Enviroment.


* PRINCIPIOS DO UNIX
 * HISTÓRICO
 >1956 | KEN THOPSON E DENNIS RITCHIE que começaram nos laboratŕios BELL LABS, MIT, AT&ET, Desenvolvendo um sistema chamado **MULTICS** algo como múltiplus, mais essa junção dessas empresa não deu muuito certo devido terem finalidade divergentes, interesses específicos.

>1969 | Começaram a desenvolver um sitema chamado UNICs -> Unix origem do 'X' do GNULINUX em assembry.

>1973 Começaram a desenvolver um sistema operacional chamado UNIX em uma linguagem chamada 'C'.

>KEN THOMPSON é o criador da linguagem B que mesmo apesar do tempo um dos criadores da linguagem GO.

>IDEIAS | Uma coisa, bem feira, trabrarem juntos, texto interface universal.


>MODELOS | KISS **Keep It Simple Stupid** ou seja mantenha isso simples.

>MODULAR | FLuxo da informação "fluxo -> pipe -> cano, exemplo " **programa1 | programa2** simples combinado  -> complexo.

>ABSTRAÇÃO | Representação arquivo árvore, som, impressora, FHS - Filesystem Hierarchy Standard, que começa no /.

<h3>FHS</h3>
>Filesystem Hierarchy Standart

* Origem 
>Originado Em 1996 quando começaram os trabalhos para definição desse PADRÃO juntamente com a comunidade do BSD - Univserisdade da Califórnia. Hoje é mantido pela LINUX FUNDATION.
 * fonte:
 [FHC](https://refspecs.linuxfoundation.org/fhs.shtml)
  * Wwol - Cadas programa de um jeito!
  * Unix like - "Cada coisa no lugar definido"

  ![Alt text](image-9.png)

DIRETÓRIO | Descrição
|--------|-----------
/ | Diretório Raiz
/proc| Processos
/dev |Dispositivos
/boot | Kernel, initrd
/bin | binários (E) Binários essenciais para o sistema
/sbin | binários (adm) Essenciais para adm do sistema 
/lib | bibliotecas - Biblioteca de funçãos que são usadas pelos programas que estão no /bin e /sbin
/etc | Configuração - Aqui todos os programs colocam seus arquivos de configs, para programas maiores com mais arquivos de config. tem um subdiretorio dentro do /etc
/media | Removíveis | monta conteúdo pendrives HDs etc..
/mnt | Temporários - MOntagem termporários de volumos 
/root |Usuário ROOT - Diretório particular do usuário root 
/home | usuários  - Perfil dos usuários 
/var | Variável - Conteúdo variável, ex. servidores web suas páginas ficam em um subdiretório nessa pasta, assim como dentro desse diretíro esta o /var/log que serve para monitorar mensagem de programas.
/tmp | Arquivos temporários - conteúdo desse diretório é apagado quando desliga o sistema 
/usr/bin | Binários de instalação de programas que não são os oficiais encontram -se nesse diretório
/usr/sbin | Binários de administração do sistema 
/usr/lib | Biblioteca que os programas contidos no /usr/bin e usr/sbin estão aqui nesse diretório.
/usr/share | Aquivos independente da arquitetura x86 x64
/usr/share/doc | Documentação dos programas dentro desse diretório, cada programa tem seu subdiretório.
/usr/share/man | Manuais dos programas. 

![Alt text](image-8.png)


<COMANDOS BÁSICOS - PROGRAMAS>

1.  cd (Alterar diretório)
2.  ls (Listar conteúdo de diretórios)
3.  mv (mover)
4.  mkdir (criar diretório)
5.  rmdir (remover diretório vazio)
6.  rm -rf (remover diretório não vazio)
7.  cat (imprime saída do conteúdo de arquivo de tela)
8.  less (paginador de tela e permite pesquisar temro usando o `/`)
9.  grep (filtrar termos)
10. find (localizar)
11. tail (ler ultimas linhas texto)
12. head (ler primeiras linhas de texto)
13. --help  (ajuda)
14. man (manual)
15. `|` (PIPE concatenar comandos)
16. `>` (direcionar saida)
17. `>>`  (acrescentar conteúdo ao final de um arquivo de texto)
18. cp (copiar arquivos/diretórios)

* [MATERIAL DE APOIO](https://www.inf.ufpr.br/cursos/ci055/artigos_linux_veteranos/gbbs14.pdf)
* [MATERIAL DE APOIO](https://cotidianoti.com.br/Comandos-basicos-linux/)



* Referẽncias
* Idéia Geral
* Princípais Diretórios
* Passeio

<h3>EDITORES DE TEXTO PURO</h3>

![Alt text](image-10.png)

* Plain text
  ```Forma de representação de codificação simplificada inicialmente por caractere - como ASCII (padrão americano para troca de informações) este usa 7 bits de 1 byte que possuem um tamanho de 8 para representar os caracteres - São representados nesse padrão o alfabeto como também alguns caractesrs especiais como ASPAS, CIFRÃO, PORCENTAGEM etc.. os primeiros 32 caracteres de 0 a 31  são caracteres de controle, a exemplo o BEEP, atualmente existe a necessidade muito mais representação de caracteres, porque possuem vários outros idiomas que adotam cacaractres que não estão inclusos dentre esses 128 disponíveis no ASCII. Com isso ele foi evoluindo e hoje existem vários outros padrões, ATUALMENTE o mais utilizado é o UTF-8 (também chamadao de UNICODE) que quer dizer Unicode Transformation Code, este usa também como unidade um BYTE, porém pode usar de 1 até 4 bytes para representar cada 1 dos caracteres e com isso a quantidade de caracteres podendo ser representada pelo UTF-8 passa de um muilhão de caracteres, uma observação é que o ele é também compatível com o padrão ASCII uma vez que este possui a mesma representação no UTF-8, em resumo os caracteres representados pelo ASCII também é representado no UTF-8```.

  * IMPORTÂNCIA
   
    * Linguagem de programação
    * Unix-like
    * Arquivos de configuração 
    * Ambiente gráfico 
    * pluma
    * gedit
    * geany
    * kate
    * kwrite 

  * USO EM TERMINAIS
    * Nano, joe
    * vim vim-basic
    * nvi -> 4.4BSD
    * mcedit -> Midnight Commander.  

* Conteúdo adicional
    * (https://pt.wikipedia.org/wiki/ASCII)
    * (https://pt.wikipedia.org/wiki/UTF-8)
    * (https://pt.wikipedia.org/wiki/Texto_simples)

* Repositório para estudo de funcionalidades
    * (https://github.com/calangohc/apresentacoes/tree/master/vim-magnun)

* Videos (palestra VIM)
    * (https://www.youtube.com/watch?v=UUzW46SeLhg)
    

* SOBREVIVÊNCIA NO "VIM"
 * [Material de apoio](https://github.com/calangohc/apresentacoes/tree/master/vim-magnun) 

 * MODOS
   * COMANDO
   * VISUAL
   * INSERÇÃO
   * LINHA DE COMANDO

<h3>PACOTES</h3> 



* ASCII e derivados
* Unicode -> utf-8 compatível 

<h3>SISTEMAS DE ARQUIVOS</h3>
<h4>Conceitos</h4>

* Código Fonte
* Compilador
* Biblioteca de desenvolvimento
* Conhecimento Técnico Específico

>PACOTE:
* Um pacote de software é a menor unidade que pode ser instalado que usa pacotes, um poacote pode contar um único programa. Algumas vezes funciona por si só, apenas pensas daquele programa. Em outras circunstãncias precisa de bibliotecas de funções que são usadas por vários programas, Logo um pacote pode ser compostos por várias dessas bibliotecas. Há pacores ainda de documentação, como dentre outros como :
  * Fontes
  * Icones 
  * Temas de ambiente
  * Dados
  * Textos

>Um pacote pode ter mais de um programa, e um programa para funcionar pode precisar de vários pacotes. Porém existem pacotes que podem precisar de dependências, o que são essas **DEPENDÊNCIAS** são outros pacotes que aquele pacote que eu quero instalar precisa para que ele funcione, ou seja um apcote precisa de outro pacote, para que seja instalao o pacote A precisa do pacote B que essa por é necessário para o funcionando do pacote B. Pacotes debian vem recomendando existalação de pacotes dependentes. 

![Alt text](image-11.png)

>O **PACOTE .DEB** nada mais é do que um arquivo compactadom em um formato chamado AR, esse arquivo compactado tem tudo que é preciso para isntalar aquele específico  pacote, como informações do que ele depende, as info daquele pacote vai fazer, quem fez, endereço do desenvolvedor, a imagem acima mostra a estrutura de uma pacote.

* \user
 * \bin (mostra os binários, ou seja esse pacote instala dois programas)

* \share (informação compartilhada do pacote)
 * \doc (Onde será alocada toda a documentação de todos os pacotes que são instalados)
 
* \man
 * \man1 (Manuais do programa - todo programa precisa ter uma página de manual seguindo assim a estrutura de empacotamento das disribuições)

* Pacotes também trazem scripts que são pequenos progarmas interpletados que poderão serem executados em alguns momentos no processo de instalação., antes da instalação, após remoção.

* A instalação de um pacote DEIAN não somente é a instalação de um pacote  e esse pacote realizar o que quizer em seu ambiente, esse pacote é interpletado, é aberto, é observado pelo gestor de pacotes e todas essas ações são praticadas, e existem registros dessas ações, logo é possivel saber quando um pacote foi instalado, se um pacote esta isntalado, usa-se no debian um sistema que consegue gerir, tomar contas dos pacotes para que possamos ter controle do nosso sistema de maneira completa. Um dos arquivos que vem no pacote é o **MD5SUMS** esse aruivo traz o hash da integridade dos arquivos. Cada arquivo que foi incluido por esse pacote é possível verificar mantém a integridade aos que foram produzidos. Podendo a qualquer tempo checar quanto a integridade dos mesmos.


<h3> BASE APT</h3>

* INSTALAR 
* REMOVER 
  * Remove tudo que o pacote instalou, mais mantém por uma questão de facilidade em alguns pacotes específicos, arquivos de configuração, caso instale um novo pacote ele não substituirá os arquivos ce configuração.

* ATUALIZAR LISTA 

  * Durante a instalação de um pacote , a ferramenta irá verificar existẽncia de dependência e caso precise irá instalar. Importante de tempos em tempos atualizar a base de pacotes disponíveis.

* ATUALIZAR SISTEMA 
  * compara a versão dos pacotes instalada com as versões diponíveis na lista para instalação, cabe isso ao gestor de pacotes realizar esse processo.

* PURGAR 
  * Remove o pacote instalado bem como suas ramificações, a exemplo: arquivos de configurações.

* ARQUIVO DEB

<h3>REPOSITÓRIOS</h3>

 * Onde estão localizados  o espelhos dos pacotes disponíveis na WEB., que funciona via HTTP, HTTPS, FTP.

 * /etc/apt/sources.list -  (espelhos de repositórios ficam espalhados pelo mundo inteiro)

 >Com essa estruruta de pacotes para quem não esta acostumado precisa verncer um paradígma de donwloade des progranas exe do universo windows, não há uma gestão de pacotes. Nos pacotes Linux tem seus próprios arquivos, não pode um arquivo estar em dois pacotes ao mesmo tempo, a forma de encontrar um programa é diferente, ou seja é priorizado a fonte oficial do sistema operacional.

<h3> GERENCIADORES DE PACOTE </h3>

 * DPKG 
 * APT  - 
 * APT-GET 
 * APTITUDE

 * SYNAPTIC
 * GNOME-SOFTWARE
 * DISCOVER
 * "LOJINHA" 

 <h3> BÁSICO </h3>
  
  COMANDO | Descrição
|--------|-----------
apt update | Consulta a lista de repostiórios que vocẽ configurou, acessa os sites e baixa a lista atualizada de todos os pacotes que estão lá disponíveis, bem como todas as informações de cada um desses pacote. 
apt search pesquisa | Pesquisa pacote com base em um parâmetro que deseja pesquisar, nome do pacote, bem como a funcionalidade do pacote, ex. `apt search pdf` irá pesquisar pacotes que manipulam arquivos PDFs.
apt install nome-pacote | Instalar um pacote
apt-remove | Desinstalar o pacote, se for um arquivo que possui arquivos de configuração separado, esses serão preservados. Para remover o pacote bem como seus arquivos de configurações, usamos o apt-purge e o nome do pacote `apt purge pacote`.
apt-upgrade | Serve para atualizar seu sistema, ele vai comparar na base do apt tudo que vocẽ tem instalado com toda  a lista de pacotes que esta disponíveis nos repostiórios, para saber se na lista de repositórios um pacote que vocẽ tem instalado e que possua uma versão mais recente.
apt full-upgrade | Atualização completa, porém caso tenha um pacote instalado que ja não faça mais parte na nova versão, esse será removido, em algumas circunstâncias será necessário remover alguns pacotes que vocẽ tem instalado.
dpkg -l | Lista todos os pacotes cum sua devida versão que temos instalado em nossa maquina.
dpkg -L | Saber arquivos que um pacote instalou em nossa maquina esse comando te retorna.

>Obs: cuidados ao remover pacote, observe as dependẽncias, na lista de pacotes  pode mostrar uma sigra `RC` que indica que o pacote foi removido porém seus arquivos de configuração permanecem.

<h3>SISTEMAS DE ARQUIVOS</h3>

* ABSTRAÇÕES
* ANALOGIA
* CONCEITOS
* TIPOS
* CARACTERISTICAS.

>Imagine que você tenha um terreno, e precisa alocar um espaço nesse terreno para um estacionamento, de forma rotativo, na análoogia esse terreno se chama partição. Porém esse terreno precisa ser preparado para ser usado como estacionamento, como faixas, Organização de como os carros vão entrar e sair, Cabine, nessa cabine precisara ter uma pessoa para realizar o controle de entradas e saidas, bem como o registro. Toda essa analogia e a de criar um sistema de arquivos, ou seja, o procedimento conhecido como formatar, eu formato uma partição, preparo um terreno para ser um estacionamento.Nessa história toda os carreos serão armazenados usando um método, como será o procedimento de quando chegar o carro, quando alguém for resgatar o carro. Nesse método também ta envolvido na cabine uma planchetinha que armazena informações características daquele carro, não o próprio carro mais as informações relativas ao carro, Placa, Vaga que ele foi colocado, nome do motorista, a hora que se deu o estacionamento. Tudo isso devidamente registrado na planchetinha, na nossa análogia isso é chamado a **ÁREA DE CONTROLE** veja que os carros são alocados de forma que um não atrapalhe o outro, é possível que você remova um carro mesmo que ele tenha chegado antes. Da mesma forma que os veículos são alocados em um armazenamento, os dados são alocado no **SISTEMA DE ARQUIVOS** , agora imagine que você tem o outro terreno com a mesma metragem quadrada e você quer nele também montar um estacioanmento, porém agora é um outro tipo de estacionamento, nesse este será perto de um porto de passageiros, esse tipo agora de estacionamento a forma de fazer as coisas é diferente porque os passageios vão chegando com seus carros e vão estacionando, mais eles não precisam tira os veículos do local por um tempo de 15, 20 ou 1 HR., vão todo mundo para o navio, realizam seu cruzeiro e na volta que removerão seus veículos. Observe que nesse estacionamento como no outro também foram pintadas faixas, cabine e se pensou na organização, como como esses veículos serã retiradas. Também nessa cabine desse estacionamento teremos uma planchetinha que descreve o metodo como esses carros vão ser armazendos contendo algumas informações específicas desse jeito guardar o carro. Além dessa plancheta também precisamos de uma pessoa que dentro da nossa analogia é um módulo do **KERNEL** o Kernel precisa conhecer, e ele conhece isso através de um módulo como é que as infomrações são armazenadas. O estacionamento funcionou, os carreos foram guardados e podem também serem recuperados, mais veja na ultima imagem tem um carro atrás de outro., notem que ambos os estacionamento são iguais, note que os estacionamento possuem o mesmo tamanho, a mesma dimensão. Mais em virutde da finalidade que cada estacionamento possui, a maneira de armazenar os carros é diferente o que provoca várias coisas, por exemplo, um estacionaemnto o processo de tirar um carro e colocar um carro é muito fácil, ja no segundo é muito mais dificil isso, caso precise tirar um carro que esteja em local onde outros carros estão obstruindo, será necessário remover outros carros para somente poder remover o carro específico que deseja. De maneira que é possível determinar que cada sistema pos,ui suas características adequada de alocação, pode ser adequada a uma finalidade mais não a outra, cada sistema de arquivo é melhor em uma ou outra condição, não existe um sistema de arquivo melhor em todos os sentidos. um conceito importante e que temos uma estrutura ai que são camadas de abstração.![Alt text](image-16.png) Os programas desconhecem essa história, isso é apenas na abstração que o KERNEL engrega para eles. E nós u suários entendemos que os arquivos estão em diretórios, mais nós ja sabemos que os arquivos são armazenados em uma partição usando um sitema de arquivos e o kernel tem acesso a isso porque ele possui um módulo que conhece o método, a forma com que as informações são armazenadas naquele disco, mais para o kernel te entragar essa abstração ele precisa de módulos de onde  o disco esta conectado, em que barramento, um módulo especifico, cada disco pode ter um módulo ou driver diferente para funcionar. É importante saber que um disco não tem diretório, não tem arquivo. O disco tem blocos de 1 e 0, então o disco não entende essa estrutura, a estrutura é uma abstração que esses módulos e o kernel promovem para ficar fácil para os aplicativos e para nós.![Alt text](image-17.png) 
Existse um conceito chamado **INODE** que é um espaço na planchetinha localizada na área de dados que contém infomrações chamadas  **METADADOS** sobre um arquivo ou sobre um diretório. Os **METADADOS** são informações relativas ao arquivo mais não o dado em si, são informações como:
* tempo
* permissões
* tipo 
* tamanho
* endereço  500 - (endereço de um bloco da área onde esses dados do arquivo efetivamente estão) nesse caso o endreço do bloco que contém os dados desse arquivo é o 500.

> Cada **INODE** possui um número único no sistema da arquivos.

>Quando **APAGAMOS** uma informação no disco, e limpado apenas a  informação de dados do bloco  que contém os dados do diretório/arquivo, ou seja do ponto de vista de nossa abstração esse dado foi removido, porém os dados em si permanecem, os dados não são nem tocados, os blocos do disco que são utilizados para armazenar esses dados não são nem mexidos, por isso é possível recuperar com aplicativos específicos e não mais acessando a pasta ou local onde estavão. Apenas não sera mostrados/visualizados mais com comandos de listagem, nem abrindo o local onde antes mostra as informações do arquivo. Com isso o **INODE** é liberado e novamente pode receber outras informações. 

>Quando **FORMATAMOS UMA PARTIÇÃO** com os dados não acontecem nada, apenas é limpa a ára de controle, portando as informações dos **INODES** serão todas sobrescritas, mais os dodos mesmo dos arquivos vão continuar lá. Poratndo quando formatamos um disco para passar para frente ou descartar, outra pessoa pode usar uma ferramenta de rcuperação para recuperar esses dados, uma ferramenta linux muito boa para esse tipo de operação é o **FOREMOST**.

>Quando **MOVEMOS** um arquivo na realidade estamos apagando a infomação do arquivo que ele faz parte e copiando essas informaçõe para dos dados de outro diretório que agora ele fará parte. o **INODE** do arquivo não vai ser troacado, de modo que não havera necessidade de transferir uma grande quantidade de dados.

![Alt text](image-12.png)

![Alt text](image-14.png)

![Alt text](image-15.png)


<h3>SISTEMA DE ARQUIVOS</h3>

![Alt text](image-18.png)

<h3>JOURNALING</h3>
<h4>Ordem de escrever - escrita efetiva</h4>


>Toda vez que o kernel recebe uma ordem para escrever dados no dispositivo de armazenamento, ele anota nessa área chamaa Jornaling que recebeu essa ordem. Quando efetivamente o kernel trocando informações com o disco grava essa informação no disco ele volta la no Jornaling e reaiza um ticket , ou seja uma marcação informando que o arquivo foi gravado. Os sistemas de arquivos que usam esse recurso de Jornaling, eles são mais facilmente recuperados a sua integridade quando por exemplo uma interrupção de energia elétrica, porque quando o sistema volta no Jornaling e verifica algumas operações que não foram tikadas, logo ele sabe os INODES que ficaram sem ou com altearções. em um sitesmta de arquivo que **NÃO TEM JORNALING** no caso o EXT2, esse processo é muito mais demorado porque o programa tem que scanear todos os inodes disponíveis para ver quais estão e quais não estão sendo utilizados, para que a gente tenha certeza que tudo aquilo que a gente ordenou foi efetivamente escrito, a gente costuma usando um programa usado chamado **SYNC** o SYNC diz ao kernel para o kernel acelerar o processo, e ele vai dar essa ordem para escrever efetivamenete tudo que tem que estar  escrito e só é liberado quando tudo estiver devimdamente inscrito. podemos comparar esse procedimento quando recebemos a msg **VOCÊ JA PODE REMOVER SEU PENDRIVE COM SEGURANÇA** todas as ordens para escrever uma informação ou para apagar ja foram realizadas, ou seja o sistema de arquivos está todo integro.

<h4>FRAGMENTAÇÃO (SKLACKSPACE)</h4>
![Alt text](image-19.png)

1 - Ocupa dois blocos e o pedacinho de um terceiro, o resto não pode ser usado, não tem como ser indexado. (essa parte que não pode ser usado é chamada dE **slackspace**) assim segue os mesmos arquivos em cores divergentes.
2 - Algum programa mandou apgar-se o arquivo amarelo, ou seja esses blocos estão disponíveis para uso, porém os dados que estavam ali vão continuar da mesma forma sem mexer. 
3 - Algum momento um outro pograma manou gravar o arquivo azul, esse precisava de 3 blocos e mais ou um outro tanto de espaço, ele irá grava no espaço que antes estava utilizado o bloco amarelo que foi apagado,o restante do arquivo azul terá que ser gravado em blocos disponiveis mais a frente.  Ou seja não foi gravdo de forma contígua, ficando assim o arquivo azul fragmentado, espalhados pelo disco.

>Sistemas como FAT e NTFS são mais sucetiveis a fragmentações, ja em sistemas Linux em sisteams EXT4 é bastante insignificante o nível de fragmentação, embora também exista aplicativo para realizar tal procedimento em linux.

<h4>TIPOS DE SISTEMA DE ARQUIVOS</h4>

SITEMA DE ARQUIVOS | Descrição
|--------|-----------
ext2 | usado muito tempo, não possui Jornaling 
ext e ext4 | Sistemas mais atuais e que possuem sistema de Jornaling
squartfs, ISO-9660 | São sistemas de arquivos para apenas leitura, ou seja quando cria os dados ja estão todos alí, não tem como editar arquivos, remover ou inserir sobrescrevendo assim os que ali ja existem, são sistemas de arquivos para apenas READ ONLY(somente leitura)
xfs,jfs,zfs,btrfs | Tipos mais usados em unixlike, GNULinux
FAT-32,NTFS, ExtFat(pendrivers atuais)  | Sistema de arquivos 
windows.
hfs | Sistemas MAC-OS.

* Visualizando sistema de arquivos.
 * `cat/proc/fileystems`

 * MATERIAL DE APOIO 

  * [Inode](https://pt.wikipedia.org/wiki/N%C3%B3-i)
  * [Jornaling](https://pt.wikipedia.org/wiki/Journaling)
  * [Comparação de sistemas de arquivos](https://en.wikipedia.org/wiki/Comparison_of_file_systems)
  * [Ext4](https://pt.wikipedia.org/wiki/Ext4)
  * [ReiserFS](https://pt.wikipedia.org/wiki/ReiserFS)
  * [Btrfs](https://pt.wikipedia.org/wiki/Btrfs)
  * [NTFS](https://pt.wikipedia.org/wiki/NTFS)

<h4>TRABALHANDO COM SISTEMA DE ARQUIVOS</h4>

 * Dipositivos 
  * A representação dos dispositivos pousseum diretório específico para eles que é o `/dev` então os discos ou dispoistivos de armazenamento recebe o nome de um arquivo que vai permitir o acesso direto a esse dispositivo como na imagem abaixo:

  ![Alt text](image-20.png)

  >Em maquinas virtuais os dispostivivos recebem o nome de `VD``VDA - Primeiro disco virtual` `VDB-segundo disco virtual`, sequencilalmente nomeando suas partições como `VDA1-primeira partição do primeiro disco` `VDAB-Segunda partipção do primeiro disco` seguindo logiamente para partições quantas tiverem, assim como de mesma forma os discos `VDB1 - Primeira partição do disco B` `VDB2 - segunda partição do disco B.`

  >Em tercnologias mais modernas que usam o discos NVME o arranjo ficará da seguinte forma: `/dev/nvme0n1p1 - Primeira parte  nvme0n1 - Disco, p1 - Partição`

 * Particionamento 
  
  ![Alt text](image-21.png)

  >São partes de um disco, no GNU podemos usar mais de um formato de particionamento, o mais habitual é o particionamento GPT e o particonamento DOS/MBR. Nos sistemas GPT podemos alocar até 128 partições, ja em partições DOS/MBR como mostra a figura só poderemos usar 4 partições primárias, ou criar uma partição extenida para inserção de mais partições lógicas.

 * Criar FS
  * "FORMATAR" Preparar área de controle.

  `mkfs.ext4, mkfs.vfat,mkfs.reiserfs`

  * "VERIFICAR"  integridades dos arquivos
   * `fsck`
   * `win hybernation`
   * "**embromation** - quando desligamos maquinas com sistemas windows, na realidade ele não desliga totalmente o equipamento, deixa a maquina em um estado intermediário(Hibernado) e caso vocẽ tente escrever nessa partição a partir de um sistema linux estando em dual boot você vai receber uma msg informando que a unidade esta hibernada. "

 * MONTAR MÍDIAS 

 ![Alt text](image-22.png)
 
 * Espaço
>Espaço usado em tese seria a soma to total de arquivos gravados no disco, porém lembre-se que nem sempre um arquivo ocupa totalmente um bloco, o que irá gerar uma sobra, sendo aqui um pouco menor do espaço ocupado, é o espaço que não pode ser usado. **AINDA ASSIM** existe um espaço reservado para o usuário **ROOT**.

 ![Alt text](image-23.png)

 <h4>MÃO DA MASSA - CRIANO E APAGANDO PARTIÇÕES</h4>

  * **FDISK**

  >OBS o "*" apontado na partição do disco informa ao BOOT LOADER que ali possui um boot, ou seja ela é inicializável.

>NOTA - Para listar as partitions que o kernel entendeu pode usar o comando `cat /proc/partitions`.

 COMANDO | Descrição
|--------|-----------
`fdisk /dev/sda` | diz ao FDISK para trabalhar com o dispositivo diretamente
`m` | Mostra menu de ajuda 
`d` | Exclui uma partição, será mostrada numero de partições pedindo para selecionar a partição desejada para apagar, por padrão se apenas pressionar ENTER aparára a última.
`w` | Grava as alterações, lembre-se caso saia do sistema sem antes confirmar as alterações com pressionando `w` nada será aplicado.
`g` | Cria uma nova tabela de partições  GPT
`n` | Cria uma nova partição
`t` | Escolhe tipo de sistema de arquivo a ser utilizada 
`ls /dev/sda*` | Para visualizar a lista de partições disponíveis 

>Na dúvida digite `q` para sair da tela.

>NOTA - Note que com a listagem longa observer que na primeira coluna começa com "b" o quer dizer que trata-se de um dispositivo de blooco, ou seja que o dispositivo envia informações em bloco, trata-se de um arquivo que representa um dispositivo.

 * **FORMATAR** - CRIAR O SITEMA DE ARQUIVOS
 
 COMANDO | Descrição
|--------|-----------
`mkfs.ext4` | Realiza a formatação da partição.

>NOTA - caso queira saber em algum momento o que é acessado quando digitamos um comando, para usar o comando `which mkfs.txt4`, mostra onde esta o binário.

>NOTA - Durante a formatação é preparada a área de controle sobre os blocos que estão sendo usados , os que não esta sendo usados, e o backup desses blocos, quando a gente formata também e criado para aquela partição um **ID** para que quando em um futuro assim desejar, quisera montar automaticamente essa unidade, isso poderár ser feito.

 * Formatado no nosso caso o **SDA2** vamos formatar a primeira partição com o comando:
  * `mkfs.vfat /dev/sda1`  - Após isso é preciso montar para somente assim puder escrver informações nessa unidade/partição criada/formatada, para isso usamos o comando abaixo:
  * `mount /dev/sda2 /mnt` - Aqui passamos o comando **mount** o **caminho do dispositivo** e **ponto de montagem**, ou seja o diretório que essa partição será montada.
 * Demontando partição montada
  * `sudo umount /mnt`

>NOTA - para visualizar lista de dispositivos de armazamento, bem como seus espaços utilizados e disponíveis use os comandos `df -h` ou `df -H`.

* VERIFICAR A **INTEGRIDADE** DO DISCO

>NOTA: Integridade é verificar se toda a estrutura da área de controle esta ok, se foi montada adequadamente ou desmontada, toda vez que montamos uma partição é escrito lá que ela foi montada 
e quando desmontamos essa alteração e realizada para o status de desmontada.

 * `fsck.ext4 /dev/sda`

* OBTER INFORMAÇÕES SOBRE O SISTEMA DE ARQUIVOS
   
 * `dumpe2fs /dev/sda2` - Traz informações soebre o sistema de arquivos, são várias, uma bacana de ver é a última vez que foi checado, numero de INODES, DATA DE CRIAÇÃO DO SISTEMA DE ARQUIVOS, ULTIMA VEZ QUE ELE FOI MONTADO, ULTIMA VEZ QUE ELE FOI ESCRITO. 

* OBTER O QUE ESTA TOMANDO MAIS ESPAÇO EM SEU DISCO
 * `du -shc /*`  - Soma a quantidade de epaço usado em todos os diretórios, pode ser usado com a variação **SORT** para ordenar, pode também especificar um diretório  unico como:
 * `du -shc /home/*`

* MATERIAL DE APOIO 

  * [Hibernação do Windows](https://learn.microsoft.com/pt-BR/troubleshoot/windows-client/deployment/disable-and-re-enable-hibernation)

  * Removendo os metadados nts
    `mount -t ntfs-3g -o remove_hiberfile /dev/your_NTFS_partition /mount/point`

<h3>AULA-18 | RESOLVER PROBLEMAS COM LOGs</h3>


* LOGS
 * Mensagens recebidas e registradas no sistema que permite um adminsitrador realizar autitorias acerca de programas  ou do sistema, de modo que ajude na manutenção e correção de problemas no SO.


 * Localização dos arquivos de LOGs
    `/var/log`

>NOTA: Alguns programas mantém um local próprio seu para manutenção dos seus logs, é o caso do `APT` mantendo um registrode todas a instalação/atualizações que foram feitas encontam-se sem seu repositorio de log.

* SERVIDOR DE LOGS
 * Possui um programa startado que não possui iteração com o usuário, este fica aguardando uma conexão de rede que vai enviar msgs. No caso os programas, sejam eles tanto locais que rodam na mesma maquina que esse serviço esta em execução, como de outras máquinas ligadas a esse servidor ligados através da rede. A exmplo podemos ter uma maquina servidora de de logs que reunia nela todos logs de um servidor de rede, em resumo o servidor de logs é um servido que recebe mensagens e registro isso de modo que possa ser em determinado momento que seja necessário visualizar esse logs, para fins principalmente de solução de problema.
    * Sevidores de LOGs disponíveis 
      * Mensagens locais 
      * rede 
        * syslog
        * rsyslog
        * syslog-ng 

* CONCEITOS

![Alt text](image-24.png)
 * Quadro acima destaca as principais categorias de mensagens recebidas dos logs, bem como seu nível crítico ou não de importância.


* PRINCIPAIS ARQUIVOS 

 Localização | Descrição
|------------|-----------
`/var/log/syslog` | este mantém a maior concentração de logs do sistema.
`/var/log/auth.log` | Mantém mensagens de login no sistema, informações de sucesso, falha e tentativas de login.
`/var/kernel.log` | Mantém mensagens relativas ao kernel do sistema, a exemplo um módulo do sistema que é carregado.

* ENTENDENDO AS MENSAGENS 

 * Estrutura de uma mensagem de log
  `horário:máquina:programa[processo]<criticidade>:mensagem`

* LOGGER
 * Trata-se de um programa separado que serve para você usar em script, que através deste você pode encaminhar mensagems para os servidor de LOGs, de modo simples se você digitar o comando `logger e a mensagem` essa mensagem vai aparecer no servidor de log.
 ![Alt text](image-25.png)

* RTFL
 * Read The Fantastic Logs

* USANDO O `grep` e `tail -f` PARA LEITURA DOS ARQUIVOS DE LOGs

 Comandos | Descrição
|------------|-----------
`ls -lrsh` | lista os arquivos de log de forma l=longa, r=recusivo, s=Tamanho, h=Humano
`du -shc /var/log/*` | Visualiza o tamanho total dos arquivos de  log está ocupando na máquina.
`tail -f /var/log/syslog` | Lista em tempo reais captura de logs do sistema, usando o parâmetro -f=folowing, verá que sera mostrado em tempo real as informaçoes do dispositivo inserido na pora usb, como nome do dispositivos, porta usb conectada dentre outras informações, isso graças a um processo chamado **UDEV**, nesse momento um módulo do kernel caso precise, também será iniciado para ser possível o uso desse dispositivo.
`tail-f auth.log` | Mostra as informações/mensagens, no caso o servidor de logs separa categorias de mensagens em arquivos, no caso aqui será mostrado as mensagens de autenticação, opodendo assim acompanhar os logins realizados e out tentativas de login com sucesso e falha no sistema.
`tail /var/log/kernel.log` | Exeibe ultimas 10 linhas de logs do kernel,
`grep fail /var/log/syslog` | Exibe falhas ao carregar módulos

 >FONTES:
 [Rsyslog](https://www.rsyslog.com)
 [Rsyslog - Wikipedia](https://en.wikipedia.org/wiki/Rsyslog)
 [Manual do Administrador](https://debian-handbook.info/browse/pt-BR/stable/sect.syslog.html)


<h3>TUTORIAIS</h3>
<h4>RECUPERAR A SENHA DO ROOT</h2>

 * REDEFINIR SENHA
   * Boot grub
     * Reboot a maquina até a tela do GRUB.
     * Na linha de carregamento do sistema **PRESSIONE A LETRA E NO TECLADO**
     
     ![Alt text](image-27.png)
     
     * Tela de edição, após presisonar letra E na linha de carreagmento do Sistema Operacional 
     
   * Alterar  linha linux 
     * Na linha que possui o **LINUX** navegue nela até onde aparece  o termo **ro quiet** e apage a palavra quiet e altere **ro** por **rw** e acresente os termos após rw **init=/bin/bash** como na imagem abaixo, esse procedimento altera o padrão de carregamento do linux para apenas carregar o intepretador de comandos, carreagndo dessa forma ja teremos o root sem precisar de senha, ai podemos a partir disso gera uma nova senha. feito as alterações, pressione **F10** para o grub bootar novamente .

     ![Alt text](image-28.png)

   * Definir senha 
    * Após carregar o sistema caindo no interpretador de comandos use o comando **passwd** e crie uma nova senha e pronto.

    * Reboot 
     * Reinicie a maquina dessa vez pressionando enter na linha de carregamento  do grub que corresponde ao Sistema Operacional. 
     !!FINALIZADO!!







 