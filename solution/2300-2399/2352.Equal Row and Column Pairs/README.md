# [2352. 相等行列对](https://leetcode.cn/problems/equal-row-and-column-pairs)

[English Version](/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给你一个下标从 <strong>0</strong> 开始、大小为 <code>n x n</code> 的整数矩阵 <code>grid</code> ，返回满足 <code>R<sub>i</sub></code><em> </em>行和<em> </em><code>C<sub>j</sub></code><em> </em>列相等的行列对<em> </em><code>(R<sub>i</sub>, C<sub>j</sub>)</code><em> </em>的数目<em>。</em></p>

<p>如果行和列以相同的顺序包含相同的元素（即相等的数组），则认为二者是相等的。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/images/ex1.jpg" style="width: 150px; height: 153px;" /></p>

<pre>
<strong>输入：</strong>grid = [[3,2,1],[1,7,6],[2,7,7]]
<strong>输出：</strong>1
<strong>解释：</strong>存在一对相等行列对：
- (第 2 行，第 1 列)：[2,7,7]
</pre>

<p><strong>示例 2：</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/images/ex2.jpg" style="width: 200px; height: 209px;" /></p>

<pre>
<strong>输入：</strong>grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
<strong>输出：</strong>3
<strong>解释：</strong>存在三对相等行列对：
- (第 0 行，第 0 列)：[3,1,2,2]
- (第 2 行, 第 2 列)：[2,4,2,2]
- (第 3 行, 第 2 列)：[2,4,2,2]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

**方法一：模拟**

将矩阵 `grid` 的每一行以及每一列进行比较，如果相等，那么就是一对相等行列对，答案加一。

时间复杂度 $O(n^3)$，空间复杂度 $O(1)$。其中 $n$ 为矩阵 `grid` 的行数或列数。

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        g = [list(col) for col in zip(*grid)]
        return sum(row == col for row in grid for col in g)
```

```python
class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        n = len(grid)
        ans = 0
        for i in range(n):
            for j in range(n):
                ans += all(grid[i][k] == grid[k][j] for k in range(n))
        return ans
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    public int equalPairs(int[][] grid) {
        int n = grid.length;
        int[][] g = new int[n][n];
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < n; ++i) {
                g[i][j] = grid[j][i];
            }
        }
        int ans = 0;
        for (var row : grid) {
            for (var col : g) {
                int ok = 1;
                for (int i = 0; i < n; ++i) {
                    if (row[i] != col[i]) {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
            }
        }
        return ans;
    }
}
```

```java
class Solution {
    public int equalPairs(int[][] grid) {
        int n = grid.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int ok = 1;
                for (int k = 0; k < n; ++k) {
                    if (grid[i][k] != grid[k][j]) {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
            }
        }
        return ans;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> g(n, vector<int>(n));
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < n; ++i) {
                g[i][j] = grid[j][i];
            }
        }
        int ans = 0;
        for (auto& row : grid) {
            for (auto& col : g) {
                ans += row == col;
            }
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        int n = grid.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int ok = 1;
                for (int k = 0; k < n; ++k) {
                    if (grid[i][k] != grid[k][j]) {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
            }
        }
        return ans;
    }
};
```

### **Go**

```go
func equalPairs(grid [][]int) (ans int) {
	n := len(grid)
	g := make([][]int, n)
	for i := range g {
		g[i] = make([]int, n)
		for j := 0; j < n; j++ {
			g[i][j] = grid[j][i]
		}
	}
	for _, row := range grid {
		for _, col := range g {
			ok := 1
			for i, v := range row {
				if v != col[i] {
					ok = 0
					break
				}
			}
			ans += ok
		}
	}
	return
}
```

```go
func equalPairs(grid [][]int) (ans int) {
	for i := range grid {
		for j := range grid {
			ok := 1
			for k := range grid {
				if grid[i][k] != grid[k][j] {
					ok = 0
					break
				}
			}
			ans += ok
		}
	}
	return
}
```

### **TypeScript**

```ts

```

### **...**

```

```

<!-- tabs:end -->
