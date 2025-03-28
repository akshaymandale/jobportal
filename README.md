<?php
$host = 'localhost';
$user = 'root'; // Default for XAMPP
$password = ''; // Default (should be empty for XAMPP)
$dbname = 'job_portal';

$connection = new mysqli($host, $user, $password, $dbname);
if ($connection->connect_error) {
    die("Connection failed: " . $connection->connect_error);
}
?>
