---
comments: true
layout: post
title: recipie
description: 
courses: { compsci: {week: 26} }
type: hacks
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recipe Management System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        form {
            margin-bottom: 20px;
        }
        input[type="text"], textarea {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        input[type="submit"], button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        input[type="submit"]:hover, button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Recipe Management System</h1>
    <!-- this is the form to add the new recipies -->
    <h2>Add Recipe</h2>
    <form id="addRecipeForm">
        <label for="recipeName">Name:</label><br>
        <input type="text" id="recipeName" name="recipeName"><br>
        <label for="ingredients">Ingredients:</label><br>
        <textarea id="ingredients" name="ingredients" rows="4"></textarea><br>
        <label for="instructions">Instructions:</label><br>
        <textarea id="instructions" name="instructions" rows="4"></textarea><br>
        <input type="submit" value="Add Recipe">
    </form>
    <!-- function to delete any recipes -->
    <h2>Delete Recipe</h2>
    <form id="deleteRecipeForm">
        <label for="deleteRecipe">Select Recipe to Delete:</label><br>
        <select id="deleteRecipe" name="deleteRecipe">
        </select><br>
        <input type="submit" value="Delete Recipe">
    </form>
    <!-- this is form to update the recipies -->
    <h2>Update Recipe</h2>
    <form id="updateRecipeForm">
        <label for="updateRecipe">Select Recipe to Update:</label><br>
        <select id="updateRecipe" name="updateRecipe">
        </select><br>
        <label for="updatedRecipeName">Name:</label><br>
        <input type="text" id="updatedRecipeName" name="updatedRecipeName"><br>
        <label for="updatedIngredients">Ingredients:</label><br>
        <textarea id="updatedIngredients" name="updatedIngredients" rows="4"></textarea><br>
        <label for="updatedInstructions">Instructions:</label><br>
        <textarea id="updatedInstructions" name="updatedInstructions" rows="4"></textarea><br>
        <input type="submit" value="Update Recipe">
    </form>
    <!--this is the form to update the selected recipie -->
    <h2>Search Recipe by Ingredient</h2>
    <label for="searchIngredient">Enter Ingredient:</label>
    <input type="text" id="searchIngredient" name="searchIngredient">
    <button id="searchButton">Search</button>
    <!-- this is where the recipies are displayed -->
    <h2>Display All Recipes</h2>
    <div id="allRecipes">
        <!-- the recicpies are shown dynamically using JavaScript -->
    </div>
    <script>
        document.getElementById('addRecipeForm').addEventListener('submit', function(event) {
            event.preventDefault();
            const name = document.getElementById('recipeName').value;
            const ingredients = document.getElementById('ingredients').value;
            const instructions = document.getElementById('instructions').value;
            const recipe = { name, ingredients, instructions };
            recipes.push(recipe);
            updateSelectOptions();
            document.getElementById('recipeName').value = '';
            document.getElementById('ingredients').value = '';
            document.getElementById('instructions').value = '';
            displayRecipes(recipes);
            alert(`Recipe added: ${name}`);
        });
        // delete the recipies
        document.getElementById('deleteRecipeForm').addEventListener('submit', function(event) {
            event.preventDefault();
            // Get selected recipes 
            const deleteRecipeSelect = document.getElementById('deleteRecipe');
            const index = deleteRecipeSelect.selectedIndex;
            if (index !== -1) {
                // delete the recipes from recipes array
                const deletedRecipeName = recipes[index].name;
                recipes.splice(index, 1);
                // update the selectable options for the delete and update forms
                updateSelectOptions();
                // display all the recipes
                displayRecipes(recipes);
                // alert message
                alert(`Recipe deleted: ${deletedRecipeName}`);
            } else {
                alert('Please select a recipe to delete');
            }
        });
        // update recipe function
        document.getElementById('updateRecipeForm').addEventListener('submit', function(event) {
            event.preventDefault();
            // get the chosen recipe and updated valued
            const updateRecipeSelect = document.getElementById('updateRecipe');
            const index = updateRecipeSelect.selectedIndex;
            const updatedName = document.getElementById('updatedRecipeName').value;
            const updatedIngredients = document.getElementById('updatedIngredients').value;
            const updatedInstructions = document.getElementById('updatedInstructions').value;
            if (index !== -1) {
                // update the recipes in recipes array
                recipes[index].name = updatedName;
                recipes[index].ingredients = updatedIngredients;
                recipes[index].instructions = updatedInstructions;
                // display all the recipies again
                displayRecipes(recipes);
                // alet but for recipie updated
                alert(`Recipe updated: ${updatedName}`);
            } else {
                alert('Please select a recipe to update');
            }
        });
        // Form for searchig for added recipies by ingredient
        document.getElementById('searchButton').addEventListener('click', function() {
            // search ingredient
            const searchIngredient = document.getElementById('searchIngredient').value.toLowerCase();
            // function for filtering the recipies based on the searched ingredient
            const foundRecipes = recipes.filter(recipe => recipe.ingredients.toLowerCase().includes(searchIngredient));
            if (foundRecipes.length > 0) {
                // show the recipies that were found through the search
                displayRecipes(foundRecipes);
            } else {
                alert(`No recipes found containing: ${searchIngredient}`);
            }
        });
        let recipes = [
            { name: 'Spaghetti', ingredients: 'Pasta, Tomato Sauce, Ground Beef', instructions: 'Boil pasta, cook beef, mix with sauce' },
            { name: 'Chicken Stir Fry', ingredients: 'Chicken, Bell Peppers, Broccoli, Soy Sauce', instructions: 'Cook chicken, add vegetables, stir in sauce' }
        ];
        // display again
        displayRecipes(recipes);
        function displayRecipes(recipesArray) {
            const allRecipesDiv = document.getElementById('allRecipes');
            allRecipesDiv.innerHTML = '';
            recipesArray.forEach(function(recipe) {
                const recipeDiv = document.createElement('div');
                recipeDiv.innerHTML = `
                    <h3>${recipe.name}</h3>
                    <p><strong>Ingredients:</strong> ${recipe.ingredients}</p>
                    <p><strong>Instructions:</strong> ${recipe.instructions}</p>
                `;
                allRecipesDiv.appendChild(recipeDiv);
            });
        }
        function updateSelectOptions() {
            const deleteRecipeSelect = document.getElementById('deleteRecipe');
            const updateRecipeSelect = document.getElementById('updateRecipe');
            deleteRecipeSelect.innerHTML = '';
            updateRecipeSelect.innerHTML = '';
            recipes.forEach(function(recipe, index) {
                const option = document.createElement('option');
                option.text = recipe.name;
                option.value = index;
                deleteRecipeSelect.add(option);
                updateRecipeSelect.add(option.cloneNode(true));
            });
        }
    </script>
</body>
</html>
