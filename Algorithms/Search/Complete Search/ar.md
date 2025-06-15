# البحث الشامل

البحث الشامل هو تقنية قوة غاشمة حيث تقوم فعلياً بتجربة جميع التراكيب الممكنة.

لنأخذ المثال التالي:

يُعطى لديك عدد صحيح موجب زوجي $n$. يجب عليك طباعة جميع تسلسلات الأقواس المنتظمة بطول $n$.

تسلسل الأقواس المنتظم (RBS) هو تسلسل من الأقواس يكون متوازنًا بشكل صحيح ومتداخلًا بشكل سليم. رسمياً، التسلسل المكوّن من `(` و `)` هو تسلسل منتظم إذا كان:

- كل قوس مفتوح `(` له قوس مغلق مطابق `)`.
- في أي نقطة من التسلسل، لا يزيد عدد الأقواس المغلقة `)` عن عدد الأقواس المفتوحة `(` عند القراءة من اليسار إلى اليمين.

### أمثلة

تُعتبر `()`, `(())()`, `((()))`, `()()` تسلسلات أقواس منتظمة. أما `)(`, `(()`, `())(` فهي غير منتظمة (غير صالحة).

الحل بالقوة الغاشمة لهذه المشكلة سيكون بتوليد جميع التسلسلات الممكنة بطول $n$ ثم التحقق لكل تسلسل ما إذا كان منتظمًا أم لا.

يمكننا استخدام الكود التالي للتحقق مما إذا كانت السلسلة RBS أم لا:

=== "c++"
```c++
bool is_rbs(string &s) {
    int opening = 0;
    for (int i = 0; i < (int)s.size(); i++) {
        if (s[i] == '(')
            opening++;
        else
            opening--;
         //There is a closing bracket that doesn't have a corresponding opening
        if (opening < 0)
            return false;
    }
    // return true if each opening has a corresponding closing
    return opening == 0;
}
```

=== "Java"
```java
public static boolean isRbs(String s) {
    int opening = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(')
            opening++;
        else
            opening--;
        // There is a closing bracket that doesn't have a corresponding opening
        if (opening < 0)
            return false;
    }
    // return true if each opening has a corresponding closing
    return opening == 0;
}
```

=== "Python"
```python
def is_rbs(s):
    opening = 0
    for c in s:
        if c == '(':
            opening += 1
        else:
            opening -= 1
        # There is a closing bracket that doesn't have a corresponding opening
        if opening < 0:
            return False
    # return True if each opening has a corresponding closing
    return opening == 0
```

الآن، المشكلة هي توليد جميع التسلسلات الممكنة بطول $n$، حيث يكون كل حرف إما `(` أو `)`.

إحدى الطرق المعروفة لتنفيذ ذلك على الورق هي رسم شجرة كما يلي:

- تبدأ من جذر الشجرة بتسلسل فارغ.
- الحرف الأول له خياران، إما `(` أو `)`، لذلك ترسم طفلين، أحدهما به `(` والآخر به `)`.
- لكل طفل، كرر هذه العملية بإضافة طفلين، واحد بـ `(` والآخر بـ `)`، مكوناً جميع التراكيب بطول 2.
- الورقة هي عقدة ليس لها أبناء، وعمق الشجرة هو المسافة بين عقدة الورقة والجذر.
- في هذه المرحلة، لدينا شجرة بعمق 2، وكل مسار من الجذر إلى ورقة يمثل تركيبًا مختلفًا، لذا فإن جميع الأوراق معًا تمثل جميع التراكيب بطول 2.
- إذا أردنا الحصول على جميع التراكيب بطول 3، فيمكننا رسم طفلين لكل ورقة، أحدهما بـ `(` والآخر بـ `)`.
- لذلك، إذا كان لدينا شجرة بعمق $d$ ورسمنا طفلين لجميع الأوراق، فسنحصل على شجرة بعمق $d+1$.
- لتوليد جميع التسلسلات الممكنة بطول $n$، نحتاج إلى رسم شجرة بعمق $n$.
- في النهاية، يمكننا التحقق من كل تسلسل لتحديد ما إذا كان تسلسلًا منتظمًا أم لا.

فيما يلي مثال عندما تكون $n = 4$:

<div align="center">
    <img src="images/draw-tree.gif" alt="recursion tree">
</div>


### التوليد العودي

لإنشاء دالة تولد جميع هذه التسلسلات، يمكننا اتباع مفهوم الشجرة. نبدأ من العمق 0، حيث يؤدي كل اختيار إلى العمق 1، وكل مستوى عمق يضاعف الاحتمالات حتى نصل إلى العمق $n$.

إحدى طرق التنفيذ هي كتابة دالة لكل مستوى عمق. نبدأ باستدعاء الدالة في العمق `0`، وتقوم كل دالة باستدعاء مستوى العمق التالي بالاختيارين الممكنين.

على سبيل المثال، عندما تكون $n = 4$، الكود التالي يولد جميع التسلسلات:

=== "c++"
```c++
void depth0(string s);
void depth1(string s);
void depth2(string s);
void depth3(string s);
void depth4(string s);

void depth0() {
    string s;
    depth1(s + '(');
    depth1(s + ')');
}

void depth1(string s) {
    depth2(s + '(');
    depth2(s + ')');
}

void depth2(string s) {
    depth3(s + '(');
    depth3(s + ')');
}

void depth3(string s) {
    depth4(s + '(');
    depth4(s + ')');
}

void depth4(string s) {
    if (is_rbs(s))
        cout << s << endl;
}

int main() {
    depth0();
    return 0;
}
```

=== "Java"
```java
public class Main {
    static void depth0() {
        String s = "";
        depth1(s + "(");
        depth1(s + ")");
    }

    static void depth1(String s) {
        depth2(s + "(");
        depth2(s + ")");
    }

    static void depth2(String s) {
        depth3(s + "(");
        depth3(s + ")");
    }

    static void depth3(String s) {
        depth4(s + "(");
        depth4(s + ")");
    }

    static void depth4(String s) {
        if (isRbs(s))
            System.out.println(s);
    }

    public static void main(String[] args) {
        depth0();
    }
}
```

=== "Python"
```python
def depth0():
    s = ""
    depth1(s + "(")
    depth1(s + ")")

def depth1(s):
    depth2(s + "(")
    depth2(s + ")")

def depth2(s):
    depth3(s + "(")
    depth3(s + ")")

def depth3(s):
    depth4(s + "(")
    depth4(s + ")")

def depth4(s):
    if is_rbs(s):
        print(s)

depth0()
```

### النهج العودي المحسن

يتطلب النهج السابق كتابة $n$ دوال منفصلة. ومع ذلك، نظرًا لأن كل دالة عند العمق $i$ تحاول خيارين وتستدعي الدالة عند العمق $i + 1$، يمكننا استبدال ذلك بدالة عودية واحدة يكون فيها شرط النهاية عند الوصول إلى العمق $n$.

يبدو الحل العودي المحسن كما يلي:

=== "c++"
```c++
void generate_rbs(int depth, string s, int n) {
    if (depth == n) {
        if (is_rbs(s)) {
            cout << s << endl;
        }
        return;
    }
    generate_rbs(depth + 1, s + "(", n);
    generate_rbs(depth + 1, s + ")", n);
}

int main() {
    int n;
    cin >> n;
    generate_rbs(0, "", n);
    return 0;
}
```

=== "Java"
```java
import java.util.Scanner;

public class Main {

    static void generateRbs(int depth, String s, int n) {
        if (depth == n) {
            if (isRbs(s)) {
                System.out.println(s);
            }
            return;
        }
        generateRbs(depth + 1, s + "(", n);
        generateRbs(depth + 1, s + ")", n);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        generateRbs(0, "", n);
    }
}
```

=== "Python"
```python
def generate_rbs(depth, s, n):
    if depth == n:
        if is_rbs(s):
            print(s)
        return
    generate_rbs(depth + 1, s + "(", n)
    generate_rbs(depth + 1, s + ")", n)

n = int(input())
generate_rbs(0, "", n)
```

تعمل هذه الدالة العودية على توليد جميع تسلسلات الأقواس بطول $n$ بشكل صحيح، مع التأكد من طباعة التسلسلات الصالحة فقط.

### تحليل التعقيد

- عدد الاستدعاءات العودية:
  
  - تشكل العودية شجرة ثنائية الارتفاع $n$.
  - عدد الأوراق يساوي $2^n$، وهو أيضًا عدد التراكيب الممكنة.
  - إجمالي عدد الاستدعاءات هو $1 + 2 + 4 + \ldots + 2^n = 2^{n+1} - 1$.

- العمل المنجز عند العقد غير الورقية:
  
  - كل استدعاء عودي يبني سلسلة جديدة بإلحاق '(' أو ')' إلى $s$. إلحاق حرف إلى سلسلة بطول $k$ (في C++) يكلف $O(k)$ (لأنه ينسخ السلسلة).

- العمل المنجز عند كل ورقة:
  
  - عند كل ورقة (عندما يكون depth == n)، يتم استدعاء is_rbs(s) على سلسلة بطول $n$.
  - يمكن التحقق مما إذا كانت السلسلة RBS في وقت $O(n)$.

- إجمالي العمل:
  
  - عند كل ورقة: $O(n)$
  - عدد الأوراق: $2^n$
  - إجمالي زمن التحقق: $O(n \cdot 2^n)$
  - عبر جميع الاستدعاءات، يجمع مجموع تكلفة عمليات السلسلة أيضًا إلى $O(n \cdot 2^n)$، لأن كل ورقة يتم الوصول إليها عبر مسار من $n$ عمليات إلحاق، وعدد الأوراق $2^n$.

لذا فإن التعقيد الكلي هو $O(n \cdot 2^n)$.

### تنفيذ مختلف

نظرًا لأن تمرير السلسلة $s$ كوسيط يجعل تعقيد الاستدعاء $O(\mathrm{len}(s))$، يمكننا إرسال $s$ بالمرجعية لتحسين الفعالية.

على أي حال، عند انتهاء الدالة والعودة إلى المستدعي، نحتاج إلى ضمان أن تبدو $s$ كما كانت قبل استدعاء الدالة. لتحقيق ذلك، يجب التراجع عن التغييرات، لذا سيكون الكود المصحح كالتالي:

=== "c++"
```c++
void generate_rbs(int depth, string &s, int n) {
    if (depth == n) {
        if (is_rbs(s)) {
            cout << s << endl;
        }
        return;
    }
    s += "(";
    generate_rbs(depth + 1, s, n);
    s.pop_back();
    s += ")";
    generate_rbs(depth + 1, s, n);
    s.pop_back();
    // After calling generate_rbs(depth + 1, s, n), we ensure that string s
    // is the same as it was before the call. Using pop_back() removes
    // the change we made, so we return with no modifications to s.
}

int main() {
    int n;
    cin >> n;
    string s = "";
    generate_rbs(0, s, n);
    return 0;
}
```

=== "Java"
```java
import java.util.Scanner;

public class Main {

    static void generateRbs(int depth, StringBuilder s, int n) {
        if (depth == n) {
            if (isRbs(s.toString())) {
                System.out.println(s.toString());
            }
            return;
        }
        s.append('(');
        generateRbs(depth + 1, s, n);
        s.deleteCharAt(s.length() - 1);

        s.append(')');
        generateRbs(depth + 1, s, n);
        s.deleteCharAt(s.length() - 1);
        // After calling generateRbs(depth + 1, s, n), we ensure that s
        // is the same as it was before the call. Using deleteCharAt removes
        // the change we made, so we return with no modifications to s.
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        StringBuilder s = new StringBuilder();
        generateRbs(0, s, n);
    }
}
```

=== "Python"
```python
def generate_rbs(depth, s, n):
    if depth == n:
        if is_rbs(''.join(s)):
            print(''.join(s))
        return
    s.append('(')
    generate_rbs(depth + 1, s, n)
    s.pop()

    s.append(')')
    generate_rbs(depth + 1, s, n)
    s.pop()
    # After calling generate_rbs(depth + 1, s, n), we ensure that s
    # is the same as it was before the call. Using pop() removes
    # the change we made, so we return with no modifications to s.

n = int(input())
s = []
generate_rbs(0, s, n)
```

لقد قللنا العبء الناتج عن نسخ السلسلة في الاستدعاءات العودية، لكن دالة التحقق عند كل ورقة لا تزال لها تعقيد $O(n)$، لذا يبقى التعقيد الكلي $O(n \cdot 2^n)$.

يمكننا تحسين ذلك عن طريق إجراء التحقق أثناء الاستدعاءات العودية. يمكننا إضافة وسيط عدّاد (counter) يحسب الفرق بين عدد الأقواس المفتوحة والمغلقة. إذا أصبح العداد سالبًا، يمكننا العودة فورًا، لأن جميع التسلسلات تحت هذا الفرع ستكون غير صالحة:

=== "c++"
```c++
void generate_rbs(int depth, string &s, int n, int counter) {
    if (counter < 0)
        return;
    if (depth == n) {
        if (counter == 0) {
            cout << s << endl;
        }
        return;
    }
    s += "(";
    generate_rbs(depth + 1, s, n, counter + 1);
    s.pop_back();
    s += ")";
    generate_rbs(depth + 1, s, n, counter - 1);
    s.pop_back();
}
...
```

=== "Java"
```java
import java.util.Scanner;

public class Main {
    static void generateRbs(int depth, StringBuilder s, int n, int counter) {
        if (counter < 0)
            return;
        if (depth == n) {
            if (counter == 0) {
                System.out.println(s.toString());
            }
            return;
        }
        s.append('(');
        generateRbs(depth + 1, s, n, counter + 1);
        s.deleteCharAt(s.length() - 1);

        s.append(')');
        generateRbs(depth + 1, s, n, counter - 1);
        s.deleteCharAt(s.length() - 1);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        StringBuilder s = new StringBuilder();
        generateRbs(0, s, n, 0);
    }
}
```

=== "Python"
```python
def generate_rbs(depth, s, n, counter):
    if counter < 0:
        return
    if depth == n:
        if counter == 0:
            print(''.join(s))
        return
    s.append('(')
    generate_rbs(depth + 1, s, n, counter + 1)
    s.pop()

    s.append(')')
    generate_rbs(depth + 1, s, n, counter - 1)
    s.pop()

n = int(input())
s = []
generate_rbs(0, s, n, 0)
```

الآن، العمل المنجز عند كل ورقة وعقدة غير ورقية هو $O(1)$. مع التحقق من العداد السلبي والعودة المبكرة، نتخطى العديد من الفروع، مما يجعل الحل أسرع بكثير، لكن أسوأ تعقيد للحالة لا يزال قريبًا من $O(2^n)$.
