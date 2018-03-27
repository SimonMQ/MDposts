#### 使用__slots__
在给某类创建一个实例后，我们可以给实例动态的添加属性。如果想限制实例随意创建属性，可以采用__slots__方法。
```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名
    
s = Student()
s.name = 'Michael' # 没毛病
s.age = 25
s.score = 90 # 报错
```
__slots__只对当前的类起作用，对继承的子类则无效。此时可以在子类中也定义__slots__，那么子类实例可定义的属性包括其自身定义的__slots__和父类定义的__slots__。
#### 使用@property
@property装饰器可以将一个方法变成属性调用。当我们给一个方法func()加上@property时，我们便可以调用s.func属性。另外，@property会自动创建一个@func.setter装饰器，用来给func方法赋值。调用时可用s.func=100来赋值。
```python
class Screen(object):
    @property
    def width(self):
        return self._width

    @width.setter
    def width(self, value):
        self._width = value

    @property
    def height(self):
        return self._height

    @height.setter
    def height(self, value):
        self._height = value

    @property
    def resolution(self):
        return self._width * self._height
```
#### 多重继承
例如，Dog类可以继承自Animal类的Mammal子类，Dog类又同时属于Animal类的Runnable子类，这样逐个定义会很繁琐。我们可以在定义Dog类的时候给它多个继承父类：
``` python
class Dog(Mammal, Runnable):
    pass
```
通过多重继承，一个子类就可以拥有多个父类的功能。
- MixIn
通常一个类的继承关系都是从主线单一继承过来的，如果混入额外的功能可以通过多重继承实现，这种设计称为MinIN。
```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```
#### 定制类
- __str__
- __iter__
- __getitem__
- __getattr__
- __call__
#### 枚举类
#### 元类