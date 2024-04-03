---
layout: default
comments: true
title: Pulse
---
<body>
<div>
    <h1>Welcome to the Pulse page!</h1>
    <p>Here you can store information about your heartrate and how much you exercise!!</p>
</div>

<div>
    <div>
        <h2>Enter your pulse number to see on average how much exercise people do to have that heartrate</h2>
        <form id="formToGetOnePulseDetail">
            <label for="Pulse">Pulse number to get details:</label><br />
            <input type="text" id="Pulse" name="Pulse"><br />
            <button type="submit" value="btnToGetPulseDetail" id="get_pulsey">Submit</button>
        </form>
    </div>
</div>

<div>
    <div>
        <h2>Add a new pulse number</h2>
        <form id="myForm">
            <label for="Pulse">What is your pulse rate now?</label><br />
            <input type="text" id="Pulse" name="Pulse"><br />
            <label for="Exercise">What level of exercise did you just do? - 1 for none, 2 for mild, 3 for extreme</label><br />
            <input type="text" id="Exercise" name="Exercise"><br />
            <button type="submit" value="Submit" id="create_pulsey" >Submit</button>
        </form>
    </div>
</div>

<div>
    <h2>Pulse Data from all users</h2>
    <table id="PulseTable">
        <thead>
            <tr>
                <th>Pulse Number</th>
                <th>Exercise</th>
                <th>Edit</th>
                <th>Delete</th>
            </tr>
        </thead>
    </table>
</div>

<div id="editModalBackdrop" class="modal-backdrop">
	<div id="editModal" class="modal-content">
		<button id="closeModal" class="close-modal">X</button>
		<form id="editForm">
			<label for="editActive";">Pulse Number</label><br />
			<input type="text" id="editActive" name="editActive"><br />
            <label for="editExercise";">Exercise</label><br />
			<input type="text" id="editExercise" name="editExercise"><br />
			<input type="submit" value="Update">
		</form>
	</div>
</div>
</body>

<style>
    /* ... (existing styles) ... */
    /* Add styles for the table */
    #PulseTable {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    #PulseTable th, #PulseTable td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    #PulseTable th {
        background-color: #f2f2f2;
    }
    /* Add styles for the background */
    body {
        background-color: #FAE5DE; /* Light pink background color */
        font-family: Arial, sans-serif; /* Set your preferred font */
        text-align: center
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
        background-color: #ff66cc; /* Pink background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #PulseTable button {
        background-color: #ff3399; /* Dark pink background color */
        color: white;
        padding: 5px 10px;
        margin: 2px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>

<style>
	.modal-backdrop {
		display: none;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(255, 192, 203, 0.7); /* Light pink background color */
		z-index: 1;
	}

	.modal-content {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #ff66cc; /* Pink background color */
		padding: 40px;
		z-index: 2;
	}

	.close-modal {
		position: absolute;
		top: 10px;
		right: 10px;
		cursor: pointer;
		background: none;
		border: none;
		font-size: 24px;
		color: white;
	}

	.wrapper,
	section {
		max-width: 900px;
	}
</style>

<script type="module">
    // Importing URI and options from config.js file
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';

    // Defining API_URL using imported URI
    const API_URL = uri + '/api/pulses/';

    // Getting create button element and adding event listener to it
    const createbutton = document.getElementById("create_pulsey");
    createbutton.addEventListener("click", submitForm);

    // Getting get pulse button element and adding event listener to it
    const getPulsebutton = document.getElementById("get_pulsey");
    getPulsebutton.addEventListener("click", getOnePulse);

    // Function to load all pulse items
    function loadItems() {
        fetch(API_URL, {
            method: 'GET'
            // headers: options.headers,
            //body: JSON.stringify(body),
        })
        .then((response) => response.json())
        .then(data => {
            displayItems(data);
        })
        .catch(error => {
            console.error("Error calling get all pulses:", error);
        });
    }

    // Function to display pulse items
    function displayItems(items) {
        const table = document.getElementById("PulseTable");

        items.forEach((pulse) => {
            const row = table.insertRow();
            row.setAttribute("data-id", pulse.id);

            ["Active", "Exercise"].forEach((field) => {
                const cell = row.insertCell();
                cell.innerText = pulse[field];
            });

            const editCell = row.insertCell();
            const editButton = document.createElement("button");
            editButton.innerHTML = "Edit";
            editButton.addEventListener("click", () => editPulse(pulse.Active, pulse.Exercise));
            editCell.appendChild(editButton);

            const deleteCell = row.insertCell();
            const deleteButton = document.createElement("button");
            deleteButton.innerText = "Delete";
            //deleteButton.addEventListener("click", () => deletePulse(pulse.id, row));
            deleteButton.addEventListener("click", () => deletePulse(pulse.Active,row));
            deleteCell.appendChild(deleteButton);
        })
        .catch((error) => {
            console.error('Error:', error);
        });
    }

    // Function to show edit pulse form pop up
    function editPulse(Active, Exercise) {
        console.log('Edit Pulse:', Active);
        
        const form = document.getElementById("editForm");

        form.querySelector("#editActive").value = Active;
        form.querySelector("#editExercise").value = Exercise;

        document.getElementById("editModalBackdrop").style.display = "block"; // show pop up edit modal
    }

    // Function to delete pulse
    function deletePulse(Active, row) {
        console.log('Delete Pulse:', Active);

        const confirmation = prompt('Type "DELETE" to confirm.');
        if (confirmation === "DELETE") {
            const payload = {
                Active
            };
            console.log(JSON.stringify(payload));
            
            fetch(API_URL, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(payload)
            })
            .then((response) => {
                if (response.ok) {
                    console.log("Successfully deleted",Active);
                    location.reload();
                    return response.json();
                } else {
                    alert("Server error");
                    throw new Error("Server");
                }
            })
        }

        // Remove the row from the table
        //row.remove();
    }

    // Fetch users and ensure close modal interaction
    document.addEventListener("DOMContentLoaded", function () {
        loadItems();
        document.getElementById("closeModal").addEventListener("click", function () {
            document.getElementById("editModalBackdrop").style.display = "none"; // close pop up edit form
        });
    });

    // Function to submit form
    function submitForm(event) {
        event.preventDefault();

        const form = document.getElementById('myForm')
        const Active = form.elements['Pulse'].value
        const Exercise = parseInt(form.elements['Exercise'].value)
        
        const payload = {
            Active,
            Exercise,
        };
        console.log(JSON.stringify(payload));

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
                throw new Error("Server");
            }
        })
        .then((data) => {
            location.reload();
        })
        .catch((error) => console.error("Error:", error));
    }

    // Function to get details on one pulse
    function getOnePulse(event) {
        event.preventDefault();

        const form = document.getElementById('formToGetOnePulseDetail')
        const Active = form.elements['Pulse'].value
        const responseDiv = document.getElementById('getOnePulseDetailResponse')

        const URL_FOR_GetOne = API_URL + Active;

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
            console.log(data);
            responseDiv.textContent =  data[0]['Active'] + " has " + data[0]['exercise'] + " exercise";
        })
        .catch((error) => console.error("Error:", error));
    }
</script>

<style>
/* Add styles for markdown content */
body {
    background-color: #ffe6f2; /* Light pink background color */
    font-family: Arial, sans-serif; /* Set your preferred font */
    color: #661a33; /* Dark pink text color */
}

/* Style the headers */
h1, h2, h3 {
    color: #ff66cc; /* Dark pink header color */
}

/* Style the bullet points */
ul {
    list-style-type: none;
    padding-left: 0;
}

/* Style the list items */
li::before {
    content: "•";
    color: #ff66cc; /* Dark pink bullet color */
    font-weight: bold;
    display: inline-block;
    width: 1em;
    margin-left: -1em;
}

/* Add a border around the content */
.container {
    border: 2px solid #ff66cc; /* Dark pink border color */
    border-radius: 15px; /* Rounded corners */
    padding: 20px; /* Add padding */
    margin-bottom: 20px; /* Add margin */
}
</style>

### Enhance Your Pulse Experience

Taking care of your pulse and exercise routine is crucial for maintaining a healthy lifestyle. Here are some tips to help you make the most of your pulse data:

1. **Monitor Your Pulse Effectively**: During workouts, keep an eye on your pulse to ensure you're exercising within a safe and effective range. Use your pulse readings as a guide to adjust the intensity of your workouts and avoid overexertion.

2. **Maintain a Healthy Heart Rate**: Understanding your target heart rate zone can help you optimize your workouts for maximum benefit. Aim to keep your pulse within 50-85% of your maximum heart rate during exercise to improve cardiovascular fitness and endurance.

3. **Optimize Exercise Intensity**: Adjust your exercise intensity based on your pulse readings and fitness goals. Incorporate a mix of moderate and vigorous activities into your routine to challenge your cardiovascular system and improve overall health.

4. **Stay Informed**: Educate yourself about the relationship between pulse, exercise, and overall health. Explore reputable resources to learn more about cardiovascular fitness, pulse monitoring techniques, and the importance of regular physical activity.

By paying attention to your pulse and incorporating these tips into your routine, you can better manage your exercise regimen and support your long-term health and well-being.