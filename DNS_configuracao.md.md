# Instalação e Configuração do DNS no Debian (usando BIND9)

Este guia apresenta o passo a passo para instalar e configurar um servidor DNS no Debian utilizando o BIND9, o serviço de DNS mais popular em sistemas Linux.

---

## 1. O que é DNS?

O DNS (Domain Name System) é responsável por traduzir nomes de domínio (ex: www.exemplo.com) para endereços IP. Em redes locais ou públicas, o servidor DNS facilita o acesso a serviços usando nomes amigáveis ao invés de números.

---

## 2. Instalação do BIND9

1. **Atualize o sistema**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instale o BIND9 e ferramentas relacionadas**
   ```bash
   sudo apt install bind9 bind9utils bind9-doc dnsutils -y
   ```

3. **Verifique o status do serviço**
   ```bash
   sudo systemctl status bind9
   ```
   O serviço deve estar rodando ("active (running)").

---

## 3. Configuração Básica do Servidor DNS

### a) Definindo o domínio e IP do servidor

- Exemplo de domínio: `exemplo.local`
- IP do servidor DNS: `192.168.1.10`

### b) Editando o arquivo de configuração principal

- O arquivo principal é: `/etc/bind/named.conf.options`
- Edite para definir os servidores DNS de encaminhamento (forwarders):

   ```bash
   sudo nano /etc/bind/named.conf.options
   ```

   Adicione ou edite as linhas dentro de `options`:
   ```
   options {
       directory "/var/cache/bind";
       recursion yes;
       allow-query { any; };
       forwarders {
           8.8.8.8;  // Google DNS
           8.8.4.4;
       };
       dnssec-validation auto;
       auth-nxdomain no;
       listen-on-v6 { any; };
   };
   ```

### c) Configurando Zona Directa (exemplo.local)

1. Edite o arquivo `/etc/bind/named.conf.local` e adicione:

   ```
   zone "exemplo.local" {
       type master;
       file "/etc/bind/db.exemplo.local";
   };
   ```

2. Crie o arquivo da zona baseado no modelo `db.local`:

   ```bash
   sudo cp /etc/bind/db.local /etc/bind/db.exemplo.local
   sudo nano /etc/bind/db.exemplo.local
   ```

   Exemplo de conteúdo:
   ```
   ;
   ; Zona DNS para exemplo.local
   ;
   $TTL    604800
   @       IN      SOA     ns.exemplo.local. admin.exemplo.local. (
                                 2         ; Serial
                            604800         ; Refresh
                             86400         ; Retry
                           2419200         ; Expire
                            604800 )       ; Negative Cache TTL
   ;
   @       IN      NS      ns.exemplo.local.
   ns      IN      A       192.168.1.10
   servidor IN     A       192.168.1.10
   pc1     IN      A       192.168.1.20
   ```

### d) Configurando Zona Reversa

1. Adicione em `/etc/bind/named.conf.local`:
   ```
   zone "1.168.192.in-addr.arpa" {
       type master;
       file "/etc/bind/db.192";
   };
   ```

2. Crie o arquivo da zona reversa:

   ```bash
   sudo cp /etc/bind/db.127 /etc/bind/db.192
   sudo nano /etc/bind/db.192
   ```

   Exemplo de conteúdo:
   ```
   ;
   ; Zona reversa para 192.168.1.0/24
   ;
   $TTL    604800
   @       IN      SOA     ns.exemplo.local. admin.exemplo.local. (
                                 1         ; Serial
                            604800         ; Refresh
                             86400         ; Retry
                           2419200         ; Expire
                            604800 )       ; Negative Cache TTL
   ;
   @       IN      NS      ns.exemplo.local.
   10      IN      PTR     servidor.exemplo.local.
   20      IN      PTR     pc1.exemplo.local.
   ```

---

## 4. Verificação e Reinício do Serviço

1. **Verifique a configuração do BIND9**
   ```bash
   sudo named-checkconf
   sudo named-checkzone exemplo.local /etc/bind/db.exemplo.local
   sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/db.192
   ```

2. **Reinicie o BIND9**
   ```bash
   sudo systemctl restart bind9
   ```

3. **Teste o DNS localmente**
   ```bash
   dig @localhost servidor.exemplo.local
   dig -x 192.168.1.10 @localhost
   ```

---

## 5. Configurando Clientes para usar o DNS

- Em máquinas clientes, defina o IP do servidor DNS (`192.168.1.10`) nas configurações de rede.
- Teste usando o comando `nslookup` ou `ping servidor.exemplo.local`.

---

## 6. Dicas e Segurança

- Faça backup dos arquivos de zona.
- Restrinja as permissões dos arquivos de configuração.
- Monitore os logs em `/var/log/syslog` para erros do BIND9.

---

**Referências:**
- [Documentação Oficial do BIND9](https://wiki.debian.org/Bind9)
- [Guia Completo de DNS no Debian](https://wiki.debian.org/HowTo/dns)