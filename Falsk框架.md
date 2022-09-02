# Falsk

### 1、安装

```
pip install flask
```

### 2、基础使用

```python
创建py文件

from flask import Flask, request  # 导入Falsk、request
from flask import render_template   # 导入指定模板（新建templates文件，保存模板）

# 实例化类
app = Flask(__name__)

# 装饰器  路由
@app.route('/')
def index():
    # 模板渲染
    return render_template('index.html')

@app.route('/juran')
def demo():
    return '居然'

if __name__ == '__main__':
    # debug = True 代码调试
    # 运行
    app.run(debug=False)
```

### 3、转换器类型：

| string | 缺省值） 接受任何不包含斜杠的文本 |
| ------ | --------------------------------- |
| path   | 类似 `string` ，但可以包含斜杠    |
| int    | 接受正整数                        |
| uuid   | 接受 UUID 字符串                  |
| float  | 接受正浮点数                      |

```
from markupsafe import escape

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return f'Post {post_id}'

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return f'Subpath {escape(subpath)}'
```

### 4、代码

```python
from flask import Flask, request  # 导入Falsk、request
from flask import render_template   # 导入指定模板
from markupsafe import escape
from flask import url_for

app = Flask(__name__)

# 装饰器  路由
@app.route('/')
def index():
    # 模板渲染
    return render_template('index.html')

# 接收参数
@app.route('/<name>/')
def demo(name):
    return name

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return f'User {escape(username)}'




@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()

with app.test_request_context():
    print(url_for('index'))
    # print(url_for('index'))
    # print(url_for('login', next='/'))
    # print(url_for('profile', username='John Doe'))

@app.route('/projects/')
def projects():
    return 'The project page'

@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)

# 文件上传
@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')

# Cookie
@app.route('/')
def index():
    username = request.cookies.get('username')

# 这个静态文件在文件系统中的位置应该是 static/style.css
url_for('static', filename='style.css')

if __name__ == '__main__':
    # debug = True 代码调试
    # 运行
    app.run(debug=True)
```

