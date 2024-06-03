import java.util.ArrayList;
import java.util.NoSuchElementException;

public class FixedChildrenHeap {
    private ArrayList<Integer> heap;
    private int numberOfChildren;

    public FixedChildrenHeap(int numberOfChildren) {
        if (numberOfChildren < 2) {
            throw new IllegalArgumentException("A parent node must have at least 2 children.");
        }
        this.numberOfChildren = numberOfChildren;
        this.heap = new ArrayList<>();
    }

    public void insert(int value) {
        heap.add(value);
        heapifyUp(heap.size() - 1);
    }

    public int popMax() {
        if (heap.isEmpty()) {
            throw new NoSuchElementException("Heap is empty.");
        }
        int max = heap.get(0);
        int lastValue = heap.remove(heap.size() - 1);
        if (!heap.isEmpty()) {
            heap.set(0, lastValue);
            heapifyDown(0);
        }
        return max;
    }

    private void heapifyUp(int index) {
        int newValue = heap.get(index);
        while (index > 0) {
            int parentIndex = (index - 1) / numberOfChildren;
            int parentValue = heap.get(parentIndex);
            if (newValue <= parentValue) {
                break;
            }
            heap.set(index, parentValue);
            index = parentIndex;
        }
        heap.set(index, newValue);
    }

    private void heapifyDown(int index) {
        int size = heap.size();
        int value = heap.get(index);

        while (true) {
            int maxChildIndex = -1;
            int maxChildValue = Integer.MIN_VALUE;

            for (int i = 1; i <= numberOfChildren; i++) {
                int childIndex = numberOfChildren * index + i;
                if (childIndex < size) {
                    int childValue = heap.get(childIndex);
                    if (childValue > maxChildValue) {
                        maxChildIndex = childIndex;
                        maxChildValue = childValue;
                    }
                }
            }

            if (maxChildIndex == -1 || maxChildValue <= value) {
                break;
            }

            heap.set(index, maxChildValue);
            index = maxChildIndex;
        }

        heap.set(index, value);
    }

    public static void main(String[] args) {
        FixedChildrenHeap heap = new FixedChildrenHeap(3); // Example with each parent having 3 children
        heap.insert(10);
        heap.insert(4);
        heap.insert(15);
        heap.insert(20);
        heap.insert(0);
        heap.insert(25);

        System.out.println("Max: " + heap.popMax()); // Should print 25
        System.out.println("Max: " + heap.popMax()); // Should print 20
    }
}
