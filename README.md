# .lthread-safe.Bounded.Buffer
Data structure that can store a fixed number of elements. Once the buffer is full, any additional elements added to the buffer will block until there is space available. Similarly, if the buffer is empty and a consumer tries to remove an element, the consumer will block until an element is added to the buffer.
