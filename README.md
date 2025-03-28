-- First, drop the existing tables if they exist (ensure no data is lost if this is a production environment)
DROP TABLE IF EXISTS applications;
DROP TABLE IF EXISTS job_posts;
DROP TABLE IF EXISTS companies;
DROP TABLE IF EXISTS users;
DROP TABLE IF EXISTS categories;

-- Now create the tables again

CREATE DATABASE IF NOT EXISTS job_portal;
USE job_portal;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    role ENUM('applicant', 'employer', 'admin') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE companies (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL UNIQUE,
    name VARCHAR(255) NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users (id) ON DELETE CASCADE
);

CREATE TABLE job_posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    company_id INT NOT NULL,
    salary DECIMAL(10, 2) NOT NULL,
    location VARCHAR(100) NOT NULL,
    category_id INT NOT NULL,
    description TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (company_id) REFERENCES companies (id) ON DELETE CASCADE,
    FOREIGN KEY (category_id) REFERENCES categories (id) ON DELETE CASCADE
);

CREATE TABLE applications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    job_post_id INT NOT NULL,
    applicant_id INT NOT NULL,
    status ENUM('pending', 'reviewed', 'accepted', 'rejected') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (job_post_id) REFERENCES job_posts (id) ON DELETE CASCADE,
    FOREIGN KEY (applicant_id) REFERENCES users (id) ON DELETE CASCADE
);

USE job_portal;

-- Clear previous data in `users` table (optional)
DELETE FROM users;

-- Insert sample data into users with plain text passwords
INSERT INTO users (username, email, password, role) VALUES
('Alice2023', 'alice2023@example.com', 'password123', 'applicant'),
('BobEmployer', 'bob@example.com', 'password456', 'employer'),
('AdminUser', 'admin@example.com', 'password789', 'admin');

-- Clear previous data in `categories` table (optional)
DELETE FROM categories;

-- Insert sample data into categories
INSERT INTO categories (name) VALUES
('Software Development'),
('Marketing'),
('Data Science'),
('Design'),
('Customer Support');

-- Clear previous data in `companies` table (optional)
DELETE FROM companies;

-- Insert sample data into companies
INSERT INTO companies (user_id, name) VALUES
(2, 'Tech Solutions Ltd');

-- Clear previous data in `job_posts` table (optional)
DELETE FROM job_posts;

-- Insert sample data into job posts
INSERT INTO job_posts (title, company_id, salary, location, category_id, description) VALUES
('Senior PHP Developer', 2, 80000, 'Remote', 1, 'We are looking for a Senior PHP Developer to join our team.'),
('Marketing Specialist', 2, 60000, 'New York', 2, 'Join our marketing team to promote our services.'),
('Data Analyst', 2, 70000, 'San Francisco', 3, 'Analyze data to inform business decisions.'),
('UI/UX Designer', 2, 65000, 'Chicago', 4, 'Design user-friendly interfaces for our applications.'),
('Customer Support Representative', 2, 45000, 'Remote', 5, 'Help customers with their inquiries and issues.');

-- Clear previous data in `applications` table (optional)
DELETE FROM applications;

-- Insert sample data into applications
INSERT INTO applications (job_post_id, applicant_id) VALUES
(1, 1), -- Alice applies for Senior PHP Developer
(1, 1), -- Alice applies again for the same job (for demonstration)
(2, 1), -- Alice applies for Marketing Specialist
(3, 1); -- Alice applies for Data Analyst





//



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

----------------------------xxx--------------


Insert Categories:
First, ensure the categories table has the appropriate categories before you insert jobs.

CopyReplit
DELETE FROM categories; -- Clear existing data (optional)

INSERT INTO categories (name) VALUES
('Software Development'), -- ID 1
('Marketing'),            -- ID 2
('Data Science'),         -- ID 3
('Design'),               -- ID 4
('Customer Support');     -- ID 5
Insert Companies:
Then, verify that the companies table contains a valid company record that will be referenced by company_id in job_posts.

CopyReplit
DELETE FROM companies; -- Clear existing data (optional)

INSERT INTO companies (user_id, name) VALUES
(2, 'Tech Solutions Ltd'); -- Make sure user_id 2 exists in users table
Finally, Insert Job Posts
Once the categories and companies entries are in place, you should be able to insert job posts without issues:

CopyReplit
DELETE FROM job_posts; -- Clear existing data (optional)

INSERT INTO job_posts (title, company_id, salary, location, category_id, description) VALUES
('Senior PHP Developer', 1, 80000, 'Remote', 1, 'We are looking for a Senior PHP Developer to join our team.'),
('Marketing Specialist', 1, 60000, 'New York', 2, 'Join our marketing team to promote our services.'),
('Data Analyst', 1, 70000, 'San Francisco', 3, 'Analyze data to inform business decisions.'),
('UI/UX Designer', 1, 65000, 'Chicago', 4, 'Design user-friendly interfaces for our applications.'),
('Customer Support Representative', 1, 45000, 'Remote', 5, 'Help customers with their inquiries and issues.');
