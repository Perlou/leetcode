# 1. 两数之和

## 题目链接

- [LeetCode 中文](https://leetcode.cn/problems/two-sum)
- [LeetCode 英文](https://leetcode.com/problems/two-sum)

## 问题理解

### 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，找出数组中和为目标值的**两个**整数，并返回它们的**数组下标**。

### 输入输出

- **输入**：整数数组 `nums` 和目标值 `target`
- **输出**：两个整数的下标 `[i, j]`，使得 `nums[i] + nums[j] = target`
- **示例**：
  ```
  输入：nums = [2,7,11,15], target = 9
  输出：[0,1]
  解释：nums[0] + nums[1] = 2 + 7 = 9
  ```

### 约束条件

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- **只会存在一个有效答案**
- **不能使用两次相同的元素**（不能用同一个位置的数字两次）
- 可以按任意顺序返回答案

---

## 思路探索

### 方法一：暴力解法（双重循环）

#### 思路

最直观的想法：用两层循环遍历所有可能的数字组合，检查它们的和是否等于 `target`。

- **外层循环**：遍历每个元素 `nums[i]`
- **内层循环**：从 `i+1` 开始遍历剩余元素 `nums[j]`
- **判断**：如果 `nums[i] + nums[j] === target`，返回 `[i, j]`

#### 为什么内层循环从 i+1 开始？

- 避免重复检查（如 `[0,1]` 和 `[1,0]` 是同一对）
- 避免使用同一个元素两次（如 `[0,0]`）

#### 时间复杂度

- **O(n²)** - 外层循环 n 次，内层循环平均 n/2 次

#### 空间复杂度

- **O(1)** - 只使用了常数个变量

#### 优缺点

- ✅ 简单直观，容易理解
- ❌ 效率低，数据量大时会超时

---

### 方法二：哈希表优化（一次遍历）

#### 思路

核心思想：**用空间换时间**

当遍历到 `nums[i]` 时，我们需要找 `complement = target - nums[i]`：

- **暴力做法**：再遍历一次数组查找 → O(n)
- **优化做法**：用哈希表查找 → O(1)

**算法流程**：

1. 创建一个空的 `Map`（哈希表）
2. 遍历数组：
   - 计算 `complement = target - nums[i]`
   - **先查询**：哈希表中是否存在 `complement`
     - 如果存在 → 找到答案，返回 `[map.get(complement), i]`
     - 如果不存在 → 把当前数字和下标存入哈希表
3. 继续遍历直到找到答案

#### 哈希表存储什么？

- **Key**：数组中的数值 `nums[i]`
- **Value**：该数值的下标 `i`

#### 为什么先查询再存入？

避免使用同一个元素两次。例如 `nums = [3, 3], target = 6`：

- 遍历到第一个 3 时，查询 Map 中是否有 3 → 没有，存入 `{3: 0}`
- 遍历到第二个 3 时，查询 Map 中是否有 3 → 有！返回 `[0, 1]`

如果先把所有元素存入 Map，再查询，会导致同一个元素被使用两次。

#### 时间复杂度

- **O(n)** - 只需遍历一次数组
- 哈希表的查询和插入操作都是 O(1)

#### 空间复杂度

- **O(n)** - 最坏情况下，所有元素都需要存入哈希表

#### 优缺点

- ✅ 效率高，时间复杂度线性
- ✅ 满足进阶要求（时间复杂度小于 O(n²)）
- ❌ 需要额外的空间存储哈希表

---

## 代码实现

### 方法一：暴力解法

```typescript
function twoSum(nums: number[], target: number): number[] {
  const n = nums.length;
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
  return []; // 题目保证有答案，实际不会执行到这里
}
```

```java
public int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[] { i, j };
            }
        }
    }
    return new int[] {};
}
```

```go
func twoSum(nums []int, target int) []int {
    n := len(nums)
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            if nums[i] + nums[j] == target {
                return []int{i, j}
            }
        }
    }
    return []int{}
}
```

```python
def two_sum(nums: List[int], target: int) -> List[int]:
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

**关键点**：

- 内层循环从 `i + 1` 开始，避免重复和使用同一元素
- 使用 `===` 严格相等（TypeScript 最佳实践）

---

### 方法二：哈希表优化（推荐）

```typescript
function twoSum(nums: number[], target: number): number[] {
  const hashTable = new Map<number, number>();

  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];

    if (hashTable.has(complement)) {
      return [hashTable.get(complement)!, i];
    }

    hashTable.set(nums[i], i);
  }

  return []; // 题目保证有答案，实际不会执行到这里
}
```

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    return new int[] {};
}
```

```go
func twoSum(nums []int, target int) []int {
    hashTable := make(map[int]int)
    for i, num := range nums {
        complement := target - num
        if val, ok := hashTable[complement]; ok {
            return []int{val, i}
        }
        hashTable[num] = i
    }
    return []int{}
}
```

**关键点**：

- `Map<number, number>` 类型注解（TypeScript 类型安全）
- 先查询，再存入（避免使用同一元素两次）
- `hashTable.get(complement)!` 使用非空断言（因为已经用 `has` 检查过）

---

## 两种解法对比

| 维度           | 暴力解法 | 哈希表优化       |
| -------------- | -------- | ---------------- |
| **时间复杂度** | O(n²)    | O(n)             |
| **空间复杂度** | O(1)     | O(n)             |
| **代码难度**   | 简单     | 中等             |
| **适用场景**   | 数据量小 | 数据量大（推荐） |
| **核心思想**   | 双重遍历 | 空间换时间       |

---

## 关键点总结

### 算法技巧

1. **暴力解法 → 优化思路**

   - 识别暴力解法的瓶颈（内层循环的 O(n) 查找）
   - 用哈希表将查找优化为 O(1)

2. **空间换时间**

   - 经典的算法优化思想
   - 用 O(n) 的额外空间，换取时间从 O(n²) 降到 O(n)

3. **哈希表的使用场景**
   - 需要快速查找某个值是否存在
   - 需要快速查找某个值对应的其他信息（如下标）

### 常见陷阱

1. ❌ **使用同一元素两次**

   - 错误做法：先把所有元素存入 Map，再查询
   - 正确做法：边遍历边查询，先查后存

2. ❌ **返回值顺序错误**

   - 注意先找到的下标在前：`[hashTable.get(complement), i]`

3. ❌ **使用 `==` 而不是 `===`**
   - TypeScript 推荐使用严格相等 `===`

---

## 相关题目

- [15. 三数之和](https://leetcode.cn/problems/3sum) - Two Sum 的进阶版
- [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted) - 数组有序，可用双指针
- [170. 两数之和 III - 数据结构设计](https://leetcode.cn/problems/two-sum-iii-data-structure-design) - 设计数据结构
- [653. 两数之和 IV - 输入二叉搜索树](https://leetcode.cn/problems/two-sum-iv-input-is-a-bst) - 二叉树版本

---

## 学习日志

- **2025-11-28**:
  - ✅ 第一次尝试，成功实现暴力解法（双重循环）
  - ✅ 分析时间复杂度为 O(n²)，空间复杂度为 O(1)
  - ✅ 思考优化方案，使用哈希表将时间复杂度优化到 O(n)
  - ✅ 理解"空间换时间"的核心思想
  - ✅ 掌握哈希表的应用场景和使用技巧
  - 💡 **关键收获**: 学会了如何从暴力解法出发，识别瓶颈，逐步优化算法
