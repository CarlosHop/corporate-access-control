# 🔐 Corporate Linux Access Control & RBAC Architecture

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![Security](https://img.shields.io/badge/Security-ACL%20%7C%20RBAC-blue?style=for-the-badge)

Projeto de automação em **Shell Script (Bash)** para provisionamento, padronização e auditoria de controle de acesso baseado em funções (**RBAC**) em servidores Linux corporativos. 

Utiliza isolamento de diretórios e **ACLs (Access Control Lists)** com herança padrão (*default ACLs*) para garantir conformidade com políticas de segurança da informação, princípio do menor privilégio e **LGPD**.

---

## 🏗️ Arquitetura de Diretórios

```text
/srv/corporate/
├── 📁 executivos/     # Acesso exclusivo C-Level
├── 📁 rh/             # Folha, contratos e dados sensíveis (LGPD)
├── 📁 financeiro/     # Contabilidade, fiscal e fluxo de caixa
├── 📁 ti/             # Infraestrutura, logs, backups e segurança
├── 📁 comercial/      # CRM, propostas e tabelas comerciais
├── 📁 operacional/    # Processos internos, SOPs e produção
└── 📁 publico/        # Mural e arquivos compartilhados entre toda a empresa
```
---

## 🏢 Matriz de Grupos e Permissões (RBAC)

| Departamento / Área | Grupo Linux | Diretório Alvo | Permissões | Regras de Visibilidade |
| :--- | :--- | :--- | :---: | :--- |
| **Diretoria e Executivos** | `grp_executivos` | `/srv/corporate/executivos` | `rwx` | Acesso total ao próprio diretório + leitura (`r-x`) nos demais departamentos. |
| **Recursos Humanos & DP** | `grp_rh` | `/srv/corporate/rh` | `rwx` | Isolado. Acesso apenas ao próprio grupo e leitura para Diretoria. |
| **Financeiro e Contabilidade**| `grp_financeiro` | `/srv/corporate/financeiro` | `rwx` | Isolado. Acesso apenas ao próprio grupo e leitura para Diretoria. |
| **TI e Segurança** | `grp_ti` | `/srv/corporate/ti` | `rwx` | Isolado. Acesso administrativo aos recursos de TI e leitura para Diretoria. |
| **Comercial e Vendas** | `grp_comercial` | `/srv/corporate/comercial` | `rwx` | Isolado. Acesso apenas ao próprio grupo e leitura para Diretoria. |
| **Operacional e Produção** | `grp_operacional` | `/srv/corporate/operacional` | `rwx` | Isolado. Acesso apenas ao próprio grupo e leitura para Diretoria. |
| **Geral (Todos)** | *Todos os grupos* | `/srv/corporate/publico` | `rwx` | Espaço colaborativo comum a todos os colaboradores. |

---

## 📋 Pré-requisitos

* Sistema operacional Linux (Debian, Ubuntu, RHEL, Rocky Linux, AlmaLinux).
* Privilégios de superusuário (`root` ou `sudo`).
* Pacote `acl` instalado (o script realiza a instalação automaticamente caso não detecte).

---

## 🚀 Instalação e Uso

1. **Clone o repositório:**
   ```bash
   git clone [https://github.com/CarlosHop/corporate-access-control.git](https://github.com/CarlosHop/corporate-access-control.git)
   cd corporate-access-control
   ```

2. **Conceda permissão de execução aos scripts:**
   ```bash
   chmod +x setup_structure.sh audit_permissions.sh
   ```

3. **Execute o script de provisionamento:**
   ```bash
   sudo ./setup_structure.sh
   ```

4. **Valide as permissões aplicadas (Auditoria):**
   ```bash
   sudo ./audit_permissions.sh
   ```

---

## 🛠️ Gerenciamento no Dia a Dia

Para adicionar um novo colaborador e vinculá-lo ao departamento correspondente:

```bash
# Criar usuário sem diretório home pessoal (apenas acesso de rede/arquivos) ou padrão:
sudo useradd -m -s /bin/bash nome.sobrenome

# Adicionar o usuário ao grupo do departamento (ex: Financeiro):
sudo usermod -aG grp_financeiro nome.sobrenome

# Verificar grupos do usuário:
id nome.sobrenome
```
---

## 📂 Estrutura do Repositório

```text
.
├── setup_structure.sh    # Script principal de criação de grupos, pastas e ACLs
├── audit_permissions.sh  # Script de leitura e validação das regras aplicadas
└── README.md             # Documentação técnica do projeto
```
---
