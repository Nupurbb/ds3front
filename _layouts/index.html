---
layout: default
title: Login
---
# Login Page
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #fce4ec; /* Light pink background */
            color: #333; /* Dark text color */
        }
        .container {
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background-color: #fff; /* White container background */
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Shadow effect */
            text-align: center;
        }
        input[type="text"],
        input[type="password"],
        button {
            padding: 10px; /* Add padding to inputs and button */
            margin: 5px; /* Add margin to inputs and button */
            border-radius: 5px; /* Add border radius */
            border: none; /* Remove default border */
        }
        button {
            background-color: #ff4081; /* Pink button background */
            color: #fff; /* White button text color */
            cursor: pointer; /* Change cursor to pointer on hover */
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #ff80ab; /* Lighter pink on hover */
        }
        a {
            color: #ff4081; /* Set link color to pink */
            text-decoration: none; /* Remove underline */
        }
        #error-message {
            margin-top: 10px; /* Add some space above error message */
            color: red; /* Set error message color to red */
        }
    </style>
</head>


<form action="javascript:login_user()">
    <label for="uid"><b>Username</b></label>
    <input type="text" id="uid" placeholder="Enter Username" name="uid" required>
    <label for="password"><b>Password</b></label>
    <input type="password" id="password" placeholder="Enter Password" name="password" required>
    <button class='button'>Log in</button>
    <div>
    <span class="psw">Need an account? <a href="{{site.baseurl}}/signup"> Sign Up</a></span>
    <span class="psw2">Want to delete an account? <a href="{{site.baseurl}}/delete"> Delete</a></span>
    </div>
    <div>
    <div id="error-message" style="color: red;"></div>
    <!-- <span class="psw">Want to delete? <a href="{{site.baseurl}}/delete"> Delete</a></span> -->
    </div>

</form>
<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    function login_user(){
        const url = uri + '/api/users/authenticate';
        const body = {
            uid: document.getElementById("uid").value,
            password: document.getElementById("password").value,
        };
        const authOptions = {
            ...options, 
            method: 'POST', 
            cache: 'no-cache', 
            body: JSON.stringify(body)
        };
        fetch(url, authOptions)
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                document.getElementById("error-message").innerText = errorMsg; // Display error on the screen
                return;
            }
            window.location.href = "{{site.baseurl}}/welcome";
        })
        .catch(err => {
            console.error(err);
        });
    }
    window.login_user = login_user;

</script>

</html>
