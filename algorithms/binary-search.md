# 算法：二分查找的边界写法（左闭右闭版）

二分查找的 bug 全在边界，固定一种写法背下来。

## 模板：区间 [left, right]

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1  # 右闭
    while left <= right:            # 注意是 <=
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

`left + (right - left) // 2` 防溢出（Python 不需要，但习惯是好的）。

## 为什么容易错

- 左闭右开 `[left, right)` 时，循环条件是 `left < right`，
  right 初始值是 `len(nums)`。两套写法别混用。
- `while left <= right` 配 `right = mid - 1`，
  `while left < right` 配 `right = mid`，错配就死循环或漏答案。

## 找"第一个 >= target"（lower_bound）

```python
def lower_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

返回的是下标，`left == len(nums)` 表示都不满足。
背住这两套，90% 的二分题直接套。
