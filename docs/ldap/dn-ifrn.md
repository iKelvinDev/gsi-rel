# Explicação de DN do LDAP no IFRN

!!! note "Observações"

    Uso excessivo de cabeçalhos.


### A Distinguished Name (DN) é o caminho completo de um objeto dentro do diretório LDAP. Ele representa a localização hierárquica exata de um objeto no diretório.

### O DN fornece uma descrição completa da hierarquia onde o objeto está localizado no LDAP. O DN completo no IFRN é:

```bash
CN=20221148060028,OU=Alunos,OU=DG-PAR,OU=RE,OU=IFRN,DC=ifrn,DC=local
```

## 1. DN (Distinguished Name)

### DN é a descrição completa da hierarquia do objeto dentro do diretório LDAP. No exemplo, o DN completo é:

```bash
CN=20221148060028,OU=Alunos,OU=DG-PAR,OU=RE,OU=IFRN,DC=ifrn,DC=local
```

### Esse DN identifica o usuário com o valor CN=20221148060028 dentro da hierarquia do LDAP.


## 2. RDN (Relative Distinguished Name)

### O RDN é a parte que define o objeto no seu contexto imediato. No caso, o RDN é:

```bash
CN=20221148060028
```

### Aqui, CN (Common Name) representa o nome comum do objeto, e o valor 20221148060028 é o identificador único deste objeto (usuário).


## 3. DC (Domain Component)

### O DC é utilizado para identificar os componentes do domínio da organização. No exemplo, temos 2:

```bash
DC=ifrn,DC=local
```

### Isso mostra que o objeto pertence ao domínio `ifrn.local`, que provavelmente é o domínio da rede do IFRN (Instituto Federal do Rio Grande do Norte).

## 4. OU (Organizational Unit)

### OU representa as Unidades Organizacionais dentro da hierarquia LDAP. No exemplo, temos várias OUs:

```bash
OU=Alunos
OU=DG-PAR
OU=RE
OU=IFRN
```

### Essas OUs organizam o objeto em diferentes subunidades hierárquicas, como descrito abaixo:

```bash
OU=Alunos: Unidade organizacional de estudantes.

OU=DG-PAR: Provavelmente uma subunidade relacionada ao departamento DG-PAR.

OU=RE: Outra unidade organizacional, talvez relacionada à reitoria.

OU=IFRN: Unidade principal que representa a instituição IFRN.
```

[Visualizar arquivo PDF do manuscrito](manuscrito-dn-ldap-ifrn.pdf)