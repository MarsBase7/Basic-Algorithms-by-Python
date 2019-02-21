# 基础算法Python示例
Code Segments of Basic Algorithms by Python2.7

<br/>

### 二分查找
```
'''
仅当列表有序时才可用，操作数为O(log n)
'''
def binary_search(list, item):
    low = 0                #将要查找的区间最小index
    high = len(list) -1    #将要查找的区间最大index
    
    while low <= high:
        mid = (low + high) / 2    #只检查中间的元素
        guess = list[mid]
        
        if guess == item:         #找到相应元素
            return mid
         
        if guess > item:          #猜的数字大了
            high = mid - 1
        else:
            low = mid + 1         #猜的数字小了
            
    return None            #没有找到指定的元素
```

<br/>

### 选择排序
```
'''
选择排序相当于n次遍历，操作数为O(n^2)
'''
def findSmallest(arr):     #该函数找到一次遍历中的最小元素index
    smallest = arr[0]
    smallest_index = 0
    for i in range(1, len(arr)):
        if arr[i] < smallest:
            smallest = arr[i]
            smallest_index = i
    return smallest_index

def selectionSort(arr):    #对数组进行排序
    newArr = []
    for i in range(len(arr)):
        smallest = findSmallest(arr)        #使用上一个函数返回index
        newArr.append(arr.pop(smallest))    #从原数组中踢出该元素并加入排序数组
    return newArr
```

<br/>

### 递归
```
'''
递归必须要有基线条件和递归条件，本段以阶乘为例。
调用栈可能很长，内存容易溢出。可考虑尾递归，但Python不支持。
'''
def fact(x):
    if x == 1:                  #基线条件（不再调用）
        return 1
    else:                       #递归条件（调用栈）
        return x * fact(x-1)

'''
其他的例子
例1: 以递归方式写sum函数
'''
def sum(list):
    if list == []:
        return 0
    else:
        return list[0] + list[1:]
        
'''
例2: 以递归方式计算列表包含的元素数
'''
def count(list):
    if list == []:
        return 0
    else:
        return 1 + count(list[1:])
      
'''
例3: 以递归方式找出列表中最大的数字
'''
def max(list):
    if len(list) == 2:
        return list[0] if list[0] > list[1] else list[1]
    else:
        return list[0] if list[0] > max([1:]) else max([1:])
```

<br/>

### 快速排序
```
'''
快速排序的操作数与基准值选择有关（平均为O(nlogn)，最差为O(n^2)），尽量随机选择基准值。
由于一般情况下快速排序的单位操作时间比合并排序快很多，因此即使合并排序的操作数始终为O(nlogn)，快速排序还是被更多地使用。
'''
import random
def quicksort(array):
    if len(array) < 2:                             #基线条件：为空或只包含一个元素的数组，已是有序的
        return array
    else:
        index = random.randint(0, len(array)-1)    #递归条件，随机选择基准值
        pivot = array[index]
        
        less = [i for i in array[:index]+array[index+1:] if i <= pivot]      #所有小于基准值的元素数组  
        greater = [i for i in array[:index]+array[index+1:] if i > pivot]    #所有大于基准值的元素数组
        
        return quicksort(less) + [pivot] + quicksort(greater)
```

<br/>

### 散列表
```
'''
Python中的散列表实现为字典 dict()
本例将散列表用作网站缓存
'''
cache = {}
def get_page(url):
    if cache.get(url):
        return cache[url]        #返回缓存的数据
    else:
        data = get_data_from_server(url)
        cache[url] = data        #先将数据保存到缓存中
        return data
```

<br/>

### 广度优先搜索
```
'''
BFS算法主要应用于无加权图，以解决：1.是否存在A到B的路径；2.如果有路径则最短路径（段数最少）是什么
操作数为O(V+E)，V为图的顶点数，E为图的边数
本例将寻找某人的人际网中关系最近的医生（朋友，比朋友的朋友，关系近）
'''
from collections import deque
def search(name):
    search_queue = deque()         #使用队列，先进先出，按顺序检查，否则找到的不一定是最短路径          
    search_queue += graph[name]    #队列从指定的某人开始，graph{}为已知的人际网的散列表
    searched = ()                  #此tuple用来记录算法检查过的人（元组的检索速度比数组快）
    
    while search_queue:
        person = search_queue.popleft()
        if person not in searched:               #确认是否被检查过
            if person_is_doctor(person):         #通过另一个函数判断是否为医生
                print person + " is a doctor!"
                return True
            else:
                search_queue += graph[person]    #将下一层关系加入队列
                searched += (person,)            #被标记为已检查过
    return False
```

<br/>

### 狄克斯特拉算法
```
'''
狄克斯特拉(Dijkstra)算法关键理念：找出图中最便宜的节点，并确保没有到该节点的更便宜的路径
用于在加权图中查找最短路径，仅当权重为正时狄克斯特拉算法才管用
如果图中包含负权边，需要使用贝尔曼-福德(Bellman ford)算法
'''
#创建三个散列表：#

#GRAPH表，记录上面的整个图的关系
graph = {}
graph['start'] = {'a': 6, 'b': 2}
graph['a'] = {'fin': 1}
graph['b'] = {'a': 3, 'fin': 5}
graph['fin'] = {}

#COSTS表，记录每个节点从起点出发的已知开销（后续更新），未知的用inf代替
costs = {'a': 6, 'b': 2, 'fin': float('inf')}

#PARENTS表，记录每个节点的父节点（后续更新），最后可在该表找到最短路径
parents = {'a': 'start', 'b': 'start', 'fin': None}

processed = ()    #记录已经处理的节点，之后不用再处理

def find_lowest_cost_node(costs):
    lowest_cost = float('inf')
    lowest_cost_node = None
    for node in costs:
        cost = costs[node]
        if cost < lowest_cost and node not in processed:    #更低的开销、没有被处理过
            lowest_cost = cost
            lowest_cost_node = node
    return lowest_cost_node

node = find_lowest_cost_node(costs)    #在未处理的节点中找出开销最小的点

while node is not None:                #循环在所有节点都被处理过后结束
    cost = costs[node]
    neighbors = graph[node]
    for n in neighbors.keys():         #遍历当前节点的所有邻居
        new_cost = cost + neighbors[n]
        if  new_cost < costs[n]:       #如果从起点经过当前节点到该邻居的开销更小
            costs[n] = new_cost        #则更新该邻居的开销
            parents[n] = node          #并更新该邻居的父节点为当前节点
    processed += (node,)               #遍历之后该节点标记为已处理过
    node = find_lowest_cost_node(costs)
```
