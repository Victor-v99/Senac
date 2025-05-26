# Instalação e Configuração do Active Directory no Windows Server

Este guia detalha o processo para instalar e configurar o Active Directory Domain Services (AD DS) no Windows Server (2016, 2019 ou 2022).

---

## 1. Pré-requisitos

- Windows Server instalado, atualizado e com IP fixo configurado.
- Nome do servidor configurado (hostname) — ex: DC1.
- Usuário com permissões de administrador.

---

## 2. Instalação da Função Active Directory

### a) Utilizando o Server Manager (Gerenciador do Servidor)

1. Abra o **Server Manager**.
2. Clique em **Manage** > **Add Roles and Features** (Gerenciar > Adicionar Funções e Recursos).
3. Clique em **Next** até chegar em **Server Roles**.
4. Selecione **Active Directory Domain Services** e clique em **Add Features** se solicitado.
5. Prossiga clicando em **Next** até a tela de confirmação e clique em **Install**.
6. Aguarde a conclusão da instalação.

### b) Utilizando o PowerShell

Você pode instalar pelo PowerShell:

```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

---

## 3. Promoção do Servidor a Controlador de Domínio

Após instalar a função, é preciso promover o servidor a Controlador de Domínio:

1. No **Server Manager**, clique no alerta amarelo próximo ao topo e selecione **Promote this server to a domain controller**.
2. Escolha uma das opções:
   - **Add a new forest** (Adicionar nova floresta): caso seja o primeiro domínio.
   - **Add a domain controller to an existing domain** (Adicionar controlador a domínio existente): caso já exista um domínio.
3. Se for uma nova floresta:
   - **Root domain name**: Informe o nome do domínio (ex: empresa.local).
   - Clique em **Next**.
4. Defina as opções de domínio:
   - **Domain Controller capabilities**: mantenha selecionado DNS e GC (Global Catalog).
   - Defina a senha de **Directory Services Restore Mode (DSRM)**.
   - Clique em **Next**.
5. Prossiga aceitando os padrões sugeridos até a tela de revisão.
6. Clique em **Install** para iniciar a promoção.
7. O servidor irá reiniciar automaticamente ao final do processo.

---

## 4. Pós-instalação e Testes

1. Após reiniciar, faça login com o usuário Administrador do domínio.
2. No **Server Manager**, vá em **Tools** > **Active Directory Users and Computers** para gerenciar usuários, grupos, computadores, unidades organizacionais etc.
3. Teste a resolução DNS e a autenticação no domínio.

---

## 5. Adicionando Computadores ao Domínio

1. Em um computador cliente (Windows), abra as propriedades do sistema.
2. Clique em **Alterar configurações** > **Domínio**.
3. Insira o nome do domínio (ex: empresa.local).
4. Quando solicitado, forneça credenciais de administrador do domínio.
5. Reinicie o computador para finalizar a operação.

---

## 6. Boas Práticas

- Utilize sempre IP fixo no controlador de domínio.
- Faça backup regular do AD e das políticas de grupo.
- Monitore logs de eventos para possíveis falhas ou ataques.
- Use senhas fortes para contas administrativas.
- Realize atualizações de segurança regularmente.

---

**Referências:**
- [Documentação Oficial Microsoft - AD DS](https://learn.microsoft.com/pt-br/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-)
- [Guia Completo da Microsoft](https://learn.microsoft.com/pt-br/windows-server/identity/ad-ds/get-started/windows-server-2016-forest-and-domain-functional-levels)