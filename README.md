# Snakes and Ladders

You are given an n x n integer matrix board where the cells are labeled from 1 to n2 in a Boustrophedon style starting from the bottom left of the board (i.e. board[n - 1][0]) and alternating direction each row.

You start on square 1 of the board. In each move, starting from square curr, do the following:

Choose a destination square next with a label in the range [curr + 1, min(curr + 6, n2)].
This choice simulates the result of a standard 6-sided die roll: i.e., there are always at most 6 destinations, regardless of the size of the board.
If next has a snake or ladder, you must move to the destination of that snake or ladder. Otherwise, you move to next.
The game ends when you reach the square n2.
A board square on row r and column c has a snake or ladder if board[r][c] != -1. The destination of that snake or ladder is board[r][c]. Squares 1 and n2 are not the starting points of any snake or ladder.

Note that you only take a snake or ladder at most once per move. If the destination to a snake or ladder is the start of another snake or ladder, you do not follow the subsequent snake or ladder.

For example, suppose the board is [[-1,4],[-1,3]], and on the first move, your destination square is 2. You follow the ladder to square 3, but do not follow the subsequent ladder to 4.
Return the least number of moves required to reach the square n2. If it is not possible to reach the square, return -1.

```
Example 1:


Input: board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation: 
In the beginning, you start at square 1 (at row 5, column 0).
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 and must take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
This is the lowest possible number of moves to reach the last square, so return 4.
Example 2:

Input: board = [[-1,-1],[-1,3]]
Output: 1
``` 

### Constraints:

1. n == board.length == board[i].length
2. 2 <= n <= 20
3. board[i][j] is either -1 or in the range [1, n2].
4. The squares labeled 1 and n2 are not the starting points of any snake or ladder.

## Implementation 1 : BFS
```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        int n = board.length;
        // BFS queue for (currentPosition, moveCount)
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{1, 0}); // Start at square 1 with 0 moves
        
        boolean[] visited = new boolean[n * n + 1];
        visited[1] = true; // Mark the first square as visited

        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int pos = curr[0];
            int moves = curr[1];
            // If we reached the final square
            if (pos == n * n) {
                return moves;
            }
            // Try moving 1 to 6 steps ahead
            for (int i = 1; i <= 6; i++) {
                int nextPos = pos + i;
                if (nextPos > n * n) break; // Ensure we stay within bounds
                // Calculate the board value considering snakes or ladders
                int boardValue = getBoardValue(board, nextPos, n);
                int finalPos = boardValue == -1 ? nextPos : boardValue;
                // Process if the final position has not been visited
                if (!visited[finalPos]) {
                    visited[finalPos] = true;
                    queue.offer(new int[]{finalPos, moves + 1});
                }
            }
        } 
        // If we exhaust the queue without finding a path
        return -1;
    }
    
    // Helper function to get the board value in 2D Boustrophedon order
    private int getBoardValue(int[][] board, int pos, int n) {
        int quot = (pos - 1) / n;
        int row = n - 1 - quot;

        int rem = (pos - 1) % n;
        int col = row % 2 != n % 2 ? rem : n - 1 - rem; // Zigzag pattern

        return board[row][col];
    }
}
```

# References :
https://leetcode.com/problems/snakes-and-ladders/editorial
