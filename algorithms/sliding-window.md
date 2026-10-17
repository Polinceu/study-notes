# 算法：滑动窗口，求最长无重复子串

## 模板：右指针扩张，左指针收缩

```python
def length_of_longest_substring(s):
    seen = set()
    left = 0
    best = 0
    for right, ch in enumerate(s):
        while ch in seen:      # 重复了，左指针右移直到不重复
            seen.discard(s[left])
            left += 1
        seen.add(ch)
        best = max(best, right - left + 1)
    return best
```

窗口 `[left, right]` 始终保持"无重复"，
右指针每步走一格，左指针只在必要时收缩，
整体 O(n)。

## 什么时候用滑动窗口

- 连续子数组/子串问题。
- 求满足条件的最长/最短区间。
- 关键词："连续""最长""至多 K 个不同"。

## 变形：固定窗口大小

```python
def max_sum_k(nums, k):
    window = sum(nums[:k])
    best = window
    for i in range(k, len(nums)):
        window += nums[i] - nums[i - k]
        best = max(best, window)
    return best
```

固定窗口不用收缩，滑过去就行，O(n)。
记住"扩张-收缩"这个节奏，窗口题就通了。
