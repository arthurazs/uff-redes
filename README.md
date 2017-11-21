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
    - [Experimento 1 *(Switch)*](#experimento-1-switch)
    - [Experimento 2 *(NAT)*](#experimento-2-nat)
- [Considerações Finais](#considerações-finais)

## Objetivo Geral

Este laboratório tem por objetivo aplicar os conceitos de Endereçamento e Encaminhamento da camada de Rede do modelo TCP/IP.

## Objetivo Específico

O laboratório irá demonstrar a criação de redes de computadores e a comunicação entre estas, exemplificando o uso de: Endereço IP, Máscara sub-rede, Gateway e rota default, *CIDR*, *NAT* e *ICMP*. Pacotes da camada de rede serão analisados utilizando *Wireshark*.

## Metodologia

O laboratório será dividido em 3 partes.

### Instalação

Primeiramente haverá um tutorial para que os alunos instalem o *Netkit*, programa utilizado para simulação de redes. Serão explicados os arquivos de configuração utilizados para a criação de cenários de redes.

### Experimento 1

Em seguida, será utilizado um cenário simples com 3 máquinas ligadas em um roteador, havendo comunicação apenas entre 2 delas. Os alunos irão acompanhar um passo a passo utilizando comandos reais de Linux para a configuração do **Endereço IP**, **Máscara de Rede**, ***Gateway*** e **Rota *Default*** com o objetivo de conectar a 3a máquina na rede. O programa ***Wireshark*** será utilizado para analisar pacotes IP que serão enviados entre as máquinas utilizando o protocolo ***ICMP*** (*Ping*).

### Experimento 2

O terceiro passo consiste em apresentar um exemplo mais complexo. Nesta etapa, será introduzido o conceito de ***NAT***. Todas as máquinas estarão configuradas com ***CIDR*** 192.168.0.0/25, entretanto uma delas não terá conexão por estar utilizando um IP fora da faixa do prefixo da rede indicada. Os alunos deverão identificar o problema e resolvê-lo, gerando uma discussão ao fim desta etapa sobre o problema identificado e uma revisão de quais IPs poderiam ser utilizados dentro daquela rede indicada pelo ***CIDR*** proposto. O programa ***Wireshark*** será utilizado para analisar pacotes IP que entre as máquinas utilizando o protocolo ***ICMP*** (*traceroute*/*tracert*).

## Ambiente

O laboratório será executado em um Sistema Operacional ***Linux***, irá utilizar o programa ***Netkit*** para emular a criação de redes de computadores e o programa ***Wireshark*** para análise de pacotes IPs.

## Roteiro

Antes de iniciar as simulações, devemos instalar e configurar as ferramentas necessárias.

### Instalação e Configuração

Nosso ambiente de simulação *Linux* irá precisar das ferramentas ***Wireshark*** e ***Netkit***.

#### Instalação do Wireshark

Abra o terminal e execute o seguinte comando para instalar o ***Wireshark***:

` $ sudo apt-get install wireshark `

#### Pré-requisitos do Netkit

Primeiramente, acesse a página de downloads do [Netkit](http://wiki.netkit.org/index.php/Download_Official) e baixe os três arquivos em *Latest Stable Release*:

- [netkit-2.8.tar.bz2](http://wiki.netkit.org/download/netkit/netkit-2.8.tar.bz2)
- [netkit-filesystem-i386-F5.2.tar.bz2](http://wiki.netkit.org/download/netkit-filesystem/netkit-filesystem-i386-F5.2.tar.bz2)
- [netkit-kernel-i386-K2.8.tar.bz2](http://wiki.netkit.org/download/netkit-kernel/netkit-kernel-i386-K2.8.tar.bz2)

Se seu ambiente Linux for baseado em 64-bit, você deverá instalar algumas bibliotecas extras. Para verificar seu ambiente Linux, execute o comando a seguir em seu terminal:

	$ uname -i
	x86_64
		
Se o resultado for **i386**, você pode pular este passo. Caso o resultado for **x86_64**, você deverá executar os seguintes comandos em seu terminal:

	$ sudo apt-get update
	$ sudo apt install lib32readline6
	$ sudo apt-get install libc6-i386

#### Instalação do Netkit

Após obter os arquivos necessários do Netkit, mova os `*.tar.bz2` para a pasta `/home/usuario/`, lembrando de substituir *usuario* pelo seu nome de usuário.

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
	
Você deverá repetir os comandos acima toda vez que abrir um novo terminal. Para evitar esse problema, adicione essas linhas no final do arquivo `~/.bashrc`

Para verificar se sua instalação foi bem sucedida, execute:

	$ cd $NETKIT_HOME 
	$ sh check_configuration.sh

###  Experimento 1 *(Switch)*

###  Experimento 2 *(NAT)*

## Considerações Finais

**Dúvidas?**

Arquivos de configuração em [github.com/arthurazs/uff-redes](https://github.com/arthurazs/uff-redes).
