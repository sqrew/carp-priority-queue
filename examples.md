# Examples

## Custom Structs

You can use the `PriorityQueue` with custom types by providing a comparison function.

```carp
(deftype Task [priority Int, name String])

(defmodule Task
  (defn compare [a b]
    (Int.> @(Task.priority a) @(Task.priority b)))
)

(defn main []
  (let [pq (PriorityQueue.new)]
    (do
      (push &pq (Task.init 1 @"low-priority") &Task.compare)
      (push &pq (Task.init 10 @"high-priority") &Task.compare)
      
      (match (pop &pq &Task.compare)
        (Maybe.Just t) (IO.println (Task.name &t)) ;; high-priority
        (Maybe.Nothing) ()))))
```

## Building from an Array (Heapify)

If you already have a collection of data, `from-array` is the fastest way to turn it into a heap ($O(n)$).

```carp
(let [items [10 50 20 40 30]
      pq (from-array items &IntRef.>)]
  (peek &pq)) ;; Returns (Maybe.Just 50)
```

## Top-K Selection

A common use for `push-pop` is maintaining a fixed-size heap of the "top K" elements.

```carp
(let [pq (PriorityQueue.new)
      limit 3]
  (do
    (foreach [x [10 50 20 40 30]]
      (if (< (length &pq) limit)
        (push &pq @x &IntRef.<)
        (ignore (push-pop &pq @x &IntRef.<))))
    ;; pq now contains the 3 largest elements: 30, 40, 50
    ))
```
