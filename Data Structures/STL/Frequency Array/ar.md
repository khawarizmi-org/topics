
### مصفوفات التكرار

مصفوفات التكرار، المعروفة أيضًا بجداول التكرار، هي هياكل بيانات تُستخدم لحساب عدد مرات ظهور العناصر في مجموعة بيانات بكفاءة، خاصةً عندما تكون نطاقات العناصر محدودة. هذه التقنية مفيدة جدًا في سيناريوهات مثل العد، والفرز، والاستعلام السريع.

#### المفهوم الأساسي

مصفوفة التكرار هي مصفوفة حيث يتوافق كل فهرس مع قيمة في مجموعة البيانات، والقيمة عند ذلك الفهرس تمثل عدد مرات ظهور تلك القيمة.

على سبيل المثال، بالنظر إلى مصفوفة أعداد صحيحة:

```plaintext
arr = [1, 3, 2, 3, 1, 5, 2, 2]
```

ستبدو مصفوفة التكرار كما يلي:

```plaintext
frequency = [0, 2, 3, 2, 0, 1]
// الفهرس 0: القيمة 0 تظهر 0 مرات
// الفهرس 1: القيمة 1 تظهر 2 مرات
// الفهرس 2: القيمة 2 تظهر 3 مرات
// الفهرس 3: القيمة 3 تظهر 2 مرات
// الفهرس 4: القيمة 4 تظهر 0 مرات
// الفهرس 5: القيمة 5 تظهر 1 مرة
```

#### المزايا
- **الكفاءة:** يمكن لمصفوفات التكرار الإجابة بكفاءة على استفسارات عدد مرات ظهور عناصر محددة.
- **البساطة:** تنفيذ سهل وتمثيل بديهي.
- **العد المحسن:** يتيح تعقيد O(1) لاستعلامات التكرار.

#### حالات الاستخدام
- **الفرز:** تسهل مصفوفات التكرار خوارزمية الفرز بالعد، وهي خوارزمية غير معتمدة على المقارنات.
- **إنشاء الهيستوجرام:** إنشاء سريع للهيستوجرامات للتحليل الإحصائي.
- **التعرف على الأنماط:** مفيدة في الخوارزميات التي تتطلب عد مرات الظهور، مثل تحليل تكرار الأحرف في السلاسل.

#### القيود
- **نطاق محدود:** مثالية لمجموعات البيانات التي لها نطاق قيم معروف ومحدود. غير فعالة للنطاقات الكبيرة أو المجموعات المتفرقة.
- **استهلاك الذاكرة:** قد تكون غير فعالة من حيث المساحة إذا كان نطاق القيم أكبر بكثير من عدد العناصر.

#### مثال على التنفيذ

=== "c++"

```c++
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    int arr[n];
    int max_element = 0; // نستخدم هذا لمعرفة حجم مصفوفة التكرار.

    for (int i = 0 ; i < n ; i++) {
        cin >> arr[i];
        max_element = max(max_element, arr[i]);
    }

    // يجب أن يكون حجم مصفوفة التكرار مساويًا لـ max_element + 1،
    // لأن المصفوفة تبدأ من الفهرس صفر.
    int freq[max_element + 1] = {}; // نساويها بـ {} للتأكد من أن جميع عناصرها أصفار في البداية.
    for (int i = 0 ; i < n ; i++) {
        freq[arr[i]]++; // في كل مرة نزور الرقم في المصفوفة، نزيد تكراره بمقدار واحد
    }

    for (int i = 1 ; i <= max_element ; i++) {
        cout << "frequency of " << i << " is " << freq[i] << endl;
    }
}
```

=== "Java"

```java
import java.util.Scanner;

public class FrequencyArray {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n;  
        n = sc.nextInt();
        int[] arr = new int[n];
        int max_element = 0; // نستخدم هذا لمعرفة حجم مصفوفة التكرار.

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            max_element = Math.max(max_element, arr[i]);
        }

        // يجب أن يكون حجم مصفوفة التكرار مساويًا لـ max_element + 1،
        // لأن المصفوفة تبدأ من الفهرس صفر.
        int[] freq = new int[max_element + 1]; // نساويها بـ {} للتأكد من أن جميع عناصرها أصفار في البداية.
        for (int i = 0; i < n; i++) {
            freq[arr[i]]++; // في كل مرة نزور الرقم في المصفوفة، نزيد تكراره بمقدار واحد
        }

        for (int i = 1; i <= max_element; i++) {
            System.out.println("frequency of " + i + " is " + freq[i]);
        }
    }
}
```

=== "Python"

```python
n = int(input())
arr = [0] * n
max_element = 0  # نستخدم هذا لمعرفة حجم مصفوفة التكرار.

for i in range(n):
    arr[i] = int(input())
    max_element = max(max_element, arr[i])

# يجب أن يكون حجم مصفوفة التكرار مساويًا لـ max_element + 1،
# لأن المصفوفة تبدأ من الفهرس صفر.
freq = [0] * (max_element + 1)  # نُنشئها كقائمة من الأصفار للتأكد من أن جميع عناصرها صفر في البداية.
for i in range(n):
    freq[arr[i]] += 1  # في كل مرة نزور الرقم في المصفوفة، نزيد تكراره بمقدار واحد

for i in range(1, max_element + 1):
    print(f"frequency of {i} is {freq[i]}")
```
