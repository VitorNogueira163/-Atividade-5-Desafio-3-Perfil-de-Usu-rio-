<?php
session_start();

// Configurações do Banco de Dados
$host = "localhost";
$db_user = "root"; // Altere se o seu usuário for diferente
$db_pass = "";     // Altere se o seu banco tiver senha
$dbname = "exercicio";

$erro = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $usuario_input = trim($_POST['usuario']);
    $senha_input = $_POST['senha'];
    $cor_escolhida = $_POST['cor_fundo'];

    if (!empty($usuario_input) && !empty($senha_input)) {
        // Conexão com o banco de dados usando MySQLi
        $conn = new mysqli($host, $db_user, $db_pass, $dbname);

        if ($conn->connect_error) {
            die("Falha na conexão: " . $conn->connect_error);
        }

        // Consulta preparada para buscar o usuário
        $stmt = $conn->prepare("SELECT senha FROM usuarios WHERE usuario = ?");
        $stmt->bind_param("s", $usuario_input);
        $stmt->execute();
        $result = $stmt->get_result();

        if ($result->num_rows === 1) {
            $row = $result->fetch_assoc();
            
            // Verifica a senha (usa password_verify se a senha foi criptografada no cadastro)
            // Se você salvou em texto limpo, use: if ($senha_input === $row['senha'])
            if (password_verify($senha_input, $row['senha'])) {
                // Login bem-sucedido: Salva na sessão
                $_SESSION['usuario'] = $usuario_input;
                $_SESSION['cor_fundo'] = $cor_fundo;
                
                $stmt->close();
                $conn->close();
                
                header("Location: 15d_perfil.php");
                exit();
            } else {
                $erro = "Senha incorreta.";
            }
        } else {
            $erro = "Usuário não encontrado.";
        }

        $stmt->close();
        $conn->close();
    } else {
        $erro = "Por favor, preencha todos os campos.";
    }
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Login - Sistema de Perfil</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f4; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .login-container { background: #fff; padding: 30px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); width: 300px; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: bold; }
        .form-group input, .form-group select { width: 100%; padding: 8px; box-sizing: border-box; border: 1px solid #ccc; border-radius: 4px; }
        .btn { background-color: #007BFF; color: white; border: none; padding: 10px; width: 100%; border-radius: 4px; cursor: pointer; font-size: 16px; }
        .btn:hover { background-color: #0056b3; }
        .erro { color: red; margin-bottom: 15px; text-align: center; }
    </style>
</head>
<body>

<div class="login-container">
    <h2>Área de Login</h2>
    
    <?php if (!empty($erro)): ?>
        <div class="erro"><?php echo $erro; ?></div>
    <?php endif; ?>

    <form action="15d_login.php" method="POST">
        <div class="form-group">
            <label for="usuario">Usuário:</label>
            <input type="text" id="usuario" name="usuario" required>
        </div>
        
        <div class="form-group">
            <label for="senha">Senha:</label>
            <input type="password" id="senha" name="senha" required>
        </div>
        
        <div class="form-group">
            <label for="cor_fundo">Escolha a cor de fundo do perfil:</label>
            <select id="cor_fundo" name="cor_fundo">
                <option value="#ffffff">Branco (Padrão)</option>
                <option value="#f8d7da">Rosa Claro</option>
                <option value="#d1ecf1">Azul Claro</option>
                <option value="#d4edda">Verde Claro</option>
                <option value="#fff3cd">Amarelo Claro</option>
            </select>
        </div>
        
        <button type="submit" class="btn">Entrar</button>
    </form>
</div>

</body>
</html>
