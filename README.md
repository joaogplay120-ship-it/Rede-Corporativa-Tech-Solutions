Explicação detalhada de todo o processo : 

1. Criação da VLAN de Administração

O primeiro passo foi configurar a VLAN 10, identificada como Admin.

Uma VLAN (Virtual Local Area Network) permite dividir uma rede física em redes lógicas diferentes. Nesse caso, a VLAN 10 representa o setor de Administração.

Também foi configurada a porta FastEthernet 0/24 como Trunk. Essa porta é importante porque permite transportar, pelo mesmo enlace, o tráfego de diferentes VLANs.

Depois, as portas FastEthernet 0/1 até 0/10 foram configuradas para pertencer exclusivamente à VLAN 10. Portanto, os dispositivos conectados nessas portas passam a fazer parte da rede administrativa.

2. Criação da VLAN de Desenvolvimento

Em seguida, foi criada e configurada a VLAN 20, chamada DEV.

Assim como na VLAN de Administração, foram utilizadas as portas FastEthernet 0/1 até 0/10, porém direcionadas para a VLAN 20 no contexto apresentado para o setor de Desenvolvimento.

Dessa forma, a rede fica logicamente dividida entre:

VLAN 10 → Admin
VLAN 20 → DEV

Essa separação ajuda a organizar os dispositivos e o tráfego de cada setor.

3. Configuração do Core Switch

Depois da criação das VLANs, foi realizada a configuração do Core Switch.

O primeiro comando utilizado é:

enable

Ele muda o equipamento do modo básico para o modo privilegiado, permitindo executar comandos de configuração e visualização mais avançados.

Depois é utilizado:

configure terminal

Esse comando entra no modo de configuração global. É nesse ambiente que são criadas VLANs, configuradas interfaces e realizadas outras alterações no equipamento.

4. Identificação das VLANs

Na sequência são utilizados os comandos:

vlan 10
vlan 20

Eles criam as VLANs correspondentes.

Depois são definidos os nomes:

name Admin
name DEV

Os nomes facilitam a identificação da finalidade de cada VLAN. Assim, ao visualizar a configuração, fica mais fácil saber que a VLAN 10 corresponde à Administração e a VLAN 20 ao Desenvolvimento.

5. Configuração da porta Trunk

Depois foi configurada a interface:

interface FastEthernet 0/24

Essa etapa seleciona a porta 0/24 para que suas configurações possam ser alteradas.

Em seguida:

switchport mode trunk

A porta passa a funcionar como Trunk.

O Trunk permite que o tráfego de várias VLANs seja transportado pelo mesmo enlace. Isso é fundamental para a comunicação entre o switch e o roteador quando é utilizada a técnica Router-on-a-Stick.

6. Configuração das portas de acesso

Depois são selecionadas várias portas ao mesmo tempo através de:

interface range FastEthernet 0/1 - 10

Em vez de configurar cada porta individualmente, esse comando permite aplicar a mesma configuração a todo o intervalo de portas.

Depois é utilizado:

switchport mode access

Isso transforma essas portas em portas de acesso, destinadas aos dispositivos finais, como computadores, impressoras ou outros equipamentos.

Por fim, é utilizado o comando para associar a porta à VLAN correspondente, como:

switchport access vlan 10

Assim, a porta passa a fazer parte diretamente da VLAN 10.

7. Ativação das interfaces

Outro comando importante utilizado no processo é:

no shutdown

Ele ativa a interface.

Isso é necessário porque, em determinados equipamentos Cisco, interfaces podem estar desativadas inicialmente. O comando permite colocar a interface em funcionamento.

8. Configuração do roteador com Router-on-a-Stick

Depois começa a configuração do roteador principal.

Foi criada a subinterface:

GigabitEthernet 0/0.10

Essa é uma interface lógica associada à interface física GigabitEthernet 0/0. Ela permite que o roteador trabalhe com diferentes VLANs utilizando um único enlace físico.

Essa técnica é chamada de Router-on-a-Stick.

Para a VLAN 10, foi configurado:

encapsulation dot1Q 10

Esse comando informa ao roteador que aquela subinterface está relacionada à VLAN 10 e utiliza o padrão IEEE 802.1Q para identificar os pacotes.

Depois foi definido:

ip address 192.168.10.1 255.255.255.0

Esse endereço funciona como o Gateway Padrão da VLAN 10.

9. Configuração da VLAN 20 no roteador

O mesmo princípio é aplicado à VLAN 20.

O roteador possui a subinterface:

Gi0/0.20

com o endereço:

192.168.20.1/24

Esse endereço funciona como o Gateway da VLAN 20 (DEV).

Portanto, temos:

VLAN 10 → Gateway: 192.168.10.1
VLAN 20 → Gateway: 192.168.20.1

Isso permite que o roteador faça o encaminhamento necessário entre as redes.

10. Configuração do DHCP

Depois foi configurado o serviço de DHCP.

O servidor DHCP/DNS possui o endereço:

192.168.10.2/24

e utiliza 192.168.10.1 como gateway.

O documento mostra também a utilização do comando:

ip helper-address 192.168.10.2

Esse comando faz o roteador funcionar como um DHCP Relay.

Isso é necessário porque uma solicitação DHCP inicialmente utiliza broadcast, e o roteador pode receber essa solicitação de uma VLAN e encaminhá-la para o servidor DHCP centralizado.

No documento, o servidor DHCP está localizado na VLAN 10, enquanto computadores da VLAN 20 também precisam receber endereços IP. O ip helper-address permite esse encaminhamento para o servidor DHCP.

11. Saída e salvamento da configuração

Durante a configuração são utilizados:

exit

para voltar um nível na configuração, e:

end

para sair dos submodos e retornar diretamente ao modo privilegiado.

Por fim:

write memory

salva a configuração realizada para que ela permaneça mesmo depois de o equipamento ser reiniciado ou desligado.

12. Conexão dos dispositivos

Após a configuração lógica, o documento apresenta a rede com os dispositivos conectados.

A estrutura inclui:

Roteador principal;
Switch;
Servidor DHCP/DNS;
Servidor de arquivos;
Impressora da Administração;
Impressora do Desenvolvimento;
PCs da Administração;
PCs do Desenvolvimento.

A partir desse momento, a rede física está conectada e os equipamentos podem receber suas configurações de rede.

13. Configuração dos servidores e impressoras

Em seguida aparecem as etapas relacionadas aos serviços (pools) e às impressoras.

O servidor DHCP/DNS recebe o endereço fixo:

192.168.10.2/24

Já o servidor de arquivos recebe:

192.168.10.3/24

Ambos estão na VLAN 10 e utilizam 192.168.10.1 como gateway.

As impressoras também recebem endereços definidos:

Printer-Admin → 192.168.10.10
Printer-DEV → 192.168.20.10

Cada impressora fica na VLAN correspondente ao seu setor.

14. Distribuição dos IPs para os computadores

Depois são configurados os computadores para receber IP por DHCP.

Os computadores da Administração recebem endereços da rede:

192.168.10.20 em diante

e utilizam:

192.168.10.1

como gateway.

Já os computadores DEV recebem endereços da rede:

192.168.20.20 em diante

e utilizam:

192.168.20.1

como gateway.

Isso significa que os computadores não precisam ter seus IPs configurados manualmente: o servidor DHCP fornece essas informações automaticamente.

15. Teste da rede

Depois de configurar os endereços, o documento mostra a realização de testes de IP, incluindo um teste no PC DEV 01.

Essa etapa serve para verificar se o computador recebeu corretamente suas configurações de rede e se a comunicação está funcionando.

O teste é importante porque não basta apenas configurar os equipamentos: é necessário verificar se as configurações realmente funcionaram.

16. Tabela final de endereçamento

Por último, o documento apresenta uma tabela de endereçamento, que organiza todos os dispositivos, suas interfaces, endereços IP, gateways, VLANs e funções.

A configuração final fica, de forma resumida:

<img width="753" height="1086" alt="Image" src="https://github.com/user-attachments/assets/e6ff5971-3345-4b80-acc0-ea4da212e498" />

