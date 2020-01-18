<details>
<summary><b>1. 两数之和</b></summary>

#### 题目描述

给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例：**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

> 来源：力扣（LeetCode）https://leetcode-cn.com/problems/two-sum/

#### Rust

```rust
fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    let mut map = std::collections::HashMap::new();
    for (i, n) in nums.into_iter().enumerate() {
        if map.contains_key(&n) {
            return vec![*map.get(&n).unwrap() as i32, i as i32];
        }
        map.insert(target - n, i);
    }

    unreachable!();
}
```

#### Python

```python
def two_sum(nums: List[int], target: int) -> List[int]:
    nidict: Dict[int, int] = {}
    for i, n in enumerate(nums):
        if n in nidict:
            return [nidict[n], i]

        nidict[target - n] = i

    raise ValueError()
```

#### JavaScript

```javascript
const twoSum = (nums, target) => {
  const map = new Map();
  for (const [i, n] of nums.entries()) {
    if (map.has(n)) {
      return [map.get(n), i];
    }
    map.set(target - n, i);
  }

  throw new Error();
};
```

</details>
