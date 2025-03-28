<?php
require_once '../db/config.php';
header('Content-Type: application/json');

if ($_SERVER['REQUEST_METHOD'] == 'GET') {
    $query = "SELECT job_posts.*, categories.name AS category_name FROM job_posts 
              JOIN categories ON job_posts.category_id = categories.id";
    $result = $connection->query($query);
    echo json_encode($result->fetch_all(MYSQLI_ASSOC));
}
?>
