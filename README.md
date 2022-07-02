# Алгоритмы и структуры данных

## Алгоритмы

### Кратко об алгоритме

![image](https://user-images.githubusercontent.com/61269026/176997502-498985b1-78ae-4290-abc9-b8ce04e2ce40.png)

Представить можно в виде графиков:

![image](https://user-images.githubusercontent.com/61269026/176997534-b6ad7d12-c1c3-43fb-a186-ab1916579df4.png)

_Линейный_ поиск за линейное время.
_Бинарный_ - за логарифмическое.

> Бинарный алгоритм работает только с _отсортированным_ списком

### Линейный поиск

Просто проходимся по массиву в поиске нужного элемента.

```
const array = [1,4,5,8,5,1,2,7,5,2,11]
let count = 0
function linearSearch(array, item) {
    for (let i = 0; i < array.length; i++) {
        count += 1
        if (array[i] === item) {
            return i;
        }
    }
    return null
}

console.log(linearSearch(array, 1))
console.log('count = ', count)
```

Его сложность: __O(n)__.

### Бинарный поиск

Находим элемент, который расположен посредине и сравнивем с соседями. Если элемент больше, чем предыдущий, значит левую часть массива откидываем, а правую вновь делим пополам. И так до тех пор, пока не найдется нужный элемент.

```
const array = [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
let count = 0

function binarySearch(array, item) {
    let start = 0
    let end = array.length
    let middle;
    let found = false
    let position = -1
    while (found === false && start <= end) {
        count+=1
        middle = Math.floor((start + end) / 2);
        if (array[middle] === item) {
            found = true
            position = middle
            return position;
        }
        if (item < array[middle]) {
            end = middle - 1
        } else {
            start = middle + 1
        }
    }
    return position;
}

```

С помощью рекурсии (_рекурсия_ - функция, которая вызывает сама себя. Рекурсия всегда должна иметь условие, при котором вызов функции прекращается. Иначе будет переполнение стека вызова):

```
function recursiveBinarySearch(array, item, start, end) {
    let middle = Math.floor((start + end) / 2);
    count += 1
    if (item === array[middle]) {
        return middle
    }
    if (item < array[middle]) {
        return recursiveBinarySearch(array, item, 0, middle - 1 )
    } else {
        return recursiveBinarySearch(array, item, middle + 1, end )
    }
}

console.log(recursiveBinarySearch(array, 0, 0, array.length))
console.log(count)
```

Его сложность: __O(log2n)__.

### Сортировка алгоритмом выбора

Есть массив неупорядоченных элементов. В цикле находим минимальный и меняем его с первым элементом. Затем снова проходимся по массиву и находим минимальное значение, но меняем уже со вторым элементом. Далее с третим, четвертым и так далее, пока всё не отсортируется.

```
const arr = [0,3,2,5,6,8,1,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,6,2,35,6,3,32] // [0,1,1,2,3.......]
let count = 0

function selectionSort(array) {
    for (let i = 0; i < array.length; i++) {
        let indexMin = i
        for (let j = i+1; j < array.length; j++) {
            if (array[j] < array[indexMin]) {
                indexMin = j
            }
            count += 1
        }
        let tmp = array[i]
        array[i] = array[indexMin]
        array[indexMin] = tmp
    }
    return array
}

console.log(selectionSort(arr))
console.log(arr.length) // O(n*n)
console.log('count = ', count)
```

Его сложность: __O(n<sup>2</sup>)__.

> Если точно, то __O(1/2(n<sup>2</sup>)__), но почему-то дробь не учитыввается тут..

### Пузырьковая сортировка

Принцип:

![image](https://user-images.githubusercontent.com/61269026/176998096-ed48a184-7f60-447a-a3ca-d89e95954a15.png)

Также в двойном цикле пробегаемся по всему массиву и сравниваем попарно лежащие элементы. Если следующий элемент массива меньше предыдущего, то меняем их местами. Получается своего рода всплытие. Самый большой элемент после каждой итерации всплывает наверх.

```
const arr = [0,3,2,5,6,8,23,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,6,2,35,6,3,32,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,]
let count = 0

function bubbleSort(array) {
    for (let i = 0; i < array.length; i++) {
        for (let j = 0; j < array.length; j++) {
            if (array[j + 1] < array[j]) {
                let tmp = array[j]
                array[j] = array[j+1]
                array[j+1] = tmp
            }
            count+=1
        }
    }
    return array
}

console.log('length', arr.length)
console.log(bubbleSort(arr)) // O(n*n)
console.log('count = ', count)
```

Его сложность: __O(n<sup>2</sup>)__.

### Быстрая сортировка. Сортировка Хоара

> Разделяй и властвуй.

![image](https://user-images.githubusercontent.com/61269026/176998707-ce31105b-9325-44bc-9fb9-f7b67d1b7d21.png)

Принцип: делим массив на подмассивы. И каждый раз рекурсивно. Выбираем опорный элемент у каждого массива (можно выбрать случайно, но чаще всего берут с серединки). Все элементы, которые мень ше опорного, добавляем в один массив, а все, что больше - в другой. После такой операции получем два массива. Для каждого из них проводится такая же операция до тех пор, пока длина массива не станет равна 1. Это условие будет базовым случаем выхода из рекурсии. В итоге маленькие массивы склеиваются в один.

```
const arr = [0,3,2,5,6,8,23,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,6,2,35,6,3,32,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,9,4,2,1,2,9,6,4,1,7,-1, -5, 23,]
let count = 0

function quickSort(array) {
    if (array.length <= 1) {
        return array
    }
    let pivotIndex = Math.floor(array.length / 2);
    let pivot = array[pivotIndex]
    let less = []
    let greater = []
    for (let i = 0; i < array.length; i++) {
        count += 1
        if(i === pivotIndex)
            continue
        if (array[i] < pivot) {
            less.push(array[i])
        } else {
            greater.push(array[i])
        }
    }
    return [...quickSort(less), pivot, ...quickSort(greater)]
}

console.log(quickSort(arr))
console.log('count', count)

```

Его сложность: __O(log<sub>2</sub>n*n)__

### Графы. Поиск в ширину

Для начала немного о графе:

Граф - некоторая абстрактная структура данных, представляющая собой множество вершин и набор рёбер.

Рёбра:

- однонаправленые
- двунаправленые

![image](https://user-images.githubusercontent.com/61269026/176999365-fdc1ce60-f850-4c7d-901c-991af001d91c.png)

Суть задачи: найти, существует ли путь из точки A в точку G.

```
// Поиск в ширину в графе

const graph = {}
graph.a = ['b', 'c']
graph.b = ['f']
graph.c = ['d', 'e']
graph.d = ['f']
graph.e = ['f']
graph.f = ['g']

function breadthSearch(graph, start, end) {
    let queue = []
    queue.push(start)
    while (queue.length > 0) {
        const current = queue.shift()
        if (!graph[current]) {
            graph[current] = []
        }
        if (graph[current].includes(end)) {
            return true
        } else {
            queue = [...queue, ...graph[current]]
        }
    }
    return false
}

console.log(breadthSearch(graph, 'a', 'e'))// Поиск в ширину в графе

const graph = {}
graph.a = ['b', 'c']
graph.b = ['f']
graph.c = ['d', 'e']
graph.d = ['f']
graph.e = ['f']
graph.f = ['g']

function breadthSearch(graph, start, end) {
    let queue = []
    queue.push(start)
    while (queue.length > 0) {
        const current = queue.shift()
        if (!graph[current]) {
            graph[current] = []
        }
        if (graph[current].includes(end)) {
            return true
        } else {
            queue = [...queue, ...graph[current]]
        }
    }
    return false
}

console.log(breadthSearch(graph, 'a', 'g'))
```

### Матрица смежности

Граф в виде матрицы смежности:

![image](https://user-images.githubusercontent.com/61269026/177002507-25a59c33-97bc-4ad2-a363-c8e81df3ef4a.png)

### Алгоритм Дейкстры для поиска кратчайшего пути

Тут уже учитывается длина пройденного ребра - вес.

![image](https://user-images.githubusercontent.com/61269026/177002603-3fee14d1-9c54-48c0-a5de-3140d24a257b.png)

Кратко о логике алгоритма:
Стартовая точка - A, конечная - G. Создадим таблицу, где на первом этапе записываются значения тех вершин, в которые мы можем попасть из стартовой точки. Остальные вершины недостижимы.

<table>
	<tr >
	    <th rowspan="6">A</th>
	    <td>B</td>
	    <td>2</td>
	</tr>
	<tr>
	    <td>C</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td>D</td>
	    <td>∞</td>
	</tr>
	<tr><td>E</td>
	    <td>∞</td>
	</tr>
	<tr>
	    <td>F</td>
	    <td>∞</td>
	</tr>
	<tr>
	    <td>G</td>
	    <td>∞</td>
	</tr>
</table>

На втором этапе отмечаем вершины как уже рассмотренные.

<table>
	<tr >
	    <th rowspan="6">A</th>
	    <td>B</td>
	    <td>2</td>
	</tr>
	<tr>
	    <td>C</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td>D</td>
	    <td>6</td>
	</tr>
	<tr><td>E</td>
	    <td>3</td>
	</tr>
	<tr>
	    <td>F</td>
	    <td>9</td>
	</tr>
	<tr>
	    <td>G</td>
	    <td>∞</td>
	</tr>
</table>

На третьем этапе рассматриваем вершины, в которые можно попасть из точек B и C (точки как уже рассмотренные). В таблице значения от А до В и С.

<table>
	<tr >
	    <th rowspan="6">A</th>
	    <td>B</td>
	    <td>2</td>
	</tr>
	<tr>
	    <td>C</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td>D</td>
	    <td>6</td>
	</tr>
	<tr><td>E</td>
	    <td>3</td>
	</tr>
	<tr>
	    <td>F</td>
	    <td>9</td>
	</tr>
	<tr>
	    <td>G</td>
	    <td>10</td>
	</tr>
</table>

На следующем этапе достигаем точки G, но происходит перерасчет. Находим путь до F, который оказывается короче.

<table>
	<tr >
	    <th rowspan="6">A</th>
	    <td>B</td>
	    <td>2</td>
	</tr>
	<tr>
	    <td>C</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td>D</td>
	    <td>6</td>
	</tr>
	<tr><td>E</td>
	    <td>3</td>
	</tr>
	<tr>
	    <td>F</td>
	    <td>4</td>
	</tr>
	<tr>
	    <td>G</td>
	    <td>5</td>
	</tr>
</table>

![image](https://user-images.githubusercontent.com/61269026/177003256-6cd809cd-7a8d-4318-add4-7fc5e2e89241.png)

```
// Поиск кратчайшего пути в графе

const graph = {}
graph.a = {b: 2, c: 1}
graph.b = {f: 7}
graph.c = {d: 5, e: 2}
graph.d = {f: 2}
graph.e = {f: 1}
graph.f = {g: 1}
graph.g = {}

function shortPath(graph, start, end) {
    const costs = {}
    const processed = []
    let neighbors = {}
    Object.keys(graph).forEach(node => {
        if (node !== start) {
            let value = graph[start][node]
            costs[node] = value || 100000000
        }
    })
    let node = findNodeLowestCost(costs, processed)
    while (node) {
        const cost = costs[node]
        neighbors = graph[node]
        Object.keys(neighbors).forEach(neighbor => {
            let newCost = cost + neighbors[neighbor]
            if (newCost < costs[neighbor]) {
                costs[neighbor] = newCost
            }
        })
        processed.push(node)
        node = findNodeLowestCost(costs, processed)
    }
    return costs
}


function findNodeLowestCost(costs, processed) {
    let lowestCost = 100000000
    let lowestNode;
    Object.keys(costs).forEach(node => {
        let cost = costs[node]
        if (cost < lowestCost && !processed.includes(node)) {
            lowestCost = cost
            lowestNode = node
        }
    })
    return lowestNode
}

console.log(shortPath(graph, 'a', 'g'));
```

### Рекурсивный обход дерева n-размерности

Деревья - рекурсивная структура данных, где каждый узел - также дерево, но для данного дерева каждый узел - поддерево.

![image](https://user-images.githubusercontent.com/61269026/177003659-c874358f-a527-4740-9d08-bf2849bbe2c0.png)

```
const tree = [
    {
        v: 5,
        c: [
            {
                v:10,
                c: [
                    {
                        v:11,
                    }
                ]
            },
            {
                v:7,
                c: [
                    {
                        v:5,
                        c: [
                            {
                                v:1
                            }
                        ]
                    }
                ]
            }
        ]
    },
    {
        v: 5,
        c: [
            {
                v:10
            },
            {
                v:15
            }
        ]
    }
]

const recursive = (tree) => {
    let sum = 0;
    tree.forEach(node => {
        sum += node.v
        if(!node.c) {
            return node.v
        }
        sum += recursive(node.c)
    })
    return sum
}

console.log(recursive(tree))
```

### Итеративный обход дерева n-размерности

```
const iteration = (tree) => {
    if (!tree.length) {
        return 0
    }
    let sum = 0
    let stack = []
    tree.forEach(node => stack.push(node));
    while (stack.length) {
        const node = stack.pop()
        sum += node.v
        if (node.c) {
            node.c.forEach(child => stack.push(child))
        }
    }
    return sum
}

console.log(iteration(tree))
```

### Кеширование вычислений

```
function cashFunction(fn) {
    const cash = {}
    return function (n) {
        if (cash[n]) {
            console.log('Взято из кеша', cash[n])
            return cash[n]
        }
        let result = fn(n)
        console.log('Посчитала функция = ', result)
        cash[n] = result
        return result;
    };
}

function factorial(n) {
    let result = 1
    while (n != 1) {
        result *= n
        n -= 1
    }
    return result
}

const cashFactorial = cashFunction(factorial)

cashFactorial(5)
cashFactorial(4)
cashFactorial(3)
cashFactorial(4)
cashFactorial(5)
cashFactorial(1)
```

## Структуры данных

### Очередь

![image](https://user-images.githubusercontent.com/61269026/176999441-0201d765-d7e4-4d3f-b436-39fa6c924550.png) 

__FIFO - FIRST IN FIRST OUT__

### Стек

![image](https://user-images.githubusercontent.com/61269026/177003864-9c05c6ee-8077-4477-bcf5-3c8716d54d7a.png)

__LIFO - LAST IN FIRST OUT__

### Массив

Массив - последовательный набор каких-то объектов. Занимает конкретный участок в памяти. Изначально определено, сколько элементов в нём будет находиться.

![image](https://user-images.githubusercontent.com/61269026/177004307-f9f53e0c-9c32-4398-ade7-a5208149e6ca.png)

__O(1)__ - Получить элемент
__O(n)__ - Добавить элемент / удалить
__O(n)__ - Поиск

### Связный список

Тут уже каждый отдельно взятый элемент списка занимает отдельное место в памяти. Связанность списка происходит за счёт того, что каждый предыдущий элемент хранит ссылку на следующий элемент, который лежит списке.

![image](https://user-images.githubusercontent.com/61269026/177004337-627f97da-d03b-40f7-bd02-7ed7a13bcb8c.png)

__O(1)__ - вставка / удаление в начало / конец /  если знаем место
__O(n)__ - вставка в произвольное место, если не знаем место
__O(n)__ - Поиск

```
class LinkedList {
    constructor() {
        this.size = 0
        this.root = null
    }

    add(value) {
        if (this.size === 0) {
            this.root = new Node(value);
            this.size += 1;
            return true;
        }
        let node = this.root
        while (node.next) {
            node = node.next
        }
        let newNode = new Node(value)
        node.next = newNode
        this.size += 1
    }

    getSize() {
        return this.size
    }

    print() {
        let result = []
        let node = this.root
        while (node) {
            result.push(node.value)
            node = node.next
        }
        console.log(result);
    }
}

class Node {
    constructor(value) {
        this.value = value
        this.next = null
    }
}

const list = new LinkedList()
list.add(5)
list.add(3)
list.add(2)
list.add(5)
list.add(7)

list.print()
```

### Бинарное дерево поиска

Бинарное дерево - структура данных, где каждый узел также является деревом (структура рекурсивна). Причем у каждого узла может быть только два потомка. Добавляются узлы тож особым способом: если добавляемое в дерево значение меньше по значению, чем текущий угол, то значение уходит в левое поддерево, если больше - в правое. Правая часть поддерева выстраивается с большими значениями, левая - с меньшими.

![image](https://user-images.githubusercontent.com/61269026/177004653-8619059d-f504-4537-b2d3-b342a098dd8b.png)

__O(log2n)__ - Вставка / удаление
__O(log2n)__ - Поиск

```
class BinaryTree {
    constructor() {
        this.root = null
    }

    add(value) {
        if (!this.root) {
            this.root = new TreeNode(value)
        } else {
            let node = this.root
            let newNode = new TreeNode(value)
            while (node) {
                if (value > node.value) {
                    if (!node.right) {
                        break
                    }
                    node = node.right
                } else {
                    if (!node.left) {
                        break
                    }
                    node = node.left
                }
            }
            if (value > node.value) {
                node.right = newNode
            } else {
                node.left = newNode
            }
        }
    }

    print(root = this.root) {
        if (!root) {
            return true;
        }
        console.log(root.value);
        this.print(root.left)
        this.print(root.right)
    }
}

class TreeNode {
    constructor(value) {
        this.value = value
        this.left = null
        this.right = null
    }
}

const tree = new BinaryTree()
tree.add(5)
tree.add(2)
tree.add(6)
tree.add(2)
tree.add(1)
tree.print()
```

### Словарь, карта, map

Хранит в себе пары ключ-значение, где значение получем по ключу. Обычный объект в JS - уже является map. За константное время можем добавлять элементы в структуру и извлекать их оттуда.

```
const map = new Map()
const objKey = {id:5}
map.set(objKey, "ulbi tv")

console.log(map.get(objKey));
```

### Set, множество

Хранит только уникальные значения. Как ключ можно хранить не только строковые значения, но и объекты.

```
const set = new Set()

set.add(5)
set.add(5)
set.add(5)
set.add(5)
set.add(5)
set.add(4)
set.add(3)
console.log(set)
```