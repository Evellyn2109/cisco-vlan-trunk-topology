# cisco-vlan-trunk-topology
Repositório acadêmico da prática de Redes de Computadores no Cisco Packet Tracer, focado na configuração de VLANs (10 e 20) e enlaces Trunk (padrão IEEE 802.1Q) em arquitetura de rede hierárquica.

# Relatório Técnico: Configuração de VLANs e Enlaces Trunk (IEEE 802.1Q)

**Disciplina:** Redes de Computadores  
**Atividade:** Prática de Simulação de Ambiente Hierárquico — Enlaces Trunk  


---

##  1. Introdução e Fundamentação Teórica

Esta atividade prática teve como objetivo implementar e validar a segmentação de rede por meio de **VLANs (Virtual Local Area Networks)** e a agregação de tráfego através de **enlaces Trunk** em uma topologia de rede hierárquica dividida em três camadas (Núcleo, Distribuição e Acesso).

* **Encapsulamento (IEEE 802.1Q) :** Padrão da indústria que insere uma tag de 4 bytes no quadro Ethernet original, permitindo identificar a qual VLAN o tráfego pertence ao trafegar por um único enlace físico partilhado (entroncamento).
* **VLAN Nativa:** Configurada padronizadamente na **VLAN 1** em todos os enlaces Trunk para o tratamento de tráfego não etiquetado, prevenindo alertas de incompatibilidade (*Native VLAN Mismatch*).

---

##  2. Mapeamento da Topologia e Identificação de VLANs

###  Identificação das VLANs
* **VLAN 10:** Alunos (Portas ímpares nos switches de acesso).
* **VLAN 20:** Visitantes (Portas pares nos switches de acesso).
* **VLAN 1:** Gestão e VLAN Nativa padrão dos troncos.

###  Mapeamento da Infraestrutura Hierárquica
* **Camada de Núcleo (Core):** `SW-CORE0`, `SW-CORE1`
* **Camada de Distribuição:** `SW-DIST0`, `SW-DIST1`
* **Camada de Acesso:** `SW-ACESS0`, 'SW-ACESS1', `SW-ACESS2`, `SW-ACESS3`

---

##  3. Relação Completa de Comandos Cisco IOS (CLI)

Abaixo encontram-se os comandos sequenciais executados em cada camada da topologia para forçar o entroncamento e liberar o tráfego das VLANs.

### A. Configuração nos Switches de Acesso (Exemplo: `SW-ACESS3`)
```text
enable
configure terminal
interface fastEthernet 0/1
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 10,20
 end

B. Configuração nos Switches de Distribuição (SW-DIST0 e SW-DIST1)
Plaintext
enable
configure terminal
interface range gigabitEthernet 1/0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 10,20
 end

C. Configuração nos Switches de Núcleo (SW-CORE0 e SW-CORE1)
No SW-CORE0:
Plaintext
enable
configure terminal
interface gigabitEthernet 1/1/2
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 10,20
interface range gigabitEthernet 1/0/2 - 5
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 10,20
 end

No SW-CORE1:
Plaintext
enable
configure terminal
interface range gigabitEthernet 1/1/1 - 2
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 10,20
 end

 4. Validação e Testes de Conectividade
A verificação do estado dos troncos foi efetuada através do comando show interfaces trunk, confirmando o status trunking, o encapsulamento 802.1q e as VLANs permitidas (10,20).

O isolamento e a transmissão de dados foram testados com sucesso via utilitário ICMP (Ping) entre dispositivos da mesma VLAN através de múltiplos saltos de switches, retornando 0% de perda de pacotes entre dispositivos da mesma VLAN através de múltiplos saltos de switches, retornando 0% de perda de pacotes.
