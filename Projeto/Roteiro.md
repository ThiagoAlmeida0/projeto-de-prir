# CRIAÇÃO DE UMA REDE VIRTUAL COM O UBUNTU SERVER

Com o objetivo de colocar em prática os conhecimentos adquiridos ao longo das matérias de redes de computadores (PIRD e SERD), este roteiro detalha os procedimentos necessários para a criação de um ambiente de rede com 8 máquinas virtuais com o Ubuntu Server. 

## Sumário

### Introdução
* Descrição do projeto
* Configurações de hardware
* Topologia da rede
* Configuração dos IPs

### Criação das máquinas virtuais

#

## Introdução

### Descrição do projeto

Com o objetivo de colocar em prática os conhecimentos adquiridos ao longo das matérias de redes de computadores, este roteiro detalha os procedimentos necessários para a criação de um ambiente de rede com 8 máquinas virtuais (VM), que utilizam o Sistema Operacionl Ubuntu Server. Para isso, serão utilizados 4 computadores do laboratório de redes, que devem se conectar através de um switch, para resultar em servidores conectados em um mesma rede e com acesso remoto através do ssh.

### Configurações de hardware		

As máquinas virtuais devem ser criadas seguindo os especificações abaixo:
* memória
* bla
* bla

*FOTO DAS CONFIGURAÇÕES DE HARDWARE AQUI*

### Topologia da rede

 A topologia utilizada nesse projeto é a estrela. Neste tipo, 
 
 
 
 ![Topografia drawio](https://user-images.githubusercontent.com/88728695/187520442-13669a17-05a3-4130-8b4a-03c7406bc523.png)

 

### Configuração dos IPs

Para melhor organização das máquinas, as organizaremos em uma tabela com suas descrições, IPs, Hostnames, FQDNs e aliases, seguindo o seguinte padrão:
- Descrição:
- IP: ` 192.168.24.[i] `
  - [ i ] varia entre 99 e 105
- Hostname: `grupo7-vm[x]-pc[y]` 
  - [ x ] varia entre 1 e 2
  - [ y ] varia entre 1 e 4 
- FQDN: `[nome] grupo7-924.ifalara.net`
- Aliase: 3 primeiras letras dos nomes e sobrenomes dos integrantes do grupo

Tabela 1: Definições de endereços IPs da Rede e Nomes de Hosts

|  DESCRIÇÃO       |       IP        |      HOSTNAME     |          FQDN                      |     ALIASE       |
|------------------|-----------------|-------------------|------------------------------------|------------------|
| VM1-PC1          | 192.168.24.98   |   grupo7-vm1-pc1  | alan.grupo7-924.ifalara.net        | ala              |               
| VM2-PC1          | 192.168.24.99   |   grupo7-vm2-pc1  | barbosa.grupo7-924.ifalara.net     | bab              |
| VM1-PC2          | 192.168.24.100  |   grupo7-vm1-pc2  | isadora.grupo7-924.ifalara.net     | isa              |                
| VM2-PC2          | 192.168.24.101  |   grupo7-vm2-pc2  | macedo.grupo7-924.ifalara.net      | mac              |
| VM1-PC3          | 192.168.24.102  |   grupo7-vm1-pc3  | thiago.grupo7-924.ifalara.net      | thi              |
| VM2-PC3          | 192.168.24.103  |   grupo7-vm2-pc3  | almeida.grupo7-924.ifalara.net     | alm              |
| VM1-PC4          | 192.168.24.104  |   grupo7-vm1-pc4  | janjan.grupo7-924.ifalara.net      | jan              |
| VM2-PC4          | 192.168.24.105  |   grupo7-vm2-pc4  | silva.grupo7-924.ifalara.net       | sil              |

## Criação das Máquinas Virtuais

1. O primeiro passo consiste na criação de uma pasta para salvar as VMs. Em nosso caso, a pasta criada foi “924-grupo7” dentro da pasta “VM” de “labredes”.

![Pasta do grupo](https://user-images.githubusercontent.com/88728695/187516969-d07a7ae3-c672-49a4-b56b-f244c3f3ed1e.png)

2. Em seguida, abrimos o virtualbox e criamos duas máquinas virtuais, seguindo os padrões de hardware já comentados anteriormente.  A criação das máquinas consiste em:
    - Importar o appliance, que está salvo na subpasta “images” da pasta “labredes”.
     
    `Arquivo >  `
    
    ![Importação do appliance](https://user-images.githubusercontent.com/88728695/187517914-f256b80f-16e3-4021-bf60-3b6546bc845f.png)

    - Mudar o nome da VM, seguindo o padrão da tabela;
    - Mudar a pasta padrão para a pasta que foi criada.

![Criacao da máquina](https://user-images.githubusercontent.com/88728695/187518320-0aac1e6c-1f86-4c28-9aa0-59d57b5a9684.png)

Com as VMs já criadas, é necessário fazer a instalação do pacote net-tools para que tudo funcione corretamente. O comando que a ser utilizado é:

`sudo apt-get install net-tools`

![login feito ](https://user-images.githubusercontent.com/88728695/187567249-2c8698d3-ba8b-4a25-b5eb-98bb572b07ee.png)

Em seguida, é necessário logar como administrador:
` Login: administrador
| Senha: adminifal `

## Configuração dos IPs

Após logar nas VMs:

1. Primeiro é necessário verificar o nome do arquivo netplan, digitando o comando:
`ls -la /etc/netplan`
  
2. Acesse o arquivo .YAML
`sudo nano /etc/netplan/01-netcfg.yaml`
   >A saída que foi dada ao digitar o primeiro comando será utilizada para acessar o arquivo, sendo digitada depois de `netplan/`.

3. Ao abrir o arquivo, ele apresentará as seguintes configurações:
 
 ```shell
network:
  ethernets:
    enp0os3:
      dhcp4: true
  version: 2
```
   - Primeiro, insira os campos `addresses` e `gateway4` dentro de `enp0os3`.
   - Depois, desative o dhcp4, mudando o seu valor para false.
  >Para salvar as modficações, digite Ctrl+x, depois y e aperte enter

Para aplicar efetivamente as mudanças, digite o comando:

`sudo netplan apply`

Ao final dessa etapa o arquivo `.YAML` deverá estar assim:

![configuração estática do ip](https://user-images.githubusercontent.com/88728695/187564956-a75ee26a-fc85-4cc9-9d14-d8bd1f335352.png)

>A identação correta é de extrema importância nessa etapa. Para que não ocorra nenhum erro, utilize a tecla espaço para realizar essa ação, e não a tecla tab.

* Para visualizar se as mudanças foram efetivadas, digite o comando:
`ifconfig -a`

#

### Configuração dos hostnames

O próximo passo foi adicionar os hostnames em cada VM, de acordo com a tabela. Isso deve ser realizado através do comando 

`sudo hostnamectl set-hostname <hostname>`

![atribuindo nomes aos servidores hostname](https://user-images.githubusercontent.com/88728695/187567989-f8e9758a-71b2-451e-8215-55a7a3ef2156.png)


## SSH e firewall

Agora iniciamos o processo de preparação para a instalação do servidor ssh. Para isso, novamente digitamos o comando `sudo nano /etc/netplan/01-netcfg.yaml`.
Ao abrir o arquivo você deverá:
- Comentar a linha de IP
- Comentar a linha de gateway4
- Reativar o dhcp (true)
-
![comentando as linhas IP e ativando o dhcp nas configurações do netplan](https://user-images.githubusercontent.com/88728695/187570512-680b4427-1e84-4140-911a-ae354e654af2.png)

- Deverá, também, trocar a configuração de rede do adaptador 1 deve ser trocada para NAT

Posteriormente, é necessário atualizar os pacotes com as novas definições e versões do repositório do Ubuntu, utilizando os comandos:

 `sudo apt update`
 
 `sudo apt upgrade -y`
 
![atualizando as definições e versoes de pacotes dos repositórios ubuntu](https://user-images.githubusercontent.com/88728695/187571620-fff01831-6b6a-45c2-b372-0031247974e5.png)

Após a finalização dessa etapa, nós iniciamos a instalação do servidor servidor ssh. Ao digitar o comando `systemctl status ssh` verificamos que o shh estava inativo, então é necessário que ele seja instalado. Desse modo, utilizamos o comando:

`sudo apt-get install openssh-server`
>Você pode digitar novamente o primeiro comando para se certificar que está tudo certo

Após a instação do ssh é necessário ajustar o firewall, a fim de permitir conexões remotas. Para ajustá-lo digite:
```shell
sudo ufw allow ssh
sudo ufw enable
```
![ativando o firewall](https://user-images.githubusercontent.com/88728695/187575175-65d6b0f5-665d-4f3c-8248-66c660c7c904.png)

## Acesso remoto SSH com Host Only

## Nomes estáticos 




