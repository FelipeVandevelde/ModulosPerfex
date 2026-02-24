# Opções nos modulos

Módulos podem:
* Criar suas próprias opções na tabela `tbloptions`
* Usar funções para ler e inserir dados na tabela `tbloptions`
* Utilizar sistema chave-valor para armazenar e identificar as opções

## `tbloptions` 

Ele funciona como uma *tabela de configurações* no banco de dados e é bastante usado para guardar preferências, chaves de API, textos personalizados, ativar/desativar funcionalidades, etc.

Essa tabela tem basicamente duas colunas principais:
* `name` (VARCHAR): o nome da opção
* `value` (LONGTEXT): o valor da opção

### Exemplo de conteúdo

| name | value |
| :-- | :-- |
| companyname | Minha Empresa Ltda |
| default_language | portuguese |
| smtp_host | smtp.gmail.com |

## Como salvar uma opção?

Para adicionar ou atualizar uma opção, usa-se a função helper:
```php
update_option('nome_da_opcao', 'valor');
```
> Se a opção já existir, ela será atualizada. Se não existir, ela será criada.

## Como ler/pegar uma opção?

Para obter o valor de uma opção, usa-se:
```php
get_option('nome_da_opcao');
```
> Isso retornará o valor da opção, ou uma string vazia se não existir.

## Como deletar uma opção?

Se quiser remover uma opção:
```php
delete_option('nome_da_opcao');
```

## Observações importantes

* Os valores são sempre salvos como texto (string). Se você salvar um array, o Perfex irá **serializar** esse array antes de gravar no banco.
* Para arrays ou objetos, use:
```php
update_option('minha_opcao_array', serialize($meu_array));
```
E para recuperar:
```php
$meu_array = unserialize(get_option('minha_opcao_array'));
```