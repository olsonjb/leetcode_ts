# Intuition
- The test case strings get crazy long, so make as few retrievals as possible
- Modifying in place takes a lot longer, and doesn't save any memory for a string in JS because we have to copy the sections of the string via `substr`

# Approach
Iterate through each character in the string. Use a counter to keep track of how many times in a row we've seen the character. If we have seen the character less than 3 times in a row, append it to a new string to return.

# Complexity
- Time complexity:
$$O(n)$$ - Beats ~50%. 

- Space complexity:
$$O(n)$$ - Beats 100%. `fancyStr` can be the same size as `s`

# Code
```typescript []
function makeFancyString(s: string): string {
    let charCount = 0;
    // Keep track of previous char so we don't have to retrieve last item from fancyStr each time
    let prevChar = '';
    // Use a new string because it's faster than modifying in-place
    let fancyStr = '';

    for (let i = 0; i < s.length; i++) {
        // Store to avoid retrieving multiple times
        const currentChar = s[i];

        if (currentChar === prevChar) {
            if (charCount === 2) {
                // If we already have two don't add to our return string
                // Since nothing changed we don't need to update `prevChar` or `charCount`
                continue;
            } else {
                charCount++;
            }
        } else {
            // New char so reset the count
            charCount = 1;
        }

        fancyStr += currentChar;
        prevChar = currentChar;
    }

    return fancyStr;
};
```
