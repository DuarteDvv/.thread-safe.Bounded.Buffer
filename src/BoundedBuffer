import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BoundedBuffer {
    private final int capacity;
    private final Object[] items = new Object[capacity];
    private int putIndex = 0;
    private int takeIndex = 0;
    private int count = 0;
    private final Lock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();

    public BoundedBuffer(int capacity) {
        this.capacity = capacity;
    }

   public void put(Object item) throws InterruptedException {
    lock.lock();
    try {
        // Enquanto o buffer estiver cheio, bloqueamos a thread e esperamos até que ele não esteja mais cheio
        while (count == capacity) {
            notFull.await();
        }

        // Inserimos o elemento no buffer e atualizamos os índices e a contagem de elementos
        items[putIndex] = item;
        putIndex = (putIndex + 1) % capacity;
        count++;

        // Sinalizamos a Condição de bloqueio notEmpty, indicando que agora há pelo menos um elemento no buffer
        notEmpty.signal();
    } finally {
        lock.unlock();
    }
}
    public Object take() throws InterruptedException {
    lock.lock();
    try {
        // Enquanto o buffer estiver vazio, bloqueamos a thread e esperamos até que ele não esteja mais vazio
        while (count == 0) {
            notEmpty.await();
        }

        // Removemos o elemento do buffer e atualizamos os índices e a contagem de elementos
        Object item = items[takeIndex];
        takeIndex = (takeIndex + 1) % capacity;
        count--;

        // Sinalizamos a Condição de bloqueio notFull, indicando que agora o buffer não está mais cheio
        notFull.signal();
        return item;
    } finally {
        lock.unlock();
    }
  }
}
