<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Job Portal</title>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            loadJobListings();

            document.getElementById('login-form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const username = document.getElementById('username').value;
                const password = document.getElementById('password').value;

                const response = await fetch('api/authenticate.php', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({ username, password }),
                });

                const result = await response.json();
                console.log(result);
                if (result.success) {
                    alert(`Logged in as ${result.role}`);
                } else {
                    alert(result.message);
                }
            });
        });

        async function loadJobListings() {
            const response = await fetch('api/job_posts.php');
            const jobs = await response.json();
            const jobList = document.getElementById('job-list');

            jobs.forEach(job => {
                const li = document.createElement('li');
                li.innerText = `${job.title} - ${job.location} - $${job.salary}`;
                jobList.appendChild(li);
            });
        }
    </script>
</head>
<body>
    <h1>Job Portal</h1>
    
    <div id="login">
        <h2>Login</h2>
        <form id="login-form">
            <input type="text" id="username" name="username" placeholder="Username" required>
            <input type="password" id="password" name="password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>
    </div>

    <div id="job-posts">
        <h2>Job Listings</h2>
        <ul id="job-list"></ul>
    </div>
</body>
</html>
