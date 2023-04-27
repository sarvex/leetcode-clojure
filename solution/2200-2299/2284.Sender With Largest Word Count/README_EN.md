# [2284. Sender With Largest Word Count](https://leetcode.com/problems/sender-with-largest-word-count)

[中文文档](/solution/2200-2299/2284.Sender%20With%20Largest%20Word%20Count/README.md)

## Description

<p>You have a chat log of <code>n</code> messages. You are given two string arrays <code>messages</code> and <code>senders</code> where <code>messages[i]</code> is a <strong>message</strong> sent by <code>senders[i]</code>.</p>

<p>A <strong>message</strong> is list of <strong>words</strong> that are separated by a single space with no leading or trailing spaces. The <strong>word count</strong> of a sender is the total number of <strong>words</strong> sent by the sender. Note that a sender may send more than one message.</p>

<p>Return <em>the sender with the <strong>largest</strong> word count</em>. If there is more than one sender with the largest word count, return <em>the one with the <strong>lexicographically largest</strong> name</em>.</p>

<p><strong>Note:</strong></p>

<ul>
	<li>Uppercase letters come before lowercase letters in lexicographical order.</li>
	<li><code>&quot;Alice&quot;</code> and <code>&quot;alice&quot;</code> are distinct.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> messages = [&quot;Hello userTwooo&quot;,&quot;Hi userThree&quot;,&quot;Wonderful day Alice&quot;,&quot;Nice day userThree&quot;], senders = [&quot;Alice&quot;,&quot;userTwo&quot;,&quot;userThree&quot;,&quot;Alice&quot;]
<strong>Output:</strong> &quot;Alice&quot;
<strong>Explanation:</strong> Alice sends a total of 2 + 3 = 5 words.
userTwo sends a total of 2 words.
userThree sends a total of 3 words.
Since Alice has the largest word count, we return &quot;Alice&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> messages = [&quot;How is leetcode for everyone&quot;,&quot;Leetcode is useful for practice&quot;], senders = [&quot;Bob&quot;,&quot;Charlie&quot;]
<strong>Output:</strong> &quot;Charlie&quot;
<strong>Explanation:</strong> Bob sends a total of 5 words.
Charlie sends a total of 5 words.
Since there is a tie for the largest word count, we return the sender with the lexicographically larger name, Charlie.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == messages.length == senders.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= messages[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= senders[i].length &lt;= 10</code></li>
	<li><code>messages[i]</code> consists of uppercase and lowercase English letters and <code>&#39; &#39;</code>.</li>
	<li>All the words in <code>messages[i]</code> are separated by <strong>a single space</strong>.</li>
	<li><code>messages[i]</code> does not have leading or trailing spaces.</li>
	<li><code>senders[i]</code> consists of uppercase and lowercase English letters only.</li>
</ul>

## Solutions

<!-- tabs:start -->

### **Python3**

```python
class Solution:
    def largestWordCount(self, messages: List[str], senders: List[str]) -> str:
        cnt = Counter()
        for msg, sender in zip(messages, senders):
            cnt[sender] += msg.count(' ') + 1
        ans = ''
        for sender, v in cnt.items():
            if cnt[ans] < v or (cnt[ans] == v and ans < sender):
                ans = sender
        return ans
```

### **Java**

```java
class Solution {
    public String largestWordCount(String[] messages, String[] senders) {
        Map<String, Integer> cnt = new HashMap<>();
        int n = senders.length;
        for (int i = 0; i < n; ++i) {
            int v = 1;
            for (int j = 0; j < messages[i].length(); ++j) {
                if (messages[i].charAt(j) == ' ') {
                    ++v;
                }
            }
            cnt.merge(senders[i], v, Integer::sum);
        }
        String ans = senders[0];
        for (var e : cnt.entrySet()) {
            String sender = e.getKey();
            if (cnt.get(ans) < cnt.get(sender)
                || (cnt.get(ans) == cnt.get(sender) && ans.compareTo(sender) < 0)) {
                ans = sender;
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
    string largestWordCount(vector<string>& messages, vector<string>& senders) {
        unordered_map<string, int> cnt;
        int n = senders.size();
        for (int i = 0; i < n; ++i) {
            int v = count(messages[i].begin(), messages[i].end(), ' ') + 1;
            cnt[senders[i]] += v;
        }
        string ans = senders[0];
        for (auto& [sender, v] : cnt) {
            if (cnt[ans] < v || (cnt[ans] == v && ans < sender)) {
                ans = sender;
            }
        }
        return ans;
    }
};
```

### **Go**

```go
func largestWordCount(messages []string, senders []string) (ans string) {
	cnt := map[string]int{}
	for i, msg := range messages {
		v := strings.Count(msg, " ") + 1
		cnt[senders[i]] += v
	}
	for sender, v := range cnt {
		if cnt[ans] < v || (cnt[ans] == v && ans < sender) {
			ans = sender
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
