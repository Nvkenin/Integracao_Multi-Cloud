# VPN Site-to-Site entre Azure e AWS

## Objetivo

Estabelecer uma VPN site-to-site entre Azure e AWS que permita comunicação privada entre VMs em sub-redes distintas de cada nuvem.

---

## Etapa 1: Criar VMs em redes privadas

### 1.1 Azure VM

```bash
az vm create \
  --resource-group rg-azure-aws \
  --name vm-azure \
  --image UbuntuLTS \
  --vnet-name vnet-azure \
  --subnet subnet-privada \
  --private-ip-address 10.0.0.4 \
  --authentication-type ssh \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/id_rsa.pub
```

### 1.2 AWS EC2

```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --count 1 \
  --instance-type t3.micro \
  --subnet-id subnet-xxxxxxxx \
  --private-ip-address 10.1.0.4 \
  --key-name my-key \
  --security-group-ids sg-xxxxxxxx
```

---

## Etapa 2: Configurar a VPN

### 2.1 Azure

- Criar o Virtual Network Gateway (VPN)
- Criar o Local Network Gateway com o IP público da AWS
- Criar a Conexão VPN usando uma shared key

### 2.2 AWS

- Criar o Virtual Private Gateway e associar à VPC
- Criar o Customer Gateway com IP do Azure Gateway
- Criar a VPN Connection com a mesma shared key
- Habilitar rotas estáticas (se necessário)

---

## Etapa 3: Configurar rotas e segurança

- Azure: criar rota para 10.1.0.0/24 via Gateway
- AWS: criar rota para 10.0.0.0/24 via Gateway
- Security Groups/NSGs: permitir SSH, ICMP entre as sub-redes privadas

---

## Etapa 4: Linux como máquina de gerenciamento (sem bastion)

Dica: Use a VM Linux de cada lado para acessar, gerenciar e testar a rede por SSH e ping. Não é necessário IP público.

```bash
sudo apt update
sudo apt install -y iputils-ping net-tools traceroute
```

### Conexão entre VMs

- De Azure: ping 10.1.0.4, ssh ec2-user@10.1.0.4
- De AWS: ping 10.0.0.4, ssh azureuser@10.0.0.4

---

## Etapa 5: Evidências e Diagrama

- Prints: Salvar evidências dos testes de conectividade
- Diagrama: Criar no draw.io e exportar para PNG + salvar o .drawio
- Estrutura de diretórios:

```cmd
/docs
  diagrama.drawio
  diagrama.png
/evidencias
  ping-azure-aws.png
  ssh-test.png
README.md
```

---

## Etapa 6: Documentação para GitHub

Crie um README.md com:

- Descrição do projeto
- Passo a passo
- Diagrama e evidências
- Créditos

---

## Conclusão

Com este ambiente, é possível conectar redes privadas Azure e AWS de forma segura, sem exposição pública, e usando Linux como ponto de gestão e testes.

---
