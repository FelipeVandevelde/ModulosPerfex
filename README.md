# Perfex

O PerfexCRM é um software online de gestão de clientes, nele as empregas podem ganhar velocidade e otimizar os processos internos para prestadores de serviços.

# Modulos

Além de toda a estrutura poder ser configurada em um servidor eles possuem uma sessão onde é possível criar _modulos_.

Os modulos são sistemas/integrações de fora do Perfex que podem ser integrado a ele, além disso os modulos podem ser montados por desenvolvedores e só podem ser vendidos no site [CodeCanyon](https://codecanyon.net/?irgwc=1&clickid=SS-U0GzC3xycTow2nEyA4Rm5UksTJYx6Rxyk0k0&iradid=275988&irpid=1226883&iradtype=ONLINE_TRACKING_LINK&irmptype=mediapartner&mp_value1=&utm_campaign=af_impact_radius_1226883&utm_medium=affiliate&utm_source=impact_radius).

## Estrutura

A estrutura é baseada no CodeIgniter, que é o framework utilizado pelo Perfex, o padrão de criação de modulo consiste em:

```bash
modules/
└── nameModule/
    ├── config/
    │   └── config.php
    ├── controllers/
    │   └── NameModule.php
    ├── helpers/
    │   └── nameModule_helper.php
    ├── language/
    │   ├── english/
    │   │   └── nameModule_lang.php
    │   ├── portuguese_br/
    │   │   └── nameModule_lang.php
    ├── libraries/
    │   └── (opcional, para classes específicas)
    ├── models/
    │   └── NameModule_model.php
    ├── views/
    │   └── (suas views, ex.: manage.php, create.php)
    ├── install.php
    └── nameModule.php
```

### 1. :file_folder:`config/`

Existe para guardar arquivos de configuração do módulo, como opções padrão, constantes ou integrações.

#### 1.1 `config/config.php`

- Função: Arquivo para definir configurações padrão do módulo, como chaves de configuração ou valores padrão.
- Quando usar: Sempre que precisar de variáveis globais para o módulo.
- Exemplo:

```php
defined('BASEPATH') or exit('No direct script access allowed');

$config['nameModule_option'] = 'value';
```

### 2. `controllers/`

Existe para armazena os controladores que processam as requisições HTTP e ligam modelo + visão.

#### 2.1 `controllers/NameModule.php`

- Função: Controlador principal do módulo.
- Responsabilidade:
  - Receber as requisições HTTP.
  - Carregar modelos.
  - Definir qual view será exibida.
- Padrão: Nome da classe com a inicial maiúscula e igual ao nome da pasta.
- Exemplo:

```php
class NameModule extends AdminController {
    public function index() {
        $this->load->view('nameModule/manage');
    }
}
```

### 3. `helpers/`

Existe para funções auxiliares reutilizáveis (não orientadas a objeto).

#### 3.1 `helpers/nameModule_helper.php`

- Função: Funções auxiliares para facilitar operações comuns.
- Responsabilidade:
  - Reutilizar código.
  - Evitar duplicação de funções
- Exemplo:

```php
function nameModule_custom_function($param) {
    return strtoupper($param);
}
```

### 4. `language/`

- Função: Arquivos de tradução.
- Responsabilidade:
  - Tornar o módulo multilíngue.
  - Guardar textos exibidos ao usuário.
- Subpastas: Uma para cada idioma suportado, ex.: `english`, `portuguese_br`.
- Exemplo:

```php
$lang['nameModule_title'] = 'Módulo de Exemplo';
```

### 5. `libraries/`

- Função: Guardar classes de biblioteca específicas (ex.: APIs, processamento complexo) que não se encaixam como helpers ou modelos.
- Responsabilidade:
  - Encapsular lógicas complexas.
  - Criar componentes reutilizáveis.
- Observação: Opcional, usado apenas quando há necessidade de classes próprias.

### 6. `models/`

Existe para armazena as classes que interagem com o banco de dados.

#### 6.1 `models/NameModule_model.php`

- Função: Modelo de dados.
- Responsabilidade:
  - Interagir com o banco de dados.
  - Executar queries e lógica de negócio.
- Padrão: Nome da classe com inicial maiúscula e sufixo \_model.
- Exemplo:

```php
class NameModule_model extends App_Model {
  public function get_all() {
    return $this->db->get('nameModule_table')->result_array();
  }
}
```

### 7. `views/`

- Função: Arquivos de interface (HTML + PHP).
- Responsabilidade:
  - Exibir conteúdo para o usuário.
  - Utilizar dados enviados pelo controlador.
- Exemplo: `manage.php`, `create.php`.

### 8. `install.php`

Script de instalação do módulo. Normalmente para criar tabelas no banco, definir opções, permissões etc.

### 9. `nameModule.php`

Arquivo principal do módulo, define metadata:

```php
<?php
defined('BASEPATH') or exit('No direct script access allowed');

$name = 'Name Module';
$description = 'Descrição do módulo.';
$version = '1.0.0';
$author = 'Seu Nome';
```

Perfex usa esse arquivo para listar o módulo, ativar, desativar etc.
