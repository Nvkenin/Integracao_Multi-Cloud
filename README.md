#  VPN Site-to-Site entre Azure e AWS (GUI)

---

##  Objetivo
Criar uma VPN Site-to-Site entre Microsoft Azure e Amazon Web Services (AWS), permitindo a comunicação entre VMs nas duas nuvens usando apenas IPs privados.

---

##  Azure (Portal)

### 1. Criar Resource Group
- Acesse: [https://portal.azure.com](https://portal.azure.com)
- Pesquise por **Resource Groups > Create**
- Nome: `rg-azure-aws-raul`
- Região: mesma da VNet

---

### 2. Criar Virtual Network (VNet)
- Pesquise: **Virtual Networks > Create**
- Nome: `vnet-vpn-raul`
- Endereço CIDR: `10.1.0.0/24`
- Sub-rede: `sub-priv-raul` — `10.1.0.0/24`
- Grupo de recurso: `rg-azure-aws-raul`

---

### 3. Criar Gateway Subnet
- Vá para a VNet > **Subnets > + Gateway Subnet**
- Nome: `GatewaySubnet`
- CIDR: `10.1.0.224/27` (reserva para o gateway)

---

### 4. Criar Virtual Network Gateway
- Pesquise: **Virtual Network Gateway > Create**
- Nome: `vng-raul`
- Tipo: **VPN**
- Gateway type: `Route-based`
- SKU: `VpnGw1`
- VNet: `vnet-vpn-raul`
- IP público: novo IP chamado `ip-vng-raul`

---

### 5. Criar Local Network Gateway (AWS)
- Pesquise: **Local Network Gateway > Create**
- Nome: `lng-aws-raul`
- IP público: (copie do Virtual Private Gateway da AWS)
- Prefixo de endereço: `10.0.1.0/24`

---

### 6. Criar Conexão VPN com AWS
- Vá para o `vng-raul`
- Acesse **Connections > + Add**
  - Nome: `conn-aws-raul`
  - Tipo: `Site-to-Site (IPSec)`
  - Gateway local: `lng-aws-raul`
  - Shared key: `vpnkey123`

---

### 7. Criar VM de teste (Linux)
- Pesquise: **Virtual Machines > Create**
- Nome: `VM-Provider-Raul`
- SO: Ubuntu Server
- Rede: `vnet-vpn-raul`, Sub-rede: `sub-priv-raul`
- IP público: **nenhum**
- Chave SSH: gerar ou usar existente

---

### 8. Route Table
- Crie route table para `10.0.1.0/24 → Virtual Network Gateway`
- Associe à sub-rede privada

---

### 9. NSG (Segurança)
- Libere **ICMP** e **SSH (porta 22)** da rede `10.0.1.0/24`

---

##  AWS (Console)

### 1. Criar VPC
- Console: [https://console.aws.amazon.com/vpc](https://console.aws.amazon.com/vpc)
- VPC Dashboard > **Create VPC**
  - Nome: `vpc-vpn-raul`
  - CIDR: `10.0.1.0/24`

---

### 2. Criar Sub-rede
- Subnets > Create
  - Nome: `sub-priv-raul`
  - CIDR: `10.0.1.0/24`
  - VPC: `vpc-vpn-raul`

---

### 3. Criar Virtual Private Gateway
- VPC Dashboard > Virtual Private Gateways > Create
  - Nome: `vpg-raul`
  - ASN padrão
- Após criação: clique em **Attach to VPC** → `vpc-vpn-raul`

---

### 4. Criar Customer Gateway (Azure)
- VPC Dashboard > Customer Gateways > Create
  - Nome: `cg-azure-raul`
  - Tipo: Static
  - IP: (IP público do Azure VPN Gateway)
  - ASN: `65000` (ou padrão)

---

### 5. Criar VPN Connection
- VPN Connections > Create
  - Nome: `vpn-azure-raul`
  - Gateway: `vpg-raul`
  - Customer Gateway: `cg-azure-raul`
  - Routing: Static
  - Static route: `10.1.0.0/24`
  - Shared key: `vpnkey123`

---

### 6. Criar EC2 de teste
- EC2 > Launch Instance
  - Nome: `EC2-VPN-Raul`
  - AMI: Ubuntu ou Amazon Linux
  - VPC: `vpc-vpn-raul`
  - Subnet: `sub-priv-raul`
  - IP público: **nenhum**
  - Chave SSH: usar existente

---

### 7. Route Table
- Ir à route table da `sub-priv-raul`
- Adicionar rota para `10.1.0.0/24 → Virtual Private Gateway`

---

### 8. Security Group
- Libere:
  - ICMP (ping)
  - Porta 22 (SSH)
  - Origem: `10.1.0.0/24`

---

##  Testes de Conectividade

No terminal da VM de cada lado:

```bash

ping 10.1.0.4      # De AWS para Azure
ping 10.0.1.4      # De Azure para AWS
ssh azureuser@10.0.1.4
ssh ec2-user@10.1.0.4

```


Observações Finais

- Nenhum recurso usa IP público nas VMs

- Comunicação 100% privada via túnel VPN IPSec

- O Linux serve como estação de testes e gerenciamento — sem Bastion

- Projeto ideal para arquitetura segura entre nuvens
