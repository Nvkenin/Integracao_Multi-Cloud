<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>VPN Site-to-Site Azure + AWS</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.6; }
    h1, h2, h3 { color: #2c3e50; }
    pre { background: #f4f4f4; padding: 10px; border-radius: 5px; overflow-x: auto; }
    code { color: #c7254e; background-color: #f9f2f4; padding: 2px 4px; }
    .note { background-color: #eafaf1; padding: 10px; border-left: 4px solid #2ecc71; }
    .warning { background-color: #fdf2e9; padding: 10px; border-left: 4px solid #e67e22; }
  </style>
</head>
<body>
  <h1>VPN Site-to-Site entre Azure e AWS</h1>
  <p><strong>Autor:</strong> Seu Nome</p>

  <h2>Objetivo</h2>
  <p>Estabelecer uma VPN site-to-site entre Azure e AWS que permita comunicação privada entre VMs em sub-redes distintas de cada nuvem.</p>

  <h2>Etapa 1: Criar VMs em redes privadas</h2>

  <h3>1.1 Azure VM</h3>
  <pre><code>az vm create \
  --resource-group rg-azure-aws \
  --name vm-azure \
  --image UbuntuLTS \
  --vnet-name vnet-azure \
  --subnet subnet-privada \
  --private-ip-address 10.0.0.4 \
  --authentication-type ssh \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/id_rsa.pub</code></pre>

  <h3>1.2 AWS EC2</h3>
  <pre><code>aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --count 1 \
  --instance-type t3.micro \
  --subnet-id subnet-xxxxxxxx \
  --private-ip-address 10.1.0.4 \
  --key-name my-key \
  --security-group-ids sg-xxxxxxxx</code></pre>

  <h2>Etapa 2: Configurar a VPN</h2>

  <h3>2.1 Azure</h3>
  <ul>
    <li>Criar o <strong>Virtual Network Gateway</strong> (VPN)</li>
    <li>Criar o <strong>Local Network Gateway</strong> com o IP público da AWS</li>
    <li>Criar a <strong>Conexão VPN</strong> usando uma <em>shared key</em></li>
  </ul>

  <h3>2.2 AWS</h3>
  <ul>
    <li>Criar o <strong>Virtual Private Gateway</strong> e associar à VPC</li>
    <li>Criar o <strong>Customer Gateway</strong> com IP do Azure Gateway</li>
    <li>Criar a <strong>VPN Connection</strong> com a mesma <em>shared key</em></li>
    <li>Habilitar rotas estáticas (se necessário)</li>
  </ul>

  <h2>Etapa 3: Configurar rotas e segurança</h2>
  <ul>
    <li>Azure: criar rota para <code>10.1.0.0/24</code> via Gateway</li>
    <li>AWS: criar rota para <code>10.0.0.0/24</code> via Gateway</li>
    <li>Security Groups/NSGs: permitir SSH, ICMP entre as sub-redes privadas</li>
  </ul>

  <h2>Etapa 4: Linux como máquina de gerenciamento (sem bastion)</h2>

  <div class="note">
    <strong>Dica:</strong> Use a VM Linux de cada lado para acessar, gerenciar e testar a rede por SSH e ping. Não é necessário IP público.
  </div>

  <pre><code>sudo apt update
sudo apt install -y iputils-ping net-tools traceroute</code></pre>

  <h3>Conexão entre VMs</h3>
  <ul>
    <li>De Azure: <code>ping 10.1.0.4</code>, <code>ssh ec2-user@10.1.0.4</code></li>
    <li>De AWS: <code>ping 10.0.0.4</code>, <code>ssh azureuser@10.0.0.4</code></li>
  </ul>

  <h2>Etapa 5: Evidências e Diagrama</h2>
  <ul>
    <li><strong>Prints:</strong> Salvar evidências dos testes de conectividade</li>
    <li><strong>Diagrama:</strong> Criar no <a href="https://draw.io" target="_blank">draw.io</a> e exportar para PNG + salvar o .drawio</li>
    <li><strong>Estrutura de diretórios:</strong></li>
  </ul>

  <pre><code>/docs
  diagrama.drawio
  diagrama.png
/evidencias
  ping-azure-aws.png
  ssh-test.png
README.md</code></pre>

  <h2>Etapa 6: Documentação para GitHub</h2>
  <ul>
    <li>Crie um <code>README.md</code> com:</li>
    <ul>
      <li>Descrição do projeto</li>
      <li>Passo a passo</li>
      <li>Diagrama e evidências</li>
      <li>Créditos</li>
    </ul>
  </ul>

  <h2>Conclusão</h2>
  <p>Com este ambiente, é possível conectar redes privadas Azure e AWS de forma segura, sem exposição pública, e usando Linux como ponto de gestão e testes.</p>

  <div class="note">
    <strong>Feito por:</strong> Seu Nome | Projeto Final VPN Multi-Cloud
  </div>
</body>
</html>
