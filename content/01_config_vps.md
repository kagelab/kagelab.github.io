+++
title = "Issue #001 — Hardening de VPS Ubuntu 24.04 // Como não virar botnet"
date = 2026-04-21

[taxonomies]
categories = ["linux","infosec"]
tags = ["linux"]
 
[extra]
toc = true
+++
Como não ter sua VPS minerando crypto pra desconhecidos.

<!-- more -->

## Primeiros Passos:
### Pré-requisitos
- VPS Ubuntu 24.04
- acesso SSH

### 1) Atualizar o sistema:
```bash
$ sudo apt update && apt upgrade -y
```

### 2) Atualizar os pacotes instalados:
```bash
$ sudo apt autoremove -y
```
### 3) Criar usuário (não use o root):
```bash
# adduser hanya
# usermod -aG sudo hanya
```
Testando:
```bash
# su - hanya
```

## Configurando o Firewall
 UFW (Uncomplicated Firewall) é o firewall padrão do Ubuntu que controla quais portas do servidor podem receber conexões externas, é tipo o porteiro do prédio. Ele ajuda a proteger o servidor contra ataques e invasões.

### 1) Instalando o firewall:
```bash
$ sudo apt install ufw
```
### 2) Permitir ssh:
```bash
$ sudo ufw allow OpenSSH
```
### 3) Permitir web (para blog):
```bash
$ sudo ufw allow 80
$ sudo ufw allow 443
```
### 4) Ativando o firewall:
```bash
$ sudo ufw enable
```
### 5) Verificando o status:
```bash
$ sudo ufw status
```

{{ note(
header="O que cada comando faz?"
body='
- `OpenSSH` permite acesso remoto ao servidor.
- `Porta 80` permite acesso a sites HTTP.
- `Porta 443` permite conexões HTTPS seguras.
- O comando `ufw enable` ativa o firewall.
   '
   )}}


## Proteção contra ataques SSH:
O Fail2Ban é um script que fica de olho nos logs, quando detecta alguma tentativa repetida suspeita (tipo aquele bot que tenta advinhar sua senha SSH pra forçar login), ele bane o danado na hora no firewall.

## Instalando o Fail2Ban:
```bash
$ sudo apt install fail2ban
```
### Ativando:
```bash
# sudo systemctl enable fail2ban
```
### Iniciando:
```bash
# sudo systemctl start fail2ban
```

## Instalando o kit inicial:
```bash
# apt install git curl wget htop vim tmux 
```

{{ note(
header="Conteúdo do kit de segurança"
body='
- `git` usado para os projetos do github, controla as versões do código.
- `curl` conversa com APIs e servidores e tb serve para downloads.
- `wget` faz downloads.
- `tmux` abre vários terminais dentro de um só e nunca morre.
- `vim` é um editor de texto do terminal.
- `htop` monitora CPU/RAM em tempo real.
   '
   )}}

A rede é um campo de forças invisíveis — sofrer menos é apenas estar melhor preparado.
No fim das contas, não existe sistema seguro — apenas sistemas menos interessantes de atacar.. 



