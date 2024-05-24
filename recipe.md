---
layout: default
title: Recipe Suggester
permalink: /recipe
---

<style>
    body {
    background-color: #fae5de;
    }
    body {
        background-color: #FAE5DE;
        font-family: 'Arial', sans-serif;
        margin: 20px;
    }
</style>

<body>
    <h1>Recipe Recommender</h1>
    <label for="ingredients">Select ingredients:</label><br>
    <input type="checkbox" id="strawberry" name="ingredient" value="strawberry">
    <label for="strawberry"> Strawberry</label><br>
    <input type="checkbox" id="chocolate" name="ingredient" value="chocolate">
    <label for="chocolate"> Chocolate</label><br>
    <input type="checkbox" id="vanilla" name="ingredient" value="vanilla">
    <label for="vanilla"> Vanilla</label><br>
    <input type="checkbox" id="lemon" name="ingredient" value="lemon">
    <label for="lemon"> Lemon</label><br>
    <input type="checkbox" id="apple" name="ingredient" value="apple">
    <label for="apple"> Apple</label><br><br>
    <button onclick="recommend()">Recommend Recipes</button><br><br>
    <h2>Recommended Recipes:</h2>
    <ul id="List"></ul>
</body>

<script>
    const recipes = [
            { name: "Strawberry Yogurt Parfait", ingredients: ["strawberry", "vanilla"] },
            { name: "Chocolate Banana Smoothie", ingredients: ["chocolate"] },
            { name: "Vanilla Chia Seed Pudding", ingredients: ["vanilla"] },
            { name: "Lemon Poppy Seed Muffins", ingredients: ["lemon"] },
            { name: "Apple Cinnamon Oatmeal Cookies", ingredients: ["apple"] },
            { name: "Chocolate Covered Strawberries", ingredients: ["chocolate", "strawberry"] },
            { name: "Lemon Blueberry Greek Yogurt Bark", ingredients: ["lemon"] }
        ];
function recommend() {
    const selectedIngredients = Array.from(document.querySelectorAll('input[name="ingredient"]:checked')).map(checkbox => checkbox.value);
    const recommendedRecipes = recipes.filter(recipe => selectedIngredients.every(ingredient => recipe.ingredients.includes(ingredient)));

    const List = document.getElementById("List");
    List.innerHTML = "";

    if (recommendedRecipes.length === 0) {
        const li = document.createElement("li");
        li.textContent = "No recipes found for the selected ingredients.";
        List.appendChild(li);
    } else {
        recommendedRecipes.forEach(recipe => {
            const li = document.createElement("li");
            li.textContent = recipe.name;
            List.appendChild(li);
        });
    }
}
</script>
