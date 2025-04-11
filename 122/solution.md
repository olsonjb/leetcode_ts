# Intuition
We can maximize profit if **every time** the stock will be greater tomorrow than today, we buy today and sell tomorrow. If we buy under these and no other circumstances, we will maximize profits.

# Approach
I chose to base my current index `i` on the sell date. If the price today `prices[i]` is greater than the price yesterday `prices[i - 1]`, we want to buy yesterday and sell today. Add the difference, `prices[i] - prices[i - 1]`, to our running profit total.

# Complexity
- Time complexity: $$O(n)$$
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: $$O(1)$$
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```typescript []
function maxProfit(prices: number[]): number {
    let profit = 0;

    for (let i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            profit += prices[i] - prices[i - 1];
        }
    }

    return profit;
};
```
