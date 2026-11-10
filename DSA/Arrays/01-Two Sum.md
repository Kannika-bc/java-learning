# Two Sum

**LeetCode:** [Problem 1 – Two Sum](https://leetcode.com/problems/two-sum/)  
**Difficulty:** Easy  
**Topic:** Arrays, HashMap

### Problem
Given an integer array `nums` and an integer `target`, return the indices of two numbers whose sum equals the target. You cannot use the same element twice.

**Example:**
```text
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
```

## 1. Brute Force – Nested Loops

**Logic:** Check every possible pair using two loops.

```java
for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] == target) {
            return new int[]{i, j};
        }
    }
}
```

- Time: O(n²)
- Space: O(1)

## 2. Better – Sorting + Two Pointers

**Logic:** Sort values while preserving original indices. Use two pointers to find the target sum.

```java
int[][] arr = new int[nums.length][2];

for (int i = 0; i < nums.length; i++) {
    arr[i][0] = nums[i];
    arr[i][1] = i;
}

Arrays.sort(arr, (a, b) -> Integer.compare(a[0], b[0]));

int left = 0, right = arr.length - 1;

while (left < right) {
    int sum = arr[left][0] + arr[right][0];

    if (sum == target) {
        return new int[]{arr[left][1], arr[right][1]};
    } else if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

- Time: O(n log n)
- Space: O(n)

**Remember:** Preserve original indices before sorting.

## 3. Optimal – HashMap

**Logic:** Store previously visited numbers and their indices. Search for the complement.

Formula:

```java
complement = target - nums[i];
```

**Java Code:**

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

- Time: O(n) average
- Space: O(n)

**Remember:** Check complement before inserting the current element to avoid using the same element twice.

## Approach Comparison

| Approach | Time | Space |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| Two Pointers | O(n log n) | O(n) |
| HashMap | O(n) average | O(n) |

## Interview Quick Revision

- Brute force → Check every pair.
- Two pointers → Sort and move pointers.
- HashMap → Find complement in previously visited elements.
- Best approach → HashMap.
- Key concept → Complement = Target − Current Number.

**Related concepts:** Nested loops, sorting, two pointers, HashMap, time and space complexity.
