# Instalação e Configuração do Ansible no Debian

O **Ansible** é uma ferramenta de automação de configuração, gerenciamento e orquestração de servidores, muito utilizada para provisionamento de infraestrutura e deploy de aplicações em ambientes Linux.

---

## 1. Pré-requisitos

- Debian 11 ou 12 atualizado.
- Permissões de superusuário (root ou sudo).
- Acesso à internet.

---

## 2. Atualização do Sistema

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 3. Instalação do Ansible

O Ansible está disponível no repositório oficial do Debian, mas pode não ser a versão mais recente. Para ambientes de produção, recomenda-se usar o repositório oficial do projeto Ansible.

### a) Instalação pelo repositório do Debian

```bash
sudo apt install ansible -y
```

### b) Instalação da versão mais recente

1. Adicione o repositório oficial:
    ```bash
    sudo apt update
    sudo apt install -y software-properties-common
    sudo add-apt-repository --yes --update ppa:ansible/ansible
    ```
2. Instale o Ansible:
    ```bash
    sudo apt install ansible -y
    ```

---

## 4. Verificando a Instalação

```bash
ansible --version
```

---

## 5. Configuração Inicial

### a) Arquivo de Inventário

- O arquivo padrão é `/etc/ansible/hosts`.
- Exemplo:
    ```
    [servidores]
    192.168.1.10
    192.168.1.11

    [web]
    web01 ansible_host=192.168.1.20
    ```

### b) Acesso via SSH

- O Ansible utiliza SSH para conectar-se aos hosts remotos.
- Gere uma chave SSH e distribua para os servidores:
    ```bash
    ssh-keygen -t rsa -b 4096
    ssh-copy-id usuario@192.168.1.10
    ```

---

## 6. Testando a Conexão

```bash
ansible all -m ping
```

Se tudo estiver correto, o retorno será "pong" para cada host.

---

## 7. Execução de Comandos e Playbooks

- Para rodar um comando em todos os hosts:
    ```bash
    ansible all -m shell -a "uptime"
    ```

- Estrutura de um playbook básico (`site.yml`):
    ```yaml
    ---
    - hosts: all
      become: yes
      tasks:
        - name: Atualizar pacotes
          apt:
            update_cache: yes
    ```

    Execute com:
    ```bash
    ansible-playbook site.yml
    ```

---

## 8. Referências

- [Documentação Oficial Ansible](https://docs.ansible.com/)
- [Guia de Inventário Ansible](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
- [Playbooks Ansible](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
