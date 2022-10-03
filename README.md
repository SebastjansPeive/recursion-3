# Recursion Exercises (Part III)

## 1. Generate a maze

Write a program, that generates and displays a maze using recursive
backtracking. The algorithm could be described like this ([source]):

[source]: https://weblog.jamisbuck.org/2010/12/27/maze-generation-recursive-backtracking

1. Choose a starting point in the field.
2. Randomly choose a wall at that point and carve a passage through to the
   adjacent cell, but only if the adjacent cell has not been visited yet. This
   becomes the new current cell.
3. If all adjacent cells have been visited, back up to the last cell that has
   uncarved walls and repeat.
4. The algorithm ends when the process has backed all the way up to the
   starting point.

The program must output the generated maze using pseudo graphics (or graphics).

### Output example

```txt
   ___________________
  |           |       |
  |           |       |
  |    ___    |___    |
  |       |       |   |
  |       |       |   |
  |___    |_______|   |
  |   |   |           |
  |   |   |           |
  |   |   |___     ___|
  |                   |
  |                   |
  |___________________|
```

### Hints

Each cell should contain information about available exits. You could use a
slice of directions or a bit map for that. Using bit maps is easier:

```go
// Dir represents a bit mask direction for north (1), south (2), west (4) and east (8).
type Dir byte

const (
	N Dir = 1 << iota
	S
	W
	E
)

// HasExit returns true if the e[x][y] contains an exit in the specified direction.
func HasExit(e [][]Dir, x, y int, d Dir) bool { return e[x][y]&d != 0 }

func main() {
	fmt.Println(N, S, W, E)    // Output: 1 2 4 8
	exits := make([][]Dir, 10) // We start with a maze that has no exits.
	for i := range exits {
		exits[i] = make([]Dir, 10)
	}

	exits[5][5] = N | S // Cell (5, 5) allows North and South exits.

	fmt.Println(HasExit(exits, 5, 5, N)) // Output: true
	fmt.Println(HasExit(exits, 5, 5, W)) // Output: false
}
```

## 2. Solve a maze

Write a program, that finds and displays any path in a labyrinth (a map of
walls in the snippet below). The program must be implemented recursively using
depth-first approach (aka [DFS]).

[DFS]: https://en.wikipedia.org/wiki/Depth-first_search

The program must contain a tested function that locates path in a labyrinth. It
could be defined similarly to the following snippet:

```go
type (
  // Exits represents a map of exits in a maze.
  Exits [][]Dir
  // Coord represents an (X, Y) coordinate on the map.
  Coord struct { X, Y int }
)

// AnyPath find the first path found between start and end coordinates. The
// functions returns coordinates representing the path. If the path cannot be
// found, the function returns nil.
func AnyPath(e Exits, start, end Coord) []Coord
```

The function must solve the maze generated in (1).

### Output example

```txt
S - start
X - exit
   ___________________
  |***********|       |
  |***********|       |
  |*** ___ ***|___    |
  |*******|****XXX|   |
  |*******|****XXX|   |
  |___ ***|_______|   |
  |   |***|           |
  |   |***|           |
  |   |***|___     ___|
  |SSS****            |
  |SSS****            |
  |___________________|
```