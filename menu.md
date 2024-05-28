
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recipe Filter</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        #results {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Recipe Filter</h1>
    <button id="filterButton">Show Favorite and Quick Recipes</button>
    <div id="results"></div>

    <script type="module">
        document.getElementById('filterButton').addEventListener('click', () => {
            // Example list of recipes (each recipe is a dictionary for simplicity)
            const all_recipes = [
                {'name': 'Pasta', 'is_favorite': true, 'preparation_time': 25},
                {'name': 'Pizza', 'is_favorite': false, 'preparation_time': 40},
                {'name': 'Salad', 'is_favorite': true, 'preparation_time': 15},
                {'name': 'Soup', 'is_favorite': true, 'preparation_time': 30},
                {'name': 'Burger', 'is_favorite': false, 'preparation_time': 20}
            ];

            // List comprehension to create a personalized menu of favorite recipes
            const favorite_recipes = all_recipes.filter(recipe => recipe.is_favorite);
            console.log("Favorite Recipes (List Comprehension):", favorite_recipes);

            // Conventional loop to filter recipes with preparation time less than 30 minutes
            let quick_recipes = [];
            for (let recipe of favorite_recipes) {
                if (recipe.preparation_time < 30) {
                    quick_recipes.push(recipe);
                }
            }
            console.log("Quick Recipes (Conventional Loop):", quick_recipes);

            // For-each method to filter recipes with preparation time less than 30 minutes
            const quick_recipes_foreach = favorite_recipes.filter(recipe => recipe.preparation_time < 30);
            console.log("Quick Recipes (For-each Method):", quick_recipes_foreach);

            // Display results in the HTML
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = `
                <h2>Favorite Recipes:</h2>
                <ul>${favorite_recipes.map(recipe => `<li>${recipe.name} (${recipe.preparation_time} min)</li>`).join('')}</ul>
                <h2>Quick Recipes (Less than 30 min):</h2>
                <ul>${quick_recipes_foreach.map(recipe => `<li>${recipe.name} (${recipe.preparation_time} min)</li>`).join('')}</ul>
            `;
        });
    </script>
</body>
</html>
