---
layout: default
permalink: /recipe
---

<style>
    body {
        background-color: #FAE5DE;
        font-family: 'Arial', sans-serif;
        margin: 20px;
    }

    .generate-button {
        background-color: pink;
        color: rgb(255, 255, 255);
        border: none;
        border-radius: 5px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        margin-top: 20px;
        transition: background-color 0.3s;
    }

    .generate-button:hover {
        background-color: #262829; /* hover */
    }

    #genre {
        color: black; /* Changes the text color of the select box */
    }

    h3 {
        color: black;
        text-shadow: 
            -1px -1px 0 white,  
             1px -1px 0 white,
            -1px  1px 0 white,
             1px  1px 0 white; 
    }
</style>

<h3>Dessert Recipe Recommender</h3>
<select name="genre" id="genre" required style="color: black;">
    <option value="">Select...</option>
    <option value="cookies">Cookies</option>
    <option value="cake">Cake</option>
    <option value="pie">Pie</option>
    <option value="muffins">Muffins</option>
</select>

<div class="body">
    <div>
        <h2>Recipe: <span id="recipe"></span></h2> 
        <h2>Main Ingredient: <span id="ingredients"></span></h2>
        <h2>Time: <span id="time"></span></h2>
        <h2>Type: <span id="type"></span></h2>
        <button class="generate-button" onclick="generateRecipe()">Generate Recipe</button>
    </div>
</div>

<script>
    function generateRecipe() {
        const selectedGenre = document.getElementById('genre').value;
        const apiUrl = `http://127.0.0.1:8086/api/recipe/searchbygenre?genre=${selectedGenre}`;
        
        fetch(apiUrl)
            .then(response => response.json())
            .then(data => {
                if (data.length > 0) {
                    // Randomly pick one recipe from the results
                    const randomRecipe = data[Math.floor(Math.random() * data.length)];

                    const recipe = randomRecipe.recipe;
                    const ingredients = randomRecipe.ingredients;
                    const time = randomRecipe.time;
                    const type = randomRecipe.type;

                    document.getElementById('recipe').textContent = recipe;
                    document.getElementById('ingredients').textContent = ingredients;
                    document.getElementById('time').textContent = time;
                    document.getElementById('type').textContent = type;
                } else {
                    const errorMessage = document.createElement('div');
                    errorMessage.textContent = 'No recipes found for the selected genre.';
                    document.getElementById('table-container').appendChild(errorMessage);
                }
            })
            .catch(error => {
                const errorMessage = document.createElement('div');
                errorMessage.textContent = 'Error fetching recipe. Please try again later.';
                console.error('Error:', error);
                document.getElementById('table-container').appendChild(errorMessage);
            });
    }
</script>