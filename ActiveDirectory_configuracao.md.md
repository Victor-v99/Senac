# Implementação de Active Directory no Debian (Controlador de Domínio Samba AD)

O Debian não possui o Active Directory nativo da Microsoft, mas é possível implementar um **controlador de domínio compatível com Active Directory** utilizando o **Samba**. O Samba AD permite autenticação, gerenciamento de usuários, computadores e políticas de grupo, integrando máquinas Linux e Windows.

---

## 1. Pré-requisitos

- Debian atualizado (recomenda-se Debian 11 ou superior).
- Hostname configurado (exemplo: dc1).
- IP fixo configurado.
- Sincronização de horário (NTP).

---

## 2. Instalação dos Pacotes Necessários

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install samba krb5-config winbind smbclient dnsutils -y
```

Durante a instalação, algumas telas de configuração aparecerão:

- **krb5-config**: Informe o nome do seu REALM em MAIÚSCULAS (ex: EMPRESA.LOCAL).

---

## 3. Preparação do Sistema

1. **Defina o hostname e edite o hosts:**
   ```bash
   sudo hostnamectl set-hostname dc1
   sudo nano /etc/hosts
   ```
   Adicione:
   ```
   192.168.1.10   dc1.empresa.local   dc1
   ```

2. **(Opcional) Sincronize o horário:**
   ```bash
   sudo apt install chrony -y
   sudo systemctl enable --now chrony
   ```

---

## 4. Provisionamento do Domínio Active Directory

**Atenção:** O processo formata as configurações do Samba. Faça backup se já usar o Samba no sistema.

```bash
sudo systemctl stop smbd nmbd winbind
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bkp

sudo samba-tool domain provision --use-rfc2307 --interactive
```

Responda:

- **Realm:** EMPRESA.LOCAL (em maiúsculas)
- **Domain:** EMPRESA (nome curto)
- **Server Role:** domain controller
- **DNS backend:** SAMBA_INTERNAL (ou BIND9_DLZ para integração com BIND)
- **Administrator password:** Defina uma senha forte

O comando irá criar um novo arquivo `/etc/samba/smb.conf`.

---

## 5. Configuração dos Serviços

1. **Edite o arquivo `/etc/samba/smb.conf` se necessário** (normalmente o provisionamento já configura).
2. **Configure o Kerberos:**  
   Edite `/etc/krb5.conf` para garantir a configuração do Realm.
   ```
   [libdefaults]
       default_realm = EMPRESA.LOCAL
       dns_lookup_realm = false
       dns_lookup_kdc = true
   ```

---

## 6. Inicie o Samba no modo AD DC

```bash
sudo systemctl unmask samba-ad-dc
sudo systemctl enable --now samba-ad-dc
```

Desative os serviços smbd, nmbd e winbind, pois não são usados no modo AD DC:

```bash
sudo systemctl disable --now smbd nmbd winbind
```

---

## 7. Teste a Configuração

1. **Teste o Kerberos:**
   ```bash
   kinit administrator
   klist
   ```
   Deve mostrar um ticket válido.

2. **Teste o DNS interno:**
   ```bash
   host -t SRV _ldap._tcp.empresa.local.
   host -t SRV _kerberos._udp.empresa.local.
   host -t A dc1.empresa.local.
   ```

---

## 8. Adicione Computadores Windows ao Domínio

1. Configure o DNS dos clientes para apontar para o IP do controlador Samba.
2. No Windows, vá para **Configurações do Sistema > Nome do Computador > Alterar > Membro de Domínio** e insira `EMPRESA.LOCAL`.
3. Use `administrator` e a senha definida no provisionamento.

---

## 9. Gerenciamento do AD

- Usar o comando `samba-tool` para criar usuários, grupos e unidades organizacionais:
  ```bash
  samba-tool user add usuario1
  samba-tool group add grupo1
  samba-tool ou create "OU=SetorTI,DC=empresa,DC=local"
  ```

- Pode-se administrar o Samba AD também via RSAT (Remote Server Administration Tools) do Windows.

---

## 10. Referências

- [Wiki Samba AD DC](https://wiki.samba.org/index.php/Setting_up_Samba_as_an_Active_Directory_Domain_Controller)
- [Guia Debian Samba AD](https://wiki.debian.org/Samba/ActiveDirectory)