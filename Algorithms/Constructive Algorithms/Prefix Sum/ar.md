# المجموع التراكمي

## ما هو المجموع التراكمي؟  
يتم تعريف **مصفوفة المجموع التراكمي** $P$ لمصفوفة $A$ بحيث  
$$
P_i = A_1 + A_2 + \dots + A_i
$$  
باستخدام المجموع التراكمي ذو الفهرسة الواحدية (1-indexed) مع $P_0 = 0$، يمكنك الإجابة على أي استعلام مجموع جزء مصفوفة $\mathrm{sum}(A_{l..r})$ بزمن $O(1)$ كما يلي:  
$$
\mathrm{sum}(A_{l..r}) = P_r - P_{l-1}
$$  
بناء $P$ يستغرق زمن $O(n)$.

### مثال لمصفوفة $A$ والمجموع التراكمي $P$ المقابل لها:
| الفهرس | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|---------|---|---|---|---|---|---|---|
| $A$     | 0 | 1 | 1 | 0 | 6 | 5 | 3 |
| $P$     | 0 | 1 | 2 | 2 | 8 | 13| 16|

## التعقيد الزمني
- **بناء المجموع التراكمي**: $O(n)$  
- **كل استعلام ثابت**: $O(1)$  
- **تحديثات مصفوفة الفروقات**: $O(1)$ لكل تحديث → $O(m)$ لجميع التحديثات  
- **إعادة بناء المصفوفة النهائية**: $O(n)$  

بشكل عام، يعمل كلا النمطين في زمن $O(n + q)$ أو $O(n + m)$، مناسب لقيم $n$، $q$، $m$ حتى $10^5$ أو أكثر.

## المثال الأول: استعلامات جمع ثابتة

**المشكلة:**  
معطاة مصفوفة $A_1, A_2, \dots, A_n$ و$q$ استعلامات، كل منها يطلب مجموع  
$$
A_l + A_{l+1} + \dots + A_r
$$

**التطبيق ذو الفهرسة الواحدية**

=== "c++"
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, q;
    cin >> n >> q;
    vector<long long> A(n+1), P(n+1);
    for (int i = 1; i <= n; i++)
        cin >> A[i];

    P[0] = 0;
    for (int i = 1; i <= n; i++)
        P[i] = P[i-1] + A[i];

    while (q--) {
        int l, r;
        cin >> l >> r;
        cout << P[r] - P[l-1] << '\n';
    }
    return 0;
}
```

=== "Java"
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int q = Integer.parseInt(st.nextToken());
        long[] A = new long[n+1], P = new long[n+1];
        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= n; i++)
            A[i] = Long.parseLong(st.nextToken());

        P[0] = 0;
        for (int i = 1; i <= n; i++)
            P[i] = P[i-1] + A[i];

        StringBuilder sb = new StringBuilder();
        while (q-- > 0) {
            st = new StringTokenizer(br.readLine());
            int l = Integer.parseInt(st.nextToken());
            int r = Integer.parseInt(st.nextToken());
            sb.append(P[r] - P[l-1]).append('\n');
        }
        System.out.print(sb);
    }
}
```

=== "Python"
```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
A = [0] * (n+1)
# A_values = list(map(int, input().split()))
# for i in range(1, n+1): A[i] = A_values[i-1]

P = [0] * (n+1)
for i in range(1, n+1):
    P[i] = P[i-1] + A[i]

for _ in range(q):
    l, r = map(int, input().split())
    print(P[r] - P[l-1])
```

## خدعة مصفوفة الفروقات (تحديث النطاق)  
أحيانًا تحتاج إلى إجراء **عدة عمليات زيادات على نطاقات** على مصفوفة صفرية مبدئيًا، ثم فحص القيم النهائية.  
التنفيذ الخطي لكل تحديث في زمن $O(	ext{length})$ يؤدي إلى $O(n \cdot m)$، وهو بطيء جدًا إذا كان كلاهما يصل إلى $10^5$.

تتيح لك **مصفوفة الفروقات** $D$ إجراء كل تحديث للنطاق في زمن $O(1)$ مع الفهرسة الواحدية:

- لإضافة $+1$ إلى جميع العناصر $A_l, A_{l+1}, \dots, A_r$، قم بما يلي:
  ```cpp
  D[l] += 1;
  D[r+1] -= 1;     // تأكد من أن حجم D هو n+2
  ```
- بعد معالجة جميع التحديثات، أعد بناء المصفوفة $A$ بأخذ المجموع التراكمي لمصفوفة الفروقات $D$:
  ```cpp
  for(int i = 1; i <= n; i++)
      A[i] = A[i-1] + D[i];
  ```
- الآن تحتوي $A_i$ على إجمالي عدد المرات التي غطى فيها أي نطاق الفهرس $i$.

**لماذا تعمل هذه الطريقة؟**  
كل $+1$ عند $D_l$ يعني “ابدأ بإضافة 1 من هنا فصاعدًا”، وكل $-1$ عند $D_{r+1}$ يعني “توقف عن إضافة 1 بعد $r$”.

## المثال الثاني: حساب التداخلات باستخدام مصفوفة الفروقات

**المشكلة:**  
لديك $m$ قطعة $[l_i, r_i]$ حيث $1 \le l_i \le r_i \le n$. احسب لكل $i$ عدد القطع التي تغطيه.

**التطبيق ذو الفهرسة الواحدية**

=== "c++"
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    vector<long long> D(n+2, 0), cov(n+1);
    for (int i = 0; i < m; i++) {
        int l, r;
        cin >> l >> r;
        D[l] += 1;
        D[r+1] -= 1;
    }
    for (int i = 1; i <= n; i++) {
        cov[i] = cov[i-1] + D[i];
    }
    for (int i = 1; i <= n; i++) {
        cout << cov[i] << ' ';
    }
    cout << '\n';
    return 0;
}
```

=== "Java"
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        long[] D = new long[n+2], cov = new long[n+1];
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int l = Integer.parseInt(st.nextToken());
            int r = Integer.parseInt(st.nextToken());
            D[l] += 1;
            D[r+1] -= 1;
        }
        for (int i = 1; i <= n; i++)
            cov[i] = cov[i-1] + D[i];

        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++)
            sb.append(cov[i]).append(' ');
        System.out.println(sb.toString().trim());
    }
}
```

=== "Python"
```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
D = [0] * (n+2)
for _ in range(m):
    l, r = map(int, input().split())
    D[l] += 1
    D[r+1] -= 1

cov = [0] * (n+1)
for i in range(1, n+1):
    cov[i] = cov[i-1] + D[i]

print(*cov[1:])
```
