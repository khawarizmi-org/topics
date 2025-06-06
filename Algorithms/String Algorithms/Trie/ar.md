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


=== "C++"
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    Node* child[26]{};
    bool end = false;
};

class Trie {
    Node* root = new Node();
    static int id(char c){ return c - 'a'; }
public:
    void insert(const string& w){
        Node* cur = root;
        for(char ch: w){
            int i = id(ch);
            if(!cur->child[i]) cur->child[i] = new Node();
            cur = cur->child[i];
        }
        cur->end = true;
    }
    bool search(const string& w){
        Node* cur = root;
        for(char ch: w){
            int i = id(ch);
            if(!(cur = cur->child[i])) return false;
        }
        return cur->end;
    }
    bool startsWith(const string& p){
        Node* cur = root;
        for(char ch: p){
            int i = id(ch);
            if(!(cur = cur->child[i])) return false;
        }
        return true;
    }
};
```

=== "Python"
```python
class TrieNode:
    __slots__ = ("child", "end")
    def __init__(self):
        self.child = {}
        self.end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str):
        node = self.root
        for ch in word:
            node = node.child.setdefault(ch, TrieNode())
        node.end = True

    def _walk(self, s: str):
        node = self.root
        for ch in s:
            node = node.child.get(ch)
            if node is None:
                return None
        return node

    def search(self, word: str) -> bool:
        node = self._walk(word)
        return bool(node and node.end)

    def starts_with(self, prefix: str) -> bool:
        return self._walk(prefix) is not None
```

=== "JavaScript"
```js
class TrieNode{
  constructor(){
    this.child = new Map(); // char -> TrieNode
    this.end   = false;
  }
}
export class Trie{
  #root = new TrieNode();
  insert(word){
    let node = this.#root;
    for(const ch of word){
      if(!node.child.has(ch)) node.child.set(ch, new TrieNode());
      node = node.child.get(ch);
    }
    node.end = true;
  }
  #walk(str){
    let node = this.#root;
    for(const ch of str){
      node = node.child.get(ch);
      if(!node) return null;
    }
    return node;
  }
  search(word){ const n = this.#walk(word); return !!n && n.end; }
  startsWith(prefix){ return !!this.#walk(prefix); }
}
```
