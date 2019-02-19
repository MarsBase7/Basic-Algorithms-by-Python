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

