# Instalação e Configuração do Servidor DHCP no Windows Server

Este guia apresenta o passo a passo para instalar e configurar o serviço DHCP no Windows Server (2016, 2019 ou 2022).

---

## 1. Pré-requisitos

- Windows Server instalado e atualizado.
- IP fixo configurado na interface de rede do servidor.
- Usuário com permissões de administrador.

---

## 2. Instalando a Função DHCP

### a) Utilizando o Server Manager (Gerenciador do Servidor)

1. Abra o **Server Manager** (Gerenciador do Servidor).
2. Clique em **Manage** > **Add Roles and Features** (Gerenciar > Adicionar Funções e Recursos).
3. Clique em **Next** até chegar em **Server Roles**.
4. Marque a opção **DHCP Server**.
5. Clique em **Next** até a tela de confirmação e então clique em **Install**.
6. Aguarde a conclusão da instalação e clique em **Close**.

### b) Utilizando o PowerShell

Você pode instalar pelo PowerShell com o comando:

```powershell
Install-WindowsFeature -Name DHCP -IncludeManagementTools
```

---

## 3. Configuração Inicial do DHCP

Após a instalação, é necessário fazer a **configuração pós-instalação**:

1. No Server Manager, clique no alerta amarelo e selecione **Complete DHCP configuration**.
2. Siga o assistente:
   - Autorize o servidor DHCP no Active Directory (recomendado em ambientes com domínio).
   - Avance e conclua.

---

## 4. Criando um Escopo DHCP

1. Abra o console **DHCP** (Server Manager > Tools > DHCP).
2. Expanda o nome do servidor, clique com o botão direito em **IPv4** e selecione **New Scope**.
3. Siga o assistente:
   - **Name:** Nome do escopo (exemplo: RedeEmpresa).
   - **Start IP Address / End IP Address:** Defina o intervalo de IPs que serão distribuídos (ex: 192.168.1.100 a 192.168.1.200).
   - **Subnet Mask:** Máscara da rede (ex: 255.255.255.0).
   - **Default Gateway:** IP do roteador (ex: 192.168.1.1).
   - **DNS Servers:** Endereço(s) IP do(s) servidor(es) DNS.
   - **WINS Servers:** (Opcional, pode deixar em branco).
   - **Activate Scope:** Ative o escopo ao final.
4. Conclua o assistente.

---

## 5. Testando o Servidor DHCP

- Em um computador cliente na mesma rede, configure a interface de rede para obter IP automaticamente.
- O cliente deve receber um IP dentro do intervalo definido.
- No servidor, você pode ver os leases ativos em **Address Leases** no console DHCP.

---

## 6. Boas Práticas

- Utilize sempre IP fixo na interface de rede do servidor DHCP.
- Evite sobreposição de faixas de IP em diferentes escopos.
- Faça backup regular das configurações do DHCP.
- Monitore os logs do serviço DHCP para identificar possíveis problemas.

---

**Referências:**
- [Microsoft Docs - Instalar e configurar o DHCP Server](https://learn.microsoft.com/pt-br/windows-server/networking/technologies/dhcp/dhcp-deploy-wps)
- [Tutorial DHCP Server Windows](https://docs.microsoft.com/pt-br/windows-server/networking/technologies/dhcp/dhcp-top)
