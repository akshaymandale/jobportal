<?php
require_once '../db/config.php';
header('Content-Type: application/json');

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $data = json_decode(file_get_contents("php://input"));
    
    $username = $data->username;
    $password = $data->password;

    $stmt = $connection->prepare("SELECT password, role FROM users WHERE username = ?");
    $stmt->bind_param("s", $username);
    $stmt->execute();
    $stmt->store_result();

    if ($stmt->num_rows > 0) {
        $stmt->bind_result($stored_password, $role);
        $stmt->fetch();
        if ($stored_password === $password) { // Compare plain text passwords
            echo json_encode(["success" => true, "role" => $role]);
        } else {
            echo json_encode(["success" => false, "message" => "Invalid credentials"]);
        }
    } else {
        echo json_encode(["success" => false, "message" => "No user found"]);
    }
}
?>
