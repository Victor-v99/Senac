# Instalação das Ferramentas do Kali Linux no Debian

Você pode transformar seu Debian em um ambiente de pentest semelhante ao Kali Linux instalando os repositórios e ferramentas do Kali. **Atenção:** instalar ferramentas do Kali em um Debian pode trazer riscos de estabilidade e segurança, pois o Kali é voltado para testes de invasão, não para produção.

---

## 1. Pré-requisitos

- Debian 11 ou 12 atualizado.
- Permissões de root ou sudo.
- **Backup** do sistema recomendado.

---

## 2. Atualização do Sistema

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 3. Adicionando os Repositórios do Kali Linux

**Atenção:** Não é recomendado adicionar os repositórios diretamente ao `/etc/apt/sources.list` do Debian, pois isso pode corromper o sistema. O método mais seguro é instalar apenas os metapacotes de ferramentas do Kali disponíveis para Debian.

### a) Instalando o Kali Linux Tools Repository

1. Instale o pacote `kali-tools` via repositório do Kali para Debian:

```bash
echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" | sudo tee /etc/apt/sources.list.d/kali.list
```

2. Adicione a chave pública do Kali:

```bash
wget -q -O - https://archive.kali.org/archive-key.asc | sudo apt-key add -
```

3. Atualize o repositório:

```bash
sudo apt update
```

---

## 4. Instalando as Ferramentas do Kali

A instalação de todas as ferramentas do Kali (kali-linux-all) pode ocupar muito espaço. Você pode escolher grupos/metapacotes conforme sua necessidade:

- **kali-linux-top10:** As 10 principais ferramentas do Kali.
- **kali-linux-default:** Ferramentas padrão do Kali.
- **kali-linux-wireless:** Ferramentas para testes wireless.
- **kali-linux-full:** Todas as ferramentas do Kali.

### a) Exemplo - Instalando o Top 10

```bash
sudo apt install kali-linux-top10 -y
```

### b) Exemplo - Instalando o pacote padrão

```bash
sudo apt install kali-linux-default -y
```

### c) Exemplo - Instalando todas as ferramentas

```bash
sudo apt install kali-linux-full -y
```

---

## 5. Utilizando as Ferramentas

- As ferramentas instaladas ficam disponíveis normalmente via terminal.
- Use `which <nome_da_ferramenta>` ou `man <nome_da_ferramenta>` para localizar e saber mais sobre cada ferramenta.
- Use privilégios de root/sudo para executar as ferramentas de pentest.

---

## 6. Removendo o Repositório do Kali (após instalar)

Após instalar as ferramentas desejadas, recomenda-se **remover o repositório do Kali** para evitar conflitos de pacotes:

```bash
sudo rm /etc/apt/sources.list.d/kali.list
sudo apt update
```

---

## 7. Riscos e Recomendações

- **Não use em ambientes de produção.**
- Leia sobre cada ferramenta antes de usar.
- Prefira máquinas virtuais ou containers para ambientes de pentest.
- Ferramentas de segurança podem ser usadas de forma ética ou maliciosa. Use com responsabilidade!

---

## 8. Referências

- [Kali Tools Metapackages](https://www.kali.org/docs/general-use/metapackages/)
- [Documentação Oficial Kali Linux](https://www.kali.org/docs/)
- [Guia Mixando Kali e Debian](https://null-byte.wonderhowto.com/how-to/turn-debian-linux-into-kali-linux-0155201/)