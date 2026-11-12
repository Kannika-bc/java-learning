# Rotate Array – LeetCode 189

**Difficulty:** Medium  
**Topic:** Array  
**Pattern:** Two Pointers / Reversal Algorithm

## 1. Problem Statement

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example:**

```java
Input:
nums = [1, 2, 3, 4, 5, 6, 7]
k = 3

Output:
[5, 6, 7, 1, 2, 3, 4]
```

**Explanation:**

Every right rotation moves the last element to the beginning.

After 1 rotation: `[7, 1, 2, 3, 4, 5, 6]`

After 2 rotations: `[6, 7, 1, 2, 3, 4, 5]`

After 3 rotations: `[5, 6, 7, 1, 2, 3, 4]`

---

## 2. Approach 1 – Brute Force

### Idea

Rotate the array one position to the right, `k` times.

For every rotation:
1. Store the last element in a temporary variable.
2. Shift all elements one position to the right.
3. Place the stored element at index 0.

### Java Code

```java
class Solution {
    public void rotate(int[] nums, int k) {

        int n = nums.length;
        k = k % n;

        for (int r = 0; r < k; r++) {

            int last = nums[n - 1];

            for (int i = n - 1; i > 0; i--) {
                nums[i] = nums[i - 1];
            }

            nums[0] = last;
        }
    }
}
```

### Dry Run

Input:

```java
nums = [1, 2, 3, 4, 5]
k = 2
```

| Rotation | Array |
|---|---|
| Initial | [1, 2, 3, 4, 5] |
| 1 | [5, 1, 2, 3, 4] |
| 2 | [4, 5, 1, 2, 3] |

Output: `[4, 5, 1, 2, 3]`

### Complexity

- Time: O(n × k) – We shift n elements for each rotation.
- Space: O(1) – Only a temporary variable is used.

---

## 3. Approach 2 – Better (Using Extra Array)

### Idea

Create a temporary array and directly place every element at its rotated position.

**Formula:**

```java
newIndex = (i + k) % n;
```

Here:
- `i` = Original index.
- `k` = Number of rotations.
- `n` = Array length.

### Java Code

```java
class Solution {
    public void rotate(int[] nums, int k) {

        int n = nums.length;
        k = k % n;

        int[] temp = new int[n];

        for (int i = 0; i < n; i++) {
            temp[(i + k) % n] = nums[i];
        }

        for (int i = 0; i < n; i++) {
            nums[i] = temp[i];
        }
    }
}
```

### Dry Run

Input:

```java
nums = [1, 2, 3, 4, 5]
k = 2
```

| Original Index (i) | Element | New Index (i+k)%n |
|---|---|---|
| 0 | 1 | 2 |
| 1 | 2 | 3 |
| 2 | 3 | 4 |
| 3 | 4 | 0 |
| 4 | 5 | 1 |

Temporary array:

`[4, 5, 1, 2, 3]`

### Complexity

- Time: O(n) – One traversal to place elements and another to copy them.
- Space: O(n) – An additional array is created.

---

## 4. Approach 3 – Optimal (Reversal Algorithm)

### Idea

Instead of moving elements individually or creating another array, use the reversal technique.

For right rotation by `k`:

1. Reverse the entire array.
2. Reverse the first `k` elements.
3. Reverse the remaining `n-k` elements.

### Java Code

```java
class Solution {

    public void rotate(int[] nums, int k) {

        int n = nums.length;
        k = k % n;

        reverse(nums, 0, n - 1);

        reverse(nums, 0, k - 1);

        reverse(nums, k, n - 1);
    }

    public void reverse(int[] nums, int start, int end) {

        while (start < end) {

            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;

            start++;
            end--;
        }
    }
}
```

### Dry Run

Input:

```java
nums = [1, 2, 3, 4, 5, 6, 7]
k = 3
```

**Step 1: Reverse the entire array**

```text
[1, 2, 3, 4, 5, 6, 7]
                  ↓
[7, 6, 5, 4, 3, 2, 1]
```

**Step 2: Reverse the first k elements**

Reverse `[7, 6, 5]`.

```text
[5, 6, 7, 4, 3, 2, 1]
```

**Step 3: Reverse the remaining elements**

Reverse `[4, 3, 2, 1]`.

```text
[5, 6, 7, 1, 2, 3, 4]
```

**Final Output:**

```java
[5, 6, 7, 1, 2, 3, 4]
```

### Complexity

- Time: O(n) – Three reversal operations, each takes linear time at most.
- Space: O(1) – Modifies the original array without an additional array.

---

## 5. Why Do We Use k % n?

```java
k = k % n;
```

Suppose:

```java
nums = [1, 2, 3, 4, 5]
k = 7
```

The array contains 5 elements.

Rotating 5 times brings the array back to its original position.

Therefore:

```java
7 % 5 = 2
```

Rotating 7 times is equivalent to rotating 2 times.

This avoids unnecessary rotations when `k` is greater than the array length.

---

## 6. Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n × k) | O(1) |
| Extra Array | O(n) | O(n) |
| Reversal Algorithm | O(n) | O(1) |

**Optimal approach:** Reversal Algorithm.

It achieves linear time complexity and constant extra space.

---

## 7. Important Interview Points

1. What does right rotation mean? Moving the last elements to the beginning.
2. Why use `k % n`? To handle rotations greater than the array length.
3. What is in-place? Modifying the original array without using another array.
4. Why use reversal? It achieves O(n) time and O(1) extra space.
5. What is the difference between left and right rotation?
   - Right rotation moves elements towards higher indices.
   - Left rotation moves elements towards lower indices.

---

## 8. Quick Revision

**Reversal Algorithm – Remember These Three Steps:**

```java
reverse(nums, 0, n - 1);

reverse(nums, 0, k - 1);

reverse(nums, k, n - 1);
```

**Remember the sequence:**

Reverse Entire Array → Reverse First K Elements → Reverse Remaining Elements.

**Pattern:** Array Reversal + Two Pointers.

---

## 9. Related Problems

1. LeetCode 796 – Rotate String
2. LeetCode 48 – Rotate Image
3. LeetCode 151 – Reverse Words in a String
4. LeetCode 189 – Rotate Array

**Key Takeaway:** The reversal technique allows us to rotate an array to the right in O(n) time and O(1) extra space.
