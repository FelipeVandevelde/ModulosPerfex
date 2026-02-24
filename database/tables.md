# Tabelas nos modulos

Módulos podem:
* Criar suas próprias tabelas
* Ler e escrever dados nas tabelas existentes
* Usar o Query Builder do CodeIgniter (framework base do Perfex)
* Manipular as tabelas via PHP direto com SQL ou via métodos de model/controller

## Exemplo: criando tabela ao ativar o módulo

Ao ativar seu módulo, você geralmente cria a estrutura de banco com `dbDelta()` ou comandos SQL diretos.

```php
public function activate()
{
    $CI = &get_instance();

    if (!$CI->db->table_exists(db_prefix() . 'tabela_modulo')) {
        $CI->db->query("
            CREATE TABLE `" . db_prefix() . "tabela_modulo` (
                `id` INT NOT NULL AUTO_INCREMENT,
                `nome` VARCHAR(255) NOT NULL,
                `criado_em` DATETIME NOT NULL,
                PRIMARY KEY (`id`)
            ) ENGINE=InnoDB DEFAULT CHARSET=" . $CI->db->char_set . ";
        ");
    }
}
```

> ⚠️ Use sempre `db_prefix()` para manter compatibilidade com instalações que usam prefixos personalizados nas tabelas

# Interagindo com tabelas

Você pode fazer isso de várias formas:

## Inserir dados

```php
$CI->db->insert(db_prefix() . 'tabela_modulo', [
    'nome' => 'Exemplo',
    'criado_em' => date('Y-m-d H:i:s')
]);
```

## Buscar dados

```php
$dados = $CI->db->get(db_prefix() . 'tabela_modulo')->result_array();
```

## Atualizar dados

```php
$CI->db->where('id', 1);
$CI->db->update(db_prefix() . 'tabela_modulo', ['nome' => 'Novo Nome']);
```

## Deletar dados

```php
$CI->db->delete(db_prefix() . 'tabela_modulo', ['id' => 1]);
```