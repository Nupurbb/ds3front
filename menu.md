<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Menu Filter</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .recipe-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            border-bottom: 1px solid #ccc;
            padding-bottom: 5px;
        }
        .recipe-container span {
            font-weight: bold;
        }
        .ingredient-list-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        .ingredient-list-container input[type="checkbox"] {
            margin-right: 5px;
        }
    </style>
</head>
<body>
    <h1>Recipe Filter</h1>

    <!-- List 1: Sorted by Time -->
    <div>
        <h2>Recipes Sorted by Time</h2>
        <div id="timeList"></div>
        <button id="sortTimeButton">Sort by Time</button>
    </div>

    <!-- List 2: Sorted by Ingredients -->
    <div>
        <h2>Recipes Sorted by Ingredients</h2>
        <div id="ingredientList"></div>
        <button id="sortIngredientsButton">Sort by Ingredients</button>
    </div>

    <!-- Ingredient Checkbox Feature -->
    <h2>Ingredients</h2>
    <div id="ingredientCheckboxes"></div>
<script>
        const allRecipes = [
            {name: 'Pasta', type: 'Main Course', ingredients: {'pasta': 200, 'tomato sauce': 150}, is_favorite: true, preparation_time: 25, rating: 4.5},
            {name: 'Pizza', type: 'Main Course', ingredients: {'dough': 300, 'cheese': 100, 'tomato sauce': 50}, is_favorite: false, preparation_time: 40, rating: 4.7},
            {name: 'Salad', type: 'Appetizer', ingredients: {'lettuce': 100, 'tomatoes': 50, 'cucumber': 30}, is_favorite: true, preparation_time: 15, rating: 4.2},
            {name: 'Soup', type: 'Appetizer', ingredients: {'broth': 250, 'carrots': 50, 'chicken': 100}, is_favorite: true, preparation_time: 30, rating: 4.0},
            {name: 'Burger', type: 'Main Course', ingredients: {'bun': 1, 'beef patty': 150, 'lettuce': 20}, is_favorite: false, preparation_time: 20, rating: 4.6}
        ];

        let sortedByIngredients = [];
        let sortedByTime = [];
        let checkedIngredients = [];

        // Function to display recipes with ingredients
        function displayRecipesWithIngredients(recipes, containerId) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';

            recipes.forEach(recipe => {
                const recipeDiv = document.createElement('div');
                recipeDiv.classList.add('recipe-container');
                recipeDiv.innerHTML = `<span>${recipe.name}</span> - Ingredients: ${Object.keys(recipe.ingredients).join(', ')} - Time: ${recipe.preparation_time} minutes`;
                container.appendChild(recipeDiv);
            });
        }

        // Initial display of unsorted recipes under each button
        displayRecipesWithIngredients(allRecipes, 'timeList');
        document.getElementById('timeList').style.display = 'block';
        displayRecipesWithIngredients(allRecipes, 'ingredientList');
        document.getElementById('ingredientList').style.display = 'block';

        // Event listener for "Sort by Ingredients" button
        document.getElementById('sortIngredientsButton').addEventListener('click', () => {
            if (sortedByIngredients.length === 0) {
                sortedByIngredients = allRecipes.slice().sort((a, b) => {
                    const ingredientsA = Object.keys(a.ingredients).sort().join('');
                    const ingredientsB = Object.keys(b.ingredients).sort().join('');
                    return ingredientsA.localeCompare(ingredientsB);
                });
            }
            displayRecipesWithIngredients(sortedByIngredients, 'ingredientList');
            document.getElementById('ingredientList').style.display = 'block';
            document.getElementById('timeList').style.display = 'none';
        });

        // Event listener for "Sort by Time" button
        document.getElementById('sortTimeButton').addEventListener('click', () => {
            if (sortedByTime.length === 0) {
                sortedByTime = allRecipes.slice().sort((a, b) => a.preparation_time - b.preparation_time);
            }
            displayRecipesWithIngredients(sortedByTime, 'timeList');
            document.getElementById('timeList').style.display = 'block';
            document.getElementById('ingredientList').style.display = 'none';
        });

        // Function to generate ingredient checkboxes and handle changes
        function generateIngredientCheckboxes() {
            const checkboxesDiv = document.getElementById('ingredientCheckboxes');
            allIngredients = allRecipes.reduce((acc, recipe) => {
                Object.keys(recipe.ingredients).forEach(ingredient => {
                    if (!acc.includes(ingredient)) {
                        acc.push(ingredient);
                    }
                });
                return acc;
            }, []);

            allIngredients.forEach(ingredient => {
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.id = ingredient;
                checkbox.value = ingredient;
                checkbox.addEventListener('change', updateCheckedIngredients);
                const label = document.createElement('label');
                label.htmlFor = ingredient;
                label.textContent = ingredient;
                checkboxesDiv.appendChild(checkbox);
                checkboxesDiv.appendChild(label);
            });
        }

        // Update the list of checked ingredients and display matching recipes
        function updateCheckedIngredients() {
            checkedIngredients = Array.from(document.querySelectorAll('input[type="checkbox"]:checked')).map(checkbox => checkbox.value);
            const recipesWithCheckedIngredients = allRecipes.filter(recipe => {
                return checkedIngredients.every(ingredient => Object.keys(recipe.ingredients).includes(ingredient));
            });
            displayRecipesWithIngredients(recipesWithCheckedIngredients, 'ingredientList');
        }

        // Call the function to generate ingredient checkboxes
        generateIngredientCheckboxes();
    </script>
</body>
</html>
