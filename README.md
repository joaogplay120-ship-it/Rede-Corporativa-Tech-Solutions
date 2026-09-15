# - Projeto de Redes Corporativas — Tech Solutions

Este repositório contém a documentação e a topologia oficial desenvolvida no **Cisco Packet Tracer** para a infraestrutura de rede corporativa da empresa **Tech Solutions**.

---

## - Integrantes do Grupo

- João Marcos Marques Silva
- Julia Medeiros Evangelista
- Lara Pereira Alves
- João Gabriel Soares Alves da Silva
- Esther da Silva Marques
- Anna Yasmin Alves Ferreira
- Heveli Ribeiro Pereira
---

## - Descrição do Cenário

A **Tech Solutions** necessitava de uma modernização em sua infraestrutura de rede local para garantir isolamento de tráfego, segurança entre departamentos e automação na distribuição de endereços IP.

A solução implementada conta com:
- **Segmentação em VLANs:** Separação lógica entre o setor de **Administração (VLAN 10)** e o setor de **Desenvolvimento (VLAN 20)**.
- **Roteamento Inter-VLAN:** Configurado no modelo *Router-on-a-Stick* utilizando o protocolo de encapsulamento IEEE 802.1Q no **Router Principal**.
- **Serviços Centralizados:** Implementação de um servidor centralizado na VLAN 10 responsável pelos serviços de **DHCP** e **DNS**.
- **Agente de Relé DHCP (DHCP Relay):** Configuração do recurso `ip helper-address` na sub-interface da VLAN 20 do roteador para repassar as requisições de IP ao servidor central.

---
## - Vídeo de Apresentação: https://youtu.be/Hpp_HNxJD9M
## - Tabela de Endereçamento IP e VLANs

<img width="753" height="1086" alt="Image" src="https://github.com/user-attachments/assets/e6ff5971-3345-4b80-acc0-ea4da212e498" />

---

## - Blocos de Comandos para Auditoria (IOS CLI)

### 1. Roteador Principal (Cisco 1941)
```
enable
configure terminal

! Ativação da interface física principal
interface GigabitEthernet 0/0
 no shutdown
exit

! Configuração da Sub-interface VLAN 10 (Admin)
interface GigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

! Configuração da Sub-interface VLAN 20 (DEV) com DHCP Relay
interface GigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.10.2
exit

end
write memory

### 2. Configuração de Trunk nos Switches
```text
enable
configure terminal
interface FastEthernet 0/24
 switchport mode trunk
 switchport trunk allowed vlan all
exit
end
write memory
```
### 2. Configuração de Trunk nos Switches
```
enable
configure terminal
interface FastEthernet 0/24
 switchport mode trunk
 switchport trunk allowed vlan all
exit
end
write memory
```
## Guia Passo a Passo de Testes de Validação
Para validar o funcionamento completo do projeto no arquivo .pkt, siga estes passos:

Passo 1: Validação do Endereçamento Dinâmico (DHCP)
Clique em qualquer computador do setor Admin (ex: PC-Admin-01), acesse Desktop -> IP Configuration e confirme se está selecionado DHCP.

Verifique se o IP obtido pertence à faixa 192.168.10.X com o Gateway 192.168.10.1.

Repita o processo em um computador do setor DEV (ex: PC-DEV-01) e verifique se o IP obtido pertence à faixa 192.168.20.X com Gateway 192.168.20.1.

Passo 2: Testes de Conectividade Inter-VLAN (Ping)
Abra a estação PC-DEV-01, acesse a aba Desktop -> Command Prompt.

Execute o comando para testar o acesso ao Servidor DHCP/DNS:

```
ping 192.168.10.2
```
Execute o comando para testar o ping em uma estação da VLAN Admin:
```
ping 192.168.10.20
```

