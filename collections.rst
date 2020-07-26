Collections
-----------


Python cung cấp một module chứa đựng các kiểu dữ liệu bộ chứa được gọi là Collections. 
Chúng ta cùng nhau tìm hiểu và thảo luận về nó.

Dưới đây là các kiểu dữ liệu chúng ta sẽ thảo luận
-  ``defaultdict``
-  ``OrderedDict``
-  ``Counter``
-  ``deque``
-  ``namedtuple``
-  ``enum.Enum`` (dành cho Python 3.4+)

``defaultdict``
^^^^^^^^^^^^^^^^^^^

Không giống như kiểu ``dict`` bình thường, với ``defaultdict`` bạn không cần phải kiểm tra xem một khoá có tồn tại hay không.
Do đó ta có thể làm:

.. code:: python

    from collections import defaultdict

    colours = (
        ('Yasoob', 'Yellow'),
        ('Ali', 'Blue'),
        ('Arham', 'Green'),
        ('Ali', 'Black'),
        ('Yasoob', 'Red'),
        ('Ahmed', 'Silver'),
    )

    favourite_colours = defaultdict(list)

    for name, colour in colours:
        favourite_colours[name].append(colour)

    print(favourite_colours)

    # output
    # defaultdict(<type 'list'>,
    #    {'Arham': ['Green'],
    #     'Yasoob': ['Yellow', 'Red'],
    #     'Ahmed': ['Silver'],
    #     'Ali': ['Blue', 'Black']
    # })


Một trong những những điểm cần lưu ý đó là khi bạn thêm một phần tử vào trong một từ điển. Nếu khoá không có sẵn trước thì bạn sẽ gặp phải lỗi ``KeyError``. Với ``defaultdict``, bạn có thể giải quyết được ấn đề này. Hãy cùng xem các ví dụ dưới đây để hiểu thêm
**Vấn đề:**

.. code:: python

    some_dict = {}
    some_dict['colours']['favourite'] = "yellow"
    # Raises KeyError: 'colours'

**Lời giải:**

.. code:: python

    from collections import defaultdict
    tree = lambda: defaultdict(tree)
    some_dict = tree()
    some_dict['colours']['favourite'] = "yellow"
    # Code chạy bình thường


Bạn có thể hiển thị `some_dict`` sử dụng ``json.dumps`. Ví dụ

.. code:: python

    import json
    print(json.dumps(some_dict))
    # Kết quả: {"colours": {"favourite": "yellow"}}

``OrderedDict``
^^^^^^^^^^^^^^^^^^^

``OrderedDict`` duy trì thứ tự các phần tử được chèn vào. Ghi đè lên một giá trị của một khoá đã tồn tại không làm thay đổi vị trí của khoá đó. Tuy nhiên, việc xoá và chèn lại một phần tử di chuyển khoá tới cuối của từ điển.


**Vấn đề:**

.. code:: python

    colours =  {"Red" : 198, "Green" : 170, "Blue" : 160}
    for key, value in colours.items():
        print(key, value)
    #  quả:
    #   Green 170
    #   Blue 160
    #   Red 198
    #  Các phần tử được in ra theo thứ tự ngẫu nhiên
   
**Giải pháp:**

.. code:: python

    from collections import OrderedDict
    
    colours = OrderedDict([("Red", 198), ("Green", 170), ("Blue", 160)])
    for key, value in colours.items():
        print(key, value)
    # Kết quả:
    #   Red 198
    #   Green 170
    #   Blue 160
    # Thứ tự phần tử được chèn vào được duy trì

``Counter``
^^^^^^^^^^^^^^^


Counter cho phép ta đếm số lần xuất hiện của một phần tử. Ví dụ nó có thể được sử dụng để đếm số lượng các màu sắc riêng biệt.

.. code:: python

    from collections import Counter

    colours = (
        ('Yasoob', 'Yellow'),
        ('Ali', 'Blue'),
        ('Arham', 'Green'),
        ('Ali', 'Black'),
        ('Yasoob', 'Red'),
        ('Ahmed', 'Silver'),
    )

    favs = Counter(name for name, colour in colours)
    print(favs)
    # Kết quả: Counter({
    #    'Yasoob': 2,
    #    'Ali': 2,
    #    'Arham': 1,
    #    'Ahmed': 1
    # })

Chúng ta cũng có thể đếm những dòng phổ biến nhất trong một file. Ví dụ
.. code:: python

    with open('filename', 'rb') as f:
        line_count = Counter(f)
    print(line_count)

``deque``
^^^^^^^^^^^^^


``deque`` cung cấp một hàng đợi với hai đầu mở, có nghĩa là bạn có thể nối và xoá các phần tử từ cả hai đầu của hàng đầu. 
.. code:: python

    from collections import deque
Bạn có thể khởi tạo một đối tượng deque
.. code:: python

    d = deque()

``deque`` hoạt động giống như các lists trong Python, ví dụ bạn có thể làm:
.. code:: python

    d = deque()
    d.append('1')
    d.append('2')
    d.append('3')

    print(len(d))
    # Kết quả: 3

    print(d[0])
    # Kết quả: '1'

    print(d[-1])
    # Kết quả: '3'

Bạn có thể loại các giá trị ra khởi deque từ cả hai đầu
.. code:: python

    d = deque(range(5))
    print(len(d))
    # Kết quả: 5

    d.popleft()
    # Kết quả: 0

    d.pop()
    # Kết quả: 4

    print(d)
    # Kết qur: deque([1, 2, 3])


Ta cũng có thể giới hạn số lượng các phần tử mà một deque có thể chứa. Khi số lượng phần tử của một deque đạt dgiới hạn nó sẽ loại ra các phần tử ở đầu ngược lại. Hãy xem ví dụ sau:

.. code:: python

    d = deque([0, 1, 2, 3, 5], maxlen=5)
    print(d)
    # Kết quả: deque([0, 1, 2, 3, 5], maxlen=5)
    
    d.extend([6])
    print(d)
    #Kết quả: deque([1, 2, 3, 5, 6], maxlen=5)

Now whenever you insert values after 5, the leftmost value will be
popped from the list. You can also expand the list in any direction with
new values:
Bây giờ thì khi bạn chèn các giá trị đằng sau số 5, giá trị phía bên trái sẽ bị loại khỏi danh sách. Bạn có thể mở rộng danh sách ở bất cứ hướng nào với các giá trị mới.
.. code:: python

    d = deque([1,2,3,4,5])
    d.extendleft([0])
    d.extend([6,7,8])
    print(d)
    # Kết quả: deque([0, 1, 2, 3, 4, 5, 6, 7, 8])

``namedtuple``
^^^^^^^^^^^^^^^^^^


Chắc bạn đã biết về tuples. Nó là một danh sách không thay đổi được giá trị, và cho phép bạn lưu chuỗi các giá trị được phân cách bởi dấu phẩy. Khấc với lists, **Bạn không thể gán lại giá trị của một phần tử trong một tuple**. Để truy cập vào một giá trị trong một tuple bạn sử dụng các chỉ số nguyên như sau:

.. code:: python

    man = ('Ali', 30)
    print(man[0])
    # Kết quả: Ali


Vậy thì ``namedtuples`` là gì? Đây là biến thể của tuples dành cho các tác vụ đơn giản.
Với namedtuples bạn không phải sử dụng các chỉ số nguyên để truy cập các phần tử của một tuple. Bạn có thể hình dung namedtuples như là các từ điển, nhưng giá trị thì không thể thay đổi được.

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="perry", age=31, type="cat")

    print(perry)
    # Kết quả: Animal(name='perry', age=31, type='cat')

    print(perry.name)
    # Kết quả: 'perry'


Bạn có thể truy cập vào các phần tử của một tuple sử dụng dấu ``.``. Cùng tìm hiểu sâu hơn. Một tuple được đặt tên có các tham số yêu cầu. Các tham số này là tên tuple và các tên trường của tuple. Trong ví dụ phía trên, tuple của chúng ta có tên là 'Animal và các trường có tên là 'name', 'age' và 'type'. Namedtuple giúp cho các tuples dễ hiểu hơn khi nhìn vào code. Ngoài ra vì bạn không nhất thiết phải dùng các chỉ số nguyên để truy cập tuple, giúp cho code dễ bảo trì hơn. Hơn nữa, các tuples nhẹ và xài ít bộ nhớ hơn tuples bình thường. `namedtuple` nhanh hơn các từ điển. Tuy nhiên, nhớ rằng **các thuộc tính trong `namedtuple` là không thể thay đổi**. Có nghĩa là đoạn mã dưới đây sẽ không hoạt động:

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="perry", age=31, type="cat")
    perry.age = 42

    #Kết quả: Traceback (most recent call last):
    #            File "", line 1, in
    #         AttributeError: can't set attribute


Bạn nên sử dụng named tuples để làm cho code dễ hiểu hơn. **named tuples có tương thích ngược với các tuples bình thường**. Nghĩa là bạn có thể sử dụng các chỉ số nguyên với các namedtuples:
.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="perry", age=31, type="cat")
    print(perry[0])
    # Output: perry


Không kém phần quan trọng, bạn có thể chuyển đổi một namedtuple sang một từ điển. Như sau:
.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="Perry", age=31, type="cat")
    print(perry._asdict())
    # Kết quả: OrderedDict([('name', 'Perry'), ('age', 31), ...

``enum.Enum`` (Python 3.4+)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Một kiểu dữ liệu quan trọng khác là enum. Đối tượng này tích hợp trong module ``enum`` từ Python 3.4 trở đi (và có một backport trong PyPI tên là ``enum34``. ).
Enums (`enumerated type <https://en.wikipedia.org/wiki/Enumerated_type>`_) là một cách để tổ chức những thứ khác nhau.


Cùng xem ví dụ namedtuple Animal. Đối tượng này có một trường là ``type``. Vấn đề ở đây là, type là một chuỗi văn bản. Điều này gây ra một vài vấn đề. Điều gì nếu người dùng gõ ``Cat`` bởi vì họ gõ cả phím Shift? Hay là ``CAT``?  hoặc ``kitten``?


Enumerations có thể giúp ta giải quyết vấn đề này, bằng cách không dùng strings. Nhìn vào ví dụ sau:

.. code:: python

    from collections import namedtuple
    from enum import Enum

    class Species(Enum):
        cat = 1
        dog = 2
        horse = 3
        aardvark = 4
        butterfly = 5
        owl = 6
        platypus = 7
        dragon = 8
        unicorn = 9
        # The list goes on and on...

        # But we don't really care about age, so we can use an alias.
        kitten = 1
        puppy = 2

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="Perry", age=31, type=Species.cat)
    drogon = Animal(name="Drogon", age=4, type=Species.dragon)
    tom = Animal(name="Tom", age=75, type=Species.cat)
    charlie = Animal(name="Charlie", age=2, type=Species.kitten)

    # And now, some tests.
    >>> charlie.type == tom.type
    True
    >>> charlie.type
    <Species.cat: 1>

Cách làm trên tránh lỗi sai ít nhất. Chúng ta phải khai báo thật cụ thể, và chỉ nên sử dụng enumeration cho các kiểu tên.

Có ba cách để truy cập các phần tử enumeration. Ví dụ, tất cả ba phương thức trên sẽ cho bạn giá trị của ``cat``:

.. code:: python

    Species(1)
    Species['cat']
    Species.cat

Bài viết trên giới thiệu cho bạn module ``collections``. Chắc chắn rằng bạn đọc tài liệu gốc của module sau khi đọc bài viết này.
