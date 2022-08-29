---
title: C++栈
zhihu-tags: C++, 数据结构
zhihu-url: https://zhuanlan.zhihu.com/p/557149391
---


```cpp
#include<iostream>  
using namespace std;  
  
template<class T>  
class stack {  
public:  
    explicit stack(int size);  
    ~stack();  
    void push(T val);  
    T* pop();  
    bool isEmpty();  
public:  
    T* getData();  
    T getTop();  
    int getSize();  
private:  
    int size{};  
    int top{};  
    T* data;  
};  
  
template<class T>  
stack<T>::stack(int size) {  
    this->size = size;  
    data = new T[this->size];  
    for (int i = 0; i < size; ++i) {  
        data[i] = *(new T());  
    }  
    this->top = -1;  
}  
  
template<class T>  
stack<T>::~stack() {  
    delete data;  
}  
  
template<class T>  
T *stack<T>::getData() {  
    return data;  
}  
  
template<class T>  
T stack<T>::getTop() {  
    return data[top];  
}  
  
template<class T>  
int stack<T>::getSize() {  
    return size;  
}  
  
template<class T>  
void stack<T>::push(T val) {  
    if (top == size - 1) {  
        cout << "栈满" << endl;  
        return;  
    }  
    data[++top] = val;  
}  
  
template<class T>  
bool stack<T>::isEmpty() {  
    return top == -1;  
}  
  
template<class T>  
T *stack<T>::pop() {  
    if (isEmpty()) return nullptr;  
    T *temp = &data[top];  
    data[top] = *(new T());  
    top--;  
    return temp;  
}  
  
void test() {  
    stack<int> sta(15);  
    for (int i = 0; i < 10; i++) {  
        sta.push(i);  
    }  
    int* d = sta.getData();  
    int t = sta.getTop();  
    int s = sta.getSize();  
    for (int i = 0; i < s; i++) {  
        cout << d[i] << endl;  
    }  
    cout << "-----------" << endl;  
    for (int i = 0; i < 4; i++) {  
        sta.pop();  
        cout << d[t] << endl;  
    }  
}  
  
int main() {  
    test();  
    return 0;  
}
```