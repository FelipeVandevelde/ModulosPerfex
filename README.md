# Perfex

O **PerfexCRM** é um software online de gestão de clientes, nele as empregas podem ganhar velocidade e otimizar os processos internos para prestadores de serviços.

# Modulos

Além de toda a estrutura poder ser configurada em um servidor eles possuem uma sessão onde é possível criar **modulos**.

Os modulos são sistemas/integrações de fora do Perfex que podem ser integrado a ele, além disso os modulos podem ser montados por desenvolvedores e só podem ser vendidos no site [CodeCanyon](https://codecanyon.net/?irgwc=1&clickid=SS-U0GzC3xycTow2nEyA4Rm5UksTJYx6Rxyk0k0&iradid=275988&irpid=1226883&iradtype=ONLINE_TRACKING_LINK&irmptype=mediapartner&mp_value1=&utm_campaign=af_impact_radius_1226883&utm_medium=affiliate&utm_source=impact_radius).

## Estrutura

A estrutura é baseada no [CodeIgniter](https://codeigniter.com/), que é o framework utilizado pelo Perfex.

A arquitetura padrão de um modulo consiste em:

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
    ├── uninstall.php
    └── nameModule.php
```

### 1. :file_folder:`config/`

Existe para guardar arquivos de configuração do módulo, como opções padrão, constantes ou integrações.

#### 1.1 :page_facing_up:`config/config.php`

* **Função**: Arquivo para definir configurações padrão do módulo, como chaves de configuração ou valores padrão.
* **Quando usar**: Sempre que precisar de variáveis globais para o módulo.
* **Exemplo**:

```php
defined('BASEPATH') or exit('No direct script access allowed');

$config['nameModule_option'] = 'value';
```

### 2. :file_folder:`controllers/`

Existe para armazena os controladores que processam as *requisições HTTP* e ligam `models/` + `views/`.

#### 2.1 :page_facing_up:`controllers/NameModule.php`

* **Função**: Controlador principal do módulo.
* **Responsabilidade**:
  * Receber as requisições HTTP.
  * Carregar modelos.
  * Definir qual view será exibida.
* **Padrão**: Nome da classe com a inicial maiúscula e igual ao nome da pasta.
* **Exemplo**:
```php
class NameModule extends AdminController {
    public function index() {
        $this->load->view('nameModule/manage');
    }
}
```

### 3. :file_folder:`helpers/`

Existe para funções auxiliares reutilizáveis (não orientadas a objeto).

#### 3.1 :page_facing_up:`helpers/nameModule_helper.php`

* **Função**: Funções auxiliares para facilitar operações comuns.
* **Responsabilidade**:
  * Reutilizar código.
  * Evitar duplicação de funções
* **Exemplo**:
```php
function nameModule_custom_function($param) {
    return strtoupper($param);
}
```

### 4. :file_folder:`language/`

* **Função**: Arquivos de tradução.
* **Responsabilidade**:
  * Tornar o módulo multilíngue.
  * Guardar textos exibidos ao usuário.
* **Subpastas**: Uma para cada idioma suportado, ex.: `english`, `portuguese_br`.
* **Exemplo**:
```php
$lang['nameModule_title'] = 'Módulo de Exemplo';
```

### 5. :file_folder:`libraries/`

* **Função**: Guardar classes de biblioteca específicas (ex.: APIs, processamento complexo) que não se encaixam como helpers ou modelos.
* **Responsabilidade**:
  * Encapsular lógicas complexas.
  * Criar componentes reutilizáveis.
* **Observação**: Opcional, usado apenas quando há necessidade de classes próprias.

### 6. :file_folder:`models/`

Existe para armazena as classes que interagem com o banco de dados.

#### 6.1 :page_facing_up:`models/NameModule_model.php`

* **Função**: Modelo de dados.
* **Responsabilidade**:
  * Interagir com o banco de dados.
  * Executar queries e lógica de negócio.
* **Padrão**: Nome da classe com inicial maiúscula e sufixo `\_model`.
* **Exemplo**:
```php
class NameModule_model extends App_Model {
  public function get_all() {
    return $this->db->get('nameModule_table')->result_array();
  }
}
```

### 7. :file_folder:`views/`

* **Função**: Arquivos de interface (HTML + PHP).
* **Responsabilidade**:
  * Exibir conteúdo para o usuário.
  * Utilizar dados enviados pelo controlador.
* **Exemplo**: `manage.php`, `create.php`.

### 8. :page_facing_up:`install.php`

* **Função**: Script de instalação do módulo. Normalmente para criar tabelas no banco, definir opções, permissões etc.
* **Responsabilidade**:
  * Carregar configurações e inicializações necessárias.
  * Definir hooks ou eventos.
* **Exemplo**:
```php
if (!$CI->db->table_exists(db_prefix() . 'nameModule_table')) {
    $CI->db->query('CREATE TABLE ...');
}
```

### 9. :page_facing_up:`nameModule.php`

* **Função**: Arquivo principal do módulo, define metadata:
* **Responsabilidade**:
  * Carregar configurações e inicializações necessárias.
  * Definir hooks ou eventos.
* **Exemplo**:
```php
<?php

/**
 * Garante que o arquivo init do modulo não possa ser acessado diretamente, apenas dentro do aplicação 
 */
defined('BASEPATH') or exit('No direct script access allowed');

/*
Module Name: Sample Perfex CRM Module
Description: Sample module description.
Author: FelipeVandevelde
Version: 2.3.0
Requires at least: 2.3.*
Author URI: https://github.com/FelipeVandevelde
*/

defined('BASEPATH') or exit('No direct script access allowed');

hooks()->add_action('admin_init', 'nameModule_init');

function nameModule_init() {
    $CI = &get_instance();
    $CI->load->language('nameModule');
}
```
Perfex usa esse arquivo para listar o módulo, ativar, desativar etc.