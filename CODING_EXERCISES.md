## Algorithms
- Dijkstra (Shortest paths between nodes in a graph)
- Rabin-Karp (a string-searching that uses hashing to find an exact match of a pattern string in a text)
- Aho–Corasick (a string-searching that locates elements of a finite set of strings within an input text like "dictionary-matching")

## Arrays

### Given an array of size N in which every number is between 1 and N, determine if there are any duplicates
```ts
// Time complexity: O(n^2) - BAD
function hasDup (list: Array<number>): boolean {
  for (let i = 0; i < list.length; i++) {
    for (let j = i+1; j < list.length; j++) {
      if (list[i] === list[j]) return true
    }
  }
  return false
}
// Time complexity: O(n) - GOOD
function hasDup (list: Array<number>): boolean {
  list.sort()
  for (let i = 0; i < list.length - 1; i++) {
    if (list[i] === list[i+1]) return true
  }
  return false
}
```

### Given an array of words, get a list of these words bucket by anagrams
```sh
Input: ["star", "rats", "car", "arc", "xml"]
Output: [["star", "rats"], ["car", "arc"], ["xml"]]
```
```ts
function getWordsBucketByAnagram(list: Array<string>): Array<Array<string>> {
  const bucket: { [key: string]: Array<string> } = {}
  for (let i = 0; i < list.length; i++) {
    const word = list[i]
    const key = word.split('').sort().join('')
    bucket[key] = [...(bucket[key] || []), word]
  }
  return Object.keys(bucket).map(key => bucket[key]) // Same as Object.values(bucket)
}
console.log(getWordsBucketByAnagram(["star", "rats", "car", "arc", "xml"]))
```

### Given a list of calendar events, determine if any events conflicts
```sh
[
  [1, 2, 'a'],   // start, end, id
  [3, 5, 'b'],   -┐
  [4, 6, 'c'],   -┘
  [7, 10, 'd'],  -┐
  [8, 11, 'e'],   |
  [10, 12, 'f'], -┘
  [13, 14, 'g']
];
```
```ts
// SOLUTION 1: Brute force; Compare every event to every other event, Slow => O(n^2) complexity
// SOLUTION 2: If the list is sorted, you can just compare the previous event, O(n) complexity
function getConflicts (events: Array<Array<any>>) {
  return events.reduce(function (result, event, index) {
    const previousEvent = events[index-1]
    if (previousEvent && event[0] < previousEvent[1]) {
      result.push(event[2])
    }
    return result
  }, [])
}
// SOLUTION 3: Merge conflicts of the sorted list of events
function findConflicts (events) {
  let conflicts = []
  let temp = [events[0][2]]
  let end = events[0][1]
  for (let i = 1; i < events.length; i++) {
    if (events[i][0] >= end) { // No conflict :)
      if (temp.length > 1) {
        conflicts = conflicts.concat(temp)
      }
      temp = []
    }
    end = Math.max(events[i][1], end)
    temp.push(events[i][2])
  }
  if (temp.length > 1) {
    conflicts = conflicts.concat(temp)
  }
  return conflicts
}
```

### Given a table for N people, figure out every possible seating of the table for all of your friends
```sh
# SOLUTION: Power set
                         start
        leave              |             take
           |‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾|
           -                              1
    |‾‾‾‾‾‾‾‾‾‾‾‾‾|                |‾‾‾‾‾‾‾‾‾‾‾‾‾|
    --           -2               1-            12
    |             |                |             |
 |‾‾‾‾‾|       |‾‾‾‾‾|          |‾‾‾‾‾|       |‾‾‾‾‾|
---   --3     -2-   -23        1--   1-3     12-   123
                     |               |       |
                   -234             1-34    12-4  
```
```ts
/**
 * Power set
 * Time complexity: O(2^n)
 */ 
function findDinnerParties(friends: Array<string>, tableSize: number) {
  function combineFriends(
    friends: Array<string>,
    tableSize: number,
    groups: Array<Array<string>> = [],
    group: Array<string> = [],
    pos = 0
  ) {
    if (group.length === tableSize) groups.push(group)
    else if (pos < friends.length) {
      combineFriends(friends, tableSize, groups, group, pos + 1)
      
      const newGroup = [...group]
      newGroup.push(friends[pos])

      combineFriends(friends, tableSize, groups, newGroup, pos + 1)
    }
    return groups
  }

  return combineFriends(friends, tableSize)
}
findDinnerParties(['Sara', 'Juan', 'Valentina', 'Julian', 'Laura'], 3)
```

## Sorting

### Merge Sort
```ts
function mergeSort(arr: Array<number>): Array<number> {
  if(arr.length <= 1) return arr;

  const mid = Math.ceil(arr.length / 2);
  const left = mergeSort(arr.splice(0, mid));
  const right = mergeSort(arr.splice(-mid));

  const result = [];
  while(left.length && right.length){
    if(left[0] > right[0]) {
      result.push(right.shift());
    } else {
      result.push(left.shift());
    }
  }
  return [
    ...result,
    ...left,
    ...right
  ];
}
```

### Quick Sort
```ts
function quickSort(arr: Array<number>): Array<number> {
  if (arr.length < 2) {
    return arr;
  }
  const pivot = arr[Math.floor(Math.random() * arr.length)];

  const left = [];
  const right = [];
  const equal = [];

  for (let val of arr) {
    if (val < pivot) {
      left.push(val);
    } else if (val > pivot) {
      right.push(val);
    } else {
      equal.push(val);
    }
  }
  return [
    ...quickSort(left),
    ...equal,
    ...quickSort(right)
  ];
}
```

## Binary trees
```ts
class TreeNode {
  public left!: TreeNode
  public right!: TreeNode
  constructor (
    public value: number
  ) { }
}
```

### Perform in order traversal on a binary tree

```ts
function inOrder (node: TreeNode): Array<number> {
  if (!node) return []
  return [
    ...inOrder(node.left),
    node.value,
    ...inOrder(node.right)
  ]
}
function addNode (node: TreeNode, value: number) {
  if(!node) return new TreeNode(value)
  if(value >= node.value) {
    node.right = addNode(node.right, value)
  } else {
    node.left = addNode(node.left, value)
  }
  return node
}
function setNodes (node: TreeNode, values: Array<number>) {
  values.forEach(value => addNode(node, value))
  return node
}
const tree = new TreeNode(5)
setNodes(tree, [8, 4, 3, 1])
console.log(inOrder(tree)) // Output: [1, 3, 4, 5, 8]
```

### Given a binary tree, get the average value at each level of the tree
```sh
Input:
     4
    / \
   7   9
  / \   \
 10  2   6
      \
       6
      /
     2
Output: [4, 8, 6, 6, 2]
```
```ts
type Level = {
  total: number
  count: number
}

function collect (node: TreeNode, levels: Array<Level>, levelIndex = 0) {
  if(!node) return
  const level = levels[levelIndex]
  if (!level) {
    levels[levelIndex] = { total: node.value, count: 1 }
  } else {
    level.total += node.value
    level.count += 1
  }
  levelIndex++
  collect(node.left, levels, levelIndex)
  collect(node.right, levels, levelIndex)
}
function getAvgByLevel(node: TreeNode) {
  const levels: Array<Level> = []
  collect(node, levels)
  return levels.map(({ total, count }) => total / count)
}
const tree = new TreeNode(4);
tree.left = new TreeNode(7);
tree.left.left = new TreeNode(10);
tree.left.right = new TreeNode(2);
tree.left.right.right = new TreeNode(6);
tree.left.right.right.left = new TreeNode(2);
tree.right = new TreeNode(9);
tree.right.right = new TreeNode(6);
console.log(getAvgByLevel(tree))
```

## Graphs
```ts
// Using Adjacency Matrix
const graph =
  [[1, 1, 0, 0, 1, 0],
   [1, 0, 1, 0, 1, 0],
   [0, 1, 0, 1, 0, 0],
   [0, 0, 1, 0, 1, 1],
   [1, 1, 0, 1, 0, 0],
   [0, 0, 0, 1, 0, 0]];
```

### Returns whether there's a path between two nodes in a graph
```ts
/**
 * DFS is easy to implement recursively because you can use the call stack as the stack.
 * Time complexity: O(|V|^2).
 */
function hasPath(
  graph: Array<Array<number>>,
  current: number,
  goal: number
): boolean {
  const stack: Array<number> = [];
  const visited: Array<boolean> = [];
  stack.push(current);
  visited[current] = true;
  while (stack.length > 0) {
    const node = stack.pop();
    if (node === goal) {
      return true;
    }
    for (let i = 0; i < graph[node].length; i += 1) {
      if (graph[node][i] && !visited[i]) {
        stack.push(i);
        visited[i] = true;
      }
    }
  }
  return false;
}
console.log(hasPath(graph, 1, 5)) // Output: true
```

### Returns the shortest path between startNode and targetNode
```ts
/**
 * BFS visits the neighbor vertices before visiting the child vertices, and a queue is used in the search process.
 * Time complexity: O(|V|^2).
 */
function shortestPath (
  graph: Array<Array<number>>,
  startNode: number,
  targetNode: number
): Array<number> | null {
  const parents = [];
  const queue = [];
  const visited = [];
  queue.push(startNode);
  parents[startNode] = null;
  visited[startNode] = true;
  while (queue.length > 0) {
    const current = queue.shift();
    if (current === targetNode) {
      return buildPath(parents, targetNode);
    }
    for (var i = 0; i < graph.length; i += 1) {
      if (i !== current && graph[current][i] && !visited[i]) {
        parents[i] = current;
        visited[i] = true;
        queue.push(i);
      }
    }
  }
  return null;
}
function buildPath (
  parents: Array<number>,
  targetNode: number
): Array<number> {
  const result = [targetNode];
  while (parents[targetNode]) {
    targetNode = parents[targetNode];
    result.push(targetNode);
  }
  return result.reverse();
}
console.log(shortestPath(graph, 1, 5)) // Output: [1, 2, 3, 5]
```

### Given a graph represented as an adjacency lists and a source vertex, get an array of objects describing each vertex with the distance from the source and the vertex's predecessor
```ts
type Vertex = {
  distance: number,
  predecessor: number | null
}
function getVertexDistanceAndPredecessor (
  graph: Array<Array<number>>,
  source: number
): Array<Vertex> {
  const bfsInfo: Array<Vertex> = [];
  bfsInfo[source] = {
    distance: 0,
    predecessor: null
  };
  const queue: Array<number> = [];
  queue.push(source);
  
  while (queue.length > 0) {
    const current = queue.shift() as number;
    for (let i = 0; i < graph[current].length; i++) {
      const v = graph[current][i];
      if (!bfsInfo[v]) {
        bfsInfo[v] = {
          predecessor: current,
          distance: bfsInfo[current].distance + 1
        };
        queue.push(v);
      }
    }
  }
  return bfsInfo;
}
```

## Search

### Binary Search
```ts
function binarySearch(arr: Array<number>, goal: number): number {
    let low = arr[0]
    let high = arr[arr.length - 1]
    while (low < high) {
        const mid = Math.floor(low + (high-low)/2)
        if (arr[mid] > goal) {
            high = mid
        } else {
            low = mid + 1
        }
    }
    return high
}
```

## Optimization

### Given a sorted array, insert a new element keeping the order
```ts
function insertSorted(arr: Array<number>, num: number) {
    let low = 0
    let high = arr.length
    while (low < high) {
        const mid = Math.floor(low + (high-low)/2)
        if (arr[mid] - num > 0) {
            high = mid
        } else {
            low = mid + 1
        }
    }
    return [
       ...arr.slice(0, low),
       num,
       ...arr.slice(low)
    ]
}
```

## Real Life problems

### Duplicated Transactions
Sometimes when a customer is charged, there is a duplicate transaction created. We need to find those transactions so that they can be dealt with. Everything about the transaction should be identical, except the transaction id and the time at which it occurred, as there can be up to a minute delay.

A transaction is looks like:
```json
{
  "id": 123,
  "sourceAccount": "my_account",
  "targetAccount": "coffee_shop",
  "amount": -30,
  "category": "eating_out",
  "time": "2018-03-12T12:34:00Z"
}
```

```js
// Input: list of transactions (Transaction[])
findDuplicateTransactions(transactions)
// Output: list of all the duplicate transaction groups, ordered by time ascending (Transaction[][])
```

Find all transactions that have the same sourceAccount, targetAccount, category, amount, and the time difference between each consecutive transaction is less than 1 minute.
```js
Array.prototype.flatMap = function(lambda) { 
  return [].concat.apply([], this.map(lambda)) 
}
function diffMinutes(dt2, dt1) 
{
  const diff = (dt2.getTime() - dt1.getTime()) / 1000 / 60
  return Math.abs(diff)
}
// Sort with FP (Inmutability, Pure function)
function sortFP (
  transactions,
   timeProp = 'time'
) {
  return [...transactions].sort(
    (a, b) => new Date(a[timeProp]) - new Date(b[timeProp])
  )
}

// return your result from this function
function findDuplicateTransactions (transactions = []) {
  transactions = sortFP(transactions)
  /**
   * key: sourceAccount__targetAccount__category__amount
   * groups: Array<Object>]
   *   firstTime: for sorting groups  
   *   lastTime: last transaction time
   *   transactions: []
   */
  const map = {}
  transactions.forEach((tr) => {
    const key = [
      tr.sourceAccount,
      tr.targetAccount,
      tr.category,
      tr.amount
    ].join('__')
    const time = new Date(tr.time)
    const transactionMap = map[key] = map[key] || {
      key,
      groups: []
    }
    // Find the group with identical transactions
    const group = transactionMap.groups.find(
      g => diffMinutes(g.lastTime, time) <= 1
    )
    if (group) {
      group.lastTime = time
      group.transactions.push(tr)
    } else {
      transactionMap.groups.push({
        firstTime: time,
        lastTime: time,
        transactions: [tr]
      })
    }
  })
  const groups = Object.values(map).flatMap(m => m.groups)
  const result = sortFP(
    groups,
    'firstTime'
  ).reduce((duplicatedGroups, group) => {
    // Include groups with duplicated transactions
    if (group.transactions.length > 1) {
      duplicatedGroups.push(
        group.transactions
      )
    }
    return duplicatedGroups
  }, [])
  
  return result
}
```

## Credits:
- [Jackson Gabbard’s Youtube channel](https://www.youtube.com/channel/UCcdCkJKXlRoXVD03eo-q8mQ)
- [Power set](https://en.wikipedia.org/wiki/Power_set)
- [The HackerRank Interview Preparation Kit](https://www.hackerrank.com/interview/interview-preparation-kit)
- [Learn X in Y minutes](https://learnxinyminutes.com)
- [Design Patterns for Humans](https://github.com/kamranahmedse/design-patterns-for-humans)
- [50+ Data Structure and Algorithms Problems from Coding Interviews](https://dev.to/javinpaul/50-data-structure-and-algorithms-problems-from-coding-interviews-4lh2)

### Frontend
- [JavaScript Algorithms and Data Structures](https://github.com/trekhleb/javascript-algorithms)
- [JavaScript implementations of different famous Computer Science algorithms](https://mgechev.github.io/javascript-algorithms)
- [Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- [The Modern JavaScript Tutorial](https://github.com/javascript-tutorial/en.javascript.info)
- [JavaScript Garden](http://bonsaiden.github.io/JavaScript-Garden)
- [Eloquent JavaScript](https://eloquentjavascript.net)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Understanding ECMAScript 6](https://leanpub.com/understandinges6/read)
- [You Don't Know JS Yet](https://github.com/getify/You-Dont-Know-JS)
- [Functional-Light JavaScript](https://github.com/getify/Functional-Light-JS)
- [33 JS Concepts](https://github.com/adonismendozaperez/33-js-conceptos)
