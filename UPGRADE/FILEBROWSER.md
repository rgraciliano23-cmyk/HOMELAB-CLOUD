# ☁️ Cloud File Server - Oracle Cloud Infrastructure

![Ubuntu](https://img.shields.io/badge/OS-Ubuntu_22.04_LTS-orange) ![Oracle Cloud](https://img.shields.io/badge/Provider-Oracle_Cloud-red) ![FileBrowser](https://img.shields.io/badge/App-FileBrowser-green) ![Status](https://img.shields.io/badge/Status-Deployed-success)

## 🎯 Objetivo do Projeto
Implantar uma solução de armazenamento em nuvem pessoal (semelhante ao Google Drive ou Dropbox), utilizando uma instância gratuita (Free Tier) na **Oracle Cloud Infrastructure (OCI)**. 
O objetivo foi criar um ambiente seguro, persistente e acessível via navegador para upload e download de arquivos remotamente.

## 🛠️ Tech Stack & Ferramentas
* **Infraestrutura:** Oracle Cloud VM (Compute Instance).
* **Sistema Operacional:** Ubuntu Server 22.04 LTS.
* **Aplicação:** [FileBrowser](https://filebrowser.org/) (Gerenciador de arquivos web leve).
* **Gerenciamento de Processos:** Systemd (Linux).
* **Segurança & Redes:** IPTables (Netfilter), OCI Security Lists (VCN Firewall), SSH.

## 🚀 Etapas de Implementação

### 1. Provisionamento e Acesso
* Criação da instância VM na Oracle Cloud.
* Configuração de chaves SSH e conversão de formatos (`.key`/`.ppk`) para acesso via **PuTTY** e **Termius** (Mobile).
* *Desafio:* Identificação do usuário padrão da imagem (`ubuntu` vs `opc`).

### 2. Configuração do Servidor de Aplicação
Instalação do **FileBrowser** via script automatizado e configuração inicial do banco de dados local.
* Definição de porta de escuta (`8080`).
* Criação de usuários administrativos com senhas fortes.

### 3. Persistência de Serviço (Daemon)
Para garantir que a aplicação não fosse encerrada ao fechar o terminal SSH, configurei um serviço no **Systemd**.

# /etc/systemd/system/filebrowser.service
[Unit]
Description=File Browser
After=network.target

[Service]
User=ubuntu
Group=ubuntu
ExecStart=/usr/local/bin/filebrowser -d /home/ubuntu/filebrowser.db

[Install]
WantedBy=multi-user.target
4. Engenharia de Tráfego e Firewall (Camadas de Segurança)
Um dos maiores desafios foi a liberação de portas, que exigiu configuração em duas camadas distintas:

Nível do SO (Linux): Configuração do iptables para aceitar conexões TCP na porta 8080, forçando a regra para o topo da lista (INPUT 1) para evitar conflitos com regras de "Drop All" pré-existentes.

Nível da Nuvem (Oracle VCN): Configuração da Ingress Rule na Security List da VCN, permitindo tráfego 0.0.0.0/0 na porta de destino 8080.


⚡ Desafios e Soluções (Troubleshooting)
Durante o projeto, enfrentei e resolvi problemas técnicos reais:
Problema
                               
Connection Timeout
O site não carregava mesmo com o serviço rodando. Identifiquei que havia uma regra de bloqueio no Firewall do Linux tendo prioridade sobre a regra de liberação.
Solução aplicada 
Forcei a inserção da regra de liberação no topo da cadeia do IPTables (sudo iptables -I INPUT 1 ...) e corrigi a Ingress Rule no painel da Oracle.                               


Erro 500 (Permissão)
Ao tentar upload, o servidor retornava "Internal Server Error". A análise de logs mostrou que o app estava tentando escrever na raiz do sistema (/), onde o usuário ubuntu não tem permissão.
Solução aplicada
Isolei o ambiente criando um diretório dedicado (/home/ubuntu/MeusArquivos) e reconfigurei o escopo do FileBrowser para atuar apenas dentro deste diretório (config set --root).                               


Serviço Caindo                               
O app fechava ao encerrar a sessão SSH.                               
Solução aplicada                               
Implementação de serviço via systemd para execução em background e reinício automático.



📚 Aprendizados
Este projeto reforçou conhecimentos em:

Administração de sistemas Linux (CLI, Permissões, Systemd).
Conceitos de redes em nuvem (VCN, Subnets, Portas, Firewalls).
Diagnóstico de problemas (Log analysis e testes de conectividade).
Segurança básica de servidores (Usuários não-root, gerenciamento de chaves SSH).                               
                               
                               
