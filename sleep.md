<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sleep Data Management</title>
    <style>
        /* Styles for the whole page */
        body {
            margin: 0 20px; /* 20px margins on the sides */
            background-color: #FAE5DE; /* Pale pink background color */
            font-family: Arial, sans-serif; /* Set your preferred font */
        }
/* Styles for the input forms */
        form {
            margin-bottom: 20px;
        }
        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
        }
        input[type="number"],
        input[type="text"],
        select {
            width: calc(100% - 20px); /* Adjusted width for text boxes */
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        /* Shorten the input boxes */
        input[type="number"],
        input[type="text"] {
            width: calc(100% - 20px);
            max-width: 150px; /* Set the maximum width to 150 pixels */
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        select {
            width: calc(100% - 20px);
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        button {
            background-color: #ff4081; /* Bright pink background color */
            color: white;
            padding: 10px 20px; /* Adjust padding to create a rectangular shape */
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        /* Styles for the table */
        table {
            width: calc(100% - 40px); /* Adjusted width for the table */
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #ef85a8; /* Pale pink background color for headers */
            font-size: 14px; /* Adjust font size to prevent squishing */
            white-space: nowrap; /* Prevent text from wrapping */
        }
        /* Align the edit and delete buttons */
        .edit-delete-buttons {
            margin-top: 10px;
        }
        .edit-delete-buttons button {
            background-color: #e91e63; /* Darker pink background color */
            color: white;
            padding: 5px 10px;
            margin: 2px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h3>Input Hours of Sleep</h3>
    <form id="formToGetOneSleepDetail">
        <label for="Sleep"><b>How many hours of sleep did you get?:</b></label>
        <input type="number" id="Sleep" name="Sleep" min="0"  />
        <button type="submit" value="btnToGetSleepDetail" id="get_sleepy">See Sleep Details</button>
        <div id="getOneSleepDetailResponse"></div>
        <div id="sleepFacts"></div>
    </form> 
    <h3>All Sleep Data</h3>
    <table id="SleepTable">
        <thead>
            <tr>
                <th>Person ID</th>
                <th>Gender</th>
                <th>Age</th>
                <th>Occupation</th>
                <th>Sleep Duration (hours)</th>
                <th>Quality of Sleep</th>
                <th>Physical Activity Level</th>
                <th>Stress Level</th>
                <th>BMI Category</th>
                <th>Blood Pressure</th>
                <th>Heart Rate</th>
                <th>Daily Steps</th>
                <th>Sleep Disorder</th>
            </tr>
        </thead>
        <tbody id="sleepDataBody">
            <!-- Sleep data rows will be dynamically added here -->
        </tbody>
    </table>

    <!-- Buttons for edit and delete -->
    <div class="edit-delete-buttons">
        <button>Edit</button>
        <button>Delete</button>
    </div>

</body>
</html>

<script type="module">
        function displaySleepFacts(hoursSlept) {
            const factsContainer = document.getElementById("sleepFacts");
            factsContainer.innerHTML = ""; // Clear existing facts

            let fact = "";

            if (hoursSlept < 7) {
                // Facts about too little sleep
                const tooLittleFacts = [
                    "Lack of sleep can impair cognitive function.",
                    "Chronic sleep deprivation may increase the risk of heart disease.",
                    "Sleep deprivation can lead to mood swings and irritability.",
                    "Insufficient sleep has been linked to an increased risk of obesity and metabolic disorders due to disruptions in hormone regulation.",
                    "Prolonged sleep deprivation may elevate stress levels and contribute to the development of mental health disorders such as anxiety and depression."
                ];
                fact = tooLittleFacts[Math.floor(Math.random() * tooLittleFacts.length)];
            } else if (hoursSlept >= 7 && hoursSlept <= 9) {
                // Facts about maintaining a good sleep schedule
                const goodScheduleFacts = [
                    "Consistent sleep schedule helps regulate your body's internal clock.",
                    "Quality sleep improves memory and cognitive function.",
                    "Avoid caffeine and heavy meals before bedtime for better sleep.",
                    "Maintaining a consistent sleep schedule of 7-9 hours per night helps regulate the body's internal clock, promoting overall well-being and alertness.",
                    "Maintaining a healthy sleep duration of 7-9 hours each night is associated with a lower risk of obesity and metabolic disorders, as proper sleep contributes to hormonal balance and appetite regulation."
                ];
                fact = goodScheduleFacts[Math.floor(Math.random() * goodScheduleFacts.length)];
            } else if (hoursSlept > 9) {
                // Facts about too much sleep
                const tooMuchFacts = [
                    "Oversleeping may increase the risk of obesity and diabetes.",
                    "Excessive sleep can lead to daytime drowsiness and lethargy.",
                    "Long sleep duration may be a sign of underlying health issues.",
                    "Hey! WAKE UP!" // Special message for excessive sleep
                ];
                fact = tooMuchFacts[Math.floor(Math.random() * tooMuchFacts.length)];
            }

            factsContainer.innerHTML = `<h4>Sleep Fact:</h4><p>${fact}</p>`;
        }
        document.addEventListener("DOMContentLoaded", function () {
            // Update the base URL of your backend API
            const API_BASE_URL = 'http://127.0.0.1:8086';  // Update this with the actual base URL of your backend

            // Update the endpoint URL for sleep data
            const API_URL = API_BASE_URL + '/api/sleeps';

            // Load all sleep data when the page first loads
            loadAllSleepData();

            // Link getOneSleep to formToGetOneSleepDetail
            const getSleepButton = document.getElementById("get_sleepy"); // Update to match the button ID
            getSleepButton.addEventListener("click", getOneSleep);


        
            // Function to load all sleep data
            function loadAllSleepData() {
                fetch(API_URL, {
                    method: 'GET'
                }) 
                .then(response => response.json())
                .then(data => {
                    displayItems(data);
                    console.log("Response from getOneSleep function:", data);

                })
                .catch(error => {
                    console.error("Error calling get all sleep data:", error);
                });
            }

            // Function to display sleep data in the table
            function displayItems(items) {
                const table = document.getElementById("SleepTable");
                const tableBody = table.querySelector("tbody");
                tableBody.innerHTML = ""; // Clear existing rows before adding new ones

                items.forEach((sleep) => {
                    const row = tableBody.insertRow();
                    row.setAttribute("data-id", sleep.id);

                    // Define the mapping of data keys to table categories
                    const dataMapping = {
                        "Person ID": "id",
                        "Gender": "gender",
                        "Age": "age",
                        "Occupation": "occupation",
                        "Sleep Duration": "sleep_duration",
                        "Quality of Sleep": "quality_of_sleep",
                        "Physical Activity Level": "physical_activity_level",
                        "Stress Level": "stress_level",
                        "BMI Category": "bmi_category",
                        "Blood Pressure": "blood_pressure",
                        "Heart Rate": "heart_rate",
                        "Daily Steps": "daily_steps",
                        "Sleep Disorder": "sleep_disorder"
                    };

                    // Loop through the dataMapping object to populate table cells
                    for (const category in dataMapping) {
                        if (dataMapping.hasOwnProperty(category)) {
                            const cell = row.insertCell();
                            const value = sleep[dataMapping[category]];

                            // Format sleep duration if needed
                            if (category === "Sleep Duration") {
                                cell.innerText = `${value} hours`;
                            } else {
                                cell.innerText = value;
                            }
                        }
                    }

                    // Add edit and delete buttons
                    const editCell = row.insertCell();
                    editCell.innerHTML = '<button class="edit-button">Edit</button>'; // Assuming you have a class for edit button
                    const deleteCell = row.insertCell();
                    deleteCell.innerHTML = '<button class="delete-button">Delete</button>'; // Assuming you have a class for delete button
                });
            }
            
            function getOneSleep(event) {
                event.preventDefault();

                // Get the form element
                const form = document.getElementById('formToGetOneSleepDetail');

                // Get the input element from the form
                const sleepInput = form.elements['Sleep'];

                // Parse the input value as a float
                const sleepDuration = parseFloat(sleepInput.value);

                // Check if the parsed value is a valid float
                if (!isNaN(sleepDuration)) {
                    // Use the parsed float value
                    console.log("Sleep duration:", sleepDuration);

                    // Display sleep-related facts based on the number of hours slept
                    displaySleepFacts(sleepDuration);

                    // Update the API URL to include the sleep duration as a query parameter
                    fetch(`${API_BASE_URL}/api/sleeps?sleep_duration=${sleepDuration}`, {
                        method: "GET"
                    })
                    .then((response) => {
                        if (response.ok) {
                            return response.json();
                        } else {
                            // Handle server error or other error cases
                            console.error("Server error:", response.status);
                            throw new Error("Server error");
                        }
                    })
                    .then((data) => {
                        // Clear existing table rows
                        const table = document.getElementById("SleepTable");
                        const tableBody = table.querySelector("tbody");
                        tableBody.innerHTML = "";

                        // Display filtered data
                        displayItems(data);
                    })
                    .catch((error) => {
                        console.error("Error:", error);
                        // Display an error message to the user
                        // Example: alert("Error fetching sleep data. Please try again.");
                    });
                } else {
                    // Handle invalid input (e.g., show an error message)
                    console.error("Invalid input for sleep duration");
                }
            }


            // Function to submit sleep form data
            function submitSleepForm(event) {
                event.preventDefault();

                // Get form data
                const form = document.getElementById('formToGetOneSleepDetail');
                const sleepDuration = parseFloat(form.elements['sleepDuration'].value);
                const sleepQuality = parseInt(form.elements['sleepQuality'].value);
                const physicalActivityLevel = parseInt(form.elements['physicalActivityLevel'].value);
                const stressLevel = parseInt(form.elements['stressLevel'].value);
                const bmiCategory = form.elements['bmiCategory'].value;
                const bloodPressure = form.elements['bloodPressure'].value;
                const heartRate = parseInt(form.elements['heartRate'].value);
                const dailySteps = parseInt(form.elements['dailySteps'].value);
                const sleepDisorder = form.elements['sleepDisorder'].value;

                // Create payload
                const payload = {
                    sleepDuration,
                    sleepQuality,
                    physicalActivityLevel,
                    stressLevel,
                    bmiCategory,
                    bloodPressure,
                    heartRate,
                    dailySteps,
                    sleepDisorder
                };

                // Submit data to the server
                fetch(API_URL, {
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
                        throw new Error("Server error");
                    }
                })
                .then((data) => {
                    console.log("Sleep data submitted successfully:", data);
                    // Optionally, perform additional actions after successful submission
                })
                .catch((error) => {
                    console.error("Error:", error);
                });
            }

            
        });