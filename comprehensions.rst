Comprehensions
--------------

Comprehensions là một tính năng của Python bạn có thể cần chú ý. Comprehensions là các đối tượng xây dựng cho phép các tạo ra các chuỗi (sequences) từ các chuỗi khác. Một vài kiểu comprehensions được hỗ trợ trong cả Python 3 và Python 3:

-  list comprehensions
-  dictionary comprehensions
-  set comprehensions
-  generator comprehensions


Chúng ta sẽ thảo luận từng kiểu comprehensions một. Mỗi khi bạn quen với ``list``
comprehensions bạn có thể sử dụng các comprehension khác một cách dễ dàng.

``list`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^^


List comprehensions cung cấp một cách ngắn gọn để tạo ra các lists. List comprehension bao gồm các dấu ngặc vuông bao gồm một biểu diễn, sau đó là một câu ``for``, theo sau bởi 0 hay nhiều câu  ``for`` hoặc ``if``. Các biểu diễn này có thể là bất cứ thứ gì, nghĩa là bạn thể đặt tất cả các kiểu đối tượng trong các danh sách. Kết quả sẽ là một danh sách mới được tạo ra sau khi thực hiện biểu diễn với các câu lệnh  ``if`` và ``for``.

**Blueprint**

.. code:: python

    variable = [out_exp for out_exp in input_list if out_exp == 2]

Dưới đây là một ví dụ:

.. code:: python

    multiples = [i for i in range(30) if i % 3 == 0]
    print(multiples)
    # Output: [0, 3, 6, 9, 12, 15, 18, 21, 24, 27]


Đây là một cách rất hữu dụng để tạo ra các lists nhanh chóng. Nó được ưu dùng hơn sao với hàm "lọc" (``filter`` function). List comprehensions sẽ giúp ích trong tình huống bạn muốn thêm các phần tử vào một danh sách, ở đó phần tử được tạo ra trong vòng lặp  ``for``. Ví dụ bạn có thể làm điều như sau:

.. code:: python

    squared = []
    for x in range(10):
        squared.append(x**2)

Bạn có thể sử dụng list comprehensions với cú pháp đơn giản hơn. Ví dụ:
.. code:: python

    squared = [x**2 for x in range(10)]

``dict`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^^

Cách sử dụng cũng tương tự như list comprehensions. Dưới đây là một ví dụ:

.. code:: python

    mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}

    mcase_frequency = {
        k.lower(): mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0)
        for k in mcase.keys()
    }

    # mcase_frequency == {'a': 17, 'z': 3, 'b': 34}


Trong ví dụ trên chúng ta kết hợp giá trị của các khoá giống nhau nhưng khác do kiểu in hoa thường. Bạn cũng có thể hoán đổi các khoá và giá trị của một từ điển như sau:

.. code:: python

    {v: k for k, v in some_dict.items()}

``set`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^


Tương tự như list comprehensions. Sự khác biệt duy nhất là sử dụng các dấu ngoặc ``{}``. Xem một ví dụ dưới đây:

.. code:: python

    squared = {x**2 for x in [1, 1, 2]}
    print(squared)
    # Output: {1, 4}

``generator`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Tương tự như list comprehensions. Sự khác biệt duy nhất đó là ``generator`` comprehensions không cấp phát bộ nhớ cho toàn bộ danh sách mà chỉ sinh ra mỗi phần tử tại một thời điểm, do đó sử dụng hiệu  bộ nhớ.

.. code:: python

    multiples_gen = (i for i in range(30) if i % 3 == 0)
    print(multiples_gen)
    # Output: <generator object <genexpr> at 0x7fdaa8e407d8>
    for x in multiples_gen:
      print(x)
      # Outputs numbers
