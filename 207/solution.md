# Intuition
Our courses can be drawn as a graph, and any cycles in that graph make it impossible to complete our courses.

# Approach
Build a graph and run DFS to check for cycles

# Complexity
- Time complexity: $$O(V + E)$$ Will visit every node via every edge (optimized to only do so once)

- Space complexity: $$O(V + E)$$

# Code
```typescript []
function canFinish(numCourses: number, prerequisites: number[][]): boolean {
    let reqMap = new Map();

    // Build a graph in our hashmap (technically an adjacency list)
    for (const prereq of prerequisites) {
        if (reqMap.has(prereq[0])) {
            reqMap.get(prereq[0]).push(prereq[1]);
        } else {
            reqMap.set(prereq[0], [prereq[1]]);
        }
    }

    // Check for cycles - our DFS function
    const dependsOnCourse = (courseToCheck: number, visited: Set<number>) => {
        // If we've seen this course already we have a cycle
        if (visited.has(courseToCheck)) return true;
        visited.add(courseToCheck);

        // check each branch from our current node
        for (const prereq of reqMap.get(courseToCheck) ?? []) {
            if (dependsOnCourse(prereq, visited)) return true;
        }

        // Since we're editing `visited` in place, we need to clear our
        // current node so that we don't have it in `visited` 
        // when we go down a different branch
        visited.delete(courseToCheck)
        // We can prune this from our graph to avoid re-checking because we know it's safe
        reqMap.set(courseToCheck, [])

        return false;
    }

    for (const [course, prereqs] of reqMap.entries()) {
        for (const prereq of prereqs) {
            if (dependsOnCourse(prereq, new Set())) return false;
        }
    }

    return true;
};
```
