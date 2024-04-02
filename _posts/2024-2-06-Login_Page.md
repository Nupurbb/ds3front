---
toc: false
comments: false
layout: post
title: Login Page
type: tangibles
courses: { compsci: {week: 2} }
permalink: /snake
---

WELCOME FRIENDS!


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Table</title>
</head>
<body>
    <table border="1">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Username</th>
                <!-- Add more table headers based on your User model fields -->
            </tr>
        </thead>
        <tbody id="table-body">
            <!-- Data will be dynamically added here -->
        </tbody>
    </table>
    <script type="module">
        const url = '{{site.baseurl}}/api/users';
        fetch(url)
            .then(response => {
                if (!response.ok) {
                    throw new Error('Failed to fetch data');
                }
                return response.json();
            })
            .then(data => {
                // Handle the data received from the backend
                displayDataInTable(data);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        function displayDataInTable(data) {
            const tableBody = document.getElementById('table-body');
            data.forEach(user => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.user_id}</td>
                    <td>${user.username}</td>
                    <!-- Add more table cells based on your User model fields -->
                `;
                tableBody.appendChild(row);
            });
        }
    </script>

</html>


    
