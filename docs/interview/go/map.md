### map
标准库中的 map 类型并不是并发安全的。这意味着如果你有多个 goroutine 同时读写同一个 map，可能会导致数据竞争（data race）和未定义行为。为了在并发环境中安全地使用 map，Go 提供了 sync.Map 类型。


标准 map 的并发安全性非并发安全：
    标准 map 在多 goroutine 环境下不保证线程安全。如果需要在并发环境中使用 map，必须自己实现同步机制，例如使用 sync.Mutex 或 sync.RWMutex 来保护 map 的访问。

sync.Map
sync.Map 是 Go 1.9 引入的一个并发安全的 map 实现。它提供了以下特性：
    并发安全：
        可以在多个 goroutine 中同时进行读写操作而不会发生数据竞争。

    高效读取：
        对于只读操作，sync.Map 不需要加锁，因此读取效率较高。
        
    动态扩容：内部会根据需要自动调整容量。



sync.Map 的底层实现
sync.Map 的底层实现比较复杂，但可以简要概括如下：

    结构体定义：
        sync.Map 内部包含两个主要部分：一个用于读取的 read 结构和一个用于写入的 dirty 结构。
        
    read 结构：
        read 结构主要用于读取操作，它包含一个指针数组 m 和一个计数器 count。
        m 数组存储键值对。
        count 记录当前 read 结构中的条目数量。
        如果 count 为负数，表示正在进行写入操作或 read 结构已经被废弃。
        
    dirty 结构：
        dirty 结构用于写入操作，它包含一个带锁的 map 和一个删除集合 misses。
        map 存储实际的键值对。
        misses 用于记录那些在 read 结构中被删除但在 dirty 结构中还未删除的键。
        
    读取操作：
        读取操作首先尝试从 read 结构中获取数据。
        如果 read 结构中的 count 为负数或没有找到键，则会切换到 dirty 结构进行查找。
        
    写入操作：
        写入操作总是通过 dirty 结构进行。
        如果 dirty 结构不存在，则创建一个新的 dirty 结构，并将 read 结构标记为废弃。
        写入完成后，如果满足某些条件（如 dirty 结构中的条目数量达到一定阈值），会将 dirty 结构的内容复制到新的 read 结构中，以提高后续读取操作的效率。