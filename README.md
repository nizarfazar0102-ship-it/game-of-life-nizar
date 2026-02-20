import time
import os
import random

# ===============================
# ADT ARRAY 2D
# ===============================
class Array2D:
    def __init__(self, rows, cols, default=0):
        self.rows = rows
        self.cols = cols
        self.data = [[default for _ in range(cols)] for _ in range(rows)]

    def set(self, r, c, value):
        if 0 <= r < self.rows and 0 <= c < self.cols:
            self.data[r][c] = value

    def get(self, r, c):
        if 0 <= r < self.rows and 0 <= c < self.cols:
            return self.data[r][c]
        return 0

    def display(self):
        for row in self.data:
            print(" ".join("■" if x == 1 else "." for x in row))
        print()


# ===============================
# GAME OF LIFE
# ===============================
class GameOfLife:
    def __init__(self, rows, cols):
        self.grid = Array2D(rows, cols)

    def random_init(self):
        for r in range(self.grid.rows):
            for c in range(self.grid.cols):
                self.grid.set(r, c, random.choice([0, 1]))

    def neighbors(self, r, c):
        count = 0
        for dr in [-1, 0, 1]:
            for dc in [-1, 0, 1]:
                if dr == 0 and dc == 0:
                    continue
                count += self.grid.get(r + dr, c + dc)
        return count

    def step(self):
        new_grid = Array2D(self.grid.rows, self.grid.cols)

        for r in range(self.grid.rows):
            for c in range(self.grid.cols):
                alive = self.grid.get(r, c)
                n = self.neighbors(r, c)

                if alive == 1 and (n == 2 or n == 3):
                    new_grid.set(r, c, 1)
                elif alive == 0 and n == 3:
                    new_grid.set(r, c, 1)
                else:
                    new_grid.set(r, c, 0)

        self.grid = new_grid


# ===============================
# MAIN PROGRAM
# ===============================
game = GameOfLife(10, 20)
game.random_init()

for _ in range(30):
    os.system("cls" if os.name == "nt" else "clear")
    game.grid.display()
    game.step()
    time.sleep(0.4)
