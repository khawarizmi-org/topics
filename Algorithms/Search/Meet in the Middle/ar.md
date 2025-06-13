
### الالتقاء في المنتصف

**الالتقاء في المنتصف** هو أسلوب تقليدي يعتمد على **المقايضة بين الوقت والذاكرة**، يمكنه تحويل البحث الشامل ذو التعقيد $O(b^n)$ إلى ما يقارب $O(b^{n/2})$ عن طريق تقسيم المسألة إلى نصفين، وحل كل نصف بشكل مستقل، ثم دمج النتائج بكفاءة.

#### مثال على مسألة

معك مصفوفة أعداد صحيحة $a$ بطول $n$ وعدد صحيح $k$. هل يوجد جزء (تحت-تسلسل) من $a$ مجموع عناصره يساوي $k$؟

القيود:

$$
1 \le n \le 40,\quad
1 \le a_i \le 10^9,\quad
1 \le k \le 4 \cdot 10^{10}.
$$

الحل الشامل البسيط يفحص جميع $2^n$ جزءاً في زمن $O(2^n)$، وهذا كبير جداً عندما $n=40$.

---

#### تقنية الالتقاء في المنتصف:

يمكننا تقسيم المصفوفة إلى نصفين متساويين، وحساب جميع مجموعات الأجزاء لكل نصف وتخزين الناتج.

بما أن حجم المصفوفة هو $n$، يكون حجم كل نصف $n/2$، لذا عدد جميع الأجزاء لكل نصف هو $2^{\frac{n}{2}}$.

مثلاً إذا كان لدينا $a = [1, 5, 2, 4]$، نقسمها إلى نصفين $h1 = [1, 5]$ و $h2 = [2, 4]$، ثم نحسب مجموع جميع الأجزاء لكل نصف.

الآن لكل نصف، نقوم بتخزين مجموع جميع الأجزاء في مصفوفتين $s1$ و $s2$.

في المثال السابق $s1 = [0, 1, 5, 6]$ و $s2 = [0, 2, 4, 6]$ (حيث $0$ يمثل الجزء الفارغ).

أي جزء من المصفوفة الأصلية $a$، سيكون فيه بعض العناصر من النصف الأول وبعضها من النصف الثاني، لذا يكون مجموعه عبارة عن عنصر من $s1$ + عنصر من $s2$.

إذًا المسألة الآن هي البحث عن زوج من الفهارس $i$ و $j$ بحيث $s1_i + s2_j = k$.

يمكننا ذلك عن طريق المرور على عناصر $s1$، ولكل عنصر $i$ $(1 \leq i \leq |s1|)$ نبحث هل يوجد عنصر في $s2$ يساوي $k - s1_i$، ويمكننا ذلك بفرز $s2$ ثم استخدام البحث الثنائي لكل قيمة.

إذًا التعقيد الكلي يصبح $O\bigl(2^{\frac{n}{2}} \cdot \log{2^{\frac{n}{2}}}\bigr)$ أي $O\bigl(2^{\frac{n}{2}} \cdot \frac{n}{2}\bigr)$، وهو مقبول ضمن القيود المعطاة.

#### الكود:

=== "c++"

```c++
#include <vector>
#include <algorithm>

void sum_of_all_subsets(const vector<int> &a, int i, int sum, vector<long long> &res){
    if (i == (int)a.size()){
        res.push_back(sum);
        return;
    }
    sum_of_all_subsets(a, i + 1, sum, res);
    sum_of_all_subsets(a, i + 1, sum + a[i], res);
}

bool has_subset_with_sum(vector<int> a, int n, long long k){
    vector<int> h1, h2;
    for (int i = 0; i < n / 2; i++)
        h1.push_back(a[i]);
    for (int i = n / 2; i < n; i++)
        h2.push_back(a[i]);
    vector<long long> s1, s2;
    sum_of_all_subsets(h1, 0, 0, s1);
    sum_of_all_subsets(h2, 0, 0, s2);

    sort(s2.begin(), s2.end());

    for (int i = 0; i < (int)s1.size(); i++){
        if (binary_search(s2.begin(), s2.end(), k - s1[i])){
            return true;
        }
    }
    return false;
}
```

=== "Java"

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MeetInMiddle {

    private static void generateSubsetSums(List<Integer> a, int idx, long sum, List<Long> res) {
        if (idx == a.size()) {
            res.add(sum);
            return;
        }

        generateSubsetSums(a, idx + 1, sum, res);
        generateSubsetSums(a, idx + 1, sum + a.get(idx), res);
    }

    public static boolean hasSubsetWithSum(long k, List<Integer> a) {
        int n = a.size();
        List<Integer> h1 = new ArrayList<>(a.subList(0, n/2));
        List<Integer> h2 = new ArrayList<>(a.subList(n/2, n));

        List<Long> s1 = new ArrayList<>();
        List<Long> s2 = new ArrayList<>();
        generateSubsetSums(h1, 0, 0L, s1);
        generateSubsetSums(h2, 0, 0L, s2);

        Collections.sort(s2);
        for (long sum1 : s1) {
            long target = k - sum1;
            if (Collections.binarySearch(s2, target) >= 0) {
                return true;
            }
        }
        return false;
    }
}
```

=== "Python"

```python
from bisect import bisect_left

def generate_subset_sums(a, i, current_sum, res):
    if i == len(a):
        res.append(current_sum)
        return
     
    generate_subset_sums(a, i + 1, current_sum, res)
    generate_subset_sums(a, i + 1, current_sum + a[i], res)

def has_subset_with_sum(k, a):
    n = len(a)
    h1 = a[:n//2]
    h2 = a[n//2:]

    print(h1)
    print(h2)

    s1, s2 = [], []
    generate_subset_sums(h1, 0, 0, s1)
    generate_subset_sums(h2, 0, 0, s2)

    s2.sort()
    for sum1 in s1:
        target = k - sum1
        idx = bisect_left(s2, target)
        if idx < len(s2) and s2[idx] == target:
            return True
    return False
```
