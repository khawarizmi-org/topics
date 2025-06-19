# المصفوفات

## المقدمة  
المصفوفة هي مجموعة من العناصر تُعرّف بواسطة مؤشر. تخزن المصفوفات العناصر في ذاكرة متجاورة، مما يدعم الوصول والتحديث بزمن ثابت O(1). في العديد من اللغات، للمصفوفات حجم ثابت؛ بينما تدعم المصفوفات الديناميكية (مثل std::vector أو list) تغيير الحجم.

---

## التعريف والتهيئة

=== "c++"
```cpp
// مصفوفة ثابتة الحجم
int a[5];               // قيم غير محددة أوليًا
// تهيئة صريحة
int b[5] = {1, 2, 3, 4, 5};
// مصفوفة ديناميكية باستخدام std::vector
#include <vector>
std::vector<int> v(5);           // حجم 5، القيم تهيأت إلى 0
std::vector<int> u = {1,2,3,4,5};
```

=== "Java"
```java
// مصفوفة ثابتة الحجم
int[] a = new int[5];          // القيم الابتدائية 0
// تهيئة بقيم
int[] b = {1, 2, 3, 4, 5};
// مصفوفة ديناميكية عبر ArrayList
import java.util.ArrayList;
ArrayList<Integer> v = new ArrayList<>();
v.add(1);
v.add(2);
// ...
```

=== "Python"
```python
# قائمة كمصفوفة ديناميكية
a = [0] * 5                    # [0,0,0,0,0]
b = [1,2,3,4,5]
```

---

## الوصول والتحديث  
تدعم المصفوفات الفهرسة والتحديث بزمن ثابت O(1).

=== "c++"
```cpp
a[2] = 10;          // تحديث
int x = a[4];       // وصول
```

=== "Java"
```java
b[1] = 7;
int x = b[3];
```

=== "Python"
```python
u[0] = 42
x = u[2]
```

---

## التجوال  
التجوال عبر جميع العناصر بزمن O(n).

=== "c++"
```cpp
for (int i = 0; i < n; ++i) {
    cout << a[i] << " ";
}
```

=== "Java"
```java
for (int i = 0; i < a.length; i++) {
    System.out.print(a[i] + " ");
}
```

=== "Python"
```python
for x in a:
    print(x, end=" ")
```

---

## أنماط شائعة

### النافذة المنزلقة  
حساب أكبر مجموع فرعي بحجم k:
```cpp
int maxSum(vector<int>& v, int k) {
    int n = v.size();
    int sum = 0;
    for (int i = 0; i < k; ++i) sum += v[i];
    int ans = sum;
    for (int i = k; i < n; ++i) {
        sum += v[i] - v[i-k];
        ans = max(ans, sum);
    }
    return ans;
}
```

```java
int maxSum(int[] arr, int k) {
    int sum = 0, ans = 0;
    for (int i = 0; i < k; i++) sum += arr[i];
    ans = sum;
    for (int i = k; i < arr.length; i++) {
        sum += arr[i] - arr[i-k];
        ans = Math.max(ans, sum);
    }
    return ans;
}
```

```python
def max_sum(arr, k):
    s = sum(arr[:k])
    ans = s
    for i in range(k, len(arr)):
        s += arr[i] - arr[i-k]
        ans = max(ans, s)
    return ans
```

### المؤشرين  
التحقق من وجود عنصرين مجموعهما الهدف (sorted):
```cpp
bool twoSum(vector<int>& v, int target) {
    int l = 0, r = v.size()-1;
    while (l < r) {
        int s = v[l] + v[r];
        if (s == target) return true;
        else if (s < target) ++l;
        else --r;
    }
    return false;
}
```

```java
boolean twoSum(int[] a, int target) {
    int l = 0, r = a.length-1;
    while (l < r) {
        int s = a[l] + a[r];
        if (s == target) return true;
        if (s < target) l++;
        else r--;
    }
    return false;
}
```

```python
def two_sum(a, target):
    l, r = 0, len(a)-1
    while l < r:
        s = a[l] + a[r]
        if s == target: return True
        if s < target: l += 1
        else: r -= 1
    return False
```

---

## المصفوفات متعددة الأبعاد

=== "c++"
```cpp
int mat[3][4];             // 3 صفوف، 4 أعمدة
vector<vector<int>> M(3, vector<int>(4, 0));
```

=== "Java"
```java
int[][] mat = new int[3][4];   // كلها صفر
```

=== "Python"
```python
M = [[0]*4 for _ in range(3)]
```

---

## الخلاصة  
- توفر المصفوفات وصولاً وتحديثًا بزمن ثابت $O(1)$.  
- التجوال والفرز والبحث يتطلب غالبًا $O(n)$ أو $O(n \log n)$.  
- تعرّف على الأنماط الشائعة: النافذة المنزلقة، المؤشرين.  
- استخدم التركيبات الديناميكية عند الحاجة لتغيير الحجم في وقت التشغيل.
