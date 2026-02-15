## bool变量
true为非0，false为0  

## const常量  
const int Max=150//之后改变不了  

## string类  
|    定义     |  拼接   |                 输入                  |                          处理                           |
| :---------: | :-----: | :-----------------------------------: | :-----------------------------------------------------: |
| string s="" | s=s1+s2 | cin>>s<br>getline(cin,s) 可以输入一行 | s.lengeh()<br> s1=s.substr(n,m)<br>&&<br>s1=s.substr(n) |

## 引用&  
C++ 中 & 可以定义引用变量（别名），这是 C 没有的核心特性。引用本质是变量的 “只读别名”  
~~~// 该代码在 C 中编译报错（C 不认识引用语法），C++ 中正常运行
#include <iostream>
using namespace std;

// 形参是引用（int &b 等价于 变量a的别名）
void modify(int &b) {
    b = 200;  
 // 直接修改引用，等价于修改原变量a
}

int main() {
    int a = 20;
    int &ref = a; // 定义ref为a的引用（别名）
    cout << "a的值：" << a << endl;   // 输出 20
    cout << "ref的值：" << ref << endl; // 输出 20（和a共享内存）
    
    ref = 100; // 修改ref，a也会变
    cout << "修改ref后a的值：" << a << endl; // 输出 100
    
    modify(a); // 传递a（引用传参，无需取地址）
    cout << "函数修改后a的值：" << a << endl; // 输出 200
    return 0;
}  
~~~
# <center>STL</center>
## vector