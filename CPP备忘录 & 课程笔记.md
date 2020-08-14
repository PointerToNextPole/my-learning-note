# CPP备忘录 & 课程笔记



**命名法**

- 匈牙利命名法
- 驼峰命名法
- Pascal命名法



**关键字**

- alignas (since C++11)
- alignof (since C++11)
- and
- and_eq
- asm
- atomic_cancel (TM TS)
- atomic_commit (TM TS)
- atomic_noexcept (TM TS)
- auto(1)
- bitand
- bitor
- bool
- break
- case
- catch
- char
- char8_t (since C++20)
- char16_t (since C++11)
- char32_t (since C++11)
- class(1)
- compl
- concept (since C++20)
- const
- consteval (since C++20)
- constexpr (since C++11)
- constinit (since C++20)
- const_cast
- continue
- co_await (since C++20)
- co_return (since C++20)
- co_yield (since C++20)
- decltype (since C++11)
- default(1)
- delete(1)
- do
- double
- dynamic_cast
- else
- enum
- explicit
- export(1)(3)
- extern(1)
- false
- float
- for
- friend
- goto
- if
- inline(1)
- int
- long
- mutable(1)
- namespace
- new
- noexcept (since C++11)
- not
- not_eq
- nullptr (since C++11)
- operator
- or
- or_eq
- private
- protected
- public
- reflexpr (reflection TS)
- register(2)
- reinterpret_cast
- requires (since C++20)
- return
- short
- signed
- sizeof(1)
- static
- static_assert (since C++11)
- static_cast
- struct(1)
- switch
- synchronized (TM TS)
- template
- this
- thread_local (since C++11)
- throw
- true
- try
- typedef
- typeid
- typename
- union
- unsigned
- using(1)
- virtual
- void
- volatile
- wchar_t
- while
- xor
- xor_eq

(1) - meaning changed or new meaning added in C++11.
(2) - meaning changed in C++17.
(3) - meaning changed in C++20.

摘自：[C++官方文档：C++ keywords](https://en.cppreference.com/w/cpp/keyword)



**八进制的写法**

```cpp
// oct(077) == dec(63)
// 所以，输出结果为63
int a = 077;
std::cout << a;
```

**前缀与后缀写法**

```cpp
'a'    // type: char
L'a'   // type: wchar_t

"a"    // type: char[2]
L"a"   // type: wchar_t[2]
U"a"   // type: char32_t[2]

1      // type: int
1U     // type: unsigned int

0.5    // type: double
0.5f   // type: float
0.5L   // type: long double
```

摘自：[StackOverflow -- What exactly is the L prefix in C++?](https://stackoverflow.com/questions/13087219/what-exactly-is-the-l-prefix-in-c)



#### 深拷贝和浅拷贝

```cpp
//todo
```



