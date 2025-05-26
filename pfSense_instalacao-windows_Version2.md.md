# Instalação do pfSense no Windows (Virtualização)

O **pfSense** é um sistema operacional de firewall e roteamento baseado em FreeBSD. **Ele não pode ser instalado diretamente no Windows** como um programa, mas pode ser utilizado em ambientes Windows através de virtualização (VMware, VirtualBox, Hyper-V).

Abaixo, um guia detalhado sobre como instalar o pfSense em uma máquina virtual no Windows usando o VirtualBox.

---

## 1. Pré-requisitos

- Windows 10 ou 11 (64 bits)
- Permissões de administrador
- 2 GB de RAM (mínimo recomendado), 10 GB de espaço livre em disco
- Conexão à internet
- [VirtualBox](https://www.virtualbox.org/) instalado

---

## 2. Baixe o pfSense

1. Acesse o site oficial: [https://www.pfsense.org/download/](https://www.pfsense.org/download/)
2. Selecione:
   - Arquitetura: AMD64 (x86-64)
   - Installer: ISO Installer
   - Mirror: Escolha o mais próximo de você
3. Baixe o arquivo `.iso`.

---

## 3. Criando a Máquina Virtual no VirtualBox

1. Abra o VirtualBox e clique em **Novo**.
2. Insira um nome, ex: `pfSense`.
3. Tipo: **BSD**
4. Versão: **FreeBSD (64-bit)**
5. Memória: pelo menos 2048 MB (recomendado 4096 MB).
6. Disco rígido: **Criar novo disco virtual** (mínimo 10 GB).
7. Clique em **Criar**.

---

## 4. Configurando a Rede da VM

1. Com a VM criada, selecione-a e clique em **Configurações** > **Rede**.
2. Ative o **Adaptador 1** como **Bridged Adapter** (para que o pfSense tenha acesso direto à rede, funcionando como firewall).
3. (Opcional) Ative o **Adaptador 2** como “Rede Interna” para criar uma interface LAN separada.

---

## 5. Montando o ISO do pfSense

1. Em **Configurações** > **Armazenamento**, clique no ícone do drive óptico e selecione o arquivo `.iso` do pfSense baixado.
2. Salve as configurações.

---

## 6. Instalando o pfSense

1. Inicie a VM.
2. Siga o assistente de instalação do pfSense (teclado, disco, instalação padrão, etc).
3. Após a instalação, remova o ISO do drive virtual e reinicie a VM.

---

## 7. Configuração Inicial

- Defina as interfaces WAN e LAN conforme solicitado no console do pfSense.
- Anote o IP atribuído à interface LAN.

---

## 8. Acessando a Interface Web

1. No seu navegador do Windows, acesse:  
   ```
   https://<IP_LAN_DO_PFSENSE>
   ```
2. Ignore o aviso de certificado autoassinado.
3. Usuário padrão: `admin`  
   Senha padrão: `pfsense`

---

## 9. Dicas e Recomendações

- pfSense funciona como um sistema operacional próprio, gerenciado via navegador.
- Não tente instalar pfSense “dentro” do Windows, use sempre virtualização.
- Sempre mantenha backups das configurações.
- Mantenha o pfSense e o VirtualBox atualizados para segurança e estabilidade.

---

## 10. Referências

- [Documentação Oficial pfSense](https://docs.netgate.com/pfsense/en/latest/)
- [Guia de Instalação VirtualBox](https://www.virtualbox.org/manual/ch01.html)
- [Guia Rápido pfSense](https://www.pfsense.org/download/)
