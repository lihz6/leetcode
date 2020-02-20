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

<details>
<summary><b>2. 两数相加</b></summary>

#### 题目描述

给出两个**非空**的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

> 来源：力扣（LeetCode）https://leetcode-cn.com/problems/add-two-numbers/

#### Rust

```rust
struct ListNode {
    val: i32,
    next: Option<Box<ListNode>>,
}
impl ListNode {
    #[inline]
    fn new(val: i32) -> Self {
        ListNode { next: None, val }
    }
}
fn add_two_numbers(
    mut l1: Option<Box<ListNode>>,
    mut l2: Option<Box<ListNode>>,
) -> Option<Box<ListNode>> {
    let mut head = Some(Box::new(ListNode::new(0)));
    let (mut tail, mut step) = (&mut head, 0);
    loop {
        if l1.is_some() && l2.is_some() {
            let val = l1.as_ref().unwrap().val + l2.as_ref().unwrap().val + step;
            tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(val % 10)));
            tail = &mut tail.as_mut().unwrap().next;
            l1 = l1.unwrap().next;
            l2 = l2.unwrap().next;
            step = val / 10;
        } else if step > 0 {
            l1 = l1.or(l2);
            l2 = Some(Box::new(ListNode::new(step)));
            step = 0;
        } else {
            tail.as_mut().unwrap().next = l1.or(l2);
            break head.unwrap().next;
        }
    }
}
```

#### Python

```python
class ListNode:
    def __init__(self, x, next=None):
        self.val = x
        self.next = next


def add_two_numbers(l1: ListNode, l2: ListNode, carry: int = 0) -> ListNode:
    if l1 is None or l2 is None:
        if not carry:
            return l1 or l2
        return add_two_numbers(l1 or l2, ListNode(carry), 0)
    carry, val = divmod(l1.val + l2.val + carry, 10)
    return ListNode(val, add_two_numbers(l1.next, l2.next, carry))
```

#### JavaScript

```javascript
class ListNode {
  constructor(val, next = null) {
    this.val = val;
    this.next = next;
  }
}

const addTwoNumbers = (l1, l2, carry = 0) => {
  if (!l1 || !l2) {
    if (!carry) {
      return l1 || l2;
    }
    return addTwoNumbers(l1 || l2, new ListNode(carry), 0);
  }
  const val = l1.val + l2.val + carry;
  return new ListNode(
    val % 10,
    addTwoNumbers(l1.next, l2.next, Math.floor(val / 10))
  );
};
```

</details>

<details>
<summary><b>3. 无重复字符的最长子串</b></summary>

#### 题目描述

给定一个字符串，请你找出其中不含有重复字符的**最长子串**的长度。

**示例  1:**

```
输入: "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
　　　请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

> 来源：力扣（LeetCode）https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/

#### Rust

```rust
fn length_of_longest_substring(s: &str) -> i32 {
    use std::cmp::max;
    let (mut start, mut maxed, array) = (0, 0, s.as_bytes());
    for (i, c) in array.iter().enumerate() {
        let mut offset = start;
        while offset < i {
            if array[offset] == *c {
                maxed = max(maxed, i - start);
                start = offset + 1;
                break;
            }
            offset += 1;
        }
    }
    max(maxed, array.len() - start) as i32
}
```

#### Python

```python
def length_of_longest_substring(s: str) -> int:
    start, maxed = 0, 0
    for i, c in enumerate(s):
        offset = s.find(c, start, i)
        if offset > -1:
            maxed = max(maxed, i - start)
            start = offset + 1
    return max(maxed, len(s) - start)
```

#### JavaScript

```javascript
const lengthOfLongestSubstring = s => {
  let [start, maxed] = [0, 0];
  const array = Array.from(s);
  for (const [i, c] of array.entries()) {
    let offset = start;
    while (offset < i) {
      if (array[offset++] === c) {
        maxed = Math.max(maxed, i - start);
        start = offset;
        break;
      }
    }
  }
  return Math.max(maxed, s.length - start);
};
```

</details>

<details>
<summary><b>4. 寻找两个有序数组的中位数</b></summary>

#### 题目描述

给定两个大小为 m 和 n 的有序数组 `nums1` 和 `nums2`。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 `nums1` 和 `nums2` 不会同时为空。

**示例 1:**

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

**示例 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3) / 2 = 2.5
```

> 来源：力扣（LeetCode）https://leetcode-cn.com/problems/median-of-two-sorted-arrays/

#### Rust

```rust
fn median_of<'a>(mut nums1: &'a Vec<i32>, mut nums2: &'a Vec<i32>) -> f64 {
    if nums1.len() > nums2.len() {
        std::mem::swap(&mut nums1, &mut nums2);
    }
    let (l1, l2) = (nums1.len(), nums2.len());
    let (mut s1, mut m1, mut e1, total) = (0, l1, l1, (l1 + l2 + 1) >> 1);
    loop {
        let m2 = total - m1;
        if m1 > 0 && m2 < l2 && nums1[m1 - 1] > nums2[m2] {
            e1 = m1;
        } else {
            if m2 > 0 && m1 < l1 && nums2[m2 - 1] > nums1[m1] {
                s1 = m1;
            } else {
                let a = if m1 == 0 {
                    nums2[m2 - 1]
                } else if m2 == 0 {
                    nums1[m1 - 1]
                } else {
                    std::cmp::max(nums1[m1 - 1], nums2[m2 - 1])
                };
                let b = if (l1 + l2) % 2 > 0 {
                    a
                } else if m1 == l1 {
                    nums2[m2]
                } else if m2 == l2 {
                    nums1[m1]
                } else {
                    std::cmp::min(nums1[m1], nums2[m2])
                };
                break (a + b) as f64 / 2.0;
            }
        }
        m1 = (s1 + e1) >> 1;
    }
}
```

#### Python

```python
def median_of(nums1, nums2):
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    l1, l2 = len(nums1), len(nums2)
    s1, m1, e1, total = 0, l1, l1, (l1 + l2 + 1) >> 1

    while True:
        m2 = total - m1
        if m1 > 0 and m2 < l2 and nums1[m1 - 1] > nums2[m2]:
            m1, e1 = (s1 + m1) >> 1, m1
        else:
            if m2 > 0 and m1 < l1 and nums2[m2 - 1] > nums1[m1]:
                s1, m1 = m1, (m1 + e1) >> 1
            else:
                if m1 == 0:
                    a = nums2[m2 - 1]
                elif m2 == 0:
                    a = nums1[m1 - 1]
                else:
                    a = max(nums1[m1 - 1], nums2[m2 - 1])

                if (l1 + l2) % 2:
                    b = a
                elif m1 == l1:
                    b = nums2[m2]
                elif m2 == l2:
                    b = nums1[m1]
                else:
                    b = min(nums1[m1], nums2[m2])

                return (a + b) / 2.0
```

#### JavaScript

```javascript
const medianOf = (nums1, nums2) => {
  if (nums1.length > nums2.length) {
    [nums1, nums2] = [nums2, nums1];
  }
  const [l1, l2] = [nums1.length, nums2.length];
  const total = (l1 + l2 + 1) >> 1;
  let [s1, m1, e1] = [0, l1, l1];
  while (true) {
    const m2 = total - m1;
    if (m1 > 0 && m2 < l2 && nums1[m1 - 1] > nums2[m2]) {
      [m1, e1] = [(s1 + m1) >> 1, m1];
    } else {
      if (m2 > 0 && m1 < l1 && nums2[m2 - 1] > nums1[m1]) {
        [s1, m1] = [m1, (m1 + e1) >> 1];
      } else {
        const a =
          m1 === 0
            ? nums2[m2 - 1]
            : m2 === 0
            ? nums1[m1 - 1]
            : Math.max(nums1[m1 - 1], nums2[m2 - 1]);
        const b =
          (l1 + l2) % 2
            ? a
            : m1 === l1
            ? nums2[m2]
            : m2 === l2
            ? nums1[m1]
            : Math.min(nums1[m1], nums2[m2]);
        return (a + b) / 2.0;
      }
    }
  }
};
```

</details>
