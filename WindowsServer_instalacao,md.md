# Instalação do Windows Server

Este guia apresenta o passo a passo detalhado para instalar o Windows Server (exemplo: Windows Server 2019 ou 2022), seja em uma máquina física ou virtual (como VirtualBox, VMware ou Hyper-V).

---

## 1. Download da Imagem

- Baixe a ISO de instalação do Windows Server no site oficial da Microsoft (versão de avaliação disponível em [https://www.microsoft.com/evalcenter/](https://www.microsoft.com/evalcenter/)).
- Escolha a versão adequada (Standard ou Datacenter, GUI ou Core).

## 2. Preparação da Mídia de Instalação

- Grave a ISO em um pendrive utilizando o Windows USB/DVD Download Tool, Rufus, ou similar.
- Para máquinas virtuais, apenas selecione a ISO como mídia de boot no software de virtualização.

## 3. Configuração de Boot

- Insira o pendrive ou configure a VM para iniciar pela ISO.
- Ajuste a ordem de boot na BIOS/UEFI (em hardware real) para priorizar o pendrive ou unidade de DVD.

## 4. Início da Instalação

1. Ao iniciar, pressione qualquer tecla se solicitado para “boot from CD or DVD”.
2. Selecione o idioma, formato de hora/moeda e layout do teclado. Clique em **Avançar**.
3. Clique em **Instalar agora**.

## 5. Escolha da Edição

- Selecione a edição desejada (ex: Standard com Experiência Desktop para instalar com interface gráfica).
- Clique em **Avançar**.

## 6. Aceite dos Termos

- Marque a caixa “Aceito os termos de licença” e clique em **Avançar**.

## 7. Tipo de Instalação

- Escolha **Personalizada: Instalar apenas o Windows (avançado)** para instalação limpa.

## 8. Particionamento do Disco

- Selecione o disco onde o sistema será instalado.
- Apague partições antigas se necessário.
- Crie novas partições, caso queira dividir o disco (por exemplo, para arquivos e sistema separados).
- Clique em **Avançar**. O instalador criará as partições necessárias.

## 9. Instalação

- O instalador irá copiar arquivos e reiniciar o sistema automaticamente algumas vezes.

## 10. Configuração Inicial

1. Defina a senha do usuário Administrador.
2. Clique em **Concluir**.
3. Após o boot, pressione Ctrl+Alt+Del para entrar.
4. Faça login com a senha definida para o Administrador.

## 11. Configurações Pós-Instalação

- Configure o nome do computador:
  - Clique com o botão direito em “Este Computador” > Propriedades > Alterar nome do computador.
- Configure o endereço IP (caso não use DHCP):
  - Acesse Painel de Controle > Rede e Internet > Central de Rede > Alterar adaptador.
  - Clique com o botão direito na placa de rede > Propriedades > Protocolo TCP/IP v4 > Propriedades.
  - Defina IP, Máscara, Gateway e DNS.
- Atualize o sistema pelo Windows Update.
- Instale drivers e ferramentas adicionais, se necessário.

## 12. Ativação (Opcional)

- Ative o Windows Server com uma chave válida, caso possua.
- Para ambientes de teste, o período de avaliação é suficiente.

---

## Dicas

- Tire prints das principais telas do processo para incluir na documentação.
- Anote configurações de rede, nome do host e senha do administrador.
- Utilize snapshots ou cópias da VM para facilitar testes e restauração do ambiente.

---

**Referências:**
- [Guia Oficial de Instalação do Windows Server](https://learn.microsoft.com/pt-br/windows-server/get-started/installation)