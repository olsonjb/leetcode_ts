# Intuition
- We only need to swap elements from later back to the beginning, not vice versa.

# Approach
Iterate through each element of the array. Use an index variable `lastValid` to keep track of the part of the array that has previously been traversed and "cleaned". In this way, we can almost treat the single array as two arrays: a "cleaned" portion at the beginning with the element removed, and an "not yet cleaned" portion at the end. `lastValid` points to the end of the "cleaned" array, allowing us to push clean elements onto the end.

As we traverse the array, every time we come across a valid element (where `nums[i] !== val`), push it onto our clean array. `lastValid`  will then serve as the counter we return at the end.

# Complexity
- Time complexity:
$$O(n)$$ - loops through each element once

- Space complexity:
$$O(1)$$ - Memory used does not change based on size of `nums`

# Code
```typescript []
function removeElement(nums: number[], val: number): number {
    let lastValid = 0;

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== val) {
            nums[lastValid] = nums[i];
            lastValid++;
        }
    }

    return lastValid;
};
```
# Score
Beats 100% time
