# Majority Element – LeetCode 169

## Problem
Find the element that appears more than `n/2` times in an array.

Example:
```java
Input:  [2, 2, 1, 1, 1, 2, 2]
Output: 2
```

## 1. Brute Force Approach

**Logic:**
1. Use an outer loop to select each element as a candidate.
2. Initialize `count = 0`.
3. Use an inner loop to count how many times the candidate appears.
4. If `count > n/2`, return that element.
5. Otherwise, move to the next element.

**Code:**
```java
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            int count = 0;

            for (int j = 0; j < n; j++) {
                if (nums[i] == nums[j]) {
                    count++;
                }
            }

            if (count > n / 2) {
                return nums[i];
            }
        }

        return -1;
    }
}
```

**Complexity:**
- Time: O(n²)
- Space: O(1)

## 2. Optimal Approach – Boyer-Moore Voting Algorithm

**Logic:**
1. Initialize `count = 0` and `candidate = 0`.
2. Traverse the array.
3. If `count == 0`, select the current element as the candidate.
4. If the current element equals the candidate, increment count.
5. Otherwise, decrement count.
6. Return the final candidate.

**Why does it work?**

Different elements cancel each other out. Since the majority element appears more than n/2 times, it cannot be completely cancelled by the other elements.

**Code:**
```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }

            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }
}
```

**Complexity:**
- Time: O(n) – Single traversal.
- Space: O(1) – Only two extra variables.

## 3. Dry Run

Input: `[2, 2, 1, 1, 1, 2, 2]`

| Element | Candidate | Count |
|---|---:|---:|
| 2 | 2 | 1 |
| 2 | 2 | 2 |
| 1 | 2 | 1 |
| 1 | 2 | 0 |
| 1 | 1 | 1 |
| 2 | 1 | 0 |
| 2 | 2 | 1 |

Output: `2`

## 4. Quick Revision

**Brute Force:** Pick element → Count frequency → Check if count > n/2.

**Boyer-Moore:**
- `count == 0` → Update candidate.
- Same element → `count++`.
- Different element → `count--`.

**Remember:** If a majority element is not guaranteed to exist, verify the final candidate by counting its occurrences in a second traversal.
