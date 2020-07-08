Lớp
-------

Lớp là thành phần lõi của Python. Lớp đem lại nhiều sức mạnh nhưng khiến ta rơi vào tình thế lạm dụng chúng.
Trong phần này tôi sẽ chia sẻ một số mẹo và các khuyến cáo liên quan tới ``lớp`` trong Python. Nào hãy bắt đầu

1. Các biến hiện thực (instance) và Lớp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Hầu hết người mới bắt đầu lập trình và thậm chí là một số lập trình viên Python chuyên không hiểu
sự khác biệt giữa các biến hiện thực và lớp. Sự thiếu sót này khiến họ dùng không đúng các kiểu biến. Hãy cùng tìm hiểu các biến này  

The basic difference is:
Sự khác biệt căn bản giữa hai biến này là:

- Các biến hiện thực là dữ liệu đơn nhất cho mọi đối tượng
- Các biến lớp là dữ liệu được chia sẻ giữa các hiện thực khác nhau của một lớp
Cùng lấy một ví dụ

.. code:: python

    class Cal(object):
        # pi là một biến lớp
        pi = 3.142

        def __init__(self, radius):
            # self.radius là một biến hiện thực
            self.radius = radius

        def area(self):
            return self.pi * (self.radius ** 2)

    a = Cal(32)
    a.area()
    # Kết quả: 3217.408
    a.pi
    # Kết quả: 3.142
    a.pi = 43
    a.pi
    # Kết quả: 43

    b = Cal(44)
    b.area()
    # Kết quả: 6082.912
    b.pi
    # Kết quả: 3.142
    b.pi = 50
    b.pi
    # Kết quả: 50

There are not many issues while using immutable class variables. This is
the major reason due to which beginners do not try to learn more about
this subject because everything works! If you also believe that instance
and class variables can not cause any problem if used incorrectly then
check the next example.

Ta không gặp nhiều vấn đề khi sử dụng các biến lớp có giá trị không thay đổi (immutable). 
Lý do chính là khiến cho người mới bắt đầu không tìm hiểu về vấn đề này là do mọi thứ chạy bình thường!
Nếu bạn vẫn còn tin rằng các biến hiện thực và các biến lớp không thể gây ra bất cứ vấn đề khi bạn dùng chúng
sai thì hãy xem ví dụ dưới đây

.. code:: python

    class SuperClass(object):
        superpowers = []

        def __init__(self, name):
            self.name = name

        def add_superpower(self, power):
            self.superpowers.append(power)

    foo = SuperClass('foo')
    bar = SuperClass('bar')
    foo.name
    # Kết quả: 'foo'

    bar.name
    # Kết quả: 'bar'

    foo.add_superpower('fly')
    bar.superpowers
    # Kết quả: ['fly']

    foo.superpowers
    # Kết quả: ['fly']

Ví dụ trên điển hình cho việc sử dụng các biến lớp có thể thay đổi giá trị (mutable). 
Để làm cho đoạn mã của bạn an toàn trong những tình huống như trên, hãy chắc rằng
bạn không dùng các biến lớp có giá trị có thể thay đổi. Nếu bạn vẫn muốn dùng thì hãy chắc
rằng bạn biết rõ mình đang làm gì.

2. Các lớp kiểu mới
^^^^^^^^^^^^^^^^^^^^


Các lớp kiểu mới được giới thiệu trong Python 2.1 nhưng rất nhiều người 
thậm chí không biết về nó! Lý do là vì Python vẫn hỗ trợ các lớp kiểu cũ nhằm 
duy trì tương thích ngược (backward compatibility). Tôi đã nhắc tới điều này nhiều lần nhưng
tôi chưa nói sự khác biệt. Sự khác biệt chính như sau:

- Các lớp cũ không thừa hưởng từ bất cứ thứ gì
- Các lớp kiểu mới thừa hưởng từ ``object``
Nhìn vào ví dụ dưới đây:

.. code:: python

    class OldClass():
        def __init__(self):
            print('I am an old class')

    class NewClass(object):
        def __init__(self):
            print('I am a jazzy new class')

    old = OldClass()
    # Kết quả: I am an old class

    new = NewClass()
    # Kết quả: I am a jazzy new class


Việc thừa hưởng từ ``object`` cho phép các lớp kiểu mới tận dụng một vài **magic** (ma thuật).
Lợi ích chính là bạn có thể thực hiện một vài tối ưu hữu dụng như ``__slots__``. Bạn có thể 
sử dụng ``super()`` và các mô tả (descriptors). Vậy nên hãy cố gắng dùng các lớp kiểu mới.

**Chú ý:** Python 3 chỉ các các lớp kiểu mới. Do đó không quan trọng bạn phân lớp (subclass) từ ``object``
hay không, bạn vẫn đang dùng các lớp kiểu mới. Tuy nhiên bạn vẫn được khuyến khích phân lớp từ ``object``.
3. Magic Methods
^^^^^^^^^^^^^^^^

Python's classes are famous for their magic methods, commonly called
**dunder** (double underscore) methods. I am going to discuss a few of
them.

-  ``__init__``

It is a class initializer. Whenever an instance of a class is created
its ``__init__`` method is called. For example:

.. code:: python

    class GetTest(object):
        def __init__(self):
            print('Greetings!!')
        def another_method(self):
            print('I am another method which is not'
                  ' automatically called')

    a = GetTest()
    # Output: Greetings!!

    a.another_method()
    # Output: I am another method which is not automatically
    # called

You can see that ``__init__`` is called immediately after an instance is
created. You can also pass arguments to the class during its
initialization. Like this:

.. code:: python

    class GetTest(object):
        def __init__(self, name):
            print('Greetings!! {0}'.format(name))
        def another_method(self):
            print('I am another method which is not'
                  ' automatically called')

    a = GetTest('yasoob')
    # Output: Greetings!! yasoob

    # Try creating an instance without the name arguments
    b = GetTest()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: __init__() takes exactly 2 arguments (1 given)

I am sure that now you understand the ``__init__`` method.

-  ``__getitem__``

Implementing **getitem** in a class allows its instances to use the []
(indexer) operator. Here is an example:

.. code:: python

    class GetTest(object):
        def __init__(self):
            self.info = {
                'name':'Yasoob',
                'country':'Pakistan',
                'number':12345812
            }

        def __getitem__(self,i):
            return self.info[i]

    foo = GetTest()

    foo['name']
    # Output: 'Yasoob'

    foo['number']
    # Output: 12345812

Without the ``__getitem__`` method we would have got this error:

.. code:: python

    >>> foo['name']

    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'GetTest' object has no attribute '__getitem__'

.. Static, Class & Abstract methods
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

