# Endereçamento e Encaminhamento (Camada de Rede)

Por **[Arthur Zopellaro](https://github.com/arthurazs)** e **[Tatiane Sousa](http://lattes.cnpq.br/5392435833953521).**

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
        - [Executando a simulação](#executando-a-simulação)
        - [Testando a rede](#testando-a-rede)
        - [Capturando o tráfego](#capturando-o-tráfego)
        - [Detectando o erro](#detectando-o-erro)
        - [Finalizando a simulação](#finalizando-a-simulação)
    - [Experimento 2 *(NAT)*](#experimento-2-nat)
        - [Testando a rede](#testando-a-rede2)
        - [Solucionando o problema](#solucionando-o-problema)
        - [Permitindo o acesso à rede local](#permitindo-o-acesso-à-rede-local)
        - [Analisando a rota](#analisando-a-rota)
        - [Finalizando a simulação](#finalizando-a-simulação2)
- [Considerações Finais](#considerações-finais)

## Objetivo Geral

Este laboratório tem por objetivo aplicar os conceitos de Endereçamento e Encaminhamento da camada de Rede do modelo TCP/IP vistos em aula.

## Objetivo Específico

O laboratório irá demonstrar a criação de redes de computadores e a comunicação entre os nós de redes, exemplificando a utilização de: Endereço IP, Máscara sub-rede, Gateway e Rota Default, CIDR, NAT e ICMP. O tráfego gerado na simulação das redes será analisado.

## Metodologia

O laboratório será dividido em 3 partes.

### Instalação

Primeiramente haverá um tutorial para a instalação da ferramenta **Netkit**, programa utilizado para simulação de redes, e o **Wireshark**, programa utilizado para analisar o tráfego na rede.

### Experimento 1

Em seguida, será utilizado um cenário simples com 4 máquinas interligadas por um hub, havendo comunicação apenas entre 2 dessas máquinas.

![Representação da rede simulada no Experimento 1](/imagens/experimento1.jpg)

Os alunos irão discutir o motivo das duas outras máquinas não estarem se comunicando com as demais. A turma irá acompanhar um passo a passo utilizando comandos reais de Linux para a configuração do **Endereço IP** e **Máscara de sub-rede** com o objetivo de conectar as outras duas máquinas na rede. O programa **Wireshark** será utilizado para analisar datagramas IP que serão enviados entre as máquinas utilizando o protocolo **ICMP** (ping).

### Experimento 2

Para finalizar, utilizaremos um exemplo mais complexo que irá introduzir o conceito de **NAT**.

![Representação da rede simulada no Experimento 2](/imagens/experimento2.jpg)

Os alunos irão configurar a conexão entre uma rede local e a internet. O programa ***Wireshark*** será utilizado em diversas etapas deste experimento para auxiliar na detecção dos problemas envolvidos na comunicação entre uma máquina com IP privado (rede local) e uma máquina com IP público (internet). Serão analisadas as mensagens enviadas entre as máquinas utilizando os protocolos **ICMP** de **ping** e **traceroute**.

## Ambiente

O laboratório será executado em um Sistema Operacional **Linux**, utilizando o programa **Netkit** para emular a criação de redes de computadores e o programa **Wireshark** para análise do tráfego na rede.

## Roteiro

Antes de iniciar as simulações, devemos instalar e configurar as ferramentas necessárias.

### Instalação e Configuração

Nosso ambiente de simulação Linux irá utilizar as ferramentas **Wireshark** e **Netkit**.

#### Instalação do Wireshark

Abra o terminal e execute o seguinte comando para instalar o **Wireshark**:

    $ sudo apt-get install wireshark

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

---

###  Experimento 1 *(HUB)*

Este é um cenário simples com 4 máquinas interligadas por um hub, havendo comunicação apenas entre 2 dessas máquinas. O objetivo deste experimento é ambientá-lo com as ferramentas de simulação e de análise do tráfego da rede.

![Representação da rede simulada](/imagens/experimento1.jpg)

#### Executando a simulação

1. Crie uma pasta em sua `/home/usuario/` com o nome **laboratorio**.
2. Baixe o [experimento1.zip](/arquivos/experimento1.zip) e descompacte o arquivo dentro da pasta `/home/usuario/laboratorio/`.
3. No terminal, entre na pasta **experimento1**:

        $ cd
        $ cd laboratorio/experimento1/
        $ ls
        lab.conf  MAQUINA1.startup  MAQUINA3          MAQUINA4.startup
        lab.dep   MAQUINA2          MAQUINA3.startup
        MAQUINA1  MAQUINA2.startup  MAQUINA4

    O arquivo **lab.conf** contém informações gerais sobre a simulação, enquanto cada pasta **MAQUINAX** contém informações sobre a interface de rede. Os arquivos **MAQUINAX.startup** reconfiguram a interface de rede da máquina para receber as configurações atualizadas. O arquivo **lab.dep** permite a inicialização paralela das máquinas para agilizar o processo.

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

#### Testando a rede

1. Execute o comando `ifconfig` nas quatro máquinas virtuais.

    ![ifconfig sendo executado na MAQUINA1](/imagens/experimento1c.png)

2. Execute um `ping` da MAQUINA1 para todas as outras máquinas (incluindo a própria MAQUINA1). O comando é `ping -c 3 192.168.0.XXX`, não se esqueça de trocar **XXX** pelo **Endereço IP** de cada máquina.

    ![ping sendo executado da MAQUINA1 para a MAQUINA2](/imagens/experimento1d.png)

3. A MAQUINA1 consegue enviar para 2 e 3 mas não recebe resposta da 3. A MAQUINA1 não consegue enviar para a 4.

    ![Resultado do ping executado da MAQUINA1 para todas as máquinas](/imagens/experimento1e.png)

#### Capturando o tráfego

1. Nós iremos coletar informações da interface das Máquinas 2 e 3. Para salvar os arquivos em sua máquina física, execute o comando a seguir nas máquinas 2 e 3:

        # cd /hosthome/laboratorio/

    ![Trocando o diretório na MAQUINA2](/imagens/experimento1b.png)

2. Para ajudar na detecção do problema, coloque a MAQUINA2 e 3 para coletar informações sobre o tráfego de mensagens que estejam passando por suas interfaces de rede. Execute o comando abaixo, substituindo o X pelo número da máquina:

        # tcpdump -i eth0 -w capturaAX.pcap

    ![tcpdump rodando nas máquinas 2 e 3](/imagens/experimento1f.png)

3. Faça o passo [2 (Testando a rede)](#testando-a-rede) novamente (execute um `ping -c 3` da MAQUINA1 para todas as máquinas, incluindo para a MAQUINA1).

4. Após finalizar o passo anterior, vá nas máquinas 2 e 3 e aperte `Ctrl + C` para finalizar a captura de tráfego.

    ![Finalizado o tcpdump na máquina 2](/imagens/experimento1g.png)

#### Detectando o erro

1. Em sua máquina local, clique duas vezes nos arquivos gerados com extensão `*.pcap`. Analise o tráfego de informação pelo **Wireshark**, reveja o **Endereço IP** e a **Máscara sub-rede** das quatro máquinas e tente descobrir o motivo da falha na comunicação das máquinas 3 e 4.

    ![Wireshark analisando coleta de tráfego da máquina 2](/imagens/experimento1h.png)

    **DICA** Qual a diferença no erro de comunicação da máquina 3 e da máquina 4?

2. Após descobrir o problema, atualize o **Endereço IP** e a **Máscara sub-rede** de forma adequada nas máquinas sem conexão. Siga o comando abaixo, lembrando de modificar **XXX** para o Endereço IP correto e **YYY** para a Máscara sub-rede correta:

        # ifconfig eth0 192.168.0.XXX netmask 255.255.255.YYY up

3. Repita o passo [2 (Testando a rede)](#testando-a-rede) novamente. Se as mudanças corretas forem feitas, a MAQUINA1 receberá resposta de **ping** de todas as máquinas. Sinta-se a vontade para testar o **ping** partindo das outras máquinas também.

#### Finalizando a simulação

1. Para encerrar a simulação, digite `lhalt` em seu terminal **local**:

    ![Finalizando a simulação](/imagens/experimento1i.png)

2. *OPCIONAL* Caso o comando `lhalt` não funcione, utilize o comando `lcrash -F`:

    ![Finalizando a simulação com lcrash -F](/imagens/experimento1j.png)

3. *NOTA* O netkit mantém o estado das máquinas virtuais nos arquivos **MAQUINAX.disk**, possibilitando a continuação da simulação em outro momento. Estes arquivos podem ser extremamente grandes devido ao conflito entre as arquiteturas 32-bit e 64-bit.

    ![Tamanho do arquivo MAQUINA1.disk](/imagens/experimento1k.png)

    Caso tenha concluído a simulação e não deseje manter o estado das máquinas, execute o comando `lclean`:

    ![Removendo os arquivos *.disk](/imagens/experimento1l.png)

---

###  Experimento 2 *(NAT)*

Este é um cenário mais complexo que irá introduzir o conceito de **NAT**. O objetivo deste cenário é exemplificar os problemas enfrentados para conectar uma máquina local com a internet e vice-versa.

![Representação da rede simulada](/imagens/experimento2.jpg)

Todas as máquinas são inicializadas com dois usuários:

- Usuário: aluno / Senha: alu01
- Usuário: professor / Senha: pro01

#### Executando a simulação [executando-a-simulação2]

1. Baixe o [experimento2.zip](/arquivos/experimento2.zip) e descompacte o arquivo dentro da pasta `/home/usuario/laboratorio/` criada anteriormente no [Experimento 1].
2. No terminal, entre na pasta **experimento2**:

        $ cd
        $ cd laboratorio/experimento2/
        $ ls
        EMPRESA1          EMPRESA3          lab.conf          SERVIDOR
        EMPRESA1.startup  EMPRESA3.startup  lab.dep           SERVIDOR.startup
        EMPRESA2          INTERNET          ROTEADOR          shared
        EMPRESA2.startup  INTERNET.startup  ROTEADOR.startup  shared.startup

    As pastas **EMPRESA3** e **SERVIDOR** contêm um arquivo de configuração para um servidor FTP.
    Os arquivos ***.startup** apresentam outra maneira de inicializar a interface de rede de cada máquina.
    O arquivo **shared.startup** cria os usuários **aluno** e **professor** em todas as máquinas.

3. Inicie o experimento com o comando `lstart`:

        $ lstart
        ======================== Starting lab ===========================
        Lab directory: /home/arthurazs/laboratorio/experimento2
        Version:       1.0
        Author:        Arthur Zopellaro, Tatiane Sousa
        Email:         arthurazs@id.uff.br, tatiane.gsousa@gmail.com
        Web:           https://arthurazs.github.io/uff-redes
        Description:
        Experimento 2 - NAT
        =================================================================
        You chose to use parallel startup.
        Starting "ROTEADOR"...
        Starting "SERVIDOR"...
        Starting "INTERNET"...
        Starting "EMPRESA3"...
        Starting "EMPRESA2"...
        Starting "EMPRESA1"...

        The lab has been started.
        =================================================================


4. Cinco janelas serão abertas conforme a imagem abaixo:

    ![Quatro janelas XTerm abertas](/imagens/experimento1a.png)

#### Testando a rede [testando-a-rede2]

1. A conexão já está configurada entre o SERVIDOR e a INTERNET. Execute um `ping` entre essas máquinas.

    ![ping sendo executado do SERVIDOR para a INTERNET](/imagens/experimento2b.png)

2. A conexão já está configurada na rede local: Máquinas da Empresa e o SERVIDOR. Execute um `ping` entre essas máquinas.

    ![ping sendo executado do SERVIDOR para a EMPRESA1](/imagens/experimento2c.png)

3. Entretanto, a rede local não consegue se comunicar com a internet e vice-versa. Execute um `ping` entre essas duas máquinas.

    ![ping sendo executado da INTERNET para a EMPRESA1](/imagens/experimento2d.png)

4. Para uma melhor análise, vamos capturar o tráfego. Primeiro, mude para sua pasta **local** na máquina INTERNET com o comando `cd /hosthome/laboratorio/`:

    ![INTERNET acessando a pasta local](/imagens/experimento2e.png)

5. Para testar melhor, iremos executar o `ping` da INTERNET para a EMPRESA1 e vice-versa enquanto capturamos o tráfego na interface da INTERNET. Para isso, execute o comando a seguir na máquina INTERNET:

        # tcpdump -i eth0 -w captura2AI.pcap &

    **ATENÇÃO** Não se esqueça do **&** no final para garantir que o tcpdump irá rodar em paralelo, permitindo a execução de outros comandos.

6. Execute novamente um `ping` da INTERNET para a EMPRESA1 e vice-versa.

7. Antes de analisarmos o tráfego, precisamos finalizar a execução paralela do tcpdump na INTERNET. Execute o comando a seguir:

        # kill $(pidof tcpdump)

8. Analise o tráfego de informação pelo **Wireshark** e reveja os erros apresentados no terminal da INTERNET e EMPRESA1.

    Por que o `ping` da EMPRESA1 chega na INTERNET mas não consegue retornar?

    Por que a INTERNET está enviando o `ping` para o ROTEADOR? Por que o ROTEADOR responde com "Destino Inacessível"?

    **DICA** Execute o comando `netstat -nr` na INTERNET e no ROTEADOR.


#### Solucionando o problema

1. Para a INTERNET conseguir responder o `ping` da rede local, nós iremos utilizar o NAT.

    O SERVIDOR é a ponte entre a rede local e a internet, portanto ele será o responsável em modificar os pacotes IPs das máquinas locais.

2. Primeiro precisamos habilitar o encaminhamento de pacotes no SERVIDOR. Execute o comando a seguir no terminal do SERVIDOR:

        # echo 1 > /proc/sys/net/ipv4/ip_forward

3. Agora precisamos habilitar o NAT. Nós iremos utilizar o firewall iptables para controlar o Endereçamento e Encaminhamento dos pacotes. Execute os comandos a seguir no terminal do SERVIDOR:

        # iptables -F
        # iptables -F -t nat
        # iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE

    `-F` deleta todas as regras do iptables.

    `-t nat` se refere a tabela de regras NAT.

    `-A POSTROUTING` adiciona uma nova regra de pós-roteamento.

    `-o eth1` especifíca para qual interface essa regra deve ser seguida.

    `-j MASQUERADE` especifíca qual técnica será utilizada.

4. Agora será possível completar o `ping` da EMPRESA1 para a INTERNET:

    ![EMPRESA1 completando o ping](/imagens/experimento2f.png)

5. Capture o tráfego na INTERNET e analise os pacotes enviados e recebidos.

        # tcpdump -i eth0 -w captura2BI.pcap

6. Entretanto, a INTERNET ainda não tem acesso direto à rede local.

#### Permitindo o acesso à rede local

1. Primeiro vamos iniciar alguns serviços para que a INTERNET possa estabelecer conexões com a rede local.

    1. No SERVIDOR, execute `/etc/init.d/proftpd start`. Isso irá iniciar um servidor FTP na porta 20 do SERVIDOR.
    2. Na EMPRESA3, execute `/etc/init.d/proftpd start`. Isso irá iniciar um servidor FTP na porta 21 da EMPRESA3.
    3. Na EMPRESA1, execute `/etc/init.d/ssh start`. Isso irá iniciar um servidor SSH na porta 22 da EMPRESA1.

2. Teste a conexão entre a INTERNET e o SERVIDOR:

        # ftp 202.135.187.131 20

    Em usuário, digite `aluno`. Ao ser requisitado uma senha, digite `alu01`.

    ![INTERNET estabelece conexão com SERVIDOR](/imagens/experimento2g.png)

3. Digite `ls` na INTERNET para verificar se está conectado no SERVIDOR com o usuário **aluno**:

    ![INTERNET está conectada no SERVIDOR com o usuário aluno](/imagens/experimento2h.png)

4. Digite `exit` na INTERNET para encerrar a conexão.

5. Teste a conexão entre a INTERNET e a EMPRESA3:

        # ftp 202.135.187.131 21
        ftp: connect: Connection refused

    A conexão não é estabelecida pois o SERVIDOR não tem nenhuma regra para encaminhar conexões.

    **DÚVIDA** Então por que a conexão foi estabelecida com a porta 20?

6. Devemos criar regras no iptables para encaminhar conexões:

    - A porta 21 deve ser encaminhada para o IP 192.168.0.3 (FTP)
    - A porta 22 deve ser encaminhada para o IP 192.168.0.1 (SSH)

7. No SERVIDOR, execute os comandos a seguir:

        # iptables -t nat -A PREROUTING -p tcp --dport 21 -j DNAT --to 192.168.0.3
        # iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to 192.168.0.1

    `-A PREROUTING` adiciona uma nova regra de pré-roteamento.

    `-p tcp` especifíca a regra para um único protocolo de transporte.

    `--dport 21` especifíca a regra para uma única porta de destino.

    `-j DNAT` especifíca NAT de destino.

    `--to 192.168.0.3` especifíca para qual endereço aquela conexão será encaminhada.

8. Antes de testarmos as conexões, vamos colocar a EMPRESA2 para capturar o tráfego na rede local:

        # cd /hosthome/laboratorio/
        # tcpdump -i eth0 -s 1600 -w captura2C2.pcap

    `-s 1600` fixa o tamanho da captura dos dados para 1600.

9. Partindo da INTERNET, estabeleça uma conexão com o FTP da EMPRESA3 e outra conexão com o SSH da EMPRESA1:

        # ftp 202.135.187.131 21
        Name (202.135.187.131:root): aluno
        Password: alu01
        ftp> ls
        ftp> exit

    O SSH irá pedir para confirmar a chave. Digite `yes` e aperte enter.

        # ssh professor@202.135.187.131 -p 22
        Are you sure you want to continue connecting (yes/no)? yes
        professor@202.135.187.131's password: pro01
        $ ls
        $ exit

10. Volte na INTERNET, finalize o tcpdump com `CTRL + C` e analise o tráfego capturado no arquivo `captura2C2.pcap`.

#### Analisando a rota

Com tudo configurado, podemos analisar a rota que a EMPRESA1 utiliza para chegar na INTERNET.

1. Configure o SERVIDOR para capturar o tráfego na interface eth0:

        # cd /hosthome/laboratorio/
        # tcpdump -i eth0 -w captura2DS.pcap

    `-i any` permite a captura em todas as interfaces.

2. Na EMPRESA1, execute os dois comandos a seguir para traçar a rota até a INTERNET:

        # traceroute 143.102.212.100
        # tracert 143.102.212.100

    Qual a diferença na resposta dos dois comandos?

3. Volte na INTERNET, finalize o tcpdump com `CTRL + C` e analise o tráfego capturado no arquivo `captura2DS.pcap`.

    Qual a diferença entre o `traceroute` e o `tracert`?

#### Finalizando a simulação [finalizando-a-simulação2]

1. Para encerrar a simulação, digite `lhalt` em seu terminal **local**:

2. *OPCIONAL* Caso o comando `lhalt` não funcione, utilize o comando `lcrash`:

## Considerações Finais

**Dúvidas?**

Arquivos de configuração em [github.com/arthurazs/uff-redes](https://github.com/arthurazs/uff-redes).
