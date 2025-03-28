DROP TABLE IF EXISTS applications;
DROP TABLE IF EXISTS job_posts;
DROP TABLE IF EXISTS companies;
DROP TABLE IF EXISTS users;
DROP TABLE IF EXISTS categories;

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

INSERT INTO users (username, email, password, role) VALUES
('Alice2023', 'alice2023@example.com', 'password123', 'applicant'),
('BobEmployer', 'bob@example.com', 'password456', 'employer'),
('AdminUser', 'admin@example.com', 'password789', 'admin');

INSERT INTO categories (name) VALUES
('Software Development'),
('Marketing'),
('Data Science'),
('Design'),
('Customer Support');

INSERT INTO companies (user_id, name) VALUES
(2, 'Tech Solutions Ltd');

INSERT INTO job_posts (title, company_id, salary, location, category_id, description) VALUES
('Senior PHP Developer', 1, 80000, 'Remote', 1, 'We are looking for a Senior PHP Developer to join our team.'),
('Marketing Specialist', 1, 60000, 'New York', 2, 'Join our marketing team to promote our services.'),
('Data Analyst', 1, 70000, 'San Francisco', 3, 'Analyze data to inform business decisions.'),
('UI/UX Designer', 1, 65000, 'Chicago', 4, 'Design user-friendly interfaces for our applications.'),
('Customer Support Representative', 1, 45000, 'Remote', 5, 'Help customers with their inquiries and issues.');

INSERT INTO applications (job_post_id, applicant_id) VALUES
(1, 1),
(2, 1),
(3, 1);
