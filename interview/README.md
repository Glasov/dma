<details>
<summary>Вертикальная ось симметрии</summary>
Дано n точек на двумерной плоскости, найдите, существует ли такая прямая, параллельная оси Y, которая симметрично отражает данные точки.

Другими словами, ответьте, существует ли прямая, после отражения всех точек относительно данной прямой исходный набор точек совпадает с отраженными. Обратите внимание, что точки могут повторяться.

Решение должно работать быстрее чем O(n²)
<details>
<summary>Решение</summary>
<pre><code>class Solution:
    def isReflected(self, points: List[List[int]]) -> bool:
        min_x, max_x = inf, -inf
        point_set = set()
        for x, y in points:
            min_x = min(min_x, x)
            max_x = max(max_x, x)
            point_set.add((x, y))
        s = min_x + max_x
        return all((s - x, y) in point_set for x, y in points)</code></pre>
</details>
</details>

<details>
<summary>Самый длинный подмассив из единиц после удаления одного элемента</summary>
<a href="https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/solutions/3719568/beat-s-100-c-java-python-beginner-friendly/">Решение на leetcode</a>
</details>

<details>
<summary>Сжатие диапазонов</summary>
<a href="https://leetcode.com/problems/summary-ranges/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/summary-ranges/solutions/6116130/0-ms-runtime-beats-100-user-code-idea-algorithm-solving-step/">Решение на leetcode</a>
</details>

<details>
<summary>Сжатие строки</summary>
<a href="https://leetcode.com/problems/string-compression/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/string-compression/solutions/6066222/easy-way-beat-100-in-string-compression/">Решение на leetcode</a>
</details>

<details>
<summary>Зигзаговый итератор</summary>
Даны два вектора, реализуйте итератор, который будет брать элементы из них поочерёдно.

Пример:

Input:
v1 = [1,2]
v2 = [3,4,5,6]

Output: [1,3,2,4,5,6]
<details>
<summary>Решение</summary>
<pre><code>class ZigzagIterator:
    def __init__(self, v1: List[int], v2: List[int]):
        # Initialize the current pointer to 0 to start with the first vector
        self.current = 0
        # The size variable indicates the number of vectors (always 2 in this case)
        self.size = 2
        # Create a list to keep track of the current index in each vector
        self.indices = [0] * self.size
        # Store the two input vectors
        self.vectors = [v1, v2]
    def next(self) -> int:
        # Identify the current vector and its index
        vector = self.vectors[self.current]
        index = self.indices[self.current]
        # Retrieve the next element from the current vector
        result = vector[index]
        # Increment the index for the current vector
        self.indices[self.current] = index + 1
        # Move to the next vector for the following call
        self.current = (self.current + 1) % self.size
        # Return the retrieved element
        return result
    def hasNext(self) -> bool:
        # Store the starting point to avoid infinite loops
        start = self.current
        # Check if the current vector is exhausted and move to the next one if necessary
        while self.indices[self.current] == len(self.vectors[self.current]):
            # Move to the next vector
            self.current = (self.current + 1) % self.size
            # If we loop back to the start, that means all vectors are exhausted
            if self.current == start:
                return False
        # If we haven't returned yet, there's still at least one element left
        return True</code></pre>
</details>
</details>

<details>
<summary>Проверка на палиндром</summary>
<a href="https://leetcode.com/problems/valid-palindrome/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/valid-palindrome/solutions/6170976/video-transforming-the-input-string/">Решение на leetcode</a></a>
</details>

<details>
<summary>Проверка на расстояние в одно редактирование</summary>
Даны две строки s и t. Определите, находятся ли они на расстоянии в одну операцию редактирования друг от друга.

Примечание:
Существует 3 возможных варианта, при которых строки могут отличаться на одну операцию:

Вставить один символ в s, чтобы получить t

Удалить один символ из s, чтобы получить t

Заменить один символ в s, чтобы получить t

Примеры:

Пример 1:
```
Вход: s = "ab", t = "acb"
Выход: true
Объяснение: можно вставить 'c' в строку s, чтобы получить t.
```
Пример 2:
```
Вход: s = "cab", t = "ad"
Выход: false
Объяснение: нельзя получить t из s за одну операцию.
```

Пример 3:
```
Вход: s = "1203", t = "1213"
Выход: true
Объяснение: можно заменить '0' на '1', чтобы получить t.
```
<details>
<summary>Решение</summary>
<pre><code>def isOneEditDistance(s, t):
    len_s = len(s)
    len_t = len(t)
    # Обеспечим, чтобы s всегда была короче или равна t
    if len_s > len_t:
        return isOneEditDistance(t, s)
    # Если длина отличается больше чем на 1 — сразу нет
    if len_t - len_s > 1:
        return False
    found_difference = False
    i = 0  # указатель на s
    j = 0  # указатель на t
    while i < len_s and j < len_t:
        if s[i] != t[j]:
            if found_difference:
                return False  # уже была одна ошибка — вторая недопустима
            found_difference = True
            if len_s == len_t:
                # случай: замена
                i += 1
                j += 1
            else:
                # случай: вставка в s (т.е. пропустить символ в t)
                j += 1
        else:
            # символы равны — идем дальше
            i += 1
            j += 1
    # Если строки были одинаковы до конца, но длина отличается на 1 — это допустимо
    if not found_difference and len_s + 1 == len_t:
        return True
    return found_difference
</code></pre>
</details>
</details>

<details>
<summary>Сумма подмассива равная K</summary>
<a href="https://leetcode.com/problems/subarray-sum-equals-k/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/subarray-sum-equals-k/solutions/6156695/adding-number-of-current-total-k/">Решение на leetcode</a>
</details>

<details>
<summary>Передвинуть нули</summary>
<a href="https://leetcode.com/problems/move-zeroes/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/move-zeroes/solutions/5246963/video-two-pointer-solution/">Решение на leetcode</a>
</details>

<details>
<summary>Группировка анаграмм</summary>
<a href="https://leetcode.com/problems/group-anagrams/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/group-anagrams/solutions/6113105/video-create-keys-for-group-of-strings-2-solutions/">Решение на leetcode</a>
</details>

<details>
<summary>Вставка, удаление и случайный элемент за O(1)</summary>
<a href="https://leetcode.com/problems/insert-delete-getrandom-o1/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/insert-delete-getrandom-o1/solutions/4572728/beats-99-84-users-c-java-python-javascript-explained/">Решение на leetcode</a>
</details>

<details>
<summary>LRU кеш</summary>
<a href="https://leetcode.com/problems/lru-cache/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/lru-cache/solutions/6123717/video-doubly-linked-list-with-hash-table-solution/">Решение на leetcode</a>
</details>

<details>
<summary>Генерация правильных скобочных последовательностей</summary>
<a href="https://leetcode.com/problems/generate-parentheses/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/generate-parentheses/solutions/6045420/0-ms-runtime-beats-100-user-step-by-steps-solution-easy-to-understand/">Решение на leetcode</a>
</details>

<details>
<summary>Развернуть связный список</summary>
<a href="https://leetcode.com/problems/reverse-linked-list/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/reverse-linked-list/solutions/6072712/video-solution-with-visualization/">Решение на leetcode</a>
</details>

<details>
<summary>Перестановка в строке</summary>
<a href="https://leetcode.com/problems/permutation-in-string/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/permutation-in-string/solutions/6260852/video-sliding-window-hashmap-solution/">Решение на leetcode</a>
</details>

<details>
<summary>Слияние K отсортированных массивов</summary>
<a href="https://leetcode.com/problems/merge-k-sorted-lists/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/merge-k-sorted-lists/solutions/5425987/video-merge-two-lists-at-a-time/">Решение на leetcode</a>
</details>

<details>
<summary>Количество недавних вызовов</summary>
<a href="https://leetcode.com/problems/number-of-recent-calls/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/number-of-recent-calls/solutions/5390658/beats-100-both-time-and-space-java-python-c-detailed-explanation/">Решение на leetcode</a>
</details>

<details>
<summary>Правильная скобочная последовательность</summary>
<a href="https://leetcode.com/problems/valid-parentheses/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/valid-parentheses/solutions/5139933/video-2-ways-to-solve-this-question/">Решение на leetcode</a>
</details>

<details>
<summary>Максимальное количество подряд идущих единиц</summary>
Дан бинарный массив (т.е. состоящий только из 0 и 1). Нужно найти максимальное количество подряд идущих единиц, если разрешено перевернуть не более одного нуля в единицу.

Пример:
```
Вход: [1, 0, 1, 1, 0]
Выход: 4
Объяснение: Если перевернуть первый 0, получим [1, 1, 1, 1, 0], и максимальное количество подряд идущих единиц — это 4.
```

Ограничения:

Массив содержит только 0 и 1.

Длина массива — положительное число, не больше 10,000
<details>
<summary>Решение</summary>
<pre><code>public int findMaxConsecutiveOnes(int[] nums) {
    int res = 0, cur = 0, cnt = 0;
    for (int i = 0; i < nums.length; i++) {
        cnt++;
        if (nums[i] == 0) {
            cur = cnt;
            cnt = 0;
        } 
        res = Math.max(res, cnt + cur);
    }
    return res;
}
</code></pre>
</details>
</details>

<details>
<summary>Максимизировать расстояние до ближайшего человека</summary>
<a href="https://leetcode.com/problems/maximize-distance-to-closest-person/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/maximize-distance-to-closest-person/solutions/3845099/python-3-two-pointers-visualisation-beats-99-8-112ms/">Решение на leetcode</a>

P.S. перевёл, как смог :skull:
</details>

<details>
<summary>Реализовать счётчик обращений</summary>
Реализуйте класс HitCounter, который считает количество "хитов" (обращений/запросов) за последние 5 минут (300 секунд). Предполагается, что система получает временные метки (timestamp) в возрастающем порядке (в секундах).

Методы:

`hit(timestamp: int)` - Записывает один хит (обращение), произошедший в заданную timestamp (время в секундах).

`getHits(timestamp: int) -> int` - Возвращает количество обращений за последние 300 секунд, включая текущее время.

Пример
```python
counter = HitCounter()
counter.hit(1)
counter.hit(2)
counter.hit(300)
print(counter.getHits(300))  # -> 3
print(counter.getHits(301))  # -> 2 (удаляется хит в момент 1)
```

<a href="https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0362.Design%20Hit%20Counter/README_EN.md#solution-1-binary-search/">Решение</a>
</details>

<details>
<summary>Объединение интервалов</summary>
<a href="https://leetcode.com/problems/merge-intervals/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/merge-intervals/solutions/5513226/video-sorting-solution/">Решение на leetcode</a>
</details>

<details>
<summary>Удерживание дождевой воды</summary>
<a href="https://leetcode.com/problems/trapping-rain-water/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/trapping-rain-water/solutions/5126477/video-keep-max-height-on-the-both-side/">Решение на leetcode</a>
</details>

<details>
<summary>Сумма двух чисел</summary>
<a href="https://leetcode.com/problems/two-sum/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/two-sum/solutions/3619262/3-method-s-c-java-python-beginner-friendly/">Решение на leetcode</a>
</details>

<details>
<summary>Переговорные комнаты</summary>
<a href="https://neetcode.io/problems/meeting-schedule-ii/">Задача на neetcode</a>

Решение во вкладке "Solution"
</details>

<details>
<summary>Найти все анаграммы в строке</summary>
<a href="https://leetcode.com/problems/find-all-anagrams-in-a-string/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/find-all-anagrams-in-a-string/solutions/1737985/python3-sliding-window-hash-table-explained/">Решение на leetcode</a>
</details>

<details>
<summary>Проверка симметричности дерева</summary>
<a href="https://leetcode.com/problems/symmetric-tree/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/symmetric-tree/solutions/6104800/video-going-to-left-side-and-right-side-at-the-same-time/">Решение на leetcode</a>
</details>

<details>
<summary>Самая длинная подстрока без повторяющихся символов</summary>
<a href="https://leetcode.com/problems/longest-substring-without-repeating-characters/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/longest-substring-without-repeating-characters/solutions/5111376/video-3-ways-to-solve-this-question-sliding-window-set-hashing-and-the-last-position/">Решение на leetcode</a>
</details>

<details>
<summary>Валидация бинарного дерева поиска</summary>
<a href="https://leetcode.com/problems/validate-binary-search-tree/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/validate-binary-search-tree/solutions/5622933/video-check-range-of-each-node/">Решение на leetcode</a>
</details>

<details>
<summary>Количество островов</summary> 
<a href="https://leetcode.com/problems/number-of-islands/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/number-of-islands/solutions/5608697/video-check-4-directions-bonus-solutions/">Решение на leetcode</a>

P.S. на самом деле мы решали эту задачу: это по сути DFS и поиск компонент связности
</details>

<details>
<summary>Итератор вложенных списков</summary>
<a href="https://leetcode.com/problems/flatten-nested-list-iterator/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/flatten-nested-list-iterator/solutions/6280041/video-give-me-10-minutes-how-we-think-about-a-solution-python-javascript-java-c/">Решение на leetcode</a>
</details>

<details>
<summary>Последовательные символы</summary>
<a href="https://leetcode.com/problems/consecutive-characters/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/consecutive-characters/solutions/6516259/explanation-complexities-simple-solution/">Решение на leetcode</a>
</details>

<details>
<summary>Пересечение списков интервалов</summary>
<a href="https://leetcode.com/problems/interval-list-intersections/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/interval-list-intersections/solutions/6630026/python-clean-solution-with-approach-and-complexity-analysis/">Решение на leetcode</a>
</details>

<details>
<summary>Скалярное произведение сжатых векторов</summary>
Даны два сжатых вектора - каждый представлен в виде списка кортежей:

```python
[(value1, count1), (value2, count2), ...]
```
Каждый такой кортеж означает:

`count` раз подряд стоит значение `value`.

То есть вактор

`[(1, 3), (2, 2)]`

является сжатой записью вектора

`[1, 1, 1, 2, 2]`

Нужно вычислить скалярное произведение этих двух векторов.

Пример:

```python
v1 = [(1, 3), (2, 2)]      # -> [1, 1, 1, 2, 2]
v2 = [(0, 2), (3, 3)]      # -> [0, 0, 3, 3, 3]

Результат = 1×0 + 1×0 + 1×3 + 2×3 + 2×3 = 0 + 0 + 3 + 6 + 6 = 15
```

<details>
<summary>Решение</summary>
<pre><code>def dotProductCompressed(v1, v2):
    i = j = 0
    res = 0
    ptr1_val, ptr1_cnt = v1[i]
    ptr2_val, ptr2_cnt = v2[j]
    while i < len(v1) and j < len(v2):
        take = min(ptr1_cnt, ptr2_cnt)
        res += ptr1_val * ptr2_val * take
        ptr1_cnt -= take
        ptr2_cnt -= take
        if ptr1_cnt == 0:
            i += 1
            if i < len(v1):
                ptr1_val, ptr1_cnt = v1[i]
        if ptr2_cnt == 0:
            j += 1
            if j < len(v2):
                ptr2_val, ptr2_cnt = v2[j]
    return res
</code></pre>
</details>
</details>

<details>
<summary>Глубина бинарного дерева</summary>
<a href="https://leetcode.com/problems/maximum-depth-of-binary-tree/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/maximum-depth-of-binary-tree/solutions/6093733/video-dfs-and-bfs-solution/">Решение на leetcode</a>
</details>

<details>
<summary>LCA в бинарном дереве</summary>
<a href="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/solutions/6275465/video-for-everyone-who-doesn-t-understand-how-dfs-works/">Решение на leetcode</a>
</details>

<details>
<summary>K ближайших чисел в отсортированном массиве</summary>
<a href="https://leetcode.com/problems/find-k-closest-elements/description/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/find-k-closest-elements/solutions/6641951/most-beautiful-binary-search-ever/">Решение на leetcode</a>

P.S. я бы поспорил
</details>

<details>
<summary>Пересечение двух массивов</summary>
<a href="https://leetcode.com/problems/intersection-of-two-arrays/">Задача на leetcode</a>

<a href="https://leetcode.com/problems/intersection-of-two-arrays/solutions/4850780/99-beats-hashmap-easy-explanation-dry-run-c-c-java-kotlin-python3/">Решение на leetcode</a>
</details>
