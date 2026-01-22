# ResolucaoPorta22
Resolvendo erro de conexão SSH com o GitHub (porta 22 bloqueada)

#Descrição

Ao tentar acessar um repositório do GitHub via SSH, pode ocorrer o erro:

ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.


Esse problema acontece quando a rede utilizada (corporativa, educacional, VPN ou ISP) bloqueia conexões SSH na porta 22, impedindo a comunicação padrão com o GitHub.

Este passo a passo mostra como:

Criar e configurar uma chave SSH no Windows

Ajustar permissões corretas do arquivo config

Configurar o SSH para usar a porta 443, alternativa permitida pela maioria das redes

Validar a conexão com o GitHub com segurança

#PASSO A PASSO:

#Verifique se o arquivo config existe

ls $env:USERPROFILE\.ssh\config

#Se não existir, crie:

New-Item -ItemType File $env:USERPROFILE\.ssh\config

#Agora rode o icacls

icacls $env:USERPROFILE\.ssh\config /inheritance:r
icacls $env:USERPROFILE\.ssh\config /grant:r "$($env:USERNAME):(R,W)"

#Edite o arquivo config
notepad $env:USERPROFILE\.ssh\config

#Altere o host:
Host github.com
  HostName ssh.github.com
  User git
  Port 443
  IdentityFile ~/.ssh/id_ed25519

#Teste
ssh -T git@github.com


