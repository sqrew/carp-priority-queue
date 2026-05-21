# carp-priority-queue

A high-performance, generic Priority Queue (Binary Heap) for the [Carp](https://github.com/carp-lang/Carp) programming language.

## Features

- **Generic**: Works with any type that can be compared.
- **Flexible**: Can be used as a Max-Heap or Min-Heap by providing the appropriate comparison function.
- **Efficient**: $O(\log n)$ insertion and extraction.
- **Zero Allocations in Hot Paths**: Reuses the underlying array.

## Usage

```carp
(load "carp-priority-queue/priority_queue.carp")
(use PriorityQueue)

(defn main []
  (let [pq (PriorityQueue.new)]
    (do
      ;; Max-Heap behavior
      (push &pq 10 &IntRef.>)
      (push &pq 30 &IntRef.>)
      (push &pq 20 &IntRef.>)
      
      (match (pop &pq &IntRef.>)
        (Maybe.Just val) (IO.println &(str val)) ;; 30
        (Maybe.Nothing) (IO.println "Empty")))))
```

### Min-Heap

To use as a Min-Heap, just use the `<` comparison:

```carp
(push &pq 10 &IntRef.<)
(push &pq 30 &IntRef.<)
(pop &pq &IntRef.<) ;; Returns 10
```

## Testing

Run the test suite with:

```bash
carp -x test/pq_test.carp
```

## License

MIT
