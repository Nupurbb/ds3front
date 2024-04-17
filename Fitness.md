---
layout: default
title: Fitness
---
<style>
    /* ... (existing styles) ... */
    /* Add styles for the table cause its better that way*/
    #DrinkTable {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    #DrinkTable th, #DrinkTable td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    #DrinkTable th {
        background-color: #CBC3E3;
    }
        /* Add styles for form labels */
    form label {
        font-weight: bold;
        margin-bottom: 5px;
    }
    /* Add styles for buttons */
    button {
        background-color: #af4c61; /* Green background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #DrinkTable button {
        background-color: #36f321; /* Blue background color */
        color: white;
        padding: 5px 10px;
        margin: 2px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
    body {
        background-color: #FAE5DE;
        font-family: 'Arial', sans-serif;
        margin: 20px;
    }
</style>

<body>
<h3>Get info on an Exercise</h3>
<form id="formToGetOneExerciseDetail">
    <label for="Exercise"><b>Exercise name to get details:</b></label>
    <input type="text" id="Exercise" name="Exercise" /><br /><br />
    <button type="submit" value="btnToGetExerciseDetail" id="get_exercise">GetExerciseDetails</button>
    <div id="getOneExerciseDetailResponse"></div>
</form>
<br><br><br>

<h3>Create a new Exercise</h3>
<form id="myForm">
    <label for="NewExercise">New Exercise name:</label>
    <input type="text" id="NewExercise" name="NewExercise" /><br /><br />
    <label for="NewCalories">Calories burned per hour:</label>
    <input type="text" id="NewCalories" name="NewCalories" /><br /><br />
    <button type="submit" value="Submit" id="create_exercise">CreateNewExercise</button>
</form>
<br><br>

<h3>All Exercises</h3>
<table id="ExerciseTable">
    <!-- Table headers go here -->
    <thead>
        <tr>
            <th>Exercise Name</th>
            <th>Calories burned per hour</th>
            <th>Edit</th>
            <th>Delete</th>
        </tr>
    </thead>
</table>

<div id="editModalBackdrop" class="modal-backdrop">
    <div id="editModal" class="modal-content">
        <button id="closeModal" class="close-modal">X</button>
        <form id="editForm">
            <label for="editExerciseName">Exercise Name:</label>
            <input type="text" id="editExerciseName" name="editExerciseName" /><br /><br />
            <label for="editCalories">Calories burned per hour:</label>
            <input type="text" id="editCalories" name="editCalories" /><br /><br />
            <input type="submit" value="Update" />
        </form>
    </div>
</div>
<body>

<script type="module">
     import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    const API_URL = 'http://127.0.0.1:8086/api/fitness';
    const createbutton = document.getElementById("create_exercise")
    createbutton.addEventListener("click", submitForm);

    const getExercisebutton = document.getElementById("get_exercise")
    getExercisebutton.addEventListener("click", getOneExercise);

    //Get all drink items
    function loadItems() {
        fetch(API_URL + "/blah", {
            method: 'GET'
            // headers: options.headers,
            //body: JSON.stringify(body),
        }) 
        .then((response) => response.json())
        .then(data => {
            displayItems(data);
        })
        .catch(error => {
            console.error("Error calling get all exercises:", error)
        });
    }

    function displayItems(items) {
        const table = document.getElementById("ExerciseTable");

        items.forEach((exercise) => {
            const row = table.insertRow();
            row.setAttribute("data-id", exercise.id);

            ["exerciseName", "calories_burned"].forEach((field) => {
                const cell = row.insertCell();
                cell.innerText = exercise[field];
            });

            const editCell = row.insertCell();
            const editButton = document.createElement("button");
            editButton.innerHTML = "Edit";
            editButton.addEventListener("click", () => editExercise(exercise.exerciseName, exercise.calories_burned));
            editCell.appendChild(editButton);

            const deleteCell = row.insertCell();
            const deleteButton = document.createElement("button");
            deleteButton.innerText = "Delete";
            deleteButton.addEventListener("click", () => deleteExercise(exercise.exerciseName, row));
            deleteCell.appendChild(deleteButton);
        });
    }

    // function to show Edit exercise form pop up
    function editExercise(exerciseName, calories_burned) {
        console.log('Edit Exercise:', exerciseName);
        
        const form = document.getElementById("editForm");

        form.querySelector("#editExerciseName").value = exerciseName;
        form.querySelector("#editCalories").value = calories_burned;

        document.getElementById("editModalBackdrop").style.display = "block"; // show pop up edit modal
    }

    function deleteExercise(exerciseName, row) {
        console.log('Delete Exercise:', exerciseName);

        const confirmation = prompt('Type "DELETE" to confirm.');
        if (confirmation === "DELETE") {
            const payload = {
                exerciseName
            };

            fetch(API_URL + "/blah", {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(payload)
            })
            .then((response) => {
                if (response.ok) {
                    console.log("Successfully deleted", exerciseName)
                    location.reload();
                    return response.json();
                } else {
                    alert("Server error");
                    throw new Error("Server");
                }
            })
        }
    }

    document.addEventListener("DOMContentLoaded", function () {
        loadItems();
        document.getElementById("closeModal").addEventListener("click", function () {
            document.getElementById("editModalBackdrop").style.display = "none"; // close pop up edit form
        });
    });

    // Create method
    function submitForm(event) {
        event.preventDefault();

        const form = document.getElementById('myForm')
        const exerciseName = form.elements['NewExercise'].value
        const calories_burned = parseInt(form.elements['NewCalories'].value)
        
        const payload = {
            exerciseName,
            calories_burned,
        };

        fetch(API_URL + "/blah", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(payload),
        })
        .then((response) => {
            if (response.ok) {
                return response.json();
            } else {
                alert("Server error");
                throw new Error("Server");
            }
        })
        .then((data) => {
            location.reload()
        })
        .catch((error) => console.error("Error:", error));
    }

    // Function to get details on one exercise
    function getOneExercise(event) {
        event.preventDefault();

        const form = document.getElementById('formToGetOneExerciseDetail')
        const exerciseName = form.elements['Exercise'].value
        const responseDiv = document.getElementById('getOneExerciseDetailResponse')

        const URL_FOR_GetOne = API_URL + "/" + exerciseName

        fetch(URL_FOR_GetOne, {
            method: "GET"
        })
        .then((response) => {
            if (response.ok) {
                return response.json();
            } else {
                alert("Server error");
                throw new Error("Server");
            }
        })
        .then((data) => {
            console.log(data)
            responseDiv.textContent =  data[0]['exerciseName'] + " has " + data[0]['calories_burned'] + " calories"
        })
        .catch((error) => console.error("Error:", error));
    }
</script>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMI Calculator</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <h1>BMI Calculator</h1>
    <form id="bmiForm">
        <label for="height">Height (m):</label>
        <input type="number" step="0.01" id="height" name="height" required><br>
        <label for="weight">Weight (kg):</label>
        <input type="number" step="0.1" id="weight" name="weight" required><br>
        <button type="submit">Calculate BMI</button>
    </form>

 <div id="bmiResult"></div>

 <h2>Most Common BMI Values</h2>
    <canvas id="bmiChart" width="400" height="200"></canvas>

<h2>BMI Categories</h2>
    <table border="1">
        <tr>
            <th>BMI Range</th>
            <th>Category</th>
            <th>Health Risk</th>
        </tr>
        <tr>
            <td>Less than 18.5</td>
            <td>Underweight</td>
            <td>Malnutrition risk</td>
        </tr>
        <tr>
            <td>18.5 - 24.9</td>
            <td>Normal weight</td>
            <td>Low risk</td>
        </tr>
        <tr>
            <td>25 - 29.9</td>
            <td>Overweight</td>
            <td>Enhanced risk</td>
        </tr>
        <tr>
            <td>30 - 34.9</td>
            <td>Obese Class I (Moderate)</td>
            <td>Medium risk</td>
        </tr>
        <tr>
            <td>35 - 39.9</td>
            <td>Obese Class II (Severe)</td>
            <td>High risk</td>
        </tr>
        <tr>
            <td>40 or greater</td>
            <td>Obese Class III (Very severe)</td>
            <td>Very high risk</td>
        </tr>
    </table>

    <script>
        document.getElementById("bmiForm").addEventListener("submit", function(event) {
            event.preventDefault();
            const height = parseFloat(document.getElementById("height").value);
            const weight = parseFloat(document.getElementById("weight").value);
            const bmi = weight / (height * height);
            document.getElementById("bmiResult").innerText = "Your BMI is: " + bmi.toFixed(2);

            // Chart.js for most common BMI values
            const ctx = document.getElementById('bmiChart').getContext('2d');
            const chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['18.5 - 24.9', '25 - 29.9', '30 - 34.9', '35 - 39.9', '40 or greater'],
                    datasets: [{
                        label: 'Most Common BMI Values',
                        data: [50, 30, 15, 5, 2], // Example data, you can replace this with actual data
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.2)',
                            'rgba(54, 162, 235, 0.2)',
                            'rgba(255, 206, 86, 0.2)',
                            'rgba(75, 192, 192, 0.2)',
                            'rgba(153, 102, 255, 0.2)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true
                            }
                        }]
                    }
                }
            });
        });
    </script>
</body>
</html>
