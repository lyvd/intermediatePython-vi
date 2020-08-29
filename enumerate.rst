Enumerate
---------


Enumerate là một hàm tích hợp sẵn (built-in) của Python. Tính hữu dụng của hàm này không thể tóm tắt chỉ trong một dòng đọc. 
Hầu hết những người mới bắt đầu với Python hay thậm chí những người đã làm một thời gian cũng khoong biết về enumberate.
Hàm này cho phép ta lặp (loop) qua một thứ gì đó với một biến đếm tự động. Nhìn vào ví dụ dưới đây:

.. code:: python
    
    my_list = ['apple', 'banana', 'grapes', 'pear']
    for counter, value in enumerate(my_list):
        print counter, value

    # Kết quả:
    # 0 apple
    # 1 banana
    # 2 grapes
    # 3 pear


Hơn thế nữa! ``enumerate`` còn chấp nhận một tham số tuỳ chọn cho phép ta chỉ định chỉ số bắt đầu của biến đếm

.. code:: python

    my_list = ['apple', 'banana', 'grapes', 'pear']
    for c, value in enumerate(my_list, 1):
        print(c, value)

    # Kết quả:
    # 1 apple
    # 2 banana
    # 3 grapes
    # 4 pear

Việc chỉ định chỉ số bắt đầu này hữu ích khi ta muốn tạo một danh sách các tuples chứa phần tử và chỉ số từ một list. Ví dụ

.. code:: python

    my_list = ['apple', 'banana', 'grapes', 'pear']
    counter_list = list(enumerate(my_list, 1))
    print(counter_list)
    #Kết quả: [(1, 'apple'), (2, 'banana'), (3, 'grapes'), (4, 'pear')]

