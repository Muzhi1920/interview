# Trie树
![avatar](img/trie.jpg)

- Trie树的search
```cpp
struct TrieNode {
    public:
        TrieNode *child[26];
        bool isWord;
        TrieNode():isWord(false) {
            for (auto &a:child) 
                a=NULL;
        }
};
bool search(word,node,i):
    if i==word.size:
        return node->isWord
    if word[i]!='.':  //无特殊字符递归搜索
        return node->child[word[i]-'a'] && search(word,node->child[word[i]-'a'],i+1)
    else：    //有特殊字符时在当前子节点遍历
        for(auto a:node->child){
            if a and search(word,a,i+1)
                return true;
        }
        return false;
```