# Desafio 3: Perfil de Usuário com Banco de Dados (15d_desafio3)

Este projeto consiste em um sistema de autenticação (login) integrado a um banco de dados MySQL, onde o usuário valida suas credenciais e personaliza a experiência visual da sua página de perfil escolhendo uma cor de fundo dinamicamente.

## 🚀 Funcionalidades

*   **Autenticação Segura:** Validação de login diretamente na tabela `usuarios` do banco de dados `exercicio`.
*   **Persistência de Sessão:** Uso de `session_start()` para reter o estado de login e as preferências do usuário entre as páginas.
*   **Estilização Dinâmica:** Aplicação da cor de fundo escolhida pelo usuário no momento do login na página de perfil.
*   **Controle de Acesso:** Proteção de rota na página de perfil (redireciona para o login se o usuário não estiver autenticado).
*   **Logout Seguro:** Encerramento completo da sessão com destruição dos dados guardados no servidor.

## 📁 Estrutura do Projeto

O desafio é composto por 3 arquivos principais:

1.  **`15d_login.php`:** Contém o formulário de login (Usuário, Senha e Seleção de Cor), processa a validação com o banco via *Prepared Statements* e inicia a sessão.
2.  **`15d_perfil.php`:** Página restrita que exibe a mensagem de boas-vindas e injeta dinamicamente a cor de fundo escolhida no CSS.
3.  **`15d_logout.php`:** Arquivo responsável por limpar o array de sessões, destruir a sessão ativa e redirecionar o usuário com segurança.

## 🗄️ Pré-requisitos e Banco de Dados

O sistema depende do banco de dados criado no exercício anterior. Certifique-se de que o servidor local (XAMPP, WampServer, Laragon, etc.) esteja ativo e com a seguinte estrutura configurada:

*   **Banco de Dados:** `exercicio`
*   **Tabela:** `usuarios`

### Estrutura sugerida da tabela:
```sql
CREATE DATABASE IF NOT EXISTS exercicio;
USE exercicio;

CREATE TABLE IF NOT EXISTS usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    usuario VARCHAR(50) NOT NULL UNIQUE,
    senha VARCHAR(255) NOT NULL
);
```
> 💡 *Nota: O código de validação foi desenvolvido utilizando criptografia segura `password_verify()`. Certifique-se de que as senhas na tabela foram criadas utilizando `password_hash()` no script de cadastro.*

## 🔧 Como Executar o Projeto

1.  Clone ou copie os arquivos para a pasta do seu servidor local (ex: `htdocs` no XAMPP).
2.  Certifique-se de que o serviço do Apache e MySQL estejam rodando.
3.  Importe ou crie a tabela `usuarios` no seu gerenciador de banco de dados (phpMyAdmin).
4.  Insira um usuário de teste (seja pelo formulário de cadastro prévio ou manualmente via SQL).
5.  Acesse no seu navegador: `http://localhost/seu-diretorio/15d_login.php`.

## 🛠️ Tecnologias Utilizadas

*   **PHP 8.x** (Gerenciamento de sessões, conexão MySQLi e lógica de segurança)
*   **MySQL** (Persistência dos dados de usuários)
*   **HTML5 & CSS3** (Estruturação e estilização da interface)
