# Laboratorio sobre Camada de redes  

Data do experimento 04/09/2024


##  Integrantes

| Matricula  | Nome |
| ------------- | ------------- |
| 200060783 |  Ana Beatriz Wanderley Massuh   |
| 190084570  | Arthur D'Assumpção Loureiro  |
| 200057421  | Delziron Braz de Lima  |
| 200017322  |  Emerson Luis Teles dos Santos  |

## Perguntas e Respostas

## 1 - Escolha três computadores, A, B e R e distribua os endereços IP, conforme apresentado na Figura 1 e monte as tabelas de rotas desses equipamentos, de modo que as máquinas da rede #01 consigam visualizar todas as máquinas da rede #02 (usar o comando ping para isso).

![fig1](/assets/FIG1.png)


### A) Qual o comando para atribuir um endereço IP para uma máquina Linux, usando a console (terminal)? Qual o comando para listar a tabela de rotas das máquinas (sintaxe geral)?

Primeiro comando necessario para desligar o Network Manager:

    sudo systemclt stop NetworkManager 

Atribuir  o endereço IP

    Computador A configura : sudo ifconfig eno1 172.25.0.2 netmask 255.255.255.0 up
    Computador B configura : sudo ifconfig enp2s0f0 192.168.93.2 netmask 255.255.255.0 up
    Computador R configura : sudo ifconfig eno1 172.25.0.1 netmask 255.255.255.0 up
    Computador R configura : sudo ifconfig enp2s0f0 192.168.93.1 netmask 255.255.255.0 up

Comando para configurar a tabela de rotas:

    Computador A e R configura : sudo route add -net 172.25.0.0 netmask 255.255.255.0 eno1
    Computador B e R configura : sudo route add -net 192.168.93.0 netmask 255.255.255.0 enp2s0f0

Comando para configurara a maquina R como gateway para A e B:
    
    Computador A configura: sudo route add -net 192.168.93.0 netmask 255.255.255.0 gw 172.25.0.1 eno1
    Computador B configura: sudo route add -net 172.25.0.0 netmask 255.255.255.0 gw 192.168.93.1 enp2s0f0

Comando para repassar o ip na maquina R:

    net.ipva4.ip_forward = 1

Comando para listar tabela de rotas:

    netstat -nr ou route -n


### B ) Qual a tabela de rotas das máquinas A, B e R?

#### Tabela A

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |eno1|
|192.168.93.0 | 172.25.0.1 | 255.255.255.0 | UG | 0 |0 | 0 |eno1|

#### Tabela B

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
|172.25.0.0 | 192.168.93.1 | 255.255.255.0 | UG | 0 |0 | 0 |enp2s0f0|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enp2s0f0|

#### Tabela R

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |eno1|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enp2s0f0 |




## 2 - 2. Altere a configuração anterior, de modo que as máquinas A, B e R consigam acessar a Internet, considerando o que está apresentado na Figura 2. Obs.: Implementar NAT no equipamento de saída para Internet.

![fig2](/assets/FIG2.png)

### A ) Qual o comando para atribuir um endereço IP para uma máquina Linux, usando a console (terminal)? Qual o comando para listar a tabela de rotas das máquinas (sintaxe geral)?
Primeiro comando necessario para desligar o Network Manager:

    sudo systemclt stop NetworkManager 

Atribuir  o endereço IP

    Computador A configura : sudo ifconfig eno1 172.25.0.2 netmask 255.255.255.0 up
    Computador B configura : sudo ifconfig eno2 192.168.93.2 netmask 255.255.255.0 up
    Computador R configura : sudo ifconfig eno2 192.168.93.1 netmask 255.255.255.0 up

Comando para repassar o ip nas maquinas:

    net.ipva4.ip_forward = 1

Comando para listar tabela de rotas:

    netstat -nr


### B ) Qual a tabela de rotas das máquinas A, B e R?

#### Tabela A

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
| 0.0.0.0  | 11.10.0.2 | 255.255.0.0 | UG | 0 |0 | 0 |eno1|
|11.10.0.0 | 0.0.0.0 | 255.255.0.0 | U | 0 |0 | 0 |eno1|
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enx00e04c534458|
|192.168.93.0 | 172.25.0.1 | 255.255.255.0 | UG | 0 |0 | 0 |enx00e04c534458|

#### Tabela B

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
| 0.0.0.0 | 192.168.93.1 | 255.255.255.0 | UG | 0 |0 | 0 |enp2s0f0|
|172.25.0.0 | 192.168.93.1 | 255.255.255.0 | UG | 0 |0 | 0 |enp2s0f0|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enp2s0f0|

#### Tabela R

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
| 0.0.0.0 | 172.25.0.2 | 0.0.0.0 | UG | 0 |0 | 0 |eno1|
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |eno1|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enx00e04c534458 |





### C ) Que configurações adicionais foram necessárias para todas as máquinas tenham acesso à Internet? Na resposta, relatar o equipamento e as configurações adicionais feitas.


Comandos adicionais necessários na máquina A:

    ip link show // Exibe uma lista de todas as interfaces de rede no sistema

    sudo ip link set eno1 up // Ativa a interface de rede eno1.

    sudo dhclient eno1 // Solicita um endereço IP para a interface eno1 usando o DHCP (Dynamic Host Configuration Protocol).

    sudo sysclt -w net.ipv4.ip_forward=1 // Habilita o encaminhamento de pacotes IP entre interfaces de rede no sistema. Isto é necessário para que o sistema funcione como um roteador.

    cat /proc/sys/net/ipv4/ip_forward //  Exibe o valor atual do encaminhamento de IP no sistema. Se o valor for 1, o encaminhamento de IP está ativado; se for 0, está desativado.

    sudo iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE // Adiciona uma regra na tabela nat do iptables para mascarar (NAT) o tráfego de saída na interface eno1. O alvo MASQUERADE é usado para mascarar o endereço IP de origem dos pacotes que saem da interface.

    sudo iptables -t nat -L -n -v4 // Lista as regras na tabela nat do iptables de maneira detalhada (-v), sem resolução de nomes (-n).

    
Maquina B

    sudo route add default gw 192.168.93.1 // Adiciona uma rota padrão (gateway) para o roteamento de pacotes na tabela de roteamento

Vale ressaltar que nessa questão colocamos a Internet na máquina A. 

Equipamento utilizado:

    2 Cabos de rede de 2 metros, 2 Adaptadores ethernet e 3 computadores




## 3 - Altere a configuração anterior, de modo que as máquinas A, B e R consigam acessar a Internet, considerando o que está apresentado na Figura 3. Obs.: Implementar NAT no equipamento de saída para Internet.

![fig3](/assets/FIG3.png)

### A ) Qual o comando para atribuir um endereço IP para uma máquina Linux, usando a console (terminal)? Qual o comando para listar a tabela de rotas das máquinas (sintaxe geral)?
Primeiro comando necessario para desligar o Network Manager :

    sudo systemclt stop NetworkManager 

Atribuir  o endereço IP: 

    Computador A configura : sudo ifconfig eno1 172.25.0.2 netmask 255.255.255.0 up
    Computador B configura : sudo ifconfig eno2 192.168.93.2 netmask 255.255.255.0 up
    Computador R configura : sudo ifconfig eno2 192.168.93.1 netmask 255.255.255.0 up

Comando para repassar o ip nas maquinas:

    net.ipva4.ip_forward = 1

Comando para listar tabela de rotas:

    netstat -nr


### B ) Qual a tabela de rotas das máquinas A, B e R?

#### Tabela A

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
|11.10.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |eno1|
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enx00e04c534458|
|192.168.93.0 | _gateway | 255.255.255.0 | UG | 0 |0 | 0 |enx00e04c534458|

#### Tabela B

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
| 0.0.0.0 | 192.168.93.1 | 0.0.0.0 | UG | 0 |0 | 0 |enp2s0f0|
|172.25.0.0 | 192.168.93.1 | 255.255.255.0 | UG | 0 |0 | 0 |enp2s0f0|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enp2s0f0|

#### Tabela R

| Destino |Roteador | MáscaraGen. | Opções | MSS | Janela | irtt |Iface|
| ------ | --------- | ------ | --------- | ------ | --------- | ------ | --------- |
| 0.0.0.0 | 11.10.0.2 | 0.0.0.0 | UG | 0 |0 | 0 |eth0|
| 11.10.0.0 | 0.0.0.0 | 255.255.0.0 | U | 0 |0 | 0 |eth0|
|172.25.0.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |eno1|
|192.168.93.0 | 0.0.0.0 | 255.255.255.0 | U | 0 |0 | 0 |enx00e04c534458 |



### C ) Que configurações adicionais foram necessárias para todas as máquinas tenham acesso à Internet? Na resposta, relatar o equipamento e as configurações adicionais feitas.

Na Maquina R

    ip link show

    sudo ip link set eno2 up

    sudo dhclient eno2 


Maquina R

    sudo iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE

    sudo iptables -t nat -L -n -v4

    sudo iptables -t nat -L -n -v4


Maquina A

    sudo route add default gw 172.25.0.1

Maquina B

    sudo route add default gw 192.168.93.1


# Passo a Passo Questão 01
## 1. desabilita o network manager em todas maquinas: 
`sudo systemctl stop NetworkManager`
### 2. Configura o ip de cada maquina:
#### Na maquina A: `sudo ifconfig <interface de rede de A> 172.25.0.2 netmask 255.255.255.0 up`
#### Na maquina B: `sudo ifconfig <interface de rede de B> 192.168.93.2 netmask 255.255.255.0 up`
#### Na maquina R: 
`sudo ifconfig <interface de rede de R-A> 172.25.0.1 netmask 255.255.255.0 up`
`sudo ifconfig <interface de rede de R-B> 192.168.93.1 netmask 255.255.255.0 up`

### 3. Adiciona na tabela de rotas:
#### Na maquina A: `sudo route add -net 172.25.0.0 netmask 255.255.255.0 <interface de rede de A>`
#### Na maquina B: `sudo route add -net 192.168.93.0 netmask 255.255.255.0 <interface de rede de B>`
#### Na maquina R:
`sudo route add -net 172.25.0.0 netmask 255.255.255.0 <interface de rede de R-A>`
`sudo route add -net 192.168.93.0 netmask 255.255.255.0 <interface de rede de R-B>`

### 4. Adiciona os Gateways nas Maquinas perifericas para conectar com a rede do outro:
#### Na maquina A: `sudo route add -net 192.168.93.0 netmask 255.255.255.0 gw 172.25.0.1 <interface de rede de A> `
#### Na maquina B: `sudo route add -net 172.25.0.0 netmask 255.255.255.0 gw 192.168.93.1 <interface de rede de B> `

### 5. Habilitar o ip Forward, para que R repasse o ip da conexão como um gateway:
#### Na maquina R: net.ipva4.ip_forward = 1

# Passo a Passo Questão 03:

### 1. Conecta o cabo novo;

### 2. procura a interface do cabo novo:
#### Na maquina R: `ip link show`

### 3. adiciona essa interface ao ifconfig:
#### Na maquina R: `sudo ip link set <interface do cabo novo com internet>`

### 4. Configurar o ip com internet:
#### Na maquina R: `sudo dhclient <interface do cabo novo com internet>`

### 5. Configura o NAT:
#### Na maquina R: `Sudo iptables -t nat -A POSTROUTING -o <interface de rede com internet> -j MASQUERADE`

### 6. Adiciona a maquina com internet no gateway:
#### Na maquina A: `sudo route add default gw 172.25.0.1 <interface de A>`
#### Na maquina B: `sudo route add default gw 192.168.93.1 <interface de B>`


## Componentes Utilizados

2 Cabos de rede de 2 metros, 3 Adaptadores ethernet e 3 computadores
