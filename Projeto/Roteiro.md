# CRIAÇÃO DE UMA REDE VIRTUAL COM O UBUNTU SERVER

Com o objetivo de colocar em prática os conhecimentos adquiridos ao longo das matérias de redes de computadores (PIRD e SERD), este roteiro detalha os procedimentos necessários para a criação de um ambiente de rede com 8 máquinas virtuais com o Ubuntu Server. 

# Sumário

### [Introdução](#introdução-1)
* [Descrição do projeto](#descrição-do-projeto)
* [Configurações de hardware](#configurações-de-hardware)
* [Topologia da rede](#topologia-da-rede)
* [Organização dos IPs](#organização-dos-ips)

### [Máquinas virtuais](#máquinas-virtuais)
 * [Criação das VMs](#criação-das-vms)
 * [Configuração dos IPs](#configuração-dos-ips)
 * [Configuração dos hostnames](#configuração-dos-hostnames)

### [SSH e firewall](#ssh-e-firewall)

### [Host Only](#host-only)

### [Nomes estáticos](#nomes-estáticos)
 * [Configuração dos nomes estáticos](#nomes-estáticos)
 * [Adiocionando usuários](#usuários)

### [Conexão entre as máquinas](#conexão-entre-as-máquinas)

### [Testes](#testes)
 * [PINGs](#ping)
 * [SSH](#ssh)

### [Considerações finais](#considerações-finais)
#

# Introdução

### Descrição do projeto

Com o objetivo de colocar em prática os conhecimentos adquiridos ao longo das matérias de redes de computadores, este roteiro detalha os procedimentos necessários para a criação de um ambiente de rede com 8 máquinas virtuais (VM), que utilizam o Sistema Operacionl Ubuntu Server. Para isso, serão utilizados 4 computadores do laboratório de redes, que devem se conectar através de um switch, para resultar em servidores conectados em um mesma rede e com acesso remoto através do ssh.

### Configurações de hardware		

As máquinas virtuais utilizadas contém as seguintes especificações técninas:

- Processador: 1 CPU 
- Memória RAM: 512mb
- HD: 10gb
- Memória de vídeo: 16mb

Figura 1: Configuração de hardware das VMs

<img src="Imagens/configuraçãoDaVm.png" title="Figura 23: " width="800" />


### Topologia da rede

 A topologia que utilizamos foi a do tipo estrela, nesse modelo existe um HUB central, no nosso caso um switch, que faz o gerenciamento dos dados que passam pela rede, por causa disso é nescessário que para a rede funcionar todas as máquinas devem estar conectadas ao switch por cabo de rede. Sendo assim também os dados não passam por todas as máquinas, mas somento pela qual o dado está endereçado.
 
<p><center> Figura 2: Topologia estrela</center></p>   
<img src="Imagens/topologia-estrela.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

 
### Organização dos IPs

Para melhor organização das máquinas, as organizaremos em uma tabela com suas descrições, IPs, Hostnames, FQDNs e aliases, seguindo o seguinte padrão:
- Descrição:
- IP: ` 192.168.24.[i] `
  - [ i ] varia entre 98 e 105
- Hostname: `grupo7-vm[x]-pc[y]` 
  - [ x ] varia entre 1 e 2
  - [ y ] varia entre 1 e 4 
- FQDN: `[nome] grupo7-924.ifalara.net`
- Aliase: 3 primeiras letras dos nomes e sobrenomes dos integrantes do grupo
- Faixa de rede: `97 - 105`
- Gateway: `192.168.24.97`

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

# Máquinas Virtuais

## Criação das VMs

1. O primeiro passo consiste na criação de uma pasta para salvar as VMs. Em nosso caso, a pasta criada foi “924-grupo7” dentro da pasta “VM” de “labredes”.

Figura 3: Pasta do grupo
<img src="Imagens/pasta_do_grupo.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

2. Em seguida, abrimos o virtualbox e criamos duas máquinas virtuais, seguindo os padrões de hardware já comentados anteriormente.  A criação das máquinas consiste em:
    - Importar o appliance, que está salvo na subpasta “images” da pasta “labredes”.
     
    `Arquivo > importar appliance `
    
    Figura 4: Importação do appliance
    <img src="Imagens/importação-do-appliance.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

    - Mudar o nome da VM, seguindo o padrão da tabela;
    - Mudar a pasta padrão para a pasta que foi criada.

Figura 5: Criação da VM
 <img src="Imagens/criacao-da-maquina.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />


Com as VMs já criadas, é necessário fazer login como administrador:
` Login: administrador
| Senha: adminifal `

3. Depois é necessário fazer a **instalação do pacote net-tools** para que tudo funcione corretamente. O comando que a ser utilizado é:

```shell
sudo apt-get install net-tools
```
Figura 6: Instalação do pacote net-tools
 <img src="Imagens/net-tools.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />



## Configuração dos IPs

Após logar nas VMs:

1. Primeiro é necessário verificar o nome do arquivo netplan, digitando o comando:
```shell
ls -la /etc/netplan
```
  
2. Acesse o arquivo .YAML
```shell
sudo nano /etc/netplan/01-netcfg.yaml
```
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
  >Para sair desta parte digite Ctrl+x, depois y e aperte enter

Para aplicar efetivamente as mudanças, digite o comando:

```shell
sudo netplan apply
```
>Sempre que fizer alguma alteração nesse arquivo, esse comando deverá ser digitado para aplicar a mudança.

Ao final dessa etapa o arquivo `.YAML` deverá estar assim:

Figura 7: Configuração do IP

 <img src="Imagens/ip.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

>A identação correta é de extrema importância nessa etapa. Para que não ocorra nenhum erro, utilize a tecla espaço para realizar essa ação, e não a tecla tab.

* Para visualizar se as mudanças foram efetivadas, digite o comando:

```shell
ifconfig -a
```

## Configuração dos hostnames

O próximo passo é adicionar os hostnames em cada VM, de acordo com a tabela 1. Isso deve ser realizado através do comando:

```shell
sudo hostnamectl set-hostname <hostname>
```
Figura 8: Configuração do Hostname

<img src="Imagens/hostname.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />



# SSH e firewall

Agora iniciamos o processo de preparação para a instalação do servidor ssh. 

1. Novamente digite o comando `sudo nano /etc/netplan/01-netcfg.yaml`

- Ao abrir o arquivo você deverá:
  - Comentar a linha de IP
  - Comentar a linha de gateway4
  - Reativar o dhcp (true)

Figura 9

Comentando as linhas IP e ativando o dhcp nas configurações do netplan

<img src="Imagens/topster.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

 - Deverá, também, trocar a configuração de rede do adaptador 1 para NAT.

2. Posteriormente, é necessário atualizar os pacotes com as novas definições e versões do repositório do Ubuntu, utilizando os comandos:

```shell
sudo apt update
```

```shell
sudo apt upgrade -y
```
Figura 10

<img src="Imagens/update.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

Após a finalização dessa etapa, nós iniciamos a instalação do servidor servidor ssh. 

1. Digite:
```shell
systemctl status ssh
``` 
 - verificamos que o shh estava inativo, então é necessário que ele seja instalado. Desse modo, utilizamos o comando:

```shell
sudo apt-get install openssh-server
```
Figura 11

<img src="Imagens/aa.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

>Você pode digitar novamente o primeiro comando para se certificar que está tudo certo.

2. Após a instação do ssh é necessário ajustar o firewall, a fim de permitir conexões remotas. Para ajustá-lo digite:

```shell
sudo ufw allow ssh
sudo ufw enable
```
Figura 12

<img src="Imagens/sfw.png" title="Figura 1: Topologia de Rede Estrela" width="1000" />

- Ao final desse processo você deve descomentar as linhas de ip e getway4, e também desativar o dhcp.
> Lembre-se que para que essas mudanças sejam efetivas, você deve digitar `sudo netplan apply`


# Host Only

Esta configuração permite o acesso  remoto à uma VM pelo terminal do PC via ssh.

- É necessário criar uma interface no PC para a comunicação entre o host e a VM, para isso:

1. Vá no virtual box e clique em `arquivo > Host Network Manager` e crie/habilite a interface.

Figura 13: Gerenciador de redes do hospedeiro - adaptador

<img src="Imagens/gerenciador de redes do hospedeiro -adaptador.png" title="Figura: " width="800" />

3. Depois vá em `propriedades` e habilite o servidor dhcp.

Figura 14: Gerenciador de redes do hospedeiro - DHCP

<img src="Imagens/gerenciador de redes do hospedeiro - dhcp.png" title="Figura: " width="800" />


5. Agora, no terminal do PC, verfique a existência da existência da interface ``vboxnet0`` utilizando o comando:
```shell
ifconfig -a
```
Figura 15:

<img src="Imagens/ifconfig terminal.png" title="Figura: " width="800" />

4. Volte ao virtual box e acesse as configurações de rede da sua VM (isso deve ser realizado em apenas uma da rede).
5. Habilite a placa de rede do adaptador 2 e o conecte a `Placa de rede exlcusiva de hospedeiro (Host-Only)

Figura 16: 

<img src="Imagens/adaptador2.png" title="Figura: " width="800" />


6. Ligue a VM e verifique a existência da interface ``enp0s8``. Novamente utilize `ifconfig -a`

Figura 17: Verificando a existência da interface "enp0s8"
<img src="Imagens/aplicação das das configurações e checagem das interfaces através do ifconfig -a.png" title="Figura: " width="800" />

7. Acesse o arquivo `yaml` e ative o dhcp para interface enp0s8.
```shell
sudo nano /etc/netplan/01-netcfg.yaml
```
8. Aplique as mudanças com:
```shell
sudo netplan apply
```
Figura 18: Configurando a interface 
<img src="Imagens/segundo dhcp (1).png" title="Figura: " width="800" />

9. Verfique se tudo funcionou corretamente com `ifconfig -a`

Figura 19: Interface enp0s8 funcionando
<img src="Imagens/configurações ativadas e IP da nova interface checado (1).png" title="Figura: " width="800" />

# Nomes estáticos 

1. Para configurar os nomes estáticos digite o comando:

```shell
sudo nano /etc/hosts
```

2. Utilizando a tabela 1, modifque o arquivo hosts.

Figura 20:

![_adicionando os nomes estáticos o](https://user-images.githubusercontent.com/88728695/187666306-4ee58503-b828-46a9-b7b1-948eab71a723.png)

### Usuários

- Agora, é necessário adicionar 4 usuários iguais em todas as VMs. Para melhor organização, você pode criar uma tabela.

Tabela 2: Usuários


|         VM         |      Usuário     |     Senha    |
|--------------------|------------------|--------------|
|VM1-PC1 até VM2-PC3 |      alan        |     grupo    | 
|VM1-PC1 até VM2-PC3 |     isadora      |     grupo    |   
|VM1-PC1 até VM2-PC3 |     thiago       |     grupo    | 
|VM1-PC1 até VM2-PC3 |     janjan       |     grupo    | 
|VM1-PC4 e VM2-PC4   |      alan        |    adminifal | 
|VM1-PC4 e VM2-PC4   |     isadora      |    adminifal | 
|VM1-PC4 e VM2-PC4   |     thiago       |    adminifal | 
|VM1-PC4 e VM2-PC4   |      janjan      |    adminifal | 


  - O comando para adicionar os usuários é:

```shell
 sudo adduser <usuário>
```
Figura 21
![adicionando users]![image](https://user-images.githubusercontent.com/103062558/187727405-654d5563-535a-4ae0-929c-3d4ec0e95e9d.png)


# Fazendo a conexão entre as máquinas

1. Mudar as configurações de rede do adaptador 1 para `Placa em modo Bridge`.

2. Conectar o cabo ethernet dos computadores ao switch de 8 portas.

Figura 22: Switch de 8 portas 
![switch funcionando](https://user-images.githubusercontent.com/88728695/187913392-abb48629-ec10-4bdc-9736-c45fd2e4f110.jpeg)

Figura 23:  Cabos ethernet conectados ao switch
![switch conectado](https://user-images.githubusercontent.com/88728695/187676120-481802f3-4c28-43a9-9f65-7398764acebe.jpeg)

Figura 24: Rede formada pelos 4 PCs
![thiago apontando pra rede](https://user-images.githubusercontent.com/88728695/187676216-a824f92c-c95d-44bb-b50e-dcc27a593370.jpeg)

3. Logar em outra VM da rede utilizando o comando:
     ```shell
     ssh <usuario>@<ip>
     ```
    
# Testes

Depois de seguir todos os passos acima, a última etapa do projeto é testar e verificar se tudo está funcionando corretamente. Para realizar essa verificação, utilizaremos dois tipos de teste: Ping e teste de SSH

### PING
Para esse tipo de teste serão utilizados respectivamente os IPs, hostnames, FQDNs e aliases (tabela 1).

Figura 25: Respectivamente:
- Ping vm1-pc1 para vm2-pc1 (IP)
- Ping vm1-pc1 para vm1-pc2 (Hostname)
- Ping vm1-pc1 para v2-pc2 (FQDN)
- Ping vm1-pc1 para vm1-pc3 (aliase)

<img src="Imagens/ping-pc1(vm2)-pc2-pc3(vm1).png" title="Figura 23: " width="800" />

Figura 26: Respectivamente:
- Ping vm2-pc1 para vm2-pc3 (IP)
- Ping vm2-pc1 para vm1-pc4 (Hostname)
- Ping vm2-pc1 para vm2-pc4(FQDN)
- Ping vm2-pc1 para vm1-pc1 (aliase)

<img src="Imagens/ping-pc3(vm2)-pc4-pc(vm1).png" title="Figura 23: " width="800" />

### SSH

Figura 27: Respectivamente:

- SSH vm1-pc1 para vm2-pc1 no user alan (IP) 
- SSH vm1-pc1 para vm1-pc2 no user isadora (Hostname)

<img src="Imagens/SSH_pt1.jpg" title="Figura 20: " width="800" />

Figura 28:

- SSH vm1-pc1 para v2-pc2 no user thiago (FQDN)

-Como foi a primeira vez se concectando com essa máquina e usuário também foi nescessário dar a autorização

<img src="Imagens/SSH_pt2.jpg" title="Figura 21: " width="800" />

Figura 29:

- SSH vm1-pc1 para vm1-pc3 no user janjan (aliase)
- SSH vm1-pc1 para vm2-pc3 no user janjan (aliase)

<img src="Imagens/SSH_pt3.jpg" title="Figura 22: " width="800" />

Figura 30:

- SSH vm1-pc1 para vm1-pc4 no user alan (IP) 
- SSH vm1-pc1 para vm2-pc4 no user alan (IP) 
<img src="Imagens/SSH_pt4.jpg" title="Figura 23: " width="800" />

# Considerações finais

Portanto, consideramos a importância desse projeto para o ensino da área de rede de computadores, como forma prática da teoria aplicada em sala de aula. Desse modo, ao final do projeto foi possível aplicar o conceito da topologia de rede do tipo estrela, por meio de 8 máquinas virtuais que se conectam através de um switch de 8 portas. Assim, através dos comandos de PING e SSH é possível acessar todas as máquinas que se conectam na rede local.
