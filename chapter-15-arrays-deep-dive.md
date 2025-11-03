# Chapter 15: Arrays Deep Dive

**What You'll Learn**: Advanced array techniques, common patterns, two-pointer method, sliding window, and problem-solving strategies.

## Table of Contents

1. [Introduction](#introduction)
2. [1. Two-Pointer Technique](#1-two-pointer-technique)
3. [2. Sliding Window Technique](#2-sliding-window-technique)
4. [3. Prefix Sum Technique](#3-prefix-sum-technique)
5. [4. Kadane's Algorithm (Maximum Subarray Sum)](#4-kadanes-algorithm-maximum-subarray-sum)
6. [5. Dutch National Flag Problem](#5-dutch-national-flag-problem)
7. [Key Patterns Summary](#key-patterns-summary)
8. [What's Next?](#whats-next)

---

## Introduction

Arrays are the foundation of many algorithms. Mastering array techniques will make you a better problem solver!

---

## 1. Two-Pointer Technique

**Pattern**: Use two pointers moving toward each other or in same direction.

### Example 1: Reverse Array

```cpp
void reverseArray(int arr[], int n) {
    int left = 0, right = n - 1;
    while (left < right) {
        swap(arr[left], arr[right]);
        left++;
        right--;
    }
}
// O(n) time, O(1) space
```

### Example 2: Two Sum (Sorted Array)

```cpp
pair<int, int> twoSum(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return {left, right};
        else if (sum < target) left++;
        else right--;
    }
    return {-1, -1};
}
```

---

## 2. Sliding Window Technique

**Pattern**: Maintain a window of elements, slide it across array.

### Example: Maximum Sum Subarray of Size K

```cpp
int maxSumSubarray(vector<int>& arr, int k) {
    int n = arr.size();
    if (n < k) return -1;
    
    // Initial window
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide window
    for (int i = k; i < n; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = max(maxSum, windowSum);
    }
    
    return maxSum;
}
```

---

## 3. Prefix Sum Technique

**Pattern**: Precompute cumulative sums for fast range queries.

```cpp
vector<int> computePrefixSum(vector<int>& arr) {
    int n = arr.size();
    vector<int> prefix(n);
    prefix[0] = arr[0];
    
    for (int i = 1; i < n; i++) {
        prefix[i] = prefix[i-1] + arr[i];
    }
    
    return prefix;
}

// Query sum from index l to r
int rangeSum(vector<int>& prefix, int l, int r) {
    if (l == 0) return prefix[r];
    return prefix[r] - prefix[l-1];
}
// O(1) per query after O(n) preprocessing
```

---

## 4. Kadane's Algorithm (Maximum Subarray Sum)

```cpp
int maxSubarraySum(vector<int>& arr) {
    int maxSoFar = arr[0];
    int maxEndingHere = arr[0];
    
    for (int i = 1; i < arr.size(); i++) {
        maxEndingHere = max(arr[i], maxEndingHere + arr[i]);
        maxSoFar = max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
// O(n) time, O(1) space
```

---

## 5. Dutch National Flag Problem

**Problem**: Sort array of 0s, 1s, and 2s.

```cpp
void sortColors(vector<int>& arr) {
    int low = 0, mid = 0, high = arr.size() - 1;
    
    while (mid <= high) {
        if (arr[mid] == 0) {
            swap(arr[low], arr[mid]);
            low++;
            mid++;
        } else if (arr[mid] == 1) {
            mid++;
        } else {
            swap(arr[mid], arr[high]);
            high--;
        }
    }
}
// O(n) time, O(1) space, single pass
```

---

## Key Patterns Summary

✅ **Two Pointer**: Use for sorted arrays, palindrome checks, pair problems
✅ **Sliding Window**: Fixed or variable size windows for substring/subarray problems
✅ **Prefix Sum**: Fast range queries
✅ **Kadane's**: Maximum subarray sum
✅ **Dutch Flag**: Three-way partitioning

---

## What's Next?

In **[Chapter 16](chapter-16-linked-lists.md)**, you'll learn about **Linked Lists** - dynamic data structures that don't need contiguous memory!

---

**Chapter Progress**: ✅ Chapters 1-15 Complete | 📖 Next: Chapter 16
