# Two Sum

**Difficulty:** Easy  
**Topics:** Array, HashMap  
**Pattern:** Pair Sum / Complement

## 1. Problem Statement

Given an integer array `nums` and an integer `target`, return the indices of two numbers whose sum equals the target.

**Conditions:**
- Each input has exactly one solution.
- We cannot use the same element twice.
- Return indices, not the actual numbers.

**Example:**

```java
Input:
nums = [2, 7, 11, 15]
target = 9

Output:
[0, 1]
```

**Explanation:** `nums[0] + nums[1] = 2 + 7 = 9`

---

## 2. Approach 1 – Brute Force

### Idea

Check every possible pair of numbers using two nested loops.

For every number, compare it with all the numbers that come after it.

### Java Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {

            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }

        return new int[]{};
    }
}
```

### Dry Run

**Input:**

```java
nums = [2, 7, 11, 15]
target = 9
```

| i | j | Calculation | Result |
|---|---|---|---|
| 0 | 1 | 2 + 7 = 9 | Found! |

Output:

```java
[0, 1]
```

### Complexity Analysis

- **Time Complexity: O(n²)** – Two nested loops check possible pairs.
- **Space Complexity: O(1)** – No extra data structure is used.

---

## 3. Approach 2 – Optimal (HashMap)

### Idea

Instead of checking every possible pair, use a HashMap to store previously visited numbers and their indices.

For every number, calculate its complement (the number needed to reach the target).

**Formula:**

```java
complement = target - currentNumber;
```

Check whether this complement already exists in the HashMap.

### Steps

1. Create a HashMap to store numbers and their indices.
2. Traverse the array using a loop.
3. Calculate the complement: `target - nums[i]`.
4. Check if the complement exists using `containsKey()`.
5. If it exists, return the complement's index and current index.
6. Otherwise, store the current number and its index using `put()`.

### Java Code

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

### Dry Run

**Input:**

```java
nums = [2, 7, 11, 15]
target = 9
```

Initially:

```java
map = {}
```

| Iteration | Current Number | Complement | Action |
|---|---:|---:|---|
| 1 | 2 | 7 | 7 not found. Store 2 → 0 |
| 2 | 7 | 2 | 2 found! Return indices |

**Iteration 1:**

```java
complement = 9 - 2 = 7;
```

7 is not present in the map.

Store:

```java
map.put(2, 0);
```

Map: `{2=0}`

**Iteration 2:**

```java
complement = 9 - 7 = 2;
```

2 is already present!

```java
return new int[]{map.get(2), 1};
```

Output:

```java
[0, 1]
```

### Complexity Analysis

- **Time Complexity: O(n)** – We traverse the array once. HashMap lookup and insertion take O(1) average time.
- **Space Complexity: O(n)** – HashMap can store up to n elements.

---

## 4. Brute Force vs Optimal

| Comparison | Brute Force | HashMap |
|---|---|---|
| Technique | Nested loops | HashMap |
| Time | O(n²) | O(n) average |
| Space | O(1) | O(n) |
| Traversals | Two loops | One loop |
| Efficiency | Slower | Faster |

**Why is HashMap better?**

Brute force repeatedly checks pairs. HashMap remembers previously visited numbers, allowing us to check whether the required complement exists in average O(1) time.

We trade extra space for faster execution.

---

## 5. Important HashMap Methods

| Method | Purpose |
|---|---|
| `put(key, value)` | Stores a key-value pair |
| `containsKey(key)` | Checks whether a key exists |
| `get(key)` | Returns the value associated with a key |

**In this problem:**
- Key = Number from the array
- Value = Index of that number

Example:

```java
map.put(2, 0);
```

Means number `2` is present at index `0`.

---

## 6. Important Interview Points

1. Why use HashMap? To find previously visited numbers quickly.
2. What is a complement? The number needed to reach the target.
3. Why store indices? Because the problem asks us to return indices.
4. Why check before inserting? To avoid using the same array element twice.
5. Can HashMap contain duplicate numbers? It can process duplicate array values, but a HashMap stores only one value per key. A repeated key updates its previous value.

---

## 7. Quick Revision

**Brute Force Pattern:**

```java
for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] == target) {
            return new int[]{i, j};
        }
    }
}
```

**Optimal Pattern:**

```java
HashMap<Integer, Integer> map = new HashMap<>();

for (int i = 0; i < nums.length; i++) {

    int complement = target - nums[i];

    if (map.containsKey(complement)) {
        return new int[]{map.get(complement), i};
    }

    map.put(nums[i], i);
}
```

**Remember the HashMap sequence:**

Calculate → Check → Return (if found) → Store

**Key learning:** HashMap + Complement pattern.

---

## 8. Related Problems to Practise

1. LeetCode 167 – Two Sum II (Input Array Is Sorted)
2. LeetCode 217 – Contains Duplicate
3. LeetCode 219 – Contains Duplicate II
4. LeetCode 49 – Group Anagrams
5. LeetCode 128 – Longest Consecutive Sequence
