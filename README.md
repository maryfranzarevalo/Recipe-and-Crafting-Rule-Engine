#Recipe-and-Crafting-Rule-Engine
```mermaid

classDiagram
    class Item {
        - string itemId
        - string name
        - string type
        + getDetails() string
    }

    class Ingredient {
        - int quantity
        + getQuantity() int
    }

    class Recipe {
        - string recipeId
        - string name
        - bool isReversible
        - list<Ingredient> ingredients
        - list<CraftingConstraint> constraints
        + validate() bool
        + getIngredients() list<Ingredient>
    }

    class CraftingConstraint {
        - string toolRequired
        - int timeRequired
        - int sequenceOrder
        + checkConstraint() bool
    }

    class Inventory {
        - list<Item> items
        + addItem(Item item)
        + removeItem(string itemId)
        + hasItem(string itemId) bool
    }

    class CraftingEngine {
        - list<Recipe> recipes
        - Inventory inventory
        + craft(string recipeId) CraftingResult
        + validateRecipe(Recipe recipe) bool
        + resolveDependencies()
    }

    class CraftingResult {
        - bool success
        - string message
        - list<string> steps
        + getSummary() string
    }

    class Owner {
        - string username
        + createRecipe()
        + craftItem(Recipe recipe)
    }

    Ingredient --|> Item : inherits
    Recipe --> Ingredient : contains
    Recipe --> Item : produces
    Recipe --> CraftingConstraint : has many
    CraftingEngine --> Recipe : uses
    CraftingEngine --> Inventory : manages
    CraftingEngine --> CraftingResult : returns
    Owner --> Recipe : creates
    Owner --> CraftingEngine : uses



```



```mermaid
sequenceDiagram
    participant O as Owner
    participant CE as CraftingEngine
    participant R as Recipe
    participant C as CraftingConstraint
    participant I as Inventory
    participant CR as CraftingResult

    P->>CE: craft(recipeId)
    CE->>R: validate()
    R-->>CE: Recipe valid

    CE->>R: getIngredients()
    R-->>CE: List of Ingredients

    CE->>I: hasItem(Ingredients)
    I-->>CE: Materials available

    CE->>C: checkConstraint()
    C-->>CE: Constraints satisfied

    CE->>I: removeItem(Ingredients)
    I-->>CE: Materials consumed

    CE->>CR: Create CraftingResult(success, message, steps)
    CR-->>P: getSummary()


```
