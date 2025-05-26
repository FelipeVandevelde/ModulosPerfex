# Principais ganchos

Esses são alguns dos principais ganchos do Perfex, por já serem ganchos padrões não é preciso se preocupar muito com o seu acionamento.

## admin_init

* **Função**: Executado  quando qualquer página administrativa do Perfex é carregada.
* **Sintaxe**:
```php
hooks()->add_action('admin_init', 'callback_function');
```
* **Exemplo prático**:
```php
function my_custom_admin_function() {
    log_activity('Admin area accessed');
}
hooks()->add_action('admin_init', 'my_custom_admin_function');
```

## customers_navigation_end

* **Função**: Adiciona itens no menu lateral do cliente (área do cliente).
* **Sintaxe**:
```php
hooks()->add_action('customers_navigation_end', 'my_custom_client_nav');
```
* **Exemplo prático**:
```php
function my_custom_client_nav() {
    echo '<li><a href="'.site_url('my_custom_page').'">Custom Page</a></li>';
}
hooks()->add_action('customers_navigation_end', 'my_custom_client_nav');
```

## after_client_login

* **Função**: Executado após um cliente fazer login.
* **Sintaxe**:
```php
hooks()->add_action('after_client_login', 'my_custom_after_login');
```
* **Exemplo prático**:
```php
function my_custom_after_login($client) {
    log_activity('Client '.$client['userid'].' logged in');
}
hooks()->add_action('after_client_login', 'my_custom_after_login');
```

## after_staff_login

* **Função**: Executado após um membro da equipe (staff) fazer login.
* **Sintaxe**:
```php
hooks()->add_action('after_staff_login', 'my_staff_login_log');
```
* **Exemplo prático**:
```php
function my_staff_login_log($staff_id) {
    log_activity('Staff ID '.$staff_id.' logged in');
}
hooks()->add_action('after_staff_login', 'my_staff_login_log');
```

## before_client_logout

* **Função**: Executado imediatamente antes de um cliente realizar logout.
* **Sintaxe**:
```php
hooks()->add_action('before_client_logout', 'my_custom_logout_function');
```
* **Exemplo prático**:
```php
function my_custom_logout_function($client_id) {
    log_activity('Client ID '.$client_id.' is logging out');
}
hooks()->add_action('before_client_logout', 'my_custom_logout_function');
```

## before_staff_logout

* **Função**: Executado antes de um staff realizar logout.
* **Sintaxe**:
```php
hooks()->add_action('before_staff_logout', 'my_staff_logout_log');
```
* **Exemplo prático**:
```php
function my_staff_logout_log($staff_id) {
    log_activity('Staff ID '.$staff_id.' is logging out');
}
hooks()->add_action('before_staff_logout', 'my_staff_logout_log');
```

## before_cron_run

* **Função**: Executado antes de cada execução do cron do Perfex.
* **Sintaxe**:
```php
hooks()->add_action('before_cron_run', 'my_cron_hook');
```
* **Exemplo prático**:
```php
function my_cron_hook() {
    log_activity('Cron job is about to run');
}
hooks()->add_action('before_cron_run', 'my_cron_hook');
```

## after_cron_run

* **Função**: Executado depois que o cron é executado.
* **Sintaxe**:
```php
hooks()->add_action('after_cron_run', 'my_after_cron_hook');
```
* **Exemplo prático**:
```php
function my_after_cron_hook() {
    log_activity('Cron job has finished');
}
hooks()->add_action('after_cron_run', 'my_after_cron_hook');
```

## before_send_email_template (FILTER)

* **Função**: Permite **alterar** dados do e-mail antes de ser enviado.
* **Sintaxe**:
```php
hooks()->add_filter('before_send_email_template', 'my_email_filter');
```
* **Exemplo prático**:
```php
function my_email_filter($email) {
    $email['subject'] = '[Modified] '.$email['subject'];
    return $email;
}
hooks()->add_filter('before_send_email_template', 'my_email_filter');
```

## before_invoice_added

* **Função**: Executado **antes** de adicionar uma fatura.
* **Sintaxe**:
```php
hooks()->add_action('before_invoice_added', 'my_pre_invoice_function');
```
* **Exemplo prático**:
```php
function my_pre_invoice_function($invoice_data) {
    log_activity('An invoice is about to be added');
}
hooks()->add_action('before_invoice_added', 'my_pre_invoice_function');
```

## after_invoice_added

* **Função**: Executado **depois** que uma fatura é criada.
* **Sintaxe**:
```php
hooks()->add_action('after_invoice_added', 'my_post_invoice_function');
```
* **Exemplo prático**:
```php
function my_post_invoice_function($invoice_id) {
    log_activity('Invoice ID '.$invoice_id.' has been created');
}
hooks()->add_action('after_invoice_added', 'my_post_invoice_function');
```

## app_admin_head

* **Função**: Adiciona scripts ou estilos no `<head>` do painel administrativo.
* **Sintaxe**:
```php
hooks()->add_action('app_admin_head', 'my_custom_admin_css');
```
* **Exemplo prático**:
```php
function my_custom_admin_css() {
    echo '<style>.my-custom-style { color: red; }</style>';
}
hooks()->add_action('app_admin_head', 'my_custom_admin_css');
```

## app_admin_footer

* **Função**: Adiciona scripts ou elementos no rodapé do painel administrativo.
* **Sintaxe**:
```php
hooks()->add_action('app_admin_footer', 'my_custom_admin_footer');
```
* **Exemplo prático**:
```php
function my_custom_admin_footer() {
    echo '<script>alert("Bem-vindo ao admin!");</script>';
}
hooks()->add_action('app_admin_footer', 'my_custom_admin_footer');
```

## app_customers_head

* **Função**: Adiciona conteúdo no `<head>` da área do cliente.
* **Sintaxe**:
```php
hooks()->add_action('app_customers_head', 'my_custom_client_css');
```
* **Exemplo prático**:
```php
function my_custom_client_css() {
    echo '<style>.client-custom-style { background: #000; }</style>';
}
hooks()->add_action('app_customers_head', 'my_custom_client_css');
```

## app_customers_footer

* **Função**: Adiciona conteúdo no rodapé da área do cliente.
* **Sintaxe**:
```php
hooks()->add_action('app_customers_footer', 'my_custom_client_footer');
```
* **Exemplo prático**:
```php
function my_custom_client_footer() {
    echo '<script>alert("Olá, cliente!");</script>';
}
hooks()->add_action('app_customers_footer', 'my_custom_client_footer');
```

## after_ticket_created

* **Função**: Executado após a criação de um ticket de suporte.
* **Sintaxe**:
```php
hooks()->add_action('after_ticket_created', 'my_ticket_created_function');
```
* **Exemplo prático**:
```php
function my_ticket_created_function($ticket_id) {
    log_activity('Ticket ID '.$ticket_id.' has been created');
}
hooks()->add_action('after_ticket_created', 'my_ticket_created_function');
```

## before_ticket_status_changed

* **Função**: Executado antes de alterar o status de um ticket.
* **Sintaxe**:
```php
hooks()->add_action('before_ticket_status_changed', 'my_ticket_status_change');
```
* **Exemplo prático**:
```php
function my_ticket_status_change($data) {
    log_activity('Ticket ID '.$data['ticketid'].' status will change to '.$data['status']);
}
hooks()->add_action('before_ticket_status_changed', 'my_ticket_status_change');
```

## before_project_added

* **Função**: Executado antes de criar um projeto.
* **Sintaxe**:
```php
hooks()->add_action('before_project_added', 'my_before_project_add');
```
* **Exemplo prático**:
```php
function my_before_project_add($project_data) {
    log_activity('About to add project: '.$project_data['name']);
}
hooks()->add_action('before_project_added', 'my_before_project_add');
```

## after_project_added

* **Função**: Executado depois que um projeto é adicionado.
* **Sintaxe**:
```php
hooks()->add_action('after_project_added', 'my_after_project_add');
```
* **Exemplo prático**:
```php
function my_after_project_add($project_id) {
    log_activity('Project ID '.$project_id.' was added');
}
hooks()->add_action('after_project_added', 'my_after_project_add');
```

## after_payment_added

* **Função**: Executado depois que um pagamento é registrado.
* **Sintaxe**:
```php
hooks()->add_action('after_payment_added', 'my_after_payment_hook');
```
* **Exemplo prático**:
```php
function my_after_payment_hook($payment_id) {
    log_activity('Payment ID '.$payment_id.' has been added');
}
hooks()->add_action('after_payment_added', 'my_after_payment_hook');
```