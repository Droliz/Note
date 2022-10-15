# C++队列

简单实现C++队列

```cpp
#include "iostream"  
using namespace std;  
  
template<class T>  
struct node {  
    T val;  
    node* next;  
};  
  
template<class T>  
class queue {  
public:  
    queue();  
    node<T>* getHead();  
    node<T>* getEnd();  
    bool isEmpty();  
    void push(T val);  
    node<T>* pop();  
    int getSize();  
private:  
    int size;  
    node<T> *head;  
    node<T> *end;  
};  
  
template<class T>  
queue<T>::queue() {  
    size = 0;  
    head = new node<T>();  
    head->next = nullptr;  
    end = new node<T>();  
    end->next = nullptr;  
}  
  
template<class T>  
node<T>* queue<T>::getHead() {  
    return head;  
}  
  
template<class T>  
node<T>* queue<T>::getEnd() {  
    return end;  
}  
  
template<class T>  
void queue<T>::push(T val) {  
    auto temp = new node<T>();  
    temp->val = val;  
    temp->next = nullptr;  
    if (isEmpty()) {  
        head = end = temp;  
        size++;  
        return;  
    }  
    end->next = temp;  
    end = temp;  
    size++;  
}  
  
template<class T>  
node<T>* queue<T>::pop() {  
    if (isEmpty()) return head;  
    if (!head->next) {  
        auto temp = end;  
        temp->next = nullptr;  
        head = end = new node<T>();  
        head->next = nullptr;  
        end->next = nullptr;  
        size--;  
        return temp;  
    }  
    node<T>* temp = head->next;  
    head->next = head->next->next;  
    size--;  
    return temp;  
}  
  
template<class T>  
bool queue<T>::isEmpty() {  
    return !size;  
}  
  
template<class T>  
int queue<T>::getSize() {  
    return size;  
}  
  
void test() {  
  
    queue<int> que;  
    for (int i = 0; i < 10; i++) {  
        que.push(i);  
    }  
    cout << "1、------------" << endl;  
    int size = que.getSize();  
    cout << size << endl;  
    for (int i = 0; i < size; i++) {  
        node<int> *temp = que.pop();  
        if (temp) {  
            cout << temp->val << "、";  
        }  
    }  
    cout << endl << "2、--------------" << endl;  
    cout << que.getSize() << endl;  
    cout << que.getHead()->val << endl;  
    cout << que.getEnd()->val << endl;  
}  
  
int main() {  
    test();  
    return 0;  
}
```