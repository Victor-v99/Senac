# Instalação e Configuração do DHCP no Debian

Este guia apresenta o passo a passo para instalar e configurar o serviço DHCP no Debian Linux, utilizando o pacote `isc-dhcp-server`. O DHCP (Dynamic Host Configuration Protocol) é responsável por distribuir automaticamente endereços IP e outras configurações de rede para os clientes.

---

## 1. Instalação do Servidor DHCP

1. **Atualize o sistema:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instale o pacote do servidor DHCP:**
   ```bash
   sudo apt install isc-dhcp-server -y
   ```

---

## 2. Configuração Básica

### a) Identifique a interface de rede

- Descubra o nome da interface de rede conectada à rede onde o DHCP será oferecido:
  ```bash
  ip link
  ```
  Exemplo de interface: `eth0` ou `ens33`.

- Edite o arquivo `/etc/default/isc-dhcp-server` para definir a(s) interface(s):
  ```bash
  sudo nano /etc/default/isc-dhcp-server
  ```
  Altere a linha:
  ```
  INTERFACESv4="eth0"
  INTERFACESv6=""
  ```

### b) Edite o arquivo de configuração principal

- O arquivo principal é `/etc/dhcp/dhcpd.conf`. Faça backup antes de editar:
  ```bash
  sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bkp
  sudo nano /etc/dhcp/dhcpd.conf
  ```

- Exemplo de configuração para uma rede 192.168.1.0/24:
  ```
  # Opções globais
  option domain-name "empresa.local";
  option domain-name-servers 8.8.8.8, 8.8.4.4;

  default-lease-time 600;
  max-lease-time 7200;
  authoritative;

  # Definição do escopo (faixa de IPs)
  subnet 192.168.1.0 netmask 255.255.255.0 {
      range 192.168.1.100 192.168.1.200;
      option routers 192.168.1.1;
      option broadcast-address 192.168.1.255;
      option domain-name-servers 192.168.1.10, 8.8.8.8;
      option domain-name "empresa.local";
  }
  ```

- **Explicação dos parâmetros principais:**
  - `range`: faixa de IPs a serem distribuídos.
  - `option routers`: gateway padrão.
  - `option broadcast-address`: endereço de broadcast da rede.
  - `option domain-name-servers`: servidores DNS a serem fornecidos aos clientes.

### c) Reservando IP para um cliente específico (opcional)

Para reservar um IP para um cliente com endereço MAC conhecido:
```
host pc-fixo {
    hardware ethernet 00:11:22:33:44:55;
    fixed-address 192.168.1.50;
}
```

---

## 3. Inicie e habilite o serviço DHCP

1. **Reinicie o serviço para aplicar as configurações:**
   ```bash
   sudo systemctl restart isc-dhcp-server
   ```

2. **Verifique o status:**
   ```bash
   sudo systemctl status isc-dhcp-server
   ```

3. **Habilite para iniciar automaticamente com o sistema:**
   ```bash
   sudo systemctl enable isc-dhcp-server
   ```

---

## 4. Verificação e Testes

- Em um computador cliente na mesma rede, configure para obter IP automaticamente.
- O cliente deve receber um IP dentro do range definido.
- No Debian, verifique os leases ativos com:
  ```bash
  cat /var/lib/dhcp/dhcpd.leases
  ```

---

## 5. Logs e Solução de Problemas

- Consulte os logs em `/var/log/syslog` para mensagens do DHCP:
  ```bash
  sudo tail -f /var/log/syslog
  ```

---

## 6. Dicas de Segurança e Boas Práticas

- Permita o serviço apenas na interface correta (evite compartilhar DHCP em múltiplas redes sem necessidade).
- Use firewalls para limitar o acesso externo ao servidor DHCP.
- Faça backup regular dos arquivos de configuração.

---

**Referências:**
- [Guia Oficial ISC DHCP no Debian](https://wiki.debian.org/DHCP_Server)
- [Documentação ISC DHCP](https://kb.isc.org/docs/isc-dhcp-44-manual-pages-dhcpdconf)