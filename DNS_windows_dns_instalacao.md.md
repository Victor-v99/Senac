# Instalação e Configuração do Servidor DNS no Windows Server

Este guia detalha o processo de instalação e configuração do serviço DNS no Windows Server (2016, 2019 ou 2022).

---

## 1. Pré-requisitos

- Windows Server instalado e configurado (preferencialmente como Administrador).
- IP fixo configurado na interface de rede do servidor.

---

## 2. Instalando a Função DNS

### a) Via Server Manager (Gerenciador do Servidor)

1. **Abra o Server Manager** (Gerenciador do Servidor).
2. Clique em **Manage** (Gerenciar) > **Add Roles and Features** (Adicionar Funções e Recursos).
3. Clique em **Next** até chegar em **Server Roles**.
4. Marque a opção **DNS Server**.
5. Clique em **Next** até a tela de confirmação e então clique em **Install**.
6. Aguarde a conclusão da instalação e clique em **Close**.

### b) Via PowerShell

Você também pode instalar pelo PowerShell executando:

```powershell
Install-WindowsFeature -Name DNS -IncludeManagementTools
```

---

## 3. Configurando o Servidor DNS

### a) Abrindo o Console DNS

1. No Server Manager, clique em **Tools** > **DNS** para abrir o console de gerenciamento DNS.
2. No painel esquerdo, expanda o nome do servidor.

### b) Criando uma Zona de Pesquisa Direta (Forward Lookup Zone)

1. Clique com o botão direito em **Forward Lookup Zones** > **New Zone**.
2. Siga o assistente:
   - **Zone Type:** Escolha **Primary Zone**.
   - **Zone Name:** Digite o nome do seu domínio. Exemplo: `empresa.local`
   - **Zone File:** Aceite o padrão.
   - **Dynamic Updates:** Selecione conforme a necessidade (Permitir/Negar atualizações dinâmicas).
3. Finalize o assistente.

### c) Criando uma Zona de Pesquisa Reversa (Reverse Lookup Zone)

1. Clique com o botão direito em **Reverse Lookup Zones** > **New Zone**.
2. Siga o assistente:
   - **Zone Type:** Primary Zone.
   - **Network ID:** Digite o início do seu endereço de rede (ex: para 192.168.1.0, digite 192.168.1).
   - **Zone File:** Aceite o padrão.
   - **Dynamic Updates:** Conforme a necessidade.
3. Finalize o assistente.

### d) Adicionando Registros DNS

1. Clique com o botão direito na zona criada (ex: `empresa.local`) > **New Host (A or AAAA)**.
2. Preencha:
   - **Name:** Nome do host (ex: servidor1)
   - **IP Address:** IP correspondente (ex: 192.168.1.10)
3. Clique em **Add Host**.
4. Repita para outros hosts necessários.

---

## 4. Testando o Servidor DNS

- Em um prompt de comando de outro computador na mesma rede, execute:
  ```cmd
  nslookup servidor1.empresa.local 192.168.1.10
  ```
- O resultado deve exibir o IP cadastrado para o host.

---

## 5. Configurando Clientes para Usar o DNS

- Nas estações de trabalho, configure o IP do servidor DNS como preferencial nas configurações de rede.

---

## 6. Dicas e Boas Práticas

- Sempre utilize IP fixo no servidor DNS.
- Faça backup das zonas DNS regularmente.
- Monitore os logs do serviço DNS para identificar possíveis problemas.
- Para ambientes com Active Directory, a instalação do DNS pode ser feita junto com a promoção do controlador de domínio.

---

**Referências:**
- [Documentação Oficial Microsoft - Instalar o Servidor DNS](https://learn.microsoft.com/pt-br/windows-server/networking/dns/deploy/install-a-dns-server)
- [DNS no Windows Server - Guia Completo](https://docs.microsoft.com/pt-br/windows-server/networking/dns/dns-top)
