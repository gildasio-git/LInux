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

