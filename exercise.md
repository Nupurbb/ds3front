---
layout: default
title: Exercise
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
<h3>Get Actual Weight</h3>
<h3>What is the Actual Weight?</h3>
<form id="formToGetActualWeight">
    <label for="Calories Burn"><b>Calories Burn:</b></label>
    <input type="text" id="Calories Burn" name="Calories Burn" /><br /><br />
    <label for="Dream Weight"><b>Dream Weight:</b></label>
    <input type="text" id="Dream Weight" name="Dream Weight" /><br /><br />
    <label for="Age"><b>Age:</b></label>
    <input type="text" id="Age" name="Age" /><br /><br />
    <label for="Gender"><b>Gender:</b></label>
    <input type="text" id="Gender" name="Gender" /><br /><br />
    <label for="Heart Rate"><b>Heart Rate:</b></label>
    <input type="text" id="Heart Rate" name="Heart Rate" /><br /><br />
    <label for="BMI"><b>BMI:</b></label>
    <input type="text" id="BMI" name="BMI" /><br /><br />
    <label for="Exercise Intensity"><b>Exercise Intensity:</b></label>
    <input type="text" id="Exercise Intensity" name="Exercise Intensity" /><br /><br />
    <label for="Exercise"><b>Exercise:</b></label>
    <input type="text" id="Exercise" name="Exercise" /><br /><br />
    <label for="Weather Conditions"><b>Weather Conditions:</b></label>
    <input type="text" id="Weather Conditions" name="Weather Conditions" /><br /><br />
    <label for="Duration"><b>Duration:</b></label>
    <input type="text" id="Duration" name="Duration" /><br /><br />
    <button type="submit" value="btnToGetActualWeight" id="get_ActualWeight">GetActualWeight</button>
    <div id="getActualWeightResponse"></div>
</form>
<br><br><br>
<script type="module">
import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    const API_URL = uri + '/api/predict'
    const createbutton = document.getElementById("get_ActualWeight")
    createbutton.addEventListener("click", Predict);
    function Predict(event) {
        const API_URL = uri + '/api/predict'
        event.preventDefault();
        //const formData = new FormData(event.target);
        //const drink = formData.get("Drink");
        //console.log("got form data Drink")
        //const calories = formData.get("Calories");
        const form = document.getElementById('formToGetActualWeight')
        const Calories_Burn = form.elements['Calories Burn'].value
        const Dream_Weight = form.elements['Dream Weight'].value
        const Age = form.elements['Age'].value
        const Gender = form.elements['Gender'].value
        const Heart_Rate = form.elements['Heart Rate'].value
        const BMI = form.elements['BMI'].value
        const Exercise_Intensity = form.elements['Exercise Intensity'].value
        const Exercise = form.elements['Exercise'].value
        const Weather_Conditions = form.elements['Weather Conditions'].value
        const Duration = form.elements['Duration'].value
        //const Actual_Weight = parseInt(form.elements['Actual Weight'].value)
        const payload = {
            'Calories Burn': Calories_Burn,
            'Dream Weight': Dream_Weight,
            Age,
            Gender,
            'Heart Rate': Heart_Rate,
            BMI,
            'Exercise Intensity': Exercise_Intensity,
            Exercise,
            'Weather Conditions': Weather_Conditions,
            Duration
            //Actual_Weight,
        };
        console.log(JSON.stringify(payload))
        const authOptions = {
            ...options,
            method: 'POST',
            cache: 'no-cache',
            body: JSON.stringify(payload),
            headers: {
                'Content-Type' : 'application/json',
                'Access-Control-Allow-Origin': 'include'
            }
        };
        fetch(API_URL, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(payload),
        })
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                document.getElementById("error-message").innerText = errorMsg; // Display error on the screen
                return;
            } else {
                return response.json()
            }
        })
        .then((data) => {
            console.log(data)
            getActualWeightResponse.textContent =  data['actualWeight'] + " is the predicted weight "
        })
        .catch((error) => console.error("Error:", error))
    }
    </script>
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
        background-color: #F2F2F2;
    }
    /* Add styles for the background */
    body {
        background-color: #FDD8EE; /* Set your desired background color */
        font-family: Arial, sans-serif; /* Set your preferred font */
    }
    /* Adjust the modal styles */
    .modal-backdrop {
        /* ... (existing styles) ... */
    }
    .modal-content {
        /* ... (existing styles) ... */
        color: white; /* Set the text color inside the modal */
    }
    /* Add styles for form labels */
    form label {
        font-weight: bold;
        margin-bottom: 5px;
    }
    /* Add styles for buttons */
    button {
        background-color: #FF4B71; /* Green background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #DrinkTable button {
        background-color: #FDD8EE; /* Blue background color */
        color: white;
        padding: 5px 10px;
        margin: 2px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>
