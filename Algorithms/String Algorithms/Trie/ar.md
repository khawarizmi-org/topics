# شجرة التراي (Trie)

شجرة التراي (وتُعرف أيضًا باسم **شجرة البادئة**) هي بنية بيانات (Data Structure) تُستخدم لتخزين سلاسل الحروف (Strings) بكفاءة، خاصةً عند الحاجة إلى تنفيذ عمليات البحث والإدراج والتحقق من وجود بادئات (Prefixes) بسرعة. تعتبر التراي مفيدة في تطبيقات كثيرة مثل القواميس الإلكترونية، والتصحيح التلقائي (Autocomplete)، والبحث عن الكلمات المقترحة، وغير ذلك.

## الفكرة الأساسية
- تتكون شجرة التراي من عُقد (Nodes) متصلة تمثل كل عقدة فيها حرفًا واحدًا من السلسلة.
- يبدأ المسار من العقدة الجذرية (Root Node) مرورًا ببقية الحروف حتى نهاية الكلمة.
- كل عقدة قد تحتوي على مؤشر أو إشارة إلى أنّها تمثل نهاية كلمة (End of Word).

بهذه الطريقة، يمكن مشاركة المسارات المشتركة بين الكلمات المتشابهة في البادئة، مما يحفظ الذاكرة ويسهّل عمليات البحث والإدراج بكفاءة.

---

## المزايا
1. **سرعة البحث**: يمكن البحث عن كلمة مكونة من \(m\) حروف في وقت \(O(m)\).
2. **مشاركة المسارات**: تساهم في توفير المساحة عند تخزين كلمات لها بادئة مشتركة.
3. **المطابقة الجزئية** (Prefix Matching): مثالية للبحث عن الكلمات التي تبدأ بحروف معينة (مثل الاقتراح التلقائي).

---

## الواجهة (Interface)

عادةً ما توفر شجرة التراي العمليات التالية:

1. **Insert**: إدراج كلمة في التراي.
2. **Search**: البحث عن وجود كلمة معينة في التراي.
3. **StartsWith** (اختياري): التحقق مما إذا كانت هناك كلمة تبدأ ببادئة معينة.
4. **Delete** (اختياري): حذف كلمة من التراي (يتطلب إدارة خاصة بالعُقد الفارغة).

فيما يلي مثال كامل في لغة **C++** يوضح هذه العمليات الأساسية:

```cpp
#include <iostream>
#include <string>
using namespace std;

// بنية عقدة التراي
struct TrieNode {
    // كل حرف بالإنجليزية (a-z) له 26 مؤشر (تبعاً لأبجدية اللغة الإنجليزية)
    TrieNode* children[26];
    // يشير إلى ما إذا كانت هذه العقدة تمثل نهاية كلمة
    bool isEndOfWord;

    TrieNode() {
        // تهيئة المؤشرات بالقيمة nullptr
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
        isEndOfWord = false;
    }
};

// تعريف التراي
class Trie {
private:
    TrieNode* root;

    // دالة مساعدة لتحويل الحرف إلى فهرس (0-25)
    int charToIndex(char c) {
        return c - 'a';
    }

public:
    Trie() {
        root = new TrieNode();
    }

    // إدراج كلمة في التراي
    void insert(const string &key) {
        TrieNode* currentNode = root;
        for (char c : key) {
            int index = charToIndex(c);
            // إذا لم تكن العقدة موجودة، ننشئ واحدة جديدة
            if (!currentNode->children[index]) {
                currentNode->children[index] = new TrieNode();
            }
            currentNode = currentNode->children[index];
        }
        // بعد انتهاء الحروف، نشير إلى أنّ العقدة تمثل نهاية لكلمة
        currentNode->isEndOfWord = true;
    }

    // البحث عن كلمة في التراي
    bool search(const string &key) {
        TrieNode* currentNode = root;
        for (char c : key) {
            int index = charToIndex(c);
            // إذا المؤشر فارغ فهذا يعني عدم وجود المسار المطلوب
            if (!currentNode->children[index]) {
                return false;
            }
            currentNode = currentNode->children[index];
        }
        // يجب أن تكون العقدة نهاية كلمة حتى تعتبر الكلمة موجودة فعليًا
        return (currentNode != nullptr && currentNode->isEndOfWord);
    }

    // التحقق من وجود بادئة
    bool startsWith(const string &prefix) {
        TrieNode* currentNode = root;
        for (char c : prefix) {
            int index = charToIndex(c);
            if (!currentNode->children[index]) {
                return false;
            }
            currentNode = currentNode->children[index];
        }
        return true; // وصلنا لنهاية البادئة دون انقطاع
    }
};

// اختبار التراي
int main() {
    Trie trie;

    trie.insert("hello");
    trie.insert("hell");
    trie.insert("help");
    trie.insert("helium");
    trie.insert("cat");

    cout << boolalpha; // عرض القيم المنطقية على شكل true/false

    // اختبار البحث عن كلمات
    cout << "Search for 'hello': " << trie.search("hello") << endl; // true
    cout << "Search for 'hell':  " << trie.search("hell") << endl;  // true
    cout << "Search for 'help':  " << trie.search("help") << endl;  // true
    cout << "Search for 'hel':   " << trie.search("hel") << endl;   // false

    // اختبار وجود بادئة
    cout << "Starts with 'he':   " << trie.startsWith("he") << endl; // true
    cout << "Starts with 'ca':   " << trie.startsWith("ca") << endl; // true
    cout << "Starts with 'ho':   " << trie.startsWith("ho") << endl; // false

    return 0;
}
