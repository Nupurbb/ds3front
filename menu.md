<script type="module">
# Example list of recipes (each recipe is a dictionary for simplicity)
all_recipes = [
    {'name': 'Pasta', 'is_favorite': True, 'preparation_time': 25},
    {'name': 'Pizza', 'is_favorite': False, 'preparation_time': 40},
    {'name': 'Salad', 'is_favorite': True, 'preparation_time': 15},
    {'name': 'Soup', 'is_favorite': True, 'preparation_time': 30},
    {'name': 'Burger', 'is_favorite': False, 'preparation_time': 20}
]

# List comprehension to create a personalized menu of favorite recipes
favorite_recipes = [recipe for recipe in all_recipes if recipe['is_favorite']]
print("Favorite Recipes (List Comprehension):", favorite_recipes)

# Conventional loop to filter recipes with preparation time less than 30 minutes
quick_recipes = []
for recipe in favorite_recipes:
    if recipe['preparation_time'] < 30:
        quick_recipes.append(recipe)
print("Quick Recipes (Conventional Loop):", quick_recipes)

# For-each method to filter recipes with preparation time less than 30 minutes
quick_recipes_foreach = [recipe for recipe in favorite_recipes if recipe['preparation_time'] < 30]
print("Quick Recipes (For-each Method):", quick_recipes_foreach)