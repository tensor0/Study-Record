

cin:
首先想一下自己输入的时候，是敲完一个想要输入的值然后回车，再接着敲下一个想要输入的值。这里的回车并不会被省略，也会被cin读到，但是cin遇到它就会停止读入。
```
1. cin 遇到空格，TAB，回车会停止
2. cin 读取空格，TAB，回车会忽略
```
https://cloud.tencent.com/developer/article/1518621
https://blog.csdn.net/qq_43142218/article/details/116197237

1. 字符串排序
    描述：
    链接：https://ac.nowcoder.com/acm/contest/5657/H
    来源：牛客网

    输入描述:
    输入有两行，第一行n

    第二行是n个字符串，字符串之间用空格隔开
    输出描述:
    输出一行排序后的字符串，空格隔开，无结尾空格
    ```
    输入:
    5
    c d a bb e

    输出：
    a bb c d e
    ```
    ```cpp
    #include <iostream>
    using namespace std;
    int main()
    {
        int n;
        cin>>n;
        string str[n];//因为要比较大小，但是要先把输入都存起来才能比较，所以用字符串数组存储输入
        for(int i=0; i<n; i++)//先用字符串数组把字符串都存起来
        {
            cin>>str[i];
        }
        for(int i=0;i<n;i++)//对输入的字符串进行比较
        {
            for(int j=i;j<n;j++)
            {
                if(str[j]<str[i])
                {
                    swap(str[j],str[i]);
                }
            }
        }
        for(int i=0; i<n; i++)//按照格式输出
        {
            cout<<str[i]<<' ';
        }
    }
    ```
2. 