---
title: C++单链表
zhihu-tags: c++, 数据结构
zhihu-url: https://zhuanlan.zhihu.com/p/557137667
---

# C++链表实现

简单实现C++链表


```cpp
#include<iostream>  
using namespace std;  
  
// 节点类  
template<class T>  
class node {  
public:  
    T val;  
    node<T>* next;  
};  
  
// 单向链表类  
template<class T>  
class list {  
public:  
    list();  
    ~list();  
    void push(T val);   // 末尾添加元素  
    node<T>* pop();     // 删除末尾元素，并返回删除元素节点  
    void reverseLink();  // 翻转链表  
    void add_index(int index, T val);  // 在指定索引添加值  
    void add_left(T val);  // 头插  
    void delete_left();   
    void delete_index(int index);
public:  
    node<T>* getHead(); // 获取头节点  
    int getLength();  
    bool isEmpty();  
  
private:  
    node<T>* head;  
    int length{};  
};  
  
template<class T>  
list<T>::list() {  
    head = new node<T>();  
    head->next = nullptr;  
}  
  
template<class T>  
list<T>::~list() = default;  
  
template<class T>  
void list<T>::push(T val) {  
    node<T> *p = head;  
    while (p->next) {  
        p = p->next;  
    }  
    auto *temp = new node<T>;  
    temp->val = val;  
    temp->next = nullptr;  
    p->next = temp;  
    length++;  
}  
  
template<class T>  
node<T> *list<T>::getHead() {  
    return head;  
}  
  
template<class T>  
node<T> *list<T>::pop() {  
    if (isEmpty()) return head;  
    node<T> *right = head;  
    while (right->next->next) {  
        right = right->next;  
    }  
    node<T> *temp = right->next;  
    right->next = nullptr;  
    length--;  
    return temp;  
}  
  
template<class T>  
void list<T>::reverseLink() {  
    if (isEmpty()) return;  
    node<T> *pre = nullptr;   // 翻转的头节点  
    node<T> *next = nullptr;   // 记录下一个节点  
  
    node<T> *p = head->next;  
    while (p) {  
        next = p->next;  
        p->next = pre;  
        pre = p;  
        p = next;  
    }  
    head->next = pre;  
}  
  
template<class T>  
void list<T>::add_index(int index, T val) {  
    if (index < 0) return;  
    if (index >= length) {  
        push(val);  
        return;  
    }  
    if (index == 0) {  
        add_left(val);  
        return;  
    }  
    node<T> *p = head;  
  
    for (int i = 0; i < index - 1; i++) {  
        p = p->next;  
    }  
  
    auto *temp = new node<T>;  
    temp->val = val;  
    temp->next = p->next->next;  
    p->next = temp;  
    length++;  
}  
  
template<class T>  
int list<T>::getLength() {  
    return length;  
}  
  
template<class T>  
bool list<T>::isEmpty() {  
    return !length;  
}  
  
template<class T>  
void list<T>::add_left(T val) {  
    auto *p = new node<T>;  
    p->val = val;  
    p->next = head->next;  
    head->next = p;  
    length++;  
}  
  
template<class T>  
void list<T>::delete_left() {  
    if (isEmpty()) return;  
    head->next = head->next->next;  
    length--;  
}  
  
template<class T>  
void list<T>::delete_index(int index) {  
    if (index < 0 || index > length || isEmpty()) return;  
    node<T> *p = head;  
    for (int i = 0; i < index; i++) {  
        p = p->next;  
    }  
    p->next = p->next->next;  
    length--;  
}  
  
template<typename T>  
void print(list<T> li) {  
    node<T> *res = li.getHead();  
    while (res->next) {  
        res = res->next;  
        cout << res->val << endl;  
    }  
    cout << "----------------" << endl;  
}  
  
void test() {  
    list<int> li;  
    for (int i = 1; i < 10; i++) {  
        li.push(i);  
    }  
    print(li);  
  
    li.reverseLink();  
    print(li);  
  
    li.add_index(90, 10);  
    print(li);  
  
    li.delete_index(li.getLength() - 1);  
    print(li);  
  
    li.pop();  
    print(li);  
  
    li.reverseLink();  
    print(li);  
}  
  
int main() {  
    test();  
    return 0;  
}
```