# Instalação do Samba como Controlador de Domínio do Active Directory no Alpine Linux

!!! note "Observações"

    1. Ausência de configuração do nome da máquina
    2. O pacote `samba-tools` é uma alucinação do ChatGPT
    3. Documentação incompatível com que [foi feito em sala de aula](https://asciinema.org/a/671958) e com a [Wiki do Alpine](https://wiki.alpinelinux.org/wiki/Setting_up_a_samba-ad-dc).

## Introdução

Este guia descreve como instalar e configurar o Samba como um controlador de domínio do Active Directory no Alpine Linux, com o domínio pernambuco.lab.

## Pré-requisitos

- Alpine Linux instalado.
- Acesso root ou permissões sudo.
- Conexão à Internet para baixar pacotes.

## Passo 1: Atualizar o Sistema

Atualize os repositórios e pacotes do Alpine:

```ini
apk update && apk upgrade
```

## Passo 2: Instalar Dependências

Instale os pacotes necessários para o Samba, incluindo o controlador de domínio e o Kerberos:

```ini
apk add samba samba-client samba-tools samba-dc krb5 openrc
```

## Passo 3: Configurar o Samba

### 3.1 Modificar o Arquivo /etc/hosts

Edite o arquivo /etc/hosts para incluir o hostname e o IP local:

```ini
micro /etc/hosts
```

Adicione as seguintes linhas:

```ini
127.0.0.1 localhost.localdomain localhost
10.1.1.10 pernambuco.lab pernambuco
```

### 3.2 Criar o Arquivo de Configuração

Crie um novo arquivo de configuração para o Samba:

```ini
micro /etc/samba/smb.conf
```

Adicione a seguinte configuração:

```ini
[global]
   server role = domain controller
   workgroup = pernambuco
   realm = pernambuco.lab
   netbios name = PERNAMBUCO
   passdb backend = samba4
   idmap_ldb:use rfc2307 = yes

[netlogon]
   path = /var/lib/samba/sysvol/pernambuco.lab/scripts
   read only = No

[sysvol]
   path = /var/lib/samba/sysvol
   read only = No
```

### 3.3 Criar o Banco de Dados do Samba

Inicialize o banco de dados do Samba:

```ini
samba-tool domain provision --use-rfc2307 --realm=pernambuco.lab --domain=pernambuco --adminpass='SenhaForte'
```

Substitua SenhaForte por uma senha segura.

## Passo 4: Configurar DNS

### 4.1 Adicionar o Servidor DNS

Edite o arquivo de configuração do DNS em /etc/samba/smb.conf:

```ini
micro /etc/samba/smb.conf
```

Adicione ou edite a linha:

```ini
dns forwarder = 8.8.8.8
```

### 4.2 Configuração do Kerberos

Link o arquivo krb5.conf gerado pelo Samba:

```ini
ln -sf /var/lib/samba/private/krb5.conf /etc/krb5.conf
```

### 4.3 Iniciar o Samba e o DNS

Ative os serviços do Samba:

```ini
rc-update add samba default
rc-service samba start
```

## Passo 5: Configurar o Firewall

Se você estiver usando um firewall, certifique-se de permitir o tráfego para as portas do Samba:

```ini
iptables -A INPUT -p tcp --dport 53 -j ACCEPT
iptables -A INPUT -p tcp --dport 88 -j ACCEPT
iptables -A INPUT -p tcp --dport 135 -j ACCEPT
iptables -A INPUT -p tcp --dport 139 -j ACCEPT
iptables -A INPUT -p tcp --dport 445 -j ACCEPT
iptables -A INPUT -p udp --dport 53 -j ACCEPT
iptables -A INPUT -p udp --dport 88 -j ACCEPT
iptables -A INPUT -p udp --dport 137 -j ACCEPT
iptables -A INPUT -p udp --dport 138 -j ACCEPT
```

## Passo 6: Verificar a Instalação

Verifique se o Samba está funcionando corretamente:

```ini
samba-tool domain level show
```

## Passo 7: Adicionar Usuários

Para adicionar usuários ao domínio, use o seguinte comando:

```ini
samba-tool user create nome_usuario SenhaForte
```

Substitua nome_usuario e SenhaForte pelos valores desejados.

## Conclusão

Você agora configurou um servidor Samba como controlador de domínio do Active Directory no Alpine Linux. Para mais informações, consulte a documentação oficial do Samba.

### Referências

[Samba Wiki](https://wiki.samba.org/index.php/Main_Page)

[Alpine Linux](https://wiki.alpinelinux.org/wiki/Main_Page)