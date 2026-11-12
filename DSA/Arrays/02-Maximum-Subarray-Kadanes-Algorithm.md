# Maximum Subarray Sum – Kadane's Algorithm

**LeetCode:** 53 – Maximum Subarray  
**Difficulty:** Medium  
**Topics:** Array, Dynamic Programming, Greedy  
**Pattern:** Maximum Subarray Sum

## 1. Problem Statement

Given an integer array `nums`, find the contiguous subarray (containing at least one element) that has the largest sum. Return the maximum sum.

**Example:**

```java
Input:
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Output:
6
```

**Explanation:**

The maximum-sum subarray is:

```java
[4, -1, 2, 1]
```

Sum = `4 + (-1) + 2 + 1 = 6`

**Important:** A subarray must contain continuous elements. We cannot skip elements in between.

---

## 2. Approach 1 – Brute Force

### Idea

Generate every possible subarray and calculate its sum. Keep track of the maximum sum.

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int maxSum = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++) {

            for (int j = i; j < nums.length; j++) {

                int sum = 0;

                for (int k = i; k <= j; k++) {
                    sum += nums[k];
                }

                maxSum = Math.max(maxSum, sum);
            }
        }

        return maxSum;
    }
}
```

### Explanation

- First loop (`i`): Selects the starting index.
- Second loop (`j`): Selects the ending index.
- Third loop (`k`): Calculates the sum of elements between `i` and `j`.
- `maxSum`: Stores the largest sum found.

### Complexity

- Time: O(n³) – Three nested loops.
- Space: O(1) – No extra data structure.

---

## 3. Approach 2 – Better (Running Sum)

### Idea

Instead of calculating the sum of every subarray again, maintain a running sum.

For every starting index:
1. Initialize `sum = 0`.
2. Add the next element to the running sum.
3. Update `maxSum` whenever a larger sum is found.

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int maxSum = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++) {

            int sum = 0;

            for (int j = i; j < nums.length; j++) {

                sum += nums[j];

                maxSum = Math.max(maxSum, sum);
            }
        }

        return maxSum;
    }
}
```

### Explanation

We calculate the sum incrementally instead of using another loop to recalculate it.

For example:

```java
nums = [2, -1, 3]
```

Starting from index 0:

- `[2]` → sum = 2
- `[2, -1]` → sum = 1
- `[2, -1, 3]` → sum = 4

### Complexity

- Time: O(n²) – Two nested loops.
- Space: O(1) – Only variables are used.

---

## 4. Approach 3 – Optimal (Kadane's Algorithm)

### Idea

Kadane's Algorithm finds the maximum subarray sum in a single traversal.

At every element, we have two choices:

1. Start a new subarray from the current element.
2. Continue the existing subarray by adding the current element.

If the previous running sum is negative, starting fresh may give a better result.

### Important Variables

| Variable | Purpose |
|---|---|
| `currentSum` | Maximum subarray sum ending at the current position |
| `maxSum` | Maximum subarray sum found so far |

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int currentSum = nums[0];
        int maxSum = nums[0];

        for (int i = 1; i < nums.length; i++) {

            currentSum = Math.max(nums[i], currentSum + nums[i]);

            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
}
```

### Understanding the Main Logic

**Line 1:**

```java
currentSum = Math.max(nums[i], currentSum + nums[i]);
```

Compares two choices:

- `nums[i]`: Start a new subarray from the current element.
- `currentSum + nums[i]`: Continue the previous subarray.

Choose whichever gives the larger sum.

**Line 2:**

```java
maxSum = Math.max(maxSum, currentSum);
```

Updates the maximum sum found so far.

---

## 5. Dry Run – Kadane's Algorithm

**Input:**

```java
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Initially:

```java
currentSum = -2;
maxSum = -2;
```

| Current element | Calculation | currentSum | maxSum |
|---|---|---:|---:|
| -2 | Initial value | -2 | -2 |
| 1 | max(1, -2 + 1) | 1 | 1 |
| -3 | max(-3, 1 - 3) | -2 | 1 |
| 4 | max(4, -2 + 4) | 4 | 4 |
| -1 | max(-1, 4 - 1) | 3 | 4 |
| 2 | max(2, 3 + 2) | 5 | 5 |
| 1 | max(1, 5 + 1) | 6 | 6 |
| -5 | max(-5, 6 - 5) | 1 | 6 |
| 4 | max(4, 1 + 4) | 5 | 6 |

**Final Answer: 6**

Maximum subarray: `[4, -1, 2, 1]`

---

## 6. Edge Case – All Negative Numbers

```java
nums = [-5, -2, -8]
```

Output:

```java
-2
```

Why?

The problem requires at least one element in the subarray. Therefore, we cannot return 0.

This is why we initialize:

```java
int currentSum = nums[0];
int maxSum = nums[0];
```

Instead of initializing both variables to zero.

---

## 7. Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n³) | O(1) |
| Better – Running Sum | O(n²) | O(1) |
| Kadane's Algorithm | O(n) | O(1) |

**Why is Kadane's Algorithm optimal?**

It avoids generating every possible subarray. It makes the best decision at each position and traverses the array only once.

---

## 8. Quick Revision

**Kadane's Algorithm – Remember These Steps:**

1. Initialize `currentSum` and `maxSum` with the first element.
2. Traverse the array from index 1.
3. Decide whether to start fresh or extend the existing subarray.
4. Update `maxSum`.
5. Return `maxSum`.

**Two important lines:**

```java
currentSum = Math.max(nums[i], currentSum + nums[i]);

maxSum = Math.max(maxSum, currentSum);
```

**Pattern:** Greedy choice + Dynamic Programming intuition.

---

## 9. Related Problems

1. LeetCode 918 – Maximum Sum Circular Subarray
2. LeetCode 152 – Maximum Product Subarray
3. LeetCode 121 – Best Time to Buy and Sell Stock
4. LeetCode 53 – Maximum Subarray

**Key takeaway:** Kadane's Algorithm uses the idea of either extending the previous subarray or starting a new one to find the maximum contiguous subarray sum in O(n) time and O(1) space.
