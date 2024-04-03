---
layout: default
title: Drink
---

<h3>Get nutritional label on a Drink</h3>
<form id="formToGetOneDrinkDetail">
    <label for="Drink"><b>Drink name to get details:</b></label>
    <input type="text" id="Drink" name="Drink" /><br /><br />
    <button type="submit" value="btnToGetDrinkDetail" id="get_drinky">GetDrinkDetails</button>
    <div id="getOneDrinkDetailResponse"></div>
</form>
<br><br><br>


<h3>Create a new Drink</h3>
<form id="myForm">
    <label for="Drink">New Drink name:</label>
    <input type="text" id="Drink" name="Drink" /><br /><br />
    <label for="Calories">Calories:</label>
    <input type="text" id="Calories" name="Calories" /><br /><br />
    <button type="submit" value="Submit" id="create_drinky">CreateNewDrink</button>
</form>
<br><br>

<h3>All Drinks</h3>
<table id="DrinkTable">
    <!-- Table headers go here -->
    <thead>
        <tr>
            <th>Drink Name</th>
            <th>Calories</th>
            <th>Edit</th>
            <th>Delete</th>
        </tr>
    </thead>
</table>

<div id="editModalBackdrop" class="modal-backdrop">
	<div id="editModal" class="modal-content">
		<button id="closeModal" class="close-modal">X</button>
		<form id="editForm">
			<label for="editDrinkName">Drink Name:</label>
			<input type="text" id="editDrinkName" name="editDrinkName" /><br /><br />
            <label for="editCalories">Calories:</label>
			<input type="text" id="editCalories" name="editCalories" /><br /><br />
			<input type="submit" value="Update" />
		</form>
	</div>
</div>
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
        background-color: #f2f2f2;
    }
    /* Add styles for the background */
    body {
        background-color: #e0e0e0; /* Set your desired background color */
        font-family: Comic Sans MS, cursive; /* Set your preferred font */
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
        background-color: #4caf50; /* Green background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #DrinkTable button {
        background-color: #2196F3; /* Blue background color */
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
		background-color: rgba(0, 0, 0, 0.7);
		z-index: 1;
	}

	.modal-content {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #272726;
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
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';

    const API_URL = uri + '/api/drinks/'
    const createbutton = document.getElementById("create_drinky")
    createbutton.addEventListener("click", submitForm);

    const getDrinkbutton = document.getElementById("get_drinky")
    getDrinkbutton.addEventListener("click", getOneDrink);

    //Get all drink items
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
            console.error("Error calling get all drinks:", error)
        });
    }

    function displayItems(items) {
            const table = document.getElementById("DrinkTable");

            items.forEach((drink) => {
                const row = table.insertRow();
                row.setAttribute("data-id", drink.id);

                ["drinkName", "calories"].forEach((field) => {
                    const cell = row.insertCell();
                    cell.innerText = drink[field];
                });

                const editCell = row.insertCell();
                const editButton = document.createElement("button");
                editButton.innerHTML = "Edit";
                editButton.addEventListener("click", () => editDrink(drink.drinkName, drink.calories));
                editCell.appendChild(editButton);

                const deleteCell = row.insertCell();
                const deleteButton = document.createElement("button");
                deleteButton.innerText = "Delete";
                //deleteButton.addEventListener("click", () => deleteDrink(drink.id, row));
                deleteButton.addEventListener("click", () => deleteDrink(drink.drinkName,row));
                deleteCell.appendChild(deleteButton);
            })
            .catch((error) => {
            console.error('Error:', error);
        });
    }

    // function to show Edit drink form pop up
    function editDrink(drinkName, calories) {
        // Implement edit functionalitydrinkName based on the drinkId
        console.log('Edit Drink:', drinkName);
        
        const form = document.getElementById("editForm");

		form.querySelector("#editDrinkName").value = drinkName;
		form.querySelector("#editCalories").value = calories;

        document.getElementById("editModalBackdrop").style.display = "block"; // show pop up edit modal
    }

    function deleteDrink(drinkName, row) {
        // Implement delete functionality based on the drinkId
        console.log('Delete Drink:', drinkName);

        const confirmation = prompt('Type "DELETE" to confirm.');
		if (confirmation === "DELETE") {
            const payload = {
                drinkName
            };
            console.log(JSON.stringify(payload))
            
            fetch(API_URL, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(payload)
            })
            .then((response) => {
                if (response.ok) {
                    console.log("Successfully deleted",drinkName)
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

    //create method
    function submitForm(event) {

        event.preventDefault();
        //const formData = new FormData(event.target);
        //const drink = formData.get("Drink");
        //console.log("got form data Drink")
        //const calories = formData.get("Calories");

        const form = document.getElementById('myForm')
        const drinkName = form.elements['Drink'].value
        const calories = parseInt(form.elements['Calories'].value)
        
        const payload = {
            drinkName,
            calories,
        };
        console.log(JSON.stringify(payload))

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
            location.reload()
        })
        .catch((error) => console.error("Error:", error));
    }

    //function to get details on one drink
    function getOneDrink(event) {
        event.preventDefault();
        //const formData = new FormData(event.target);
        //const drink = formData.get("Drink");
        //console.log("got form data Drink")
        //const calories = formData.get("Calories");

        const form = document.getElementById('formToGetOneDrinkDetail')
        const drinkName = form.elements['Drink'].value
        const responseDiv = document.getElementById('getOneDrinkDetailResponse')


        const URL_FOR_GetOne = API_URL + drinkName

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
            responseDiv.textContent =  data[0]['drinkName'] + " has " + data[0]['calories'] + " calories"
        })
        .catch((error) => console.error("Error:", error));
    }
</script>


### Getting less than 10 percent of daily calories from beverages: At least half of your daily fluid should come from water.


### On average, a woman should eat 2000 calories per day to maintain her weight, and she should limit her caloric intake to 1500 or less in order to lose one pound per week. For the average male to maintain his body weight, he should eat 2500 calories per day, or 2000 a day if he wants to lose one pound per week.

<style>
    /* Reset default styles for all elements */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Set background image for the entire page */
body {
    background-image: url('https://github.com/Nupurbb/ds3front/assets/142522690/289e8115-7192-4344-a677-f1ad796eac8b');
 
    background-size: cover;
    background-position: center;
    font-family: Arial, sans-serif;
}

/* Style the content area */
.content {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    background-color: rgba(255, 255, 255, 0.8);
    border-radius: 10px;
}

/* Adjust styles for form labels */
form label {
    font-weight: bold;
    margin-bottom: 5px;
}

/* Adjust styles for buttons */
button {
    background-color: #e75480; /* Dark pink background color */
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

/* Style the edit and delete buttons in the table */
#DrinkTable button {
    background-color: #2196F3; /* Blue background color */
    color: white;
    padding: 5px 10px;
    margin: 2px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}

/* Ignore certain elements or classes */
body :not(.content) {
    /* Add styles here for elements other than .content */
}

/* Ignore specific elements */
body :not(h1, p) {
    /* Add styles here for elements other than h1 and p */
}
