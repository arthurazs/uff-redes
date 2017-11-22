# Endereçamento e Encaminhamento (Camada de Rede)

Por **[Arthur Zopellaro](https://github.com/arthurazs)** e **Tatiane Sousa.**

**Conteúdo**

- [Objetivo Geral](#objetivo-geral)
- [Objetivo Específico](#objetivo-específico)
- [Metodologia](#metodologia)
    - [Instalação](#instalação)
    - [Experimento 1](#experimento-1)
    - [Experimento 2](#experimento-2)
- [Ambiente](#ambiente)
- [Roteiro](#roteiro)
    - [Instalação e Configuração](#instalação-e-configuração)
        - [Instalação do Wireshark](#instalação-do-wireshark)
        - [Pré-requisitos do Netkit](#pré-requisitos-do-netkit)
        - [Instalação do Netkit](#instalação-do-netkit)
    - [Experimento 1 *(HUB)*](#experimento-1-hub)
    - [Experimento 2 *(NAT)*](#experimento-2-nat)
- [Considerações Finais](#considerações-finais)

## Objetivo Geral

Este laboratório tem por objetivo aplicar os conceitos de Endereçamento e Encaminhamento da camada de Rede do modelo TCP/IP vistos em aula.

## Objetivo Específico

O laboratório irá demonstrar a criação de redes de computadores e a comunicação entre os nós de redes, exemplificando a utilização de: Endereço IP, Máscara sub-rede, Gateway e Rota Default, CIDR, NAT e ICMP. O tráfego gerado na simulação das redes serão analisados.

## Metodologia

O laboratório será dividido em 3 partes.

### Instalação

Primeiramente haverá um tutorial para a instalação da ferramenta **Netkit**, programa utilizado para simulação de redes, e o **Wireshark**, programa utilizado para analisar o tráfego na rede.

### Experimento 1

Em seguida, será utilizado um cenário simples com 4 máquinas interligadas por um hub, havendo comunicação apenas entre 2 dessas máquinas. Os alunos irão discutir o motivo das duas outras máquinas não estarem se comunicando com as demais. A turma irá acompanhar um passo a passo utilizando comandos reais de Linux para a configuração do **Endereço IP** e **Máscara de sub-rede** com o objetivo de conectar as outras duas máquinas na rede. O programa **Wireshark** será utilizado para analisar datagramas IP que serão enviados entre as máquinas utilizando o protocolo **ICMP** (ping).

### Experimento 2

Para finalizar, utilizaremos um exemplo mais complexo que irá introduzir o conceito de **NAT**, onde os alunos irão configurar a conexão entre uma rede local e a internet. O programa ***Wireshark*** será utilizado em diversas etapas deste experimento para auxiliar na detecção dos problemas envolvidos na comunicação entre uma máquina com IP privado (rede local) e uma máquina com IP público (internet). Serão analisados as mensagens enviados entre as máquinas utilizando os protocolos **ICMP** de **ping** e **traceroute**.

## Ambiente

O laboratório será executado em um Sistema Operacional **Linux**, utilizando o programa **Netkit** para emular a criação de redes de computadores e o programa **Wireshark** para análise do tráfego na rede.

## Roteiro

Antes de iniciar as simulações, devemos instalar e configurar as ferramentas necessárias.

### Instalação e Configuração

Nosso ambiente de simulação Linux irá utilizar as ferramentas **Wireshark** e **Netkit**.

#### Instalação do Wireshark

Abra o terminal e execute o seguinte comando para instalar o **Wireshark**:

` $ sudo apt-get install wireshark `

#### Pré-requisitos do Netkit

Primeiramente, acesse a página de downloads do [Netkit](http://wiki.netkit.org/index.php/Download_Official) e baixe os três arquivos em *Latest Stable Release*:

- [netkit-2.8.tar.bz2](http://wiki.netkit.org/download/netkit/netkit-2.8.tar.bz2)
- [netkit-filesystem-i386-F5.2.tar.bz2](http://wiki.netkit.org/download/netkit-filesystem/netkit-filesystem-i386-F5.2.tar.bz2)
- [netkit-kernel-i386-K2.8.tar.bz2](http://wiki.netkit.org/download/netkit-kernel/netkit-kernel-i386-K2.8.tar.bz2)

O netkit foi desenvolvido baseado na arquitetura 32-bit, portanto se seu ambiente Linux for baseado em 64-bit, você deverá instalar algumas bibliotecas de conversão. Para verificar seu ambiente Linux, execute o comando a seguir em seu terminal:

    $ uname -i
    x86_64

Se o resultado for **i386**, você pode pular para a [Instalação do Netkit](#instalação-do-netkit). Caso o resultado seja **x86_64**, você deverá executar os seguintes comandos em seu terminal:

    $ sudo apt-get update
    $ sudo apt install lib32readline6
    $ sudo apt-get install libc6-i386

#### Instalação do Netkit

Após obter os arquivos necessários do Netkit, mova os arquivos `*.tar.bz2` para a pasta `/home/usuario/`, lembrando de substituir *usuario* pelo seu nome de usuário.

Feito isso, abra o terminal e vá para sua *home* com o comando `cd` e siga os demais comandos conforme mostrado abaixo:

    $ cd
    $ tar -xjSf netkit-2.8.tar.bz2
    $ tar -xjSf netkit-filesystem-i386-F5.2.tar.bz2
    $ tar -xjSf netkit-kernel-i386-K2.8.tar.bz2

Após a descompactação dos arquivos, uma pasta *netkit* será criada em sua *home*. Agora você precisa indicar o local da instalação do *netkit* para o Linux:

    $ export NETKIT_HOME=~/netkit
    $ export MANPATH=:$NETKIT_HOME/man
    $ export PATH=$NETKIT_HOME/bin:$PATH
    $ . $NETKIT_HOME/bin/netkit_bash_completion

Você deverá repetir os comandos acima toda vez que abrir um novo terminal. Para evitar esse problema, adicione as quatro linhas mostradas anteriormente (sem o $) no final do arquivo `/home/usuario/.bashrc`.

Para verificar se sua instalação foi bem sucedida, execute:

    $ cd $NETKIT_HOME
    $ sh check_configuration.sh

###  Experimento 1 *(HUB)*

Este é um cenário simples com 4 máquinas interligadas por um hub, havendo comunicação apenas entre 2 dessas máquinas. O objetivo deste experimento é ambientá-lo com as ferramentas de simulação e de análise do tráfego da rede.

**IMAGEM**

 1. Crie uma pasta em sua `/home/usuario/` com o nome **laboratorio**.
 2. Baixe o [experimento1.zip] e descompacte o arquivo dentro da pasta `/home/usuario/laboratorio`.
 3. No terminal, entre na pasta **experimento1**:

        $ cd
        $ cd laboratorio/experimento1/
        $ ls
        lab.conf  MAQUINA1.startup  MAQUINA3          MAQUINA4.startup
        lab.dep   MAQUINA2          MAQUINA3.startup
        MAQUINA1  MAQUINA2.startup  MAQUINA4

    O arquivo **lab.conf** contém informações gerais sobre a simulação, enquanto cada pasta **MAQUINAX** contém informações sobre a interface de rede. Os arquivos **MAQUINAX.startup** reconfigura a interface de rede da máquina para receber as configurações atualizadas. O arquivo **lab.dep** permite a inicialização paralela das máquinas para agilizar o processo.

 4. Inicie o experimento com o comando `lstart`:

        $ lstart
        ======================== Starting lab ===========================
        Lab directory: /home/arthurazs/laboratorio/experimento1
        Version:       1.0
        Author:        Arthur Zopellaro, Tatiane Sousa
        Email:         arthurazs@id.uff.br, tatiane.gsousa@gmail.com
        Web:           https://arthurazs.github.io/uff-redes
        Description:
        Experimento 1 - HUB
        =================================================================
        You chose to use parallel startup.
        Starting "MAQUINA1"...
        Starting "MAQUINA2"...
        Starting "MAQUINA4"...
        Starting "MAQUINA3"...

        The lab has been started.
        =================================================================

 5. Quatro janelas serão abertas conforme a imagem abaixo:

    ![Quatro janelas XTerm abertas](/imagens/experimento1a.png)

 6. Nós iremos coletar informações da interface das Máquinas 2 e 3. Para salvar os arquivos em sua máquina física, execute o comando a seguir nas máquinas 2 e 3:

    `# cd /hosthome/laboratorio/experimento1/`

    ![Trocando o diretório na MAQUINA2](/imagens/experimento1b.png)

 7. Execute o comando `ifconfig` nas quatro máquinas virtuais.

    ![ifconfig sendo executado na MAQUINA1](/imagens/experimento1c.png)

 8. Execute um `ping` da MAQUINA1 para todas as outras máquinas (incluindo a própria MAQUINA1). O comando é `ping -c 3 192.168.0.XXX`, não se esqueça de trocar **XXX** pelo **Endereço IP** de cada máquina.

    ![ping sendo executado da MAQUINA1 para a MAQUINA2](/imagens/experimento1d.png)

 9. A MAQUINA1 consegue enviar para 2 e 3 mas não recebe resposta da 3. A MAQUINA1 não consegue enviar para a 4.

    ![Resultado do ping executado da MAQUINA1 para todas as máquinas](/imagens/experimento1e.png)

 10. Para ajudar na detecção do problema, coloque a MAQUINA2 e 3 para coletar informações sobre o tráfego de mensagens que estejam passando por suas interfaces de rede. Execute o comando abaixo, substituindo o X pelo número da máquina:

        # tcpdump -i eth0 -w capturaAX.pcap
    ![tcpdump rodando nas máquinas 2 e 3](/imagens/experimento1f.png)

 11. Faça o passo 8 novamente (execute um `ping -c 3` da MAQUINA1 para todas as máquinas, incluindo para a MAQUINA1).

 12. Após finalizar o passo 11, vá nas máquinas 2 e 3 e aperte `Ctrl + C` para finalizar a captura de tráfego.

    ![Finalizado o tcpdump na máquina 2](/imagens/experimento1g.png)

 13. Em sua máquina local, clique duas vezes nos arquivos gerados com extensão `*.pcap`. Analise o tráfego de informação pelo **Wireshark**, reveja o **Endereço IP** e a **Máscara sub-rede** das quatro máquinas e tente descobrir o motivo da falha na comunicação das máquinas 3 e 4.

    ![Wireshark analisando coleta de tráfego da máquina 2](/imagens/experimento1h.png)

    **DICA** Qual a diferença no erro de comunicação da máquina 3 e da máquina 4?

 14. Após descobrir o problema, atualize o **Endereço IP** e a **Máscara sub-rede** de forma adequada nas máquinas sem conexão. Siga o comando abaixo, lembrando de modificar **XXX** para o Endereço IP correto e **YYY** para a Máscara sub-rede correta:

        # ifconfig eth0 192.168.0.XXX netmask 255.255.255.YYY up

 15. Repita o passo 8 novamente. Se as mudanças corretas forem feitas, a MAQUINA1 receberá resposta de **ping** de todas as máquinas. Sinta-se a vontade para testar o **ping** partindo das outras máquinas também.

 16. Para encerrar a simulação, digite `lhalt` em seu terminal **local**:

    ![Finalizando a simulação](/imagens/experimento1i.png)

 17. *OPCIONAL* Caso o comando `lhalt` não funcione, utilize o comando `lcrash -F`:

    ![Finalizando a simulação com lcrash -F](/imagens/experimento1j.png)

 18. *NOTA* O netkit mantém o estado das máquinas virtuais nos arquivos **MAQUINAX.disk**, possibilitando a continuação da simulação em outro momento. Estes arquivos podem ser extremamente grandes por conflito entre as arquiteturas 32-bit e 64-bit.

    ![Tamanho do arquivo MAQUINA1.disk](/imagens/experimento1k.png)

    Caso tenha concluído a simulação e não deseje manter o estado das máquinas, execute o comando `lclean`:

    ![Removendo os arquivos *.disk](/imagens/experimento1l.png)

###  Experimento 2 *(NAT)*

## Considerações Finais

**Dúvidas?**

Arquivos de configuração em [github.com/arthurazs/uff-redes](https://github.com/arthurazs/uff-redes).
