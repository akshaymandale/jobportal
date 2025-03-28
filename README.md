# jobportal

USE job_portal;

-- Clear previous data in `users` table (optional)
DELETE FROM users;

-- Insert sample data into users with plain text passwords
INSERT INTO users (username, email, password, role) VALUES
('Alice2023', 'alice2023@example.com', 'password123', 'applicant'),
('BobEmployer', 'bob@example.com', 'password456', 'employer'),
('AdminUser', 'admin@example.com', 'password789', 'admin');
