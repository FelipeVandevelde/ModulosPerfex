# O que é a pasta controllers no Perfex CRM?

Dentro de um módulo personalizado no Perfex CRM, a pasta `controllers` armazena os **controladores**. Eles são arquivos PHP que:

* Recebem as requisições feitas pelo usuário (por exemplo, abrir uma página ou enviar um formulário).
* Chamam os métodos do modelo (se existir) para acessar o banco de dados.
* Carregam as `views` (páginas) para exibir para o usuário.

Ou seja, os controladores fazem a ligação entre o que o usuário faz e o que o sistema precisa fazer.

## Regras importantes

1. O nome do arquivo do controlador deve ser o mesmo nome do módulo, com a primeira letra maiúscula.
2. A classe dentro do arquivo também deve ter o mesmo nome e estender a classe `AdminController` (ou `ClientsController` se for voltado para o cliente).
3. Os métodos públicos na classe serão as rotas (URLs) que os usuários podem acessar.

# Exemplo prático: Controlador MeuModulo.php

Vamos supor que estamos criando um módulo chamado `MeuModulo.php`
Caminho: `modules/MeuModulo/controllers/MeuModulo.php`
```php
<?php

defined('BASEPATH') or exit('No direct script access allowed');

class MeuModulo extends AdminController
{
    public function __construct()
    {
        parent::__construct();

        // Carrega um model se necessário
        $this->load->model('meumodulo_model');
    }

    // Página principal do módulo
    public function index()
    {
        // Definindo o título da página
        $data['title'] = 'Meu Módulo - Página Inicial';

        // Carregando a view (página) que será exibida
        $this->load->view('meumodulo/index', $data);
    }

    // Exemplo de função para salvar um dado
    public function salvar()
    {
        $dados = $this->input->post();

        if ($this->meumodulo_model->salvar_dados($dados)) {
            set_alert('success', 'Dados salvos com sucesso!');
        } else {
            set_alert('danger', 'Erro ao salvar os dados.');
        }

        redirect(admin_url('meumodulo'));
    }
}
```