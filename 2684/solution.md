# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
- Whether or not a cell is reachable is determined by the cells in the previous column
- If we know whether the cells in the previous column are reachable, we can determine the whether the current cell is reachable
- The index of the column will be equal to the number of moves to reach a cell in that column because we always move left to right

# Approach
<!-- Describe your approach to solving the problem. -->
Iterate through the columns from left to right. Use an array to store which of the cells in the previous column were reachable.

# Complexity
- Time complexity:
$$O(m * n)$$ - in the worst case we must check every cell once

- Space complexity:
$$O(m)$$ - where m is the number of rows. This is optimal space, we only need to store whether there is a valid path leading to each cell in the previous column - one for each row.

# Code
```typescript []
function maxMoves(grid: number[][]): number {
    let max = 0;
    let validPaths = new Array(grid.length).fill(true);

    const gridAt = (row, col) => Number(grid[row]?.[col]);

    // Go along the grid left to right
    for (let colIdx = 1; colIdx < grid[0].length; colIdx++) {
        // Temp array to represent valid paths in new column without overwriting previous column (be sure to copy the array using `...`)
        const newValidPaths = [...validPaths];
        let found = false;

        for (let rowIdx = 0; rowIdx < grid.length; rowIdx++) {
            const currentVal = gridAt(rowIdx, colIdx);

            // Checks if you can get to the current cell from the cell in the previous column at the given row
            const getIsValidPath = (row) => validPaths[row] 
                && currentVal > gridAt(row, colIdx - 1);

            // For the cells at upper-left, left, and lower-left
            if (
                getIsValidPath(rowIdx - 1)
                || getIsValidPath(rowIdx)
                || getIsValidPath(rowIdx + 1)
            ) {
                // if any is valid, update the max and the valid paths for the current column
                max = colIdx;
                newValidPaths[rowIdx] = true;
                found = true;
            } else {
                newValidPaths[rowIdx] = false;
            }
        }
        // If no paths were valid in the most recent column, we're done
        if (!found) break;

        validPaths = [...newValidPaths];
    }

    return max;
}
```
# Score
Beats 33.33-66.67% on time, although I checked and this is the optimal solution. Curious what details are stopping me from hitting 100.
