# Intuition
Using a stack allows for easy backtracking up the path in case of "/.." or chained "/../.."

# Approach
See comments. Looking back, I think this could have been much simpler had I split the path at the beginning and gone word by word rather than character by character.

# Complexity
- Time complexity: $$O(n)$$ - go through path string one time.

- Space complexity: $$O(n)$$ - stack for simplified path can be same size as original path

# Code
```typescript []
function simplifyPath(path: string): string {
    // Our simplified stack of directories we will be returning
    const dirs = [];
    // Helper vars to determine when to push/pop/ignore. Term "Escaped" indicates a state where current characters are NOT directly added to our simplified path string
    let prevEscaped = false;
    let isEscaped = true;
    let dotCount = 0;

    for (const char of path) {
        // "/" Indicates we are going to have a new directory (or "/.", or "/..")
        if (char === '/') isEscaped = true;
        else if (
            // Non "." or "/" always is part of a directory name
            char !== '.'
            // If we've had more than two dots, e.g. "/.../"
            || dotCount >= 2
            // Previous character was not part of special syntax, useful for catching cases where "."s come after letters, e.g. "/home.."
            || !prevEscaped) {
            isEscaped = false;
        }

        // "/../" case
        if (char === '/' && dotCount === 2) {
            dirs.pop();
        }

        let charsToAdd = '';
        if (!isEscaped) {
            // Account for dots we passed over initially, e.g. cases "/.../" or "/.hidden"
            if (dotCount === 1) {
                charsToAdd += '.';
            } else if (dotCount === 2) {
                charsToAdd += '..';
            }

            charsToAdd += char;
        }

        if (charsToAdd) {
            if (prevEscaped) {
                // If previously escaped (i.e. last char was "/"), this is a new directory
                dirs.push(charsToAdd);
            } else {
                // Otherwise it's a character to add to the last directory
                dirs.push(dirs.pop() + charsToAdd);
            }
        }

        if (char === '.' && isEscaped) dotCount++;
        else dotCount = 0;

        prevEscaped = isEscaped;
    }

    // If we ended with ".."
    if (dotCount === 2) dirs.pop();

    return '/' + dirs.join('/');
};
```
