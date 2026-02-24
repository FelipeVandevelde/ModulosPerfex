# Database (Banco de dados)

O banco de dados do **Perfex CRM** utiliza **MySql** e possuí uma estrutura bem organizada, as tabelas normalmente utilizam `tbl` como prefixo mas podem ser alterado pelos administradores, para garantir que o modulo desenvolvido funcione **independente** de como o banco esteja prefixado é recomendado o uso da função `db_prefix()` para obter o prefixo correto.

## Exemplos de tabela padrão

|Tabela | Finalidade |
| :-- | :-- |
| `tblclients` | Armazena os dados dos clientes |
| `tblstaff` | Armazena os dados dos colaboradores |
| `tblinvoices` | Armazena as faturas |
| `tblprojects` | Armazena os projetos |
| `tbloptions` | Armazena as configurações globais |
| `tblcustomfields` | Campos personalizados |

### Relacionamento

Perfex não usa chaves estrangeiras no banco de forma explícita, mas usa campos como clientid, staffid, invoiceid, etc., para criar relacionamentos entre tabelas.

## Como módulos usam o banco de dados?

Os módulos possuem duas formas de armazenar informações, usando ***tabelas*** e usando ***opções***.

### Tabelas

* Use quando você precisa armazenar:
  * Vários registros (listas, itens, histórico, logs, etc.)
  * Dados dinâmicos inseridos por usuários ou sistemas
  * Relacionamentos entre entidades (clientes, projetos, tarefas...)
  * Informações estruturadas com vários campos

#### Exemplos

| Situação | Usar tabela? |
| :-- | :-- |
| Lista de tarefas do seu módulo | ✅ SIM |
| Histórico de sincronizações | ✅ SIM |
| Campos personalizados dos clientes | ✅ SIM |
| Comentários, uploads, notificações | ✅ SIM |

### Opções

* Use quando você precisa:
  * Armazenar configurações fixas ou únicas do módulo
  * Ter uma chave-valor simples
  * Guardar preferências do sistema
  * Definir parametrizações gerais (ex: API key, texto padrão)

#### Exemplos

| Situação | Usar tabela? |
| :-- | :-- |
| Ativar/desativar módulo | ✅ SIM |
| Chave de API externa | ✅ SIM |
| Texto padrão de notificação | ✅ SIM |
| Limite máximo de itens | ✅ SIM |

### Qual usar?

* Use **TABELA** quando:
  * Você precisa de um **CRUD** (criar, ler, atualizar, deletar dados).
  * Você armazena **informações variáveis ou históricas**.
  * O usuário irá interagir e adicionar/remover dados.
* Use **OPTION** quando:
  * Você tem **valores fixos/configurações** que controlam o comportamento do módulo.
  * Você precisa de **algo simples e rápido** de recuperar com `get_option()`.
  * O valor **não depende de múltiplos registros ou usuários**.

### Exemplo prático

**Cenário**:
Você está criando um módulo de notificações personalizadas.

**Usaria**:
* **Tabela**: para guardar cada notificação enviada (com título, destinatário, data, etc.)
* **Option**: para guardar se o módulo está ativo ou desativado (notificacoes_ativas = 1), ou o texto padrão de saudação.