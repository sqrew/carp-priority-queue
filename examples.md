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

## Implementing Dijkstra's Algorithm (Conceptual)

Priority Queues are the backbone of many graph algorithms.

```carp
(defn dijkstra [start-node]
  (let [pq (PriorityQueue.new)]
    (do
      (push &pq (NodeWeight.init start-node 0) &NodeWeight.less-than)
      (while (not (empty? &pq))
        (let [current (Maybe.unsafe-from (pop &pq &NodeWeight.less-than))]
          ;; Explore neighbors...
          )))))
```
