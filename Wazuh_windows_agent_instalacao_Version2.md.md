# Instalação e Configuração do Agente Wazuh no Windows

Este guia detalha o processo de instalação e configuração do **agente Wazuh** em sistemas Windows (Windows 10, 11, Server 2016/2019/2022), permitindo monitoramento centralizado pelo Wazuh Manager.

---

## 1. Pré-requisitos

- Servidor Wazuh Manager já instalado e acessível na rede.
- Ter o IP ou hostname do Manager.
- Permissões de administrador na máquina Windows.
- Permitir comunicação TCP (porta 1514, padrão) entre agente e Manager.

---

## 2. Download do Instalador

1. Acesse a página oficial do Wazuh para agentes Windows:  
   [https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.4.msi](https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.4.msi)  
   (Verifique a [página oficial](https://documentation.wazuh.com/current/installation-guide/installing-wazuh-agent/wazuh-agent-windows.html) para a versão mais recente)

2. Baixe o arquivo `.msi` e salve na máquina.

---

## 3. Instalação do Agente Wazuh

### a) Instalação Gráfica

1. Execute o instalador `.msi` com duplo clique (ou clique direito > Executar como administrador).
2. Clique em **Next** até a tela de configuração do Manager.
3. Insira o endereço IP ou nome do Wazuh Manager em **Wazuh Manager IP**.
4. Porta padrão: **1514** (TCP).
5. Prossiga clicando em **Next** até finalizar a instalação.

### b) Instalação via Linha de Comando (opcional)

Abra o CMD como administrador e execute:

```cmd
msiexec /i "C:\caminho\para\wazuh-agent-4.7.4.msi" /quiet WAZUH_MANAGER="IP_DO_MANAGER" WAZUH_REGISTRATION_SERVER="IP_DO_MANAGER"
```

---

## 4. Configuração Adicional

- Arquivo de configuração:  
  `C:\Program Files (x86)\ossec-agent\ossec.conf`

- Para edições avançadas, abra este arquivo no Notepad (executar como administrador).
- Garanta que a seção `<server>` contenha o endereço do Manager:

```xml
<server>
  <address>IP_DO_MANAGER</address>
  <port>1514</port>
  <protocol>tcp</protocol>
</server>
```

---

## 5. Inicializando o Serviço do Agente

1. Abra o **Prompt de Comando** (CMD) como administrador.
2. Execute:

```cmd
net start WazuhSvc
```

Ou, via interface gráfica, abra o **Services.msc**, localize **Wazuh Agent**, e clique em **Iniciar**.

---

## 6. Registro do Agente (caso necessário)

- Em alguns cenários, o Manager exige registro do agente.  
- No CMD, execute:

```cmd
"C:\Program Files (x86)\ossec-agent\agent-auth.exe" -m IP_DO_MANAGER
```
- Aguarde a confirmação de registro.

---

## 7. Testando e Validando

- No Manager (Linux), acesse o dashboard Wazuh e verifique se o agente aparece como **Active**.
- No Windows, verifique os logs em:  
  `C:\Program Files (x86)\ossec-agent\logs\ossec.log`

---

## 8. Atualização e Remoção

- Para atualizar, baixe e instale o novo `.msi` por cima da instalação existente.
- Para remover, use **Painel de Controle > Programas e Recursos** e desinstale **Wazuh Agent**.

---

## 9. Referências

- [Documentação Oficial Wazuh - Agente Windows](https://documentation.wazuh.com/current/installation-guide/installing-wazuh-agent/wazuh-agent-windows.html)
- [Wazuh - Troubleshooting Windows Agent](https://documentation.wazuh.com/current/installation-guide/installing-wazuh-agent/troubleshooting.html)
