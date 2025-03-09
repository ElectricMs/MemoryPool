系统的动态分配函数new malloc free delete

stl相关

mutex

size_t



申请一块较大的内存块，之后将这块内存的管理放在应用层进行，减小系统调用带来的开销

- 减小动态内存分配的开销
- 避免内存碎片
- 降低系统调用频率
- 拥有稳定的分配时间

适用场合：游戏开发中大量小对象、高性能计算、web服务器和数据库服务器处理大量连接和请求

缺点：需要预先分配较大的内存区域，可能浪费一些内存；对大对象的分配并不划算



### std::allocator

`allocator`是标准库提供的一个通用工具，用于管理内存分配和释放。它是STL（标准模板库）容器如`vector`、`list`等背后进行内存管理的机制。通过自定义分配器，开发者可以控制容器如何获取和释放内存，这对于优化性能或使用特定类型的内存资源非常有用。

```cpp
#include <iostream>
#include <memory> // std::allocator位于该头文件中

int main() {
    std::allocator<int> alloc; // 创建一个用于分配int类型的allocator对象
    int* p = alloc.allocate(5); // 分配能存储5个int的空间

    // 构造这些int对象
    for (size_t i = 0; i < 5; ++i) {
        alloc.construct(&p[i], i); // 初始化每个元素
    }

    // 使用这些int对象...
    for (size_t i = 0; i < 5; ++i) {
        std::cout << p[i] << " ";
    }
    std::cout << std::endl;

    // 销毁这些int对象
    for (size_t i = 0; i < 5; ++i) {
        alloc.destroy(&p[i]); // 销毁每个元素
    }

    alloc.deallocate(p, 5); // 释放分配的内存

    return 0;
}
```

### new

- **`new`**：分配内存并构造对象，是最常用的动态内存分配方式。
- **`operator new`**：只负责分配内存，不进行构造。可以被重载以提供定制化的内存分配策略。
- **`placement new`**：在已分配的内存中构造对象，适用于需要精细控制内存分配的情况，如内存池实现。

```cpp
#include <new> // 必须包含这个头文件来使用placement new

char buffer[sizeof(MyClass)]; // 预先分配的内存
MyClass* obj = new(buffer) MyClass(); // 在buffer中构造对象
```

理解和正确使用这三个概念可以帮助更高效地管理内存，避免常见的内存泄漏和其他与内存管理相关的问题。



### 自旋锁

一种轻量级的锁机制，用于实现线程同步。当一个线程尝试获取锁时，如果锁已经被其他线程占用，它不会进入阻塞状态，而是不断尝试获取锁，直到成功，这种“忙等”行为就叫作“自旋”。

在`fetchRange`函数代码中的部分：

```cpp
while (locks_[index].test_and_set(std::memory_order_acquire));
```

- `test_and_set()`是原子操作，它将locks_[index]设置为true，并返回锁的原始状态。
- 如果返回值表示锁已经被占用（true），线程会继续尝试获取锁（自旋）。
- 这种方式避免了线程切换的开销。

| 自旋锁                         | 互斥锁                   |
| ------------------------------ | ------------------------ |
| 忙等，反复尝试获取锁           | 阻塞，线程会被挂起       |
| 适合锁持有时间很短的场景       | 适合锁持有时间较长的场景 |
| 无需操作系统上下文切换，性能高 | 需要上下文切换，开销较大 |



### MemoryPool-v1 项目框架