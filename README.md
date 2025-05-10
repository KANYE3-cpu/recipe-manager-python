
import json
from recipe import Recipe

class RecipeManager:
    def __init__(self):
        self.recipes = {}

    def add_recipe(self, name, category, ingredients, instructions):
        self.recipes[name] = Recipe(name, category, ingredients, instructions)

    def get_recipe(self, name):
        return self.recipes.get(name)

    def update_recipe(self, name, category, ingredients, instructions):
        if name in self.recipes:
            self.recipes[name] = Recipe(name, category, ingredients, instructions)

    def delete_recipe(self, name):
        if name in self.recipes:
            del self.recipes[name]

    def search_recipes(self, query):
        return [r for r in self.recipes.values() if query.lower() in r.name.lower()]

    def save_to_file(self, filename='data/recipes.json'):
        with open(filename, 'w') as f:
            json.dump({k: vars(v) for k, v in self.recipes.items()}, f)

    def load_from_file(self, filename='data/recipes.json'):
        try:
            with open(filename, 'r') as f:
                data = json.load(f)
                for name, details in data.items():
                    self.recipes[name] = Recipe(**details)
        except FileNotFoundError:
            pass

def main():
    manager = RecipeManager()
    manager.load_from_file()

    while True:
        print("\nRecipe Manager")
        print("1. Add Recipe")
        print("2. View Recipe")
        print("3. Update Recipe")
        print("4. Delete Recipe")
        print("5. Search Recipes")
        print("6. Save & Exit")

        choice = input("Choose an option: ")

        if choice == '1':
            name = input("Name: ")
            category = input("Category: ")
            ingredients = input("Ingredients (comma-separated): ").split(',')
            instructions = input("Instructions: ")
            manager.add_recipe(name, category, ingredients, instructions)

        elif choice == '2':
            name = input("Recipe name: ")
            recipe = manager.get_recipe(name)
            if recipe:
                print(vars(recipe))
            else:
                print("Recipe not found.")

        elif choice == '3':
            name = input("Name to update: ")
            category = input("New Category: ")
            ingredients = input("New Ingredients: ").split(',')
            instructions = input("New Instructions: ")
            manager.update_recipe(name, category, ingredients, instructions)

        elif choice == '4':
            name = input("Recipe name to delete: ")
            manager.delete_recipe(name)

        elif choice == '5':
            query = input("Search: ")
            results = manager.search_recipes(query)
            for r in results:
                print(vars(r))

        elif choice == '6':
            manager.save_to_file()
            break

        else:
            print("Invalid choice.")