# Instalação do Debian Linux

Este guia descreve detalhadamente o processo de instalação do Debian Linux em uma máquina virtual ou física. Os passos aqui apresentados são baseados na versão Debian 12 "Bookworm", mas servem para outras versões recentes.

---

## 1. Download da Imagem

- Acesse o site oficial: [https://www.debian.org/distrib/](https://www.debian.org/distrib/)
- Baixe a imagem ISO adequada (ex: amd64 netinst para sistemas 64 bits).

## 2. Preparação da Mídia de Instalação

- Grave a imagem em um pendrive com o [Rufus](https://rufus.ie/) (Windows) ou use o comando `dd` (Linux).
- Ou configure a ISO como CD/DVD em sua máquina virtual (VirtualBox, VMware).

## 3. Inicialização

- Configure a ordem de boot no BIOS/UEFI ou nas opções da VM para iniciar pela mídia de instalação.
- Reinicie o computador ou inicie a VM.

## 4. Início da Instalação

1. Na tela inicial, escolha: **Install** ou **Graphical Install**.
2. Selecione o idioma (ex: Português do Brasil).
3. Escolha o país e o layout do teclado.

## 5. Configuração de Rede

1. Defina o hostname (nome do computador).
2. Caso solicitado, informe o domínio (pode deixar em branco).
3. Configure a rede (DHCP automático ou manual, conforme o ambiente).

## 6. Configuração de Usuário

1. Defina a senha do usuário root (administrador).
2. Crie um usuário padrão, defina nome e senha.

## 7. Particionamento de Disco

1. Escolha o método de particionamento:
   - Usar disco inteiro (mais simples para testes)
   - Manual (para ambientes de produção)
2. Selecione o disco.
3. Defina as partições:
   - `/` (raiz)
   - `swap` (memória virtual, opcional)
   - `/home` (opcional)
4. Confirme as alterações e permita que o instalador formate e utilize o disco.

## 8. Instalação do Sistema Base

- Aguarde a cópia dos arquivos essenciais do sistema.

## 9. Gerenciador de Pacotes

1. Configure o espelho (mirror) de repositórios.
2. Decida se deseja participar do envio de informações de uso do Debian (opcional).

## 10. Seleção de Software

- Escolha o ambiente gráfico (GNOME, KDE, XFCE, etc) ou nenhum, se deseja apenas o modo servidor.
- Mantenha "utilitários de sistema padrão" selecionado.

## 11. Instalação do GRUB (Bootloader)

- Instale o GRUB no disco principal (ex: /dev/sda).
- Confirme a instalação do carregador de boot.

## 12. Finalização

- Aguarde a conclusão.
- Remova a mídia de instalação ao reiniciar.

## 13. Primeiro Boot

- O sistema irá iniciar. Faça login com o usuário criado.
- Atualize o sistema com:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

---

## Dicas

- Tire prints das principais telas para documentar o processo.
- Anote configurações manuais, especialmente de rede.
- Para ambientes de produção, planeje bem as partições.

---

**Referências:**
- [Documentação oficial Debian](https://www.debian.org/releases/stable/installmanual)