Decorators
----------


Decorators là một phần quan trọng của Python. Nói một cách đơn giản: decorators là các hàm làm thay đổi tính năng của các hàm khác.
Chúng giúp cho code của ta ngắn gọn hơn và thuần Python hơn (more Pythonic). Hầu hết những người mới học Python không biết cách dùng decorators, do đó tôi sẽ chia sẻ các trường hợp ta có thể dùng decorators làm cho code tinh gọn hơn.


Đầu tiên hãy thảo luận cách viết một decorator của riêng bạn.


Decorator có thể là một khái niệm khó nắm bắt nhất. Chúng ta sẽ cùng thực hiện từng bước một để hiểu nó một cách đầy đủ.

Mọi thứ trong Python đều là một đối tượng (object):

Đầu tiên ta cần hiểu các hàm (functions) trong Python:

.. code:: python

    def hi(name="yasoob"):
        return "hi " + name

    print(hi())
    # Kết quả: 'hi yasoob'

    # Ta có thể gán một hàm cho một hàm như sau
    greet = hi
    
    # Chúng ta không sử dụng các dấu ngoặc đơn phía sau hàm hi bởi vì chúng ta không gọi hàm này.
    # thay vào đó chúng ta gán nó cho biết greet. Hãy chạy thử

    print(greet())
    # Kết quả: 'hi yasoob'

    # Điều gì sẽ xảy ra nếu chúng ta xoá đi hàm hi!
    del hi
    print(hi())
    #Kết quả: NameError

    print(greet())
    #Kết quả: 'hi yasoob'

Định nghĩa các hàm bên trong hàm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Trong Python chúng ta có thể định nghĩa các hàm bên trong các hàm khác:

.. code:: python

    def hi(name="yasoob"):
        print("now you are inside the hi() function")

        def greet():
            return "now you are in the greet() function"

        def welcome():
            return "now you are in the welcome() function"

        print(greet())
        print(welcome())
        print("now you are back in the hi() function")

    hi()
    #Kết quả:now you are inside the hi() function
    #       now you are in the greet() function
    #       now you are in the welcome() function
    #       now you are back in the hi() function
    
    # Bất cứ khi nào bạn gọi hi(), thì greet() và welcome() cũng được gọi
    # Tuy nhiên các hàm greet() và welcome() không hợp lệ khi gọi từ bên ngoài hàm hi() ví dụ

    greet()
    #Kết quả: NameError: name 'greet' is not defined

Bây giờ bạn biết cách định nghĩa các hàm trong các hàm khác. Hay nói cách khác: 
bạn có thể tạo ra các hàm lồng nhau. Bây giờ bạn cần học thêm một điều nữa, đó là 
các hàm cũng có thể trả về các hàm

Trả về các hàm từ bên trong các hàm:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is not necessary to execute a function within another function, we
can return it as an output as well:
Một hàm không nhất thiết phải thực thi ở bên trong một hàm khác, chúng ta
có thể trả về hàm này như là trả về kết quả:
.. code:: python

    def hi(name="yasoob"):
        def greet():
            return "now you are in the greet() function"

        def welcome():
            return "now you are in the welcome() function"

        if name == "yasoob":
            return greet
        else:
            return welcome

    a = hi()
    print(a)
    #Kết quả: <function greet at 0x7f2143c01500>

    # Ta có thể thấy rằng `a` hiện tại trỏ tới hàm greet() trong hi()
    #Cùng thử xem

    print(a())
    #Kết quả: now you are in the greet() function


Cùng nhìn lại đoạn mã một lần nữa. Trong khối ``if/else`` chúng ta đang trả về ``greet`` và ``welcome``, không phải là ``greet()`` và ``welcome()``.
Sao lại vậy? Bởi vì khi chúng ta đặt cặp dấu ngoặc đơn sau một hàm, hàm đó sẽ được thực thi; trái lại nếu bạn không đặt các dấu ngoặc đơn phải sau nó, khi đó nó thể được truyền đi và có thể được gán cho các biến khác. Để tôi giải thích chi tiết hơn. Khi bạn viết ``a = hi()``, ``hi()`` được thực thi và bởi vì biến name có giá trị mặc định là yasoob, hàm ``greet`` được trả về. Nếu chúng ta thay đổi câu lệnh thành ``a = hi(name = "ali")`` khi đó hàm ``welcome`` sẽ được trả về. Chúng ta cũng có thể gọi  ``hi()()`` để in ra đoạn thông điệp *now you are in the greet() function*.

Hàm là một tham số truyền cho hàm khác:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    def hi():
        return "hi yasoob!"

    def doSomethingBeforeHi(func):
        print("I am doing some boring work before executing hi()")
        print(func())

    doSomethingBeforeHi(hi)
    #Kết quả:I am doing some boring work before executing hi()
    #        hi yasoob!

Giờ thì ta đã có đủ kiến thức để học về decorators. Decorators cho phép bạn thực thi code trước và sau một hàm

Viết decorator đầu tiên của bạn:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Trong ví dụ cuối này chúng ta sẽ thực sự tạo ra một decorator! Hãy thay đổi decorator trước đó và tạo ra 
một chương trình hữu ích hơn:


.. code:: python

    def a_new_decorator(a_func):

        def wrapTheFunction():
            print("I am doing some boring work before executing a_func()")

            a_func()

            print("I am doing some boring work after executing a_func()")

        return wrapTheFunction

    def a_function_requiring_decoration():
        print("I am the function which needs some decoration to remove my foul smell")

    a_function_requiring_decoration()
    #outputs: "I am the function which needs some decoration to remove my foul smell"

    a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
    #now a_function_requiring_decoration is wrapped by wrapTheFunction()

    a_function_requiring_decoration()
    #outputs:I am doing some boring work before executing a_func()
    #        I am the function which needs some decoration to remove my foul smell
    #        I am doing some boring work after executing a_func()

Did you get it? We just applied the previously learned principles. This
is exactly what the decorators do in Python! They wrap a function and
modify its behaviour in one way or another. Now you might be
wondering why we did not use the @ anywhere in our code? That is just a
short way of making up a decorated function. Here is how we could have
run the previous code sample using @.

.. code:: python

    @a_new_decorator
    def a_function_requiring_decoration():
        """Hey you! Decorate me!"""
        print("I am the function which needs some decoration to "
              "remove my foul smell")

    a_function_requiring_decoration()
    #outputs: I am doing some boring work before executing a_func()
    #         I am the function which needs some decoration to remove my foul smell
    #         I am doing some boring work after executing a_func()

    #the @a_new_decorator is just a short way of saying:
    a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)

I hope you now have a basic understanding of how decorators work in
Python. Now there is one problem with our code. If we run:

.. code:: python

    print(a_function_requiring_decoration.__name__)
    # Output: wrapTheFunction

That's not what we expected! Its name is
"a\_function\_requiring\_decoration". Well, our function was replaced by
wrapTheFunction. It overrode the name and docstring of our function.
Luckily, Python provides us a simple function to solve this problem and
that is ``functools.wraps``. Let's modify our previous example to use
``functools.wraps``:

.. code:: python

    from functools import wraps

    def a_new_decorator(a_func):
        @wraps(a_func)
        def wrapTheFunction():
            print("I am doing some boring work before executing a_func()")
            a_func()
            print("I am doing some boring work after executing a_func()")
        return wrapTheFunction

    @a_new_decorator
    def a_function_requiring_decoration():
        """Hey yo! Decorate me!"""
        print("I am the function which needs some decoration to "
              "remove my foul smell")

    print(a_function_requiring_decoration.__name__)
    # Output: a_function_requiring_decoration

Now that is much better. Let's move on and learn some use-cases of
decorators.

**Blueprint:**

.. code:: python

    from functools import wraps
    def decorator_name(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            if not can_run:
                return "Function will not run"
            return f(*args, **kwargs)
        return decorated

    @decorator_name
    def func():
        return("Function is running")

    can_run = True
    print(func())
    # Output: Function is running

    can_run = False
    print(func())
    # Output: Function will not run

Note: ``@wraps`` takes a function to be decorated and adds the
functionality of copying over the function name, docstring, arguments
list, etc. This allows us to access the pre-decorated function's properties
in the decorator.

Use-cases:
~~~~~~~~~~

Now let's take a look at the areas where decorators really shine and
their usage makes something really easy to manage.

Authorization
~~~~~~~~~~~~

Decorators can help to check whether someone is authorized to use an
endpoint in a web application. They are extensively used in Flask web
framework and Django. Here is an example to employ decorator based
authentication:

**Example :**

.. code:: python

    from functools import wraps

    def requires_auth(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            auth = request.authorization
            if not auth or not check_auth(auth.username, auth.password):
                authenticate()
            return f(*args, **kwargs)
        return decorated

Logging
~~~~~~~~~~~~

Logging is another area where the decorators shine. Here is an example:

.. code:: python

    from functools import wraps

    def logit(func):
        @wraps(func)
        def with_logging(*args, **kwargs):
            print(func.__name__ + " was called")
            return func(*args, **kwargs)
        return with_logging

    @logit
    def addition_func(x):
       """Do some math."""
       return x + x


    result = addition_func(4)
    # Output: addition_func was called

I am sure you are already thinking about some clever uses of decorators.

Decorators with Arguments
^^^^^^^^^^^^^^^^^^^^^^^^^

Come to think of it, isn't ``@wraps`` also a decorator?  But, it takes an
argument like any normal function can do.  So, why can't we do that too?

This is because when you use the ``@my_decorator`` syntax, you are
applying a wrapper function with a single function as a parameter.
Remember, everything in Python is an object, and this includes
functions!  With that in mind, we can write a function that returns
a wrapper function.

Nesting a Decorator Within a Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let's go back to our logging example, and create a wrapper which lets
us specify a logfile to output to.

.. code:: python

    from functools import wraps
    
    def logit(logfile='out.log'):
        def logging_decorator(func):
            @wraps(func)
            def wrapped_function(*args, **kwargs):
                log_string = func.__name__ + " was called"
                print(log_string)
                # Open the logfile and append
                with open(logfile, 'a') as opened_file:
                    # Now we log to the specified logfile
                    opened_file.write(log_string + '\n')
                return func(*args, **kwargs)
            return wrapped_function
        return logging_decorator

    @logit()
    def myfunc1():
        pass
        
    myfunc1()
    # Output: myfunc1 was called
    # A file called out.log now exists, with the above string
    
    @logit(logfile='func2.log')
    def myfunc2():
        pass
    
    myfunc2()
    # Output: myfunc2 was called
    # A file called func2.log now exists, with the above string

Decorator Classes
~~~~~~~~~~~~~~~~~

Now we have our logit decorator in production, but when some parts
of our application are considered critical, failure might be
something that needs more immediate attention.  Let's say sometimes
you want to just log to a file.  Other times you want an email sent,
so the problem is brought to your attention, and still keep a log
for your own records.  This is a case for using inheritence, but
so far we've only seen functions being used to build decorators.

Luckily, classes can also be used to build decorators.  So, let's
rebuild logit as a class instead of a function.

.. code:: python

    class logit(object):
    
        _logfile = 'out.log'
    
        def __init__(self, func):
            self.func = func
        
        def __call__(self, *args):
            log_string = self.func.__name__ + " was called"
            print(log_string)
            # Open the logfile and append
            with open(self._logfile, 'a') as opened_file:
                # Now we log to the specified logfile
                opened_file.write(log_string + '\n')
            # Now, send a notification
            self.notify()
            
            # return base func
            return self.func(*args)
            
            
        
        def notify(self):
            # logit only logs, no more
            pass
    
This implementation has an additional advantage of being much cleaner than
the nested function approach, and wrapping a function still will use
the same syntax as before:

.. code:: python
    
    logit._logfile = 'out2.log' # if change log file
    @logit
    def myfunc1():
        pass
    
    myfunc1()
    # Output: myfunc1 was called

Now, let's subclass logit to add email functionality (though this topic
will not be covered here).

.. code:: python

    class email_logit(logit):
        '''
        A logit implementation for sending emails to admins
        when the function is called.
        '''
        def __init__(self, email='admin@myproject.com', *args, **kwargs):
            self.email = email
            super(email_logit, self).__init__(*args, **kwargs)
            
        def notify(self):
            # Send an email to self.email
            # Will not be implemented here
            pass

From here, ``@email_logit`` works just like ``@logit`` but sends an email
to the admin in addition to logging.
