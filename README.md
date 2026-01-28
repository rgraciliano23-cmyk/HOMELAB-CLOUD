#☁️ Homelab Cloud – Oracle Infrastructure

Homelab em nuvem provisionado na Oracle Cloud Infrastructure (OCI) utilizando o Free Tier, com foco em infraestrutura, automação, redes e serviços self-hosted, operando sem camada de virtualização adicional (ex: Proxmox).

Este repositório documenta decisões arquiteturais, parâmetros de infraestrutura e procedimentos operacionais do ambiente.

🧭 Arquitetura Geral
Modelo: Single-node cloud homelab
Tipo: IaaS (OCI Compute)
Provisionamento: Manual / futuro IaC
Virtualização: OCI Hypervisor (Paravirtualized)
Orquestração: Não aplicável (bare VM)
Gerenciamento: SSH + CLI

🌎 Localização e Domínios OCI
Parâmetro	Valor
Região	sa-saopaulo-1
Availability Domain	AD-1
Fault Domain	FD-3
Capacidade	On-Demand
Compartimento (root)

🖥️ Compute
Shape
Campo	Valor
Shape	VM.Standard.E2.1.Micro
OCPUs	1
Memória	1 GB
Network BW	0.48 Gbps
Resize	❌ Não suportado
Virtualização
Item	Configuração
Boot Mode	Paravirtualized
Firmware	UEFI_64
NIC	Paravirtualized
Volume Boot	Paravirtualized

💾 Storage
Tipo	Configuração
Boot Volume	Block Volume
Local Disk	❌ Não disponível
Criptografia em trânsito	✅ Ativada

Todos os dados persistem via OCI Block Storage.

🐧 Sistema Operacional
Campo	Valor
OS	Canonical Ubuntu
Versão	20.04 LTS
Image	Canonical-Ubuntu-20.04-2025.07.23-0

🌐 Networking
VCN
Campo	Valor
VCN	vcn-2026
Subnet	Pública
IP Público	1xx.xxx.xxx.xxx
NIC	1
Modelo de Rede

Tráfego direto via VCN pública
Sem Load Balancer
Sem NAT Gateway
Sem Bastion

🔐 Acesso e Autenticação

Acesso: SSH
Autenticação: Key-based only

🧩 Metadados da Instância
Item	Status
Instance Metadata Service	v1 + v2
Live Migration	Padrão recomendado

Utilizado para:

Bootstrap
Scripts de inicialização
Automação futura

🛡️ Segurança (Infra)
Controle	Status
Secure Boot	❌
Measured Boot	❌
TPM	❌
Encryption in Transit	✅

Segurança será reforçada em camada de SO e rede.

⚙️ Operação

Gerenciamento via SSH
Atualizações manuais
Sem painel adicional
Sem HA
Single Point of Failure assumido

📈 Limitações Conhecidas

Recursos restritos (Free Tier)
Sem escalabilidade vertical
Sem redundância
Dependência de IP público

🧭 Roadmap de Infra

 Firewall (UFW / OCI NSG)
 Hardening Ubuntu
 Docker Engine
 Docker Compose
 Reverse Proxy
 Backup externo
 Monitoramento (node-level)
 Infra as Code (Terraform OCI)

📄 Nota

Este ambiente é experimental, focado em infraestrutura, redes e automação, e não destinado a workloads críticos.
