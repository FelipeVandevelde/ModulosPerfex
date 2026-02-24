# Hooks (ganchos)

Os ganchos seguem um padrão semelhante ao do **WordPress**, baseado no conceito de ***event-driven programming***.

Eles são pontos no código onde você pode "enganchar" funções adicionais para executar em determinados momentos, como:
* Ao carregar a página do painel.
* Antes ou depois de inserir registros no banco.
* Durante processos importantes como envio de email ou geração de faturas.

Vantagens:
* Evita modificar arquivos principais.
* Facilita atualizações futuras.
* Permite criar módulos robustos e dinâmicos.

## Utiliza

Para utilizar os ganchos precisamos seguir alguns passos:

### 1. Definir uma função

Essa função será executada quando o gancho for chamado.
```php
function my_custom_function($args) {
    log_activity('My Custom Function Executed');
}
```

### 2. Registrar função no gancho

Usa-se a função `hooks()->add_action()` ou `hooks()->add_filter()`.
```php
hooks()->add_action('after_customer_login', 'my_custom_function');
```

## Como funciona

Sempre que uma função for "enganchada" o perfex internamente está armazenando essa referencia em um array ou objeto gerenciador de ganchos.
```php
hooks()->add_action('admin_init', 'my_function');
```

Quando for o momento o perfex procura todas as funções associadas a `'admin_init'` e executa cada uma delas
```php
hooks()->do_action('admin_init');
```

## Tipos de funções

Para criar um gancho é preciso utilizar uma função de definição, no Perfex CRM nos temos **dois** tipos que são **_action_** e **_filter_**

| Action                                              | Filter                                                    |
| :-------------------------------------------------- | :-------------------------------------------------------- |
| Utiliza `hooks()->add_action()`                     | Utiliza `hooks()->add_filter()`                           |
| Executa uma função em determinado ponto do sistema. | Executa uma função e permite modificar o valor retornado. |
| Não precisa retornar nada.                          | Deve sempre retornar um valor.                            |

### Sintaxe padrão

#### Para Action
```php
hooks()->add_action('hook_name', 'function_name', $priority = 10);
```

#### Para Filter
```php
hooks()->add_filter('hook_name', 'function_name', $priority = 10);
```

## Criar ganchos

O Perfex já possuí alguns [ganchos principais](./principais_ganchos.md) que podem ser utilizados, porém para modulos mais complexos e criações mais robustas muitas das vezes é preciso criar novos ganchos.

Como esses serão ganchos criados por desenvolvedores fora do Perfex é preciso tomar cuidado e prestar atenção com o gatilho também, sem ele a sua função não fará nada.

Em qualquer ponto do código que for executado pelo fluxo do Perfex, você pode colocar seu `do_action` ou `apply_filters`.
Isso vai criar um novo ponto de **extensão**.


| Aspecto | `do_action` | `apply_filters` | 
| :-- | :-- | :-- |
| Objetivo | Executar uma ou mais funções associadas a um evento, sem alterar dados.| Executar funções associadas a um evento e permitir modificar um valor antes de retorná-lo.
| Retorno | Não retorna nada. Apenas executa as funções. | Sempre retorna o valor final após todas as funções aplicarem modificações. |
| Quando usar? | Quando quer permitir que outros códigos executem algo adicional (ex.: log, notificação) sem precisar de retorno. | Quando quer permitir que outros módulos modifiquem ou transformem um valor ou dado. |
| Sintaxe | `hooks()->do_action('nome_do_gancho', $param1, $param2);` | `$valor_modificado = hooks()->apply_filters('nome_do_filtro', $valor_original, $param2);` |
| Função registrada com | `hooks()->add_action('nome_do_gancho', 'minha_funcao');` | `hooks()->add_filter('nome_do_filtro', 'minha_funcao');` | 
| Execução sequencial | Sim, todas as funções registradas são executadas na ordem de prioridade. | Sim, todas as funções são executadas na ordem de prioridade e podem alterar o valor antes de passar para a próxima. |
| Uso típico | Logs, notificações, integrações, carregamento de arquivos extras. | Alterar textos, modificar arrays, customizar configurações, mudar templates de e-mail. |
| Função callback | Recebe parâmetros, mas não precisa retornar nada. | Recebe o valor, deve retornar o valor (mesmo que não altere). |

### Exemplo de gancho

1. Defina o gatilho:
    ```php
    public function minha_funcao_personalizada() {
        // Código principal
        $data = ['chave' => 'valor'];
        
        // Gatilho do seu gancho
        hooks()->do_action('antes_de_processar_algo', $data);

        // Continua o processamento
    }
    ``` 
1. "engata" uma função:
    ```php
    // Criação do gancho
    hooks()->add_action('antes_de_processar_algo', 'minha_funcao_interceptadora');

    // Criação da função vinculada ao gancho
    function minha_funcao_interceptadora($data) {
        log_activity('Interceptou: '.json_encode($data));
    }
    ``` 

### Exemplo de filtro

1. Defina o ponto de modificação:
    ```php
    public function processa_valor($valor) {
        // Aciona o filtro passando um valor que será modificado
        $valor = hooks()->apply_filters('modifica_valor_customizado', $valor);
        return $valor;
    }
    ``` 
1. Quem quiser modifica:
    ```php
    // Criação do gancho de filtro
    hooks()->add_filter('modifica_valor_customizado', 'meu_filtro');
    
    // Criação da função vinculada ao gancho
    function meu_filtro($valor) {
        return strtoupper($valor);
    }
    ``` 

### Boas praticas

* **Use nomes claros e descritivos**: `antes_de_enviar_email`, `depois_de_processar_pagamento`.
* **Documente bem**: outros desenvolvedores saberão onde e como podem "enganchar".
* Prefira criar ganchos em pontos **antes** ou **depois** de **processos importantes**, não no meio de lógicas críticas.
* Se possível, use **filters** para permitir modificação de dados e **actions** para execução paralela.