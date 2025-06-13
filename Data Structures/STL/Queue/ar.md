# الطابور (Queue)

## التعريف

الطابور هو بنية بيانات في مكتبة STL تُستخدم لتخزين واسترجاع العناصر باستخدام نظام FIFO (أول الداخل، أول الخارج)، مما يعني أنه يمكن فقط إضافة العناصر إلى نهاية الطابور، ويمكن فقط الوصول إلى العناصر الموجودة في المقدمة (العناصر التي أُضيفت أولاً). يمكننا تشبيه الطابور بصف في متجر، حيث أول شخص يصل هو أول من يُخدم.

في الطابور، تُجرى العمليات على الطرفين: الطرف الأيسر الذي يُطلق عليه عادة "المقدمة" (front)، والطرف الأيمن الذي يُطلق عليه "النهاية" (back).

## عمليات الطابور

في الطابور، توجد بعض العمليات لإضافة عناصر جديدة، والوصول إليها، وحذفها:

* Add (Push): إدراج عنصر جديد في نهاية الطابور (الخلف).
* Remove (Pop): حذف أول عنصر أُضيف إلى الطابور (العنصر في المقدمة).
* Get (Front): إرجاع أول عنصر أُضيف إلى الطابور (العنصر في المقدمة).

جميع العمليات المدعومة في الطابور لها تعقيد زمني $O(1)$، لذا فإن التعقيد الكلي للطابور هو $O(1)$، أما التعقيد المكاني فهو $O(n)$ حيث $n$ هو عدد العناصر.

## مثال على الطابور

دعونا نلاحظ كيف يعمل الطابور عند تطبيق العمليات التالية بالترتيب:

1. Add 2
2. Add 3
3. Get
4. Add 0
5. Remove
6. Add 1
7. Remove
8. Get

<div align="center">
    <img src="images/example_1_1.png">
    <img src="images/example_1_2.png">
    <p><em>(1) في البداية الطابور فارغ، بعد العملية الأولى يُضاف 2 إلى نهاية الطابور</em></p>
    <p><em>(الآن العنصر 2 في المقدمة وأيضًا في النهاية لأنه الوحيد)</em></p>
</div>

<div align="center">
    <img src="images/example_2_1.png">
    <img src="images/example_2_2.png">
    <p><em>(2) بعد العملية الثانية يُضاف 3 إلى نهاية الطابور</em></p>
    <p><em>(الآن 2 في المقدمة و3 في النهاية)</em></p>
</div>

<div align="center">
    <img src="images/example_3_1.png">
    <p><em>(3) العملية الثالثة هي Get، والنتيجة هي العنصر في المقدمة (2)</em></p>
</div>

<div align="center">
    <img src="images/example_4_1.png">
    <img src="images/example_4_2.png">
    <p><em>(4) العملية الرابعة هي Add 0، فيُضاف 0 إلى نهاية الطابور</em></p>
    <p><em>(الآن 2 في المقدمة و0 في النهاية)</em></p>
</div>

<div align="center">
    <img src="images/example_5_1.png">
    <img src="images/example_5_2.png">
    <p><em>(5) العملية الخامسة هي Remove، فيُزال العنصر الموجود في المقدمة من الطابور</em></p>
</div>

<div align="center">
    <img src="images/example_6_1.png">
    <img src="images/example_6_2.png">
    <p><em>(6) العملية السادسة هي Add 1، فيُضاف 1 إلى نهاية الطابور</em></p>
</div>

<div align="center">
    <img src="images/example_7_1.png">
    <img src="images/example_7_2.png">
    <p><em>(7) العملية السابعة هي Remove، فيُزال العنصر الموجود في المقدمة من الطابور</em></p>
</div>

<div align="center">
    <img src="images/example_8_1.png">
    <p><em>(8) العملية الثامنة هي Get، والنتيجة هي العنصر في المقدمة (0)</em></p>
</div>

## تنفيذ الطابور

=== "C++"
```cpp
#include <iostream>
#include <queue> // رأس الطابور

using namespace std;

int main() {
    /*
    في C++:
        push: لإضافة عناصر إلى الطابور
        pop: لإزالة أول عنصر في الطابور
        front: للحصول على العنصر الموجود في المقدمة
    */

    // إنشاء طابور فارغ
    queue<int> items;

    // Add 2
    items.push(2);
    // items = 2 <- العنصر في النهاية

    // Add 3
    items.push(3);
    // items = 2, 3 <- العنصر في النهاية

    // Get
    cout << "العنصر في المقدمة هو: " << items.front() << endl;
    // يطبع 2

    // Add 0
    items.push(0);
    // items = 2, 3, 0 <- العنصر في النهاية

    // Remove
    items.pop();
    // يُزال 2، items = 3, 0 <- العنصر في النهاية

    // Add 1
    items.push(1);
    // items = 3, 0, 1 <- العنصر في النهاية

    // Remove
    items.pop();
    // يُزال 3، items = 0, 1 <- العنصر في النهاية

    // Get
    cout << "العنصر في المقدمة هو: " << items.front() << endl;
    // يطبع 0
}
```

=== "Python"
```python
from queue import Queue

# في بايثون:
# put(): لإضافة عنصر إلى الطابور
# get(): لإزالة العنصر من المقدمة
# queue.queue[0]: للوصول إلى العنصر في المقدمة (ليست طريقة رسمية لكنها تعمل)

# إنشاء طابور فارغ
items = Queue()

# Add 2
items.put(2)
# items = 2

# Add 3
items.put(3)
# items = 2, 3

# Get front item
print("العنصر في المقدمة هو:", items.queue[0])
# يطبع 2

# Add 0
items.put(0)
# items = 2, 3, 0

# Remove front item
items.get()
# items = 3, 0

# Add 1
items.put(1)
# items = 3, 0, 1

# Remove front item
items.get()
# items = 0, 1

# Get front item
print("العنصر في المقدمة هو:", items.queue[0])
# يطبع 0
```

```plaintext
الناتج:

العنصر في المقدمة هو: 2  
العنصر في المقدمة هو: 0
```
