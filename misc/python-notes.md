### List function arguements are references

```python
def fun1(x):
  print('id(x) = ', id(x))
  x[0] = 0
  print('x = ', x)

a = [1, 2, 3]
fun1(a)
print('id(a) = ', id(a))
print('a = ', a)

# Output: 
# ('id(x) = ', 140556398544944)
# ('x = ', [0, 2, 3])
# ('id(a) = ', 140556398544944)
# ('a = ', [0, 2, 3])

```

### Tuple is immutable
```python
a = (1, 'a', [2])
a[1] = 'b' # Wrong! TypeError: 'tuple' object does not support item assignment
```

### Default value of function arguement is computed when defining function
```python
def extendList(val, list=[]):
    list.append(val)
    return list

list1 = extendList(10)
list2 = extendList(123,[])
list3 = extendList('a')

print "list1 = %s" % list1 # list1 = [10, 'a']
print "list2 = %s" % list2 # list2 = [123]
print "list3 = %s" % list3 # list3 = [10, 'a']
```
Revising as following can fix this problem:
```
def extendList(val, list=None):
  if list is None:
    list = []
  list.append(val)
  return list
```

### Late binding closures
  Python's closures are late bindings. This means variables used in closures are looked up at the time the inner functions are called. In example below, value of i are looked up after all loops are finished, i is alreayd set to 4. 
```python
def create_multipliers():
    multipliers = []

    for i in range(5):
        def multiplier(x):
            return i * x
        multipliers.append(multiplier)

    return multipliers

for multiplier in create_multipliers():
    print(multiplier(2))
# result is: 
#8
#8
#8
#8
#8
```
  To fix this problem, we can modify definition of inner method by adding a default arg like:
```python
def create_multipliers():
    return [lambda x, i=i : i * x for i in range(5)]
```

### Try-except catch errors
Handle exceptions with different except handlers, or default handler
```python
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except IOError as e:
    print "I/O error({0}): {1}".format(e.errno, e.strerror)
except ValueError:
    print "Could not convert data to an integer."
except:
    print "Unexpected error:", sys.exc_info()[0]
    raise
else:
    print "no exception raised"
finally:
    print "run after all cases"
```

### Operators
  - The ternary operator in python
```python
x, y = 35, 75
smaller = x if x < y else y
print(smaller)
```

  - Exponent operator `**`
```python
print(3**3) # 27
```
  - Floor division operator `//`
```python
print(1//3) # 0
print(9//3) # 3
print(21.0//4) # 5.0
print(21.0/4) # 5.25
```