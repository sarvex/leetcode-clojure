# [1031. 两个非重叠子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-two-non-overlapping-subarrays)

[English Version](/solution/1000-1099/1031.Maximum%20Sum%20of%20Two%20Non-Overlapping%20Subarrays/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给你一个整数数组 <code>nums</code> 和两个整数 <code>firstLen</code> 和 <code>secondLen</code>，请你找出并返回两个非重叠<strong> 子数组 </strong>中元素的最大和<em>，</em>长度分别为 <code>firstLen</code> 和 <code>secondLen</code> 。</p>

<p>长度为 <code>firstLen</code> 的子数组可以出现在长为 <code>secondLen</code> 的子数组之前或之后，但二者必须是不重叠的。</p>

<p>子数组是数组的一个 <strong>连续</strong> 部分。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2
<strong>输出：</strong>20
<strong>解释：</strong>子数组的一种选择中，[9] 长度为 1，[6,5] 长度为 2。
</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2
<strong>输出：</strong>29
<strong>解释：</strong>子数组的一种选择中，[3,8,1] 长度为 3，[8,9] 长度为 2。
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3
<strong>输出：</strong>31
<strong>解释：</strong>子数组的一种选择中，[5,6,0,9] 长度为 4，[0,3,8] 长度为 3。
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 &lt;= firstLen, secondLen &lt;= 1000</code></li>
	<li><code>2 &lt;= firstLen + secondLen &lt;= 1000</code></li>
	<li><code>firstLen + secondLen &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

**方法一：前缀和 + 枚举**

我们先预处理得到数组 $nums$ 的前缀和数组 $s$，其中 $s[i]$ 表示 $nums$ 中前 $i$ 个元素的和。

接下来，我们分两种情况枚举：

假设 $firstLen$ 个元素的子数组在 $secondLen$ 个元素的子数组的左边，那么我们可以枚举 $secondLen$ 个元素的子数组的左端点 $i$，用变量 $t$ 维护左边 $firstLen$ 个元素的子数组的最大和，那么当前最大和就是 $t + s[i + secondLen] - s[i]$。其中 $s[i + secondLen] - s[i]$ 表示 $secondLen$ 个元素的子数组的和。枚举完所有的 $i$，就得到了第一种情况下的最大和。

假设 $secondLen$ 个元素的子数组在 $firstLen$ 个元素的子数组的左边，那么我们可以枚举 $firstLen$ 个元素的子数组的左端点 $i$，用变量 $t$ 维护左边 $secondLen$ 个元素的子数组的最大和，那么当前最大和就是 $t + s[i + firstLen] - s[i]$。其中 $s[i + firstLen] - s[i]$ 表示 $firstLen$ 个元素的子数组的和。枚举完所有的 $i$，就得到了第二种情况下的最大和。

取两种情况下的最大值作为答案即可。

我们也可以将上面两个情况抽取成一个函数 $f(a, b)$，那么答案就是 $\max(f(firstLen, secondLen), f(secondLen, firstLen))$。

时间复杂度 $O(n)$，空间复杂度 $O(n)$。其中 $n$ 为数组 $nums$ 的长度。

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def maxSumTwoNoOverlap(self, nums: List[int], firstLen: int, secondLen: int) -> int:
        n = len(nums)
        s = list(accumulate(nums, initial=0))
        ans = t = 0
        i = firstLen
        while i + secondLen - 1 < n:
            t = max(t, s[i] - s[i - firstLen])
            ans = max(ans, t + s[i + secondLen] - s[i])
            i += 1
        t = 0
        i = secondLen
        while i + firstLen - 1 < n:
            t = max(t, s[i] - s[i - secondLen])
            ans = max(ans, t + s[i + firstLen] - s[i])
            i += 1
        return ans
```

```python
class Solution:
    def maxSumTwoNoOverlap(self, nums: List[int], firstLen: int, secondLen: int) -> int:
        def f(a: int, b: int) -> int:
            ans = t = 0
            i = a
            while i + b - 1 < n:
                t = max(t, s[i] - s[i - a])
                ans = max(ans, t + s[i + b] - s[i])
                i += 1
            return ans

        n = len(nums)
        s = list(accumulate(nums, initial=0))
        return max(f(firstLen, secondLen), f(secondLen, firstLen))
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    public int maxSumTwoNoOverlap(int[] nums, int firstLen, int secondLen) {
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0;
        for (int i = firstLen, t = 0; i + secondLen - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - firstLen]);
            ans = Math.max(ans, t + s[i + secondLen] - s[i]);
        }
        for (int i = secondLen, t = 0; i + firstLen - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - secondLen]);
            ans = Math.max(ans, t + s[i + firstLen] - s[i]);
        }
        return ans;
    }
}
```

```java
class Solution {
    private int[] s;
    private int n;

    public int maxSumTwoNoOverlap(int[] nums, int firstLen, int secondLen) {
        n = nums.length;
        s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        return Math.max(f(firstLen, secondLen), f(secondLen, firstLen));
    }

    private int f(int a, int b) {
        int ans = 0;
        for (int i = a, t = 0; i + b - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - a]);
            ans = Math.max(ans, t + s[i + b] - s[i]);
        }
        return ans;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0;
        for (int i = firstLen, t = 0; i + secondLen - 1 < n; ++i) {
            t = max(t, s[i] - s[i - firstLen]);
            ans = max(ans, t + s[i + secondLen] - s[i]);
        }
        for (int i = secondLen, t = 0; i + firstLen - 1 < n; ++i) {
            t = max(t, s[i] - s[i - secondLen]);
            ans = max(ans, t + s[i + firstLen] - s[i]);
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        auto f = [&](int a, int b) -> int {
            int ans = 0;
            for (int i = a, t = 0; i + b - 1 < n; ++i) {
                t = max(t, s[i] - s[i - a]);
                ans = max(ans, t + s[i + b] - s[i]);
            }
            return ans;
        };
        return max(f(firstLen, secondLen), f(secondLen, firstLen));
    }
};
```

### **Go**

```go
func maxSumTwoNoOverlap(nums []int, firstLen int, secondLen int) (ans int) {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for i, t := firstLen, 0; i+secondLen-1 < n; i++ {
		t = max(t, s[i]-s[i-firstLen])
		ans = max(ans, t+s[i+secondLen]-s[i])
	}
	for i, t := secondLen, 0; i+firstLen-1 < n; i++ {
		t = max(t, s[i]-s[i-secondLen])
		ans = max(ans, t+s[i+firstLen]-s[i])
	}
	return
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

```go
func maxSumTwoNoOverlap(nums []int, firstLen int, secondLen int) (ans int) {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	f := func(a, b int) (ans int) {
		for i, t := a, 0; i+b-1 < n; i++ {
			t = max(t, s[i]-s[i-a])
			ans = max(ans, t+s[i+b]-s[i])
		}
		return
	}
	return max(f(firstLen, secondLen), f(secondLen, firstLen))
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

### **TypeScript**

```ts
function maxSumTwoNoOverlap(
    nums: number[],
    firstLen: number,
    secondLen: number,
): number {
    const n = nums.length;
    const s: number[] = new Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let ans = 0;
    for (let i = firstLen, t = 0; i + secondLen - 1 < n; ++i) {
        t = Math.max(t, s[i] - s[i - firstLen]);
        ans = Math.max(ans, t + s[i + secondLen] - s[i]);
    }
    for (let i = secondLen, t = 0; i + firstLen - 1 < n; ++i) {
        t = Math.max(t, s[i] - s[i - secondLen]);
        ans = Math.max(ans, t + s[i + firstLen] - s[i]);
    }
    return ans;
}
```

```ts
function maxSumTwoNoOverlap(
    nums: number[],
    firstLen: number,
    secondLen: number,
): number {
    const n = nums.length;
    const s: number[] = new Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    const f = (a: number, b: number): number => {
        let ans = 0;
        for (let i = a, t = 0; i + b - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - a]);
            ans = Math.max(ans, t + s[i + b] - s[i]);
        }
        return ans;
    };
    return Math.max(f(firstLen, secondLen), f(secondLen, firstLen));
}
```

### **...**

```

```

<!-- tabs:end -->
