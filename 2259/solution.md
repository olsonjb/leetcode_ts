# Intuition
1. **If the current digit is less than the following digit, removing the current digit will return a greater result.**

Examples:
- `353`: Remove the first 3 because 3 is less than 5 - `53` vs.  `35`
- `313`: Remove the last 3 because 3 is greater than 1 - `13` vs. `31`

2. **The more significant the digit the greater this difference is.**

Example:
- `35935`: Remove the first 3 because it is more significant - `5935` vs. `3593`


# Approach
Remove the *first* occurrence of the digit where the digit is less than the following digit.
If no occurrences of the digit are followed by a greater digit, remove the last occurrence. 

# Complexity
- Time complexity:
$$O(n)$$

- Space complexity:
$$O(1)$$

# Code
```typescript []
function removeDigit(number: string, digit: string): string {
    let current;

    for (let i = 0; i < number.length; i++) {
        if (number[i] === digit) {
            // Remove the digit from the string
            current = number.substr(0, i) + number.substr(i + 1);
            // If the following digit is greater than the current one,
            // we can return early.
            if (Number(digit) < Number(number[i + 1])) {
                return current;
            }
        }
    }

    // By default remove the last occurrence of the digit
    return current;
};
```
# Score
Beats 97-100%
