# Intuition
- We only care about the minimum distance
- We want to avoid duplicating work traversing the same item multiple times
- The index of the most recent appearance of the card is the only relevant piece of information for calculating the minimum distance to match the current card.

# Approach
Whenever you encounter a new card, store it's index in a hashmap. If you encounter a repeat card, use the hashmap to check the distance. If the distance is smaller than the minimum, update the minimum.

# Complexity
- Time complexity:
Loops once through the entire array of cards - $$O(n)$$

- Space complexity:
Adds up to one entry to map for each card - $$O(n)$$

# Code
```typescript []
function minimumCardPickup(cards: number[]): number {
    const seenMap = new Map();
    let min = Infinity;

    for(let i = 0; i < cards.length; i++) {
        const card = cards[i];
        const lastSeenAt = seenMap.get(card);
        // If we've seen the card before, check how many cards were in between
        if (lastSeenAt !== undefined) {
            const pickup = i - lastSeenAt + 1;
            min = Math.min(min, pickup);
        }
        // Update when the card was last seen
        seenMap.set(card, i);
    }

    // If there was no match (min == Infinity) return -1
    return min <= cards.length ? min : -1;
};
```
