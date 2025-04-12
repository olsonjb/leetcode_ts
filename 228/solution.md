_Even though this was an "easy", I have to admit how satisfied I was to write this one end-to-end in one shot, then beat 100% on time on the very first submission._

# Intuition
Because `nums` is sorted, we only need to consider the previous number we've added to our ranges.

# Approach
Because we are only concerned with the number we've most recently added to our ranges, we can use a stack `ranges` and pop/push the previous number off the stack as we go.

# Complexity
- Time complexity: $$O(n)$$ - Loop through all of nums once, and then map over the ranges to join with `->`, $$O(2n)$$ reduces to $$O(n)$$

- Space complexity: $$O(n)$$ - Creating a new list of ranges, potentially as long as nums.

# Code
```typescript []
function summaryRanges(nums: number[]): string[] {
    // stack of ranges
    const ranges = [];

    for (const num of nums) {
        // Our highest range will be on top of the stack
        const top = ranges.pop();

        if (top === undefined) {
            // If we don't have any ranges yet, push a new one
            ranges.push([num]);
        } else if (top.length === 1) {
            if (top[0] === num - 1) {
                // Our number is consecutive, add it to the range 
                ranges.push([...top, num]);
            } else {
                // Our number is not consecutive, create a new range
                ranges.push(top, [num]);
            }
        } else if (top.length === 2) {
            if (top[1] === num - 1) {
                // Our number is consecutive, replace the high end of the range
                ranges.push([top[0], num]);
            } else {
                // Our number is not consecutive, create a new range
                ranges.push(top, [num]);
            }
        }
    }

    // Turn our stack of ranges into an array of strings
    return ranges.map(range => range.join('->'));
};
```
