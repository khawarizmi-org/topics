# الخريطة (Map)

#### ماب (`map`/`TreeMap`)

في البرمجة التنافسية، غالبًا ما تحتاج إلى عدّ أو البحث عن عناصر مفاتيحها ليست أعدادًا صحيحة صغيرة (مثل السلاسل النصية، الأزواج، المعرفات المبعثرة). تفشل مصفوفة التكرار في تلك الحالات — استخدم الحاويات الترابطية بدلًا منها.

---

### الوصف

- **ماب<K,V> (في C++) / TreeMap<K,V> (في Java)**  
  - شجرة متوازنة تحت الغطاء (غالبًا شجرة أحمر–أسود).  
  - المفاتيح مخزنة بترتيبٍ مرتب.  

---

### متى نستخدمها

| السيناريو                                      | مصفوفة التكرار | ماب / TreeMap        |
|-----------------------------------------------|:--------------:|:--------------------:|
| المفاتيح **أعداد صحيحة متجاورة وصغيرة**       | ✓              | ✓ (لكن أبطأ)         |
| المفاتيح **أعداد صحيحة كبيرة/مبعثرة** (حتى 10⁹) | ✗            | ✓                    |
| المفاتيح **سلاسل نصية** (كلمات، مقاطع)        | ✗              | ✓                    |
| المفاتيح **أزواج/مجموعات** (إحداثيات)        | ✗              | ✓                    |
| نحتاج **تكرار مرتب** أو **استعلامات مدى**      | ✗              | ✓                    |

---

### التعقيد الزمني

| العملية            | ماب / TreeMap      |
|:------------------:|:------------------:|
| **الإدراج**        | O(log n)           |
| **البحث/الوصول**   | O(log n)           |
| **الحذف**          | O(log n)           |
| **التكرار**        | O(n) بترتيب المفاتيح |

---

### أمثلة التنفيذ

#### مثال على الإدراج والتكرار

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main(){
    map<string, int> frequency;

    string arr[] = {
        "cat", "hat", "apple",
        "apple", "hat", "dog",
        "hat", "cat", "cat",
        "cat", "hat", "hat"
    };

    for (string &s : arr){
        // زيادة تكرار كل سلسلة
        frequency[s]++; 
    }

    cout << "Map keys and values:" << endl;
    for (auto [key, value] : frequency) {
        cout << key << " -> " << value << endl;
    }

    return 0;
}
```

```java
import java.util.Map;
import java.util.TreeMap;

public class FrequencyCounter {
    public static void main(String[] args) {
        Map<String, Integer> frequency = new TreeMap<>();

        String[] arr = {
            "cat", "hat", "apple",
            "apple", "hat", "dog",
            "hat", "cat", "cat",
            "cat", "hat", "hat"
        };

        for (String s : arr) {
            // زيادة تكرار كل سلسلة
            frequency.put(s, frequency.getOrDefault(s, 0) + 1);
        }

        System.out.println("Map keys and values:");
        for (Map.Entry<String, Integer> entry : frequency.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

#### مثال على التحقق من تكرار مفتاح وحذفه

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main(){
    map<string, int> frequency;

    string arr[] = {
        "cat", "hat", "apple",
        "apple", "hat", "dog",
        "hat", "cat", "cat",
        "cat", "hat", "hat"
    };

    for (string &s : arr){
        frequency[s]++; 
    }

    // الوصول مباشرة لتكرار المفتاح
    cout << "Frequency of cat: " << frequency["cat"] << endl;
    cout << endl;

    // التحقق من وجود المفتاح باستخدام count
    cout << "Does banana exist? " << frequency.count("banana") << endl;
    cout << "Does apple exist? " << frequency.count("apple") << endl;
    cout << endl;

    // حجم الماب قبل وبعد الحذف
    cout << "number of elements in the map is: " << frequency.size() << endl;
    frequency.erase("cat");
    cout << "number of elements in the map is: " << frequency.size() << endl;

    return 0;
}
```

```java
import java.util.Map;
import java.util.TreeMap;

public class FrequencyMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> frequency = new TreeMap<>();

        String[] arr = {
            "cat", "hat", "apple",
            "apple", "hat", "dog",
            "hat", "cat", "cat",
            "cat", "hat", "hat"
        };

        // بناء خريطة التكرار
        for (String s : arr) {
            frequency.put(s, frequency.getOrDefault(s, 0) + 1);
        }

        // الوصول مباشرة لتكرار المفتاح
        System.out.println("Frequency of cat: " + frequency.get("cat"));
        System.out.println();

        // التحقق من وجود المفتاح
        System.out.println("Does banana exist? " + (frequency.containsKey("banana") ? 1 : 0));
        System.out.println("Does apple exist?  " + (frequency.containsKey("apple")  ? 1 : 0));
        System.out.println();

        // الحجم قبل وبعد الحذف
        System.out.println("Number of elements in the map is: " + frequency.size());
        frequency.remove("cat");
        System.out.println("Number of elements in the map is: " + frequency.size());
    }
}
```

### لماذا الماب بدل مصفوفة التكرار؟

1. **أنواع مفاتيح غير محدودة**  
   - **سلاسل نصية**: عدّ الكلمات أو المقاطع.  
   - **أزواج/مجموعات**: عدّ الإحداثيات أو حواف الرسم البياني.

2. **لا حاجة لنطاق مسبق**  
   - مصفوفة التكرار تتطلب معرفة `max_key` وتخصيص `O(max_key)`.  
   - الماب تنمو ديناميكيًا بـ `O(log n)`.

3. **بيانات مبعثرة**  
   - إذا ظهرت بعض المفاتيح فقط من نطاق ضخم، فالمصفوفة تضيع مساحة كبيرة.

4. **عمليات مرتبة** (مع ماب/TreeMap)  
   - استعلامات مدى: `lower_bound`، `upper_bound`.  
   - التكرار بترتيب المفاتيح.

> **الخلاصة:**  
> - **مصفوفات التكرار** ممتازة للمفاتيح الصحيحة الصغيرة والمعروفة.  
> - **ماب** تتعامل بكفاءة مع **السلاسل**، **الأزواج**، **المعرفات المبعثرة أو غير المعروفة** بأقل تكلفة إضافية.  
