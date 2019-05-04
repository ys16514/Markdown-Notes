# Flask

## 1. Python虚拟环境搭建

* 安装虚拟环境：`$ sudo apt-get install python3-venv`

* 创建虚拟环境：`$ python3 -m venv virtual-environment-name` (virtual-environment-name 是自定义的名称)

* 激活虚拟环境：`$ source virtual-environment-name/bin/activate`

---

## 2. Flask 应用的基本结构

* 应用初始化

    ```
    from flask import Flask
    app = Flask(__name__)
    ```

* 路由 + 视图函数

    ```
    @app.route('/')                         # 装饰器的方式完成 URL -> 视图函数 的映射
    def index():
        return "<h1>Welcome</h1>"

    def index():
        return "<h1>Welcome</h1>"
    app.add_url_rule('/', 'index', index)   # 非装饰器的方式完成 URL -> 视图函数 的映射

    @app.route('/user/<name>')              # 动态路由，变量用 <..> 标明
    def user(name):
        return "<h1>Welcome {}!</h1>".format(name)
    ```

* 启动服务器

    ```
    app.run(debug=True) # 调试模式 (代码的改动自动生效 + 异常时 Web 显示栈跟踪)
    ```

* 上下文全局变量 (只在同一个线程中全局可见，线程之间互不影响)

    * 应用上下文
        * **current_app**：当前应用的应用实例
        * **g**：处理请求时用作临时存储的对象，每次请求都会重设这个变量

    * 请求上下文
        * **request**：保存了客户端发起的 Http 请求的所有信息
        * **session**：用户会话，值为一个字典，存储请求之间需要“记住”的值

    * 接收请求 -> 创建请求上下文 -> 请求上下文入栈 -> 创建该请求的应用上下文 -> 应用上下文入栈 -> 处理逻辑 -> 请求上下文出栈 -> 应用上下文出栈

* 响应 

    ```
    # 视图函数的返回值即为响应，第一个参数为响应文本，第二个参数为状态码
    
    @app.route('/')
    def index():
        return '<h1>Success Request</h1>'                   # 状态码默认为 200

    @app.route('/')
    def index():
        return '<h1>Bad Request</h1>', 400                  # 指定状态码为 400

    @app.route('/')
    def index():
        response = flask.make_response('set cookies!')      # 返回响应对象
        response.set_cookie('hyman','123')
        return response

    @app.route('/')
    def index():
        return flask.redirect('http://www.example.com')     # 返回重定向响应

    @app.route('/')
    def index():
        flask.abort(404)                                    # 错误码响应，直接抛出异常
    ```

---

## 3. 模板

* 渲染模板

    ```
    from flask import render_template

    @app.route('/')
    def index():
        return render_template("index_html.html")
    ```

* Jinja 模板语法

    * 变量 (能识别所有类型的变量，甚至是一些复杂的类型，例如列表、字典和对象)

        ```
        <p>A value from a dictionary: {{ mydict['key'] }}.</p>
        <p>A value from a list: {{ mylist[3] }}.</p>
        <p>A value from an object's method: {{ myobj.somemethod() }}.</p>
        ```

    * if 语句

        ```
        {% if user %}
            Hello, {{ user }}!
        {% else %}
            Hello, Stranger!
        {% endif %}
        ```
    
    * for 语句

        ```
        {% for user in users %}
            Hello, {{ user }}!
        {% endfor %}
        ```

    * 宏

        ```
        {% macro foo(user) %}
            <li> Hello {{ user }} </li>
        {% endmacro %}

        {% for user in users %}
            {{ foo(user) }}
        {% endfor %}
        ```

    * 导入模板

        ```
        {% import 'macros.html' as marcos %}
        
        {% for user in users %}
                {{ macros.foo(user) }}
            {% endfor %}
        ```

    * 引用模板

        ```
        {% include 'common.html' %}
        ```

    * 继承模板
        ```
        {% extends 'base.html' %}
        ```