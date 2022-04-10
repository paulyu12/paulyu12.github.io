---
layout:     post
title:      "C++ 函数指针练习题"
# subtitle:   ""
date:       2022-04-10
author:     "Paul"
header-img: "img/post-bg-2020-12.jpg"
tags:
    - C++
    - 技术
---
C++ Primer 练习题 6.55

编写 4 个函数，再定义一个元素为函数指针的 vector 容纳这 4 个函数，最后用这个 vector 调用这几个函数

```C++
#include <iostream>
#include <vector>

using namespace std;

int sum(int a, int b) {
	return a + b;
}

int substract(int a, int b) {
	return a - b;
}

int multiply(int a, int b) {
	return a * b;
}

int divide(int a, int b) {
	if (b != 0) {
		return a / b;
	}
	
	return -1;
}

int main() {
	vector<int(*)(int, int)> vec;

	vec.push_back(sum);
	vec.push_back(substract);
	vec.push_back(multiply);
	vec.push_back(divide);

	for (vector<int(*)(int, int)>::iterator it = vec.begin(); it != vec.end(); it++) {
		int a = 10, b = 5;
		cout << (*it)(a, b) << endl;
	}

	return 0;
}
```