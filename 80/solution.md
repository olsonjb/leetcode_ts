# Intuition
- We should be able to do this in linear time
- We need to manage two locations in the array
    - Where have we sorted so far _before_ removing duplicates
    - Where have we sorted so far _after_ removing duplicates
- To determine the number of duplicates, we need to remember how many times in a row we've seen the element

# Approach
Two pointer approach: index `i` tracks where we have sorted _before_/_including_ duplicates, while index `sortedIdx` keeps track of where we have sorted _after/without_.

Setting `nums[sortedIdx] = nums[i]` is equivalent to pushing to the sorted array.

Use the previous character `prevChar` to determine whether to increment `countDuplicates`, which tracks how many times we've seen the current character. Remember to reset `countDuplicates = 0` if `prevChar !== nums[i]`.

If we've seen `nums[i]` more than twice, do not push it onto our sorted array.

# Complexity
- Time complexity: $$O(n)$$

- Space complexity: $$O(1)$$

# Code
```typescript []
function removeDuplicates(nums: number[]): number {
    let prevChar = undefined;
    let countDuplicate = 0;
    let sortedIdx = 0;

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === prevChar) countDuplicate++;
        else countDuplicate = 0;
        
        if (countDuplicate < 2) {
            prevChar = nums[i];

            nums[sortedIdx] =  nums[i];
            sortedIdx++;
        }
    }

    return sortedIdx;
};
```
