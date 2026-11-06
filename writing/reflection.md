# Space Invaders Lab 05 — Sample Reflection

Name: Instructor sample  
Date: November 13, 2026

These are sample responses that demonstrate an acceptable level of detail. A student's wording and examples will naturally differ.

---

## 1. Modules and organization

**Question:** What is a Python module? Name two modules in this project and explain what each one is responsible for.

**Sample response:** A module is a Python file that can define code and be imported by another file. In this game, `player.py` contains the player's ship and its movement and firing behavior, while `projectile.py` defines shots that both the player and invaders can use. Keeping those responsibilities in separate modules makes it easier to find and change one part of the game without searching through one very long file.

## 2. Import styles

**Question:** Find an `import module` statement and a `from module import name` statement in the project. How does using each style affect the way code refers to the imported object?

**Sample response:** `game.py` uses `import random` and then calls `random.choice(...)`, so the module name remains visible at the call site. `main.py` uses `from .game import Game`, so it can call `Game()` directly. The first style makes the source of a function especially obvious; the second is convenient when a file needs one specific class.

## 3. Connecting our modules

**Question:** How does `game.py` use code from the other project modules? Why are the imports written with a leading dot?

**Sample response:** `game.py` imports `Player`, `Invader`, `Projectile`, and `Starfield`, then coordinates instances of those classes in the main loop. The leading dot means to look for the module within the current `space_invaders` package. This lets Python locate the related files as package modules rather than treating them as unrelated top-level files.

## 4. Module responsibilities

**Question:** Give an example of a programming task that belongs in a focused module instead of the game loop. How does that separation help?

**Sample response:** The details of drawing and moving a player shot belong in `projectile.py`, not in the main event loop. The game can create and update projectiles without knowing how each projectile stores its speed or removes itself at the edge of the screen. This reduces duplicated logic and gives each module a clear responsibility.

## 5. Standard library, third-party, and local modules

**Question:** Identify one standard-library module, one third-party package, and one module written for this project. What does each provide?

**Sample response:** `random` is part of Python's standard library and chooses an invader to shoot. `pygame` is provided by the third-party `pygame-ce` dependency and handles the window, drawing, keyboard, and sprites. `space_invaders.enemy` is our own module, which defines the invader sprite. The `pyproject.toml` file records the external dependency so `uv sync` can install it.

## 6. The main guard

**Question:** What does `if __name__ == "__main__":` do in `main.py`? Why is it useful?

**Sample response:** Python sets `__name__` to `"__main__"` when a file is run as the program entry point. The guard calls `main()` in that situation, but does not start the game if another module imports `space_invaders.main`. That makes the entry-point module safer to import and easier to test or reuse.

## 7. Dependency and environment management

**Question:** Why do we list `pygame-ce` in `pyproject.toml` and run the project with `uv` instead of assuming every computer already has Pygame?

**Sample response:** Pygame is not built into Python, so the project needs to declare it as a third-party dependency. `uv sync` reads the project configuration and installs the declared dependency in the project environment. This makes setup repeatable for classmates and keeps project packages separate from unrelated Python projects.

## 8. Testing and debugging across modules

**Question:** Describe one issue that could happen at a boundary between two modules. How would you trace and test it?

**Sample response:** If the game passes a projectile speed with the wrong sign, `game.py` may create an enemy shot that travels upward instead of toward the player. I would trace where `Projectile` is created and check how `Projectile.update` uses `velocity_y`. I could then test one instance with a known positive or negative speed and confirm that its `rect.y` changes in the intended direction. Clear parameter names and small modules make that investigation manageable.
