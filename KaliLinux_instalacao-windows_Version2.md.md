# Instalação do Kali Linux no Windows (WSL)

Você pode instalar o Kali Linux diretamente no Windows utilizando o **Windows Subsystem for Linux (WSL)**. Isso permite rodar ferramentas do Kali nativamente no Windows, sem máquina virtual ou dual boot.

---

## 1. Pré-requisitos

- Windows 10 (versão 1903 ou superior) ou Windows 11.
- Permissões de administrador.

---

## 2. Ativando o WSL e o Subsistema de Máquina Virtual

Abra o **Prompt de Comando (CMD)** ou **PowerShell** como administrador e execute:

```powershell
wsl --install
```

Se seu Windows não reconhecer esse comando, ative o WSL manualmente:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Depois, reinicie o computador.

---

## 3. Instalando o Kali Linux pelo Microsoft Store

1. Abra a **Microsoft Store**.
2. Pesquise por **Kali Linux**.
3. Clique em **Obter/Instalar**.

Ou, pelo terminal, execute:

```powershell
wsl --install -d kali-linux
```

---

## 4. Configuração Inicial

1. Após a instalação, abra o **Kali Linux** pelo menu Iniciar.
2. Aguarde a configuração inicial.
3. Crie um usuário e senha quando solicitado.

---

## 5. Atualização e Instalação de Ferramentas

Atualize o Kali:

```bash
sudo apt update && sudo apt upgrade -y
```

Instale ferramentas conforme necessário, por exemplo:

```bash
sudo apt install nmap metasploit-framework hydra -y
```

---

## 6. Executando Aplicações Gráficas (Opcional)

No WSL2, é possível rodar ferramentas gráficas de pentest diretamente, se você estiver usando Windows 11 ou versões recentes do 10:

```bash
sudo apt install kali-desktop-xfce -y
```

Depois, basta lançar aplicativos gráficos normalmente, como:

```bash
wireshark
```

---

## 7. Dicas e Recomendações

- O WSL não tem as mesmas capacidades de acesso a hardware que uma máquina virtual ou instalação real. Ferramentas de sniffing e injeção de pacotes podem ser limitadas.
- Use sempre o Windows atualizado.
- Consulte a [documentação do Kali para WSL](https://www.kali.org/docs/wsl/) para ajustes avançados.

---

## 8. Removendo o Kali Linux

Se desejar remover:

```powershell
wsl --unregister kali-linux
```

---

## 9. Referências

- [Kali Linux no WSL - Documentação Oficial](https://www.kali.org/docs/wsl/)
- [WSL - Documentação Microsoft](https://docs.microsoft.com/pt-br/windows/wsl/)
