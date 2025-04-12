# Intuition
Add words to each line first, then adjust the spaces after determining how many words can fit.

# Approach
Add words to the last line until they no longer fit. At that point, determine how many spaces there are and how many are still missing. Then loop back through the spaces we already have, adding the missing ones until the line hits the max width.

Be sure to account for special cases such as the very last line, and lines with just a single word.

# Complexity
- Time complexity: $$O(n)$$ - we'll iterate through each line twice, once to determine how many words and once to fix the spaces

- Space complexity: $$O(n)$$ - size of return value `lines` is directly based on size of argument `words`

# Code
```typescript []
function fullJustify(words: string[], maxWidth: number): string[] {
    let lines = [];

    for (const word of words) {
        const last = lines.pop();

        if (!last) {
            lines.push(word);
        } else if (last.length + word.length + 1 <= maxWidth) {
            // add words to the end of the last line until they can't fit
            lines.push(`${last} ${word}`);
        } else {
            // When the word no longer fits on the last line
            // fix spaces on previous line...
            const lastLineWords = last.split(' ');
            const numChars = lastLineWords.join('').length;
            // We need at least 1 gap unless word is already max width
            const numGaps = numChars < maxWidth ? Math.max(1, lastLineWords.length - 1) : 0;
            const spacesNeeded = maxWidth - numChars - numGaps;

            // Take turns adding the missing spaces to the gaps left->right
            let gaps = new Array(numGaps).fill(' ');
            for (let j = 0; j < spacesNeeded; j++) {
                gaps[j % gaps.length] += ' ';
            }

            let newLast = '';
            for (let j = 0; j < lastLineWords.length; j++) {
                newLast += lastLineWords[j] + (gaps[j] ?? '');
            }
            lines.push(newLast);

            // ...and add our word to a new line.
            lines.push(word);
        }
    }

    // add trailing spaces to last line
    while (lines[lines.length - 1].length < maxWidth) {
        lines[lines.length - 1] += ' ';
    }
    
    return lines;
};
```
