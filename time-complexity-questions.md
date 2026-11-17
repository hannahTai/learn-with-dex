# 時間複雜度：考試與面試常見題

> 先自己想答案，再點「看答案」展開。

## 考試和面試考的不一樣

| | 考試 | 面試 |
| --- | --- | --- |
| 題型 | 給一段程式，問複雜度（選擇、填空） | 給一個問題，要你寫出解法並說明複雜度 |
| 重點 | 算得對不對 | 能不能從慢的解法優化到快的解法，並說出理由 |
| 常見陷阱 | 看起來像 O(n²) 其實是 O(n) | 只給出暴力解，沒說複雜度，也沒試著優化 |

兩種題目的核心都一樣：**找出最裡面那行，數它總共執行幾次**。

---

## 第一部分：考試常見題

### 題 1：單層迴圈

```python
for i in range(n):
    print(i)
```

<details><summary>看答案</summary>

**O(n)**。print 執行 n 次。

</details>

### 題 2：兩層迴圈

```python
for i in range(n):
    for j in range(n):
        print(i, j)
```

<details><summary>看答案</summary>

**O(n²)**。外層 n 次，每次內層也 n 次，共 n × n 次。

</details>

### 題 3：內層越跑越短

```python
for i in range(n):
    for j in range(i):
        print(i, j)
```

<details><summary>看答案</summary>

**O(n²)**。內層依序跑 0、1、2、…、n−1 次，總共 0 + 1 + … + (n−1) = n(n−1)/2 ≈ n²/2。常數 ½ 不計，還是 O(n²)。

常見錯誤：以為「內層變短了」就比較快。只是快了一半，成長速度沒變。

</details>

### 題 4：兩個迴圈前後排

```python
for i in range(n):
    print(i)
for j in range(n):
    print(j)
```

<details><summary>看答案</summary>

**O(n)**。兩個迴圈是**前後排**，不是**套在裡面**：n + n = 2n，常數不計，還是 O(n)。

常見錯誤：看到兩個迴圈就答 O(n²)。只有「一個套在另一個裡面」才是相乘。

</details>

### 題 5：每次除以 2

```python
i = n
while i > 1:
    i = i // 2
```

<details><summary>看答案</summary>

**O(log n)**。i 從 n 開始，每次砍半，砍到 1 為止，共約 log₂ n 次。跟二分搜尋同一個道理。

同理，`i = 1` 開始、每次 `i *= 2` 直到超過 n，也是 O(log n)。

</details>

### 題 6：外層 n 次，內層每次砍半

```python
for i in range(n):
    j = n
    while j > 1:
        j = j // 2
```

<details><summary>看答案</summary>

**O(n log n)**。外層 n 次，每次內層 log n 次，相乘。

</details>

### 題 7：內層是固定次數

```python
for i in range(n):
    for j in range(10):
        print(i, j)
```

<details><summary>看答案</summary>

**O(n)**。內層永遠只跑 10 次，跟 n 無關，總共 10n 次，常數不計。

常見錯誤：看到兩層迴圈就答 O(n²)。要看內層的次數**會不會隨 n 變大**。

</details>

### 題 8：外層倍增，內層跟著外層（進階陷阱）

```python
i = 1
while i < n:
    for j in range(i):
        print(j)
    i *= 2
```

<details><summary>看答案</summary>

**O(n)**。外層 i 依序是 1、2、4、8、…，內層跑 i 次，所以總共是 1 + 2 + 4 + … + (小於 n 的最大 2 的次方) < 2n。

常見錯誤：外層 log n 次 × 內層最多 n 次 = O(n log n)。這只是「上限」，不夠精確；實際加總後是 O(n)。

</details>

### 題 9：兩份不同大小的資料

```python
for a in list_a:        # 長度 n
    for b in list_b:    # 長度 m
        print(a, b)
```

<details><summary>看答案</summary>

**O(n × m)**。兩份資料大小不同，不能寫成 O(n²)。只有在題目說明 n 和 m 一樣大時，才能寫成 O(n²)。

</details>

### 題 10：迴圈裡藏著 `in`

```python
result = []
for x in data:              # data 有 n 筆
    if x not in result:
        result.append(x)
```

<details><summary>看答案</summary>

**O(n²)**。`x not in result` 要把 list 從頭找到尾，最多 O(n)，又放在 n 次的迴圈裡。

把 `result` 改成 set，`in` 查詢只要 O(1)，整段變成 O(n)。這是最常考的「一行看起來很短，其實很貴」。

</details>

### 題 11：先排序再搜尋

> 先把 n 筆資料排序，再做一次二分搜尋，總共的複雜度？

<details><summary>看答案</summary>

**O(n log n)**。排序 O(n log n) + 二分搜尋 O(log n)，只留最大項。

常見錯誤：只記得「二分搜尋是 O(log n)」，忘了排序本身的成本。

</details>

### 題 12：化簡

> 把下列化簡成最簡的 Big-O：(a) O(3n + 100) (b) O(n² + n log n) (c) O(n³ + 2ⁿ) (d) O(500)

<details><summary>看答案</summary>

| 題 | 答案 | 規則 |
| --- | --- | --- |
| (a) | O(n) | 去掉常數係數和常數項 |
| (b) | O(n²) | 只留成長最快的那一項 |
| (c) | O(2ⁿ) | 指數成長比任何多項式都快 |
| (d) | O(1) | 跟 n 無關的常數 |

</details>

### 題 13：由快到慢排列

> 將下列複雜度由快到慢排序：O(n²)、O(1)、O(n log n)、O(2ⁿ)、O(log n)、O(n)、O(n!)

<details><summary>看答案</summary>

**O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)**

</details>

### 題 14：遞迴費氏數列

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

<details><summary>看答案</summary>

**O(2ⁿ)**。每次呼叫會再分出兩個呼叫，像一棵每層翻倍的樹，深度約 n 層。而且同一個值會被重複算非常多次，例如 fib(2) 會被算上百次。

加上記憶（把算過的結果存起來），每個 fib(k) 只算一次，就降到 **O(n)**：

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

</details>

### 題 15：遞迴式

> (a) T(n) = T(n/2) + O(1) (b) T(n) = 2T(n/2) + O(n)，各是什麼複雜度？分別對應哪個演算法？

<details><summary>看答案</summary>

| 題 | 答案 | 對應演算法 | 白話 |
| --- | --- | --- | --- |
| (a) | O(log n) | 二分搜尋 | 每次砍一半，只往一邊走，每層做常數的事 |
| (b) | O(n log n) | 合併排序 | 每次切兩半、兩邊都要處理，共 log n 層，每層合計做 n 的事 |

</details>

### 常見演算法速查（背了就有分）

| 演算法 / 操作 | 平均 | 最壞 |
| --- | --- | --- |
| 二分搜尋 | O(log n) | O(log n) |
| 線性搜尋 | O(n) | O(n) |
| 合併排序 | O(n log n) | O(n log n) |
| 快速排序 | O(n log n) | **O(n²)** |
| Python `sorted()` | O(n log n) | O(n log n) |
| 泡沫排序、插入排序、選擇排序 | O(n²) | O(n²) |
| dict / set 查詢、新增 | O(1) | **O(n)** |
| BFS / DFS 走完一張圖 | O(V + E) | O(V + E) |

（V 是圖的點數，E 是邊數。）

---

## 第二部分：面試常問的概念題

面試官要聽的不是背出來的定義，而是你**能不能用自己的話講清楚，並舉出例子**。

### Q1：什麼是時間複雜度？為什麼可以忽略常數？

<details><summary>參考回答</summary>

時間複雜度描述「資料量 n 變大時，執行時間怎麼成長」，不是在算實際幾秒。

忽略常數，是因為 n 夠大時，成長速度的差異會壓過常數的差異。例如 100n 和 n²：n = 1,000 時是 10 萬比 100 萬，n² 已經比較慢；n 越大差距越懸殊。

補充（加分）：常數在實務上仍然重要。例如 pandas 的 `iterrows()` 迴圈和向量化運算都是 O(n)，實際速度卻能差上百倍。

</details>

### Q2：Big-O 指的是最壞情況嗎？

<details><summary>參考回答</summary>

Big-O 嚴格說是「上限」，表示成長不會比這個快。日常討論時，通常用來描述最壞情況。

但要分清楚平均和最壞：快速排序平均 O(n log n)、最壞 O(n²)；dict 查詢平均 O(1)、最壞 O(n)。面試時主動說明你講的是哪一種，會加分。

</details>

### Q3：什麼是空間複雜度？請舉例。

<details><summary>參考回答</summary>

空間複雜度是「資料量變大時，額外需要的記憶體怎麼成長」。

- 只用幾個變數加總一個 list：O(1) 額外空間。
- 另外建一個 set 來記錄出現過的元素：O(n) 額外空間。
- 遞迴深度 n 層：呼叫堆疊佔 O(n) 空間。

</details>

### Q4：為什麼 set / dict 查詢是 O(1)，list 是 O(n)？

<details><summary>參考回答</summary>

list 不知道東西放在哪，只能從頭一個個比對，所以是 O(n)。

set / dict 用的是雜湊表（hash table）：先把鍵丟進雜湊函式，算出它該放在哪一格，查詢時直接跳到那一格，所以平均是 O(1)。

最壞情況是很多鍵剛好算到同一格（雜湊碰撞），這時就得在那一格裡逐一比對，退化成 O(n)，但實務上很少發生。

</details>

### Q5：list 的 append 為什麼是 O(1)？它有時候不是要搬整個陣列嗎？

<details><summary>參考回答</summary>

對，list 底層是連續的陣列，空間滿了就得配置一塊更大的空間、把全部元素搬過去，那一次是 O(n)。

但 Python 每次擴充時會多預留空間，按比例放大，所以搬家很少發生。平均攤到每次 append 只有 O(1)，這叫做**均攤（amortized）O(1)**。

</details>

### Q6：快速排序什麼時候會退化成 O(n²)？

<details><summary>參考回答</summary>

快速排序每次選一個基準值（pivot），把資料分成「比它小」和「比它大」兩堆。如果每次選到的 pivot 都是最大或最小值（例如資料已經排好、又固定選第一個當 pivot），每次只能切掉一個元素，就變成 n + (n−1) + … = O(n²)。

解法：隨機選 pivot，或取三個數的中位數當 pivot。

</details>

### Q7：什麼是「用空間換時間」？請舉例。

<details><summary>參考回答</summary>

多花記憶體存東西，換取更快的執行速度。

- 去除重複：多開一個 set 記錄看過的值（O(n) 空間），讓整段從 O(n²) 變 O(n)。
- 費氏數列：把算過的結果記下來（O(n) 空間），從 O(2ⁿ) 降到 O(n)。

反過來，記憶體很緊時，也可能選擇較慢但省空間的做法。

</details>

### Q8：資料庫查詢很慢，從複雜度的角度你會怎麼想？

<details><summary>參考回答</summary>

先看查詢條件的欄位有沒有索引（index）。沒有索引時，資料庫得把整張表掃一遍，是 O(n)；有 B-tree 索引時，查詢大約是 O(log n)，資料越大差距越明顯。

代價是索引要佔空間，而且每次寫入資料都要同步更新索引，所以寫入會變慢。這也是「空間換時間」的例子。

</details>

---

## 第三部分：面試常見的實作優化題

面試很少只問「這段是什麼複雜度」，更常是給一個問題，看你怎麼從慢解法改進。

### 回答框架

1. **先說暴力解**，並說出它的時間複雜度。
2. **找瓶頸**：哪一行被執行最多次？
3. **換工具**：用更合適的資料結構（set、dict、deque、heap）或先排序。
4. **說出新的時間和空間複雜度**，以及用什麼換到的。

### 題 A：去除重複，保留原本順序

<details><summary>看解法</summary>

```python
# 暴力解：O(n²) 時間
def dedupe_slow(data):
    result = []
    for x in data:
        if x not in result:      # list 查詢 O(n)
            result.append(x)
    return result

# 優化：O(n) 時間、O(n) 空間
def dedupe_fast(data):
    seen = set()
    result = []
    for x in data:
        if x not in seen:        # set 查詢 O(1)
            seen.add(x)
            result.append(x)
    return result
```

瓶頸是 list 的 `in`，換成 set 就好。Python 3.7 以後也可以直接寫 `list(dict.fromkeys(data))`，同樣是 O(n)。

</details>

### 題 B：Two Sum（找出兩數相加等於目標值）

> 給一個數字 list 和一個目標值 target，找出兩個數字相加等於 target 的位置。

<details><summary>看解法</summary>

```python
# 暴力解：每兩個都試，O(n²)
def two_sum_slow(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return i, j

# 優化：O(n) 時間、O(n) 空間
def two_sum_fast(nums, target):
    seen = {}                        # 數字 → 位置
    for i, x in enumerate(nums):
        need = target - x
        if need in seen:             # dict 查詢 O(1)
            return seen[need], i
        seen[x] = i
```

思路：與其拿每個數字去跟所有數字配對，不如邊走邊記，每到一個數字就問「我需要的那一半，之前出現過嗎？」。這是面試出現頻率最高的題目之一。

</details>

### 題 C：判斷 list 有沒有重複的值

<details><summary>看解法</summary>

| 做法 | 時間 | 額外空間 |
| --- | --- | --- |
| 每兩個都比 | O(n²) | O(1) |
| 先排序，再看相鄰兩個是否相等 | O(n log n) | 視排序而定 |
| 用 set：`len(set(data)) != len(data)` | O(n) | O(n) |

這題適合用來展示「三種解法各有取捨」：記憶體很少時選排序，追求速度就選 set。

</details>

### 題 D：合併兩個已經排好序的 list

<details><summary>看解法</summary>

```python
# 直覺解：O((n+m) log(n+m))
merged = sorted(a + b)

# 優化：雙指標，O(n + m)
def merge(a, b):
    i = j = 0
    out = []
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            out.append(a[i]); i += 1
        else:
            out.append(b[j]); j += 1
    out.extend(a[i:])
    out.extend(b[j:])
    return out
```

關鍵是善用「已經排好序」這個條件：兩邊各放一根手指，每次取比較小的那個，每個元素只看一次。這也是合併排序的核心步驟。

</details>

### 題 E：從 n 筆資料找出最大的 k 筆

<details><summary>看解法</summary>

| 做法 | 時間 |
| --- | --- |
| 全部排序再取前 k 筆：`sorted(data, reverse=True)[:k]` | O(n log n) |
| 用 heap：`heapq.nlargest(k, data)` | O(n log k) |

k 遠小於 n 時（例如 100 萬筆裡找前 10 名），heap 明顯比較快。heap 只維持 k 個候選人，每來一筆新資料，最多花 log k 決定要不要換掉最小的那個。

</details>

### 題 F：用 list 當佇列很慢

<details><summary>看解法</summary>

```python
# 慢：pop(0) 每次 O(n)，取完 n 筆是 O(n²)
queue = list(tasks)
while queue:
    task = queue.pop(0)

# 快：popleft() 每次 O(1)，取完 n 筆是 O(n)
from collections import deque
queue = deque(tasks)
while queue:
    task = queue.popleft()
```

</details>

---

## 第四部分：LeetCode 對應題目

LeetCode 的題目本身不問「這是什麼複雜度」，但每題都有時間限制：用 O(n²) 暴力解，資料一大就會超時（Time Limit Exceeded）。所以練 LeetCode 就是在練「把複雜度降下來」。

下面按**降複雜度的手法**分類，每類由易到難排，Easy 先做。

### 1. 用 set / dict 把查詢從 O(n) 變 O(1)

| 題號 | 題目 | 難度 | 暴力解 → 目標 |
| --- | --- | --- | --- |
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | O(n²) → O(n) |
| 217 | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | Easy | O(n²) → O(n) |
| 242 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | Easy | 排序 O(n log n) → 計數 O(n) |
| 49 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | 用 dict 分組 |
| 128 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | Medium | 排序 O(n log n) → set O(n) |

### 2. 二分搜尋：O(n) → O(log n)

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 704 | [Binary Search](https://leetcode.com/problems/binary-search/) | Easy | 標準二分搜尋 |
| 35 | [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | Easy | 找不到時要回傳該插入的位置 |
| 278 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) | Easy | 不是在陣列裡找，而是在「答案範圍」裡砍半 |
| 33 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | Medium | 陣列被轉過一次，仍要做到 O(log n) |

### 3. 雙指標：善用「已排序」，O(n²) → O(n)

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 88 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) | Easy | 進階要求原地合併 |
| 125 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | Easy | 頭尾各一根指標往中間走 |
| 167 | [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | Medium | 已排序版 Two Sum，O(1) 額外空間 |
| 11 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Medium | O(n²) → O(n) 的經典 |
| 15 | [3Sum](https://leetcode.com/problems/3sum/) | Medium | 先排序 + 雙指標，O(n³) → O(n²) |

### 4. 一次走完：滑動視窗與邊走邊記

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 121 | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | Easy | 邊走邊記最低價，O(n²) → O(n) |
| 53 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | Medium | 所有子陣列 O(n²) → 一次走完 O(n) |
| 3 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium | 滑動視窗 + set |

### 5. 記憶化：O(2ⁿ) → O(n)

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 509 | [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | Easy | 記憶化的入門題 |
| 70 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Easy | 換了包裝的費氏數列 |

### 6. Heap：找前 k 名，O(n log n) → O(n log k)

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 215 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Medium | heapq 的基本用法 |
| 347 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium | 先用 dict 計數，再用 heap 取前 k |

### 7. 圖：BFS / DFS，O(V + E)

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 200 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | Medium | 每格只走一次 |
| 207 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | Medium | 判斷有沒有環（三色標記） |

### 8. SQL：視窗函式

LeetCode 也有 SQL 題庫，資料分析職缺常考。

| 題號 | 題目 | 難度 | 重點 |
| --- | --- | --- | --- |
| 184 | [Department Highest Salary](https://leetcode.com/problems/department-highest-salary/) | Medium | 每個部門最高薪：視窗函式放在子查詢裡，外層再用 WHERE 篩 |
| 185 | [Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/) | Hard | 進階版，要用 DENSE_RANK |

### 建議的練習順序

1. 先做每一類的 Easy 題：1 → 217 → 704 → 88 → 121 → 70。
2. 每題做完都問自己：**我的解法是什麼複雜度？暴力解又是什麼？**
3. 寫出暴力解也沒關係，先求能過，再用上面的「回答框架」想怎麼優化。

---

## 最後提醒

- 答複雜度前，先問自己：**最裡面那行會跑幾次？**
- 兩層迴圈不一定是 O(n²)：要看內層會不會跟著 n 變大，以及實際加總是多少。
- 一行看起來很短的程式，也可能很貴，例如 list 的 `in` 和 `pop(0)`。
- 面試時，暴力解不丟臉；但要說出它的複雜度，再主動提出怎麼優化。
