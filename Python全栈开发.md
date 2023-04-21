# Python全栈



````
目的：开发一个网站
- 前端开发：HTML，CSS，JavaScript
- Web框架：接受请求并处理
- MySQL数据库：存储数据
````



## 1. 快速开发网站(Flask)

````shell
pip intall flask
````

````python
from flask import Flask

app = Flask(__name__)

# 创建了网址/show/info 和 函数 index 的对应关系
# 以后用户在浏览器上访问 /show/info, 网站自动执行 index
@app.route('show/info')
def index():
	return "mhy"


if __name__ = '__main__':
	app.run()
````

````
浏览器可以识别很多标签+数据：
	<h1>mhy</h1>								->		浏览器看见加大加粗
	<span style='color:red;'>ddlnb</span>		->		浏览器看到字体变红色
````

* Flask框架支持从文件读取网页内容

* ````python
  from flask import Flask, render_template
  
  app = Flask(__name__)
  
  @app.route('/show/info')
  def index():
      # Flask自动打开这个文件读取内容并将内容给用户返回
      # 默认去当前项目的templates文件夹中找目标文件
      return reder_template("index.html")
  
  if __name__ == '__main__':
      app.run()
  ````

  

## 2. 浏览器能识别的标签

### 2.1 编码（head)

````html
<meta charset="UTF-8">
````



### 2.2 title  (head)

````html
<head>
    <meta charset="UTF-8">
    <title>MHY的网站</title>
</head>
````



### 2.3 标题 (body)

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    	<title>MHY的网站</title>
    </head>
    <body>
        <h1>
            一级标题
        </h1>
        <h2>
            二级标题
        </h2>
        ...
        ...
        ...
        <h6>
            六级标题
        </h6>
    </body>
</html>
````



### 2.4 div和span（body）

````html
<div>
    内容
</div>

<span>内容</span>
````

* div一次性占一整行【块级标签】

* ````html
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8">
      	<title>MHY的网站</title>
      </head>
      <body>
          <div>
              丁大佬
          </div>
          <div>
              牛逼
          </div>
      </body>
  </html>
  ````

* span有多大就占多少【行内标签/内联标签】（换行有空格，不换行没空格）

* ````html
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8">
      	<title>MHY的网站</title>
      </head>
      <body>
          <span>丁大佬</span>
          <span>牛逼</span>
          <span style="color:red;">卢大佬</span><span style="color:blue;">也牛逼</span>
      </body>
  </html>
  ````



这两个标签比较素，只具备占行占位的功能。通过CSS样式可以丰富其功能。



### 练习

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    	<title>MHY的网站</title>
    </head>
    <body>
        <h1>
            丁大佬
        </h1>
        <div>
            <span style="color:red;">时间：</span>
            <span>2022-06-20</span>
        </div>
        <div>
            <span>丁大佬</span>
        	<span>牛逼</span>
        </div>
        <h2>
            卢大佬
        </h2>
        <div>
            <span style="color:green;">时间：</span>
            <span>2022-06-20</span>
        </div>
        <div>
			<span style="color:red;">卢大佬</span><span style="color:blue;">也牛逼</span>
        </div>
    </body>
</html>
```



### 2.5 超链接

```html
<a href="http://https://github.com/SeleneXX?tab=repositories">点击跳转</a>
```

````
跳转到自己网站的其他地址
<a href="http://127.0.0.1:5000/get/news">点击跳转</a>

可以简化成
<a href="/get/news">点击跳转</a>
````

````html
# 当前页面打开
<a href="/get/news">直接跳转</a>

# 打开新的标签页
<a href="/get/news" target="_blank">打开新的标签页</a>
````



### 2.6 图片

```html
<img src="图片地址" />

直接显示其他网站的图片（前提是该网站允许转载）
<img src="https://avatars.githubusercontent.com/u/62474794?v=4" />

显示自己的图片
	- 自己的项目中创建 /static目录，图片放在/static中
<img src="/static/xxx.png" />
```

* 设置图片的高度和宽度

* ```html
  直接设置长宽像素数
  <img src="https://avatars.githubusercontent.com/u/62474794?v=4" style="height: 100px; width:200px;" />
  
  按百分比设置长宽
  <img src="https://avatars.githubusercontent.com/u/62474794?v=4" style="height: 10%; width:20%;" />
  
  只设置长或者宽则按比例缩放
  <img src="https://avatars.githubusercontent.com/u/62474794?v=4" style="height: 10%;" />
  ```



### 小结

* body标签

* ````html
  <h1></h1>
  <div></div>
  <span></span>
  <a></a>
  <img />
  ````

* 划分

* ````
  - 块级标签
  	<h1></h1>
  	<div></div>
  - 行内标签 
  	<span></span>
      <a></a>
      <img />
  ````

* 嵌套

* ````html
  <div>
      <span>xxx</span>
      <img />
      <a></a>
  </div>
  ````



### 2.7 列表

无序号列表

````html
<ul>
    <li>丁大佬</li>
    <li>卢大佬</li>
    <li>朱大佬</li>
    <li>王明</li>
</ul>
````

![image-20220728014850283](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728014850283.png)

有序号列表

````html
<ol>
    <li>丁大佬</li>
    <li>卢大佬</li>
    <li>朱大佬</li>
    <li>王明</li>
</ol>
````

![image-20220728014858712](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728014858712.png)



### 2.8 表格标签

````html
<table>
    <thead>
    	<tr>	<th>ID</th>	<th>姓名</th>	<th>年龄</th>		</tr>
    </thead>
    <tbody>
    	<tr>	<td>10</td>	<td>张三</td>	<td>19</td>		</tr>
        <tr>	<td>11</td>	<td>李四</td>	<td>19</td>		</tr>
        <tr>	<td>12</td>	<td>王五</td>	<td>19</td>		</tr>
    </tbody>
</table>
````

![image-20220728015303004](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728015303004.png)

````html
# 加边框
<table border="1">
    <thead>
    	<tr>	<th>ID</th>	<th>姓名</th>	<th>年龄</th>		</tr>
    </thead>
    <tbody>
    	<tr>	<td>10</td>	<td>张三</td>	<td>19</td>		</tr>
        <tr>	<td>11</td>	<td>李四</td>	<td>19</td>		</tr>
        <tr>	<td>12</td>	<td>王五</td>	<td>19</td>		</tr>
    </tbody>
</table>
````

![image-20220728015413185](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728015413185.png)



### 2.9 input系列（7个）

![image-20230103201021521](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230103201021521.png)

![image-20230103201216515](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230103201216515.png)

````html
<input type="text" />
<input type="password" />
<input type="file" />
````

![image-20220728015824729](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728015824729.png)

````html
# name属性是输入元素的名称
<input type="radio" name="n1">男
<input type="radio" name="n1">女

<input type="checkbox">篮球
<input type="checkbox">足球
<input type="checkbox">乒乓球
<input type="checkbox">棒球
````

![image-20220728020253950](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728020253950.png)

````html
# 普通的按钮
<input type="button" value="button提交">
# 提交表单
<input type="button" value="submit提交">
````

![image-20230103201123537](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230103201123537.png)

### 2.10 下拉框

````html
# 单选
<select>
    <option>北京</option>
    <option>上海</option>
    <option>深圳</option>
</select>

# 按住shift多选
<select multiple>
    <option>北京</option>
    <option>上海</option>
    <option>深圳</option>
</select>
````



### 2.11 多行文本

````html
<textarea></textarea>

# 规定行数
<textarea rows="3"></textarea>
````

![image-20220728021720206](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728021720206.png)



### 案例：用户注册

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <tiltle>Register</tiltle>
    </head>
    <body>
        <h1>用户注册</h1>
        <div>
            用户名：<input type="text" />
        </div>
        <div>
            密码：<input type="password" />
        </div>
        
        <div>
            性别：
            <input type="radio">男 
            <input type="radio">女
        </div>
        
        <div>
            爱好：
            <input type="checkbox">篮球 
            <input type="checkbox">足球
            <input type="checkbox">乒乓球
        </div>
        
        <div>
            城市：
            <select>
                <option>北京</option>
                <option>上海</option>
                <option>深圳</option>
            </select>
        </div>
        
        <div>
            擅长：
            <select multiple>
                <option>吃饭</option>
                <option>睡觉</option>
                <option>玩游戏</option>
            </select>
        </div>
        
        <div>
            备注：<textarea></textarea>
        </div>
        
        <div>
            <input type="button" value="button">
            <input type="submit" value="提交">
        </div>
    </body>
</html>
````



### 补充

1.  网站请求的流程

   ![image-20220728025137441](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220728025137441.png)

2. 网络请求

   * 在浏览器的URL中写入地址，点击回车访问

   * ````
     浏览器会发送数据过去，本质发送的是字符串
     GET /explore http1.1\r\nhost:...\r\nuser-agent\r\n\r\n"
     
     浏览器会发送数据过去，本质发送的是字符串
     GET /explore http1.1\r\nhost:...\r\nuser-agent\r\n\r\n数据库"
     ````

   * 浏览器向后端发送请求时

     * GET请求【URL方法/表单提交】

       * 现象：GET请求，跳转，向后端传输数据会拼接在URL上。

       * ````
         https://www.sogou.com/web?query=安卓&age=19&name=xx
         ````

       * GET请求数据会在URL中出现

     * POST请求【表单提交】

       * 现象：提交数据不在URL中而在请求体中。



### 2.12 提交

页面上的数据想要提交到 后端

* form标签包裹需要提交的数据的标签

  * 提交方式：````method="get"````

  * 提交的地址：````action="/xx/xxx/xxxx"````，可以把提交的地址链接到新的网页

    ````python
    @app.route("/GET/reg", methods=['GET'])
    def GET_register():
        # 1. 接收用户通过GET形式发送过来的数据
        print(request.args)
        # 2. 给用户返回结果
        return "注册成功"
    ````

  * 提交方式：````method="post"````

    ````python
    @app.route("/post/reg", methods=['POST'])
    def POST_register():
        # 1. 接收用户通过POST形式发送过来的数据
        print(request.form)
        # 2. 给用户返回结果
        return "注册成功"
    ````

    在form标签内必须有一个submit按钮

* ````html
  <form method="get" action="/xx/xxx/xxxx">
      用户名：<input type="text" name="uu" />
      密码：<input type="password" name="pp" />
      <input type="submit" value="submit按钮">
  </form>
  ````

* form里的标签：input/select/textarea

  * 一定要写name属性````<input type="text" name="uu" />````
  * 提交后端的格式为字典：````{"uu": 输入的用户名， "pp": 输入的密码}````



### 案例

````python
from flask import Flask, render_template, request

app = Flask(__name__)


@app.route("/register", methods=['GET', 'POST'])
def register():
    if request.method == 'GET':
        return render_template('register.html')
    else:
        user = request.form.get('user')
        pwd = request.form.get('pwd')
        gender = request.form.get('gender')
        hobby_list = request.form.get('hobby')
        city = request.form.get('city')
        skill_list = request.form.get('skill')
        more = request.form.get('more')
        print(user, pwd, gender, hobby_list, city, skill_list, more)
        # 将用户信息写入文件中实现注册，写入excel中实现注册，写入数据库中实现注册
        
        #给用户返回结果
        return "注册成功"

if __name__ == "__main__":
    app.run()
````

````html
=<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <tiltle>Register</tiltle>
    </head>
    <body>
        <h1>用户注册</h1>
        
        <form method="post" action="/register">
            <div>
                用户名：<input type="text" name="user"/>
            </div>
            <div>
                密码：<input type="password" name="pwd"/>
            </div>

            <div>
                性别：
                <input type="radio" name="gender">男 
                <input type="radio" name="gender">女
            </div>

            <div>
                爱好：
                <input type="checkbox" name="hobby" value="10">篮球 
                <input type="checkbox" name="hobby" value="20">足球
                <input type="checkbox" name="hobby" value="30">乒乓球
            </div>

            <div>
                城市：
                <select name="city">
                    <option value="bj">北京</option>
                    <option value="sh">上海</option>
                    <option value="sz">深圳</option>
                </select>
            </div>

            <div>
                擅长：
                <select name="skill" multiple>
                    <option value="100">吃饭</option>
                    <option value="101">睡觉</option>
                    <option value="102">玩游戏</option>
                </select>
            </div>

            <div>
                备注：<textarea name="more"></textarea>
            </div>

            <div>
                <input type="submit" value="提交">
            </div>
        </form>
        
    </body>
</html>
````



###总结

1. 称呼

   HTML：超文本传输语言

2. HTML标签（默认格式样式，以后通过手段可以修改）

3. HTML标签与编程语言无关



## 3. CSS样式

css，专门用来美化标签。

### 3.1 快速了解

````html
<img scr="..." style="height:100px" />

<div style="color:red;" >丁大佬牛逼</div>
````



### 3.2 CSS应用方式

* 在标签上应用

  ````html
  <img scr="..." style="height:100px" />
  
  <div style="color:red;" >丁大佬牛逼</div>
  ````

* **在head标签中应用：先定义样式然后再使用，方便样式复用**

  ````html
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8">
          <tiltle>Title</tiltle>
          <style>
              .c1{coler:red;}
          </style>
      </head>
      <body>
          <h1 class="c1">
              用户登录
          </h1>
          <form method="post" action="/login">
              用户名：<input type="text" name="username">
              密码：<input type="password" name="password">
              <input type="submit" value="提交">
          </form>
      </body>
  </html>
  ````

* **写到文件中，方便多文件复用**

  ````css
  .c1{
      height:100px;
  }
  
  .c2{
      color:red;
  }
  ````

  ````html
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8">
          <tiltle>Title</tiltle>
          <link rel="stylesheet" href="common.css"/>
      </head>
      <body>
          <h1 class="c1">
              用户登录
          </h1>
          <h1 class="c2">
              用户登录
          </h1>
      </body>
  </html>
  ````

### 3.3 CSS选择器

* ID选择器：一一对应，只能存在一个

  ````css
  #c1{
      ...
  }
  <div id="c1">...</div>
  ````

* **类选择器**

  ````css
  .c1{
      ...
  }
  <div class="c1">...</div>
  ````

* **标签选择器**，对所有该标签应用样式

  ```css
  div{
      ...
  }
  <div>...</div>
  ```

* 属性选择器

  ````css
  input[type='text']{
      border: 1px solid red;
  }
  .v1[xx="456"]{
      color: gold;
  }
  ````

  ````html
  <input type="text">
  <input type="password">
  
  <div class="v1" xx="123"></div>
  <div class="v1" xx="456"></div>
  <div class="v1" xx="789"></div>
  ````

* **后代选择器**

  ```css
  .yy li{
      color: pink;
  }
  
  .yy > a{
      # > 代表只选择儿子里面的不找孙子里的
      color: blue;
  }
  ```

  ```html
  <div class="yy">
      <a href="https://www.baidu.com">百度</a>
      <div>
          # 此处<a></a>没有选择
          <a href="https://www.google.com">谷歌</a>
      </div>
      <ul>
          <li>美国</li>
          <li>日本</li>
          <li>中国</li>
      </ul>
  </div>
  ```

多个样式&覆盖问题

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .c1{
                color: red;
                border: 1px solid red;
            }
            .c2{
                font-size: 28px;
                color: green;
            }
        </style>
    </head>
    <body>
        # c2中的color覆盖了c1中的，按照c1 c2定义的先后顺序覆盖
        <div class="c1 c2">丁大佬牛逼</div>
    </body>
</html>
````

优先定义的不被覆盖：

````html
<style>
    .c1{
        color: red !important;
        border: 1px solid red;
    }
    .c2{
        font-size: 28px;
        color: green;
    }
</style>
````



### 3.4 样式

#### 3.4.1 高度和宽度

````css
.c1{
    height: 300px;
    width: 500px;
}
````

* 宽度支持百分比
* 行内标签默认无效
* 块级标签：默认有效（若设置宽度，右侧区域空白也不给占用）

#### 3.4.2 块级和行内标签

* css样式：标签 -> display:inline-block，将块级标签的特性赋予行内标签，可以设置高度宽度

````
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .c1{
                display: inline-block;
                height: 100px;
                width: 300px;
                border: 1px solid red;
            }
        </style>
    </head>
    <body>
        <span class="c1">中国</span>
        <span class="c1">美国</span>
        <span class="c1">日本</span>
    </body>
</html>
````

主动将行内标签性质赋予块级标签，解决了上述块级标签设置宽度仍然占一整行的问题

块级变行内：display: inline

行内变块级：display: block

````html
<div style="display: inline;">中国</div>
<span style="display: blockl">美国</span>
````

注意：块级+块级&行内用的多。

#### 3.4.3 字体和颜色

* 颜色
* 大小
* 加粗
* 字体

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .c1{
                color: #00FF7F;
                font-size:58px;
                font-weight: 600;
                font-family: Microsoft Yahei;
            }
        </style>
    </head>
    <body>
        <div class="c1">中国</div>
        <div>美国</div>
    </body>
</html>
````

#### 3.4.4 文字对齐方式

````html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .c1{
                height: 59px;
                width: 300px;
                border: 1px solid red;

                text-align: center;     /* 水平方向居中*/
                line-height: 59px;     /* 在一定的高度内垂直方向居中*/
            }
        </style>
    </head>
    <body>
        <div class="c1">中国</div>
    </body>
</html>
````

#### 3.4.5 浮动

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div>
        <span>左边</span>
        <span style="float: right">右边</span>
    </div>
</body>
</html>
````

div默认是块级标签，如果浮动起来，也变成自己有多宽就占多宽

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .item{
            float: left;
            width: 280px;
            height: 170px;
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
</body>
</html>
````

如果标签浮动起来，就会脱离文档流（）。通过style="clear: both"强制回归文档流。在一个标签的孩子中使用了float，一定要强制回归文档流，不然没有办法把父亲撑起来。

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .item{
            float: left;
            width: 280px;
            height: 170px;
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <div style="background-color: blue">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div style="clear: both"></div>
    </div>
</body>
</html>
````

#### 3.4.6 内边距

标签内部设置一点距离

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .outer{
            border: 1px solid red;
            height: 200px;
            width: 200px;

            /*padding-top: 20px;*/
            /*padding-left: 20px;*/
            /*padding-right: 20px;*/
            /*padding-bottom: 20px;*/
            padding: 20px 10px 15px 20px;
            /*上右下左*/
        }
    </style>
</head>
<body>
    <div class="outer">
        <div>丁大佬牛逼</div>
        <div>不如卢大佬</div>
    </div>
</body>
</html>
````

#### 3.4.7 外边距

标签与其他标签设置距离

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div style="height: 200px; background-color: dodgerblue;"></div>
    <div style="background-color: red; height: 100px; margin-top: 20px;"></div>
</body>
</html>
````

### 案例1

![image-20220803145700907](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20220803145700907.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0px;
        }

        .header{
            background: #333;
        }

        .header .menu{
            float: left;
            color: whitesmoke;
        }

        .container{
            width: 1226px;
            margin: 0 auto;
        }

        .header .account{
            float: right;
            color: white;
        }

        .header a{
            color: #b0b0b0;
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="container">
            <div class="menu">
                <a href="">小米商城</a>
                <a href="">MIUI</a>
                <a href="">云服务</a>
                <a href="">有品</a>
                <a href="">开放平台</a>
            </div>
            <div class="account">
                <a href="">登录</a>
                <a href="">注册</a>
                <a href="">消息通知</a>
            </div>
            <div style="clear: both"></div>
        </div>
    </div>
</body>
</html>
````



### 小结

* body标签，默认有一个边距，造成页面四边都有白色间隙，去除：

  ````css
  body{
  	margin: 0;
  }
  ````

* 内容居中

  * 文本居中，文本会在该区域中居中

    ````html
    <div style="width: 200px; background-color: pink; text-align: center;">123</div>
    ````

  * 区域居中。自己要有宽度+````margin-left=auto; margin-right=auto;````

    ````html
    .container{
    	width: 980px;
        margin: 0 auto;
    }
    <div class="container">
        丁大佬牛逼
    </div>
    ````

* 父亲没有高度或者宽度，会被孩子撑起来

* 如果存在浮动，一定记得加入style="clear: both"

### 案例2

#### 1. 划分区域

![image-20230104144240516](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230104144240516.png)

#### 2. 搭建骨架

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0;
        }
        .sub_header{
            height: 100px;
            background-color: #b0b0b0;
        }
        .container{
            width: 1128px;
            margin: 0 auto;

            border-left: 1px solid red;
            border-right: 1px solid red;
        }

        .sub_header .ht{
            height: 100px;
        }
        .sub_header .logo{
            width: 234px;
            float: left;
        }

        .sub_header .menu_list{
            float: left;
        }

        .sub_header .search{
            float: right;
        }
    </style>
</head>
<body>
<div class="sub_header">
    <div class="container">
        <div class="ht logo">1</div>
        <div class="ht menu_list">2</div>

        <div class="ht search">3</div>
        <div style="clear: both;"></div>
    </div>
</div>
</body>
</html>
````

#### 3. logo区域

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0;
        }
        .sub_header{
            height: 100px;
            background-color: #b0b0b0;
        }
        .container{
            width: 1128px;
            margin: 0 auto;

            border-left: 1px solid red;
            border-right: 1px solid red;
        }

        .sub_header .ht{
            height: 100px;
        }
        .sub_header .logo{
            width: 234px;
            float: left;
            border: 1px solid red;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }


        .sub_header .menu_list{
            float: left;
        }

        .sub_header .search{
            float: right;
        }
    </style>
</head>
<body>
<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.baidu.com">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">2</div>

        <div class="ht search">3</div>
        <div style="clear: both;"></div>
    </div>
</div>
</body>
</html>
````

#### 4. 菜单部分

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0;
        }
        .sub_header{
            height: 100px;
        }
        .container{
            width: 1128px;
            margin: 0 auto;
        }

        .sub_header .ht{
            height: 100px;
        }
        .sub_header .logo{
            width: 234px;
            float: left;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }


        .sub_header .menu_list{
            float: left;
            line-height: 100px;
        }

        .sub_header .menu_list a{
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            font-family: Microsoft Yahei;

            text-decoration: none;
        }

        .sub_header .menu_list a:hover{
            color: #ff6700;
        }

        .sub_header .search{
            float: right;
        }
    </style>
</head>
<body>
<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.mi.com/">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">
            <a href="https://www.mi.com/">Xiaomi手机</a>
            <a href="https://www.mi.com/">Redmi红米</a>
            <a href="https://www.mi.com/">电视</a>
            <a href="https://www.mi.com/">笔记本</a>
            <a href="https://www.mi.com/">平板</a>
        </div>

        <div class="ht search">3</div>
        <div style="clear: both;"></div>
    </div>
</div>
</body>
</html>
````

### 案例1，2整合：顶部菜单+二级菜单

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0px;
        }

        .header{
            background: #333;
        }

        .header .menu{
            float: left;
        }

        .container{
            width: 1226px;
            /*0贴紧上边， auto左右居中*/
            margin: 0 auto;
        }

        .header .account{
            float: right;
            color: white;
        }

        .header a{
            color: #b0b0b0;
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
            text-decoration: none;
        }

        .header a:hover{
            color: white;
        }

        .sub_header{
            height: 100px;
        }

        .sub_header .ht{
            height: 100px;
        }

        .sub_header .logo{
            width: 234px;
            float: left;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }

        .sub_header .menu_list{
            float: left;
            line-height: 100px;
        }

        .sub_header .menu_list a{
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            font-family: Microsoft Yahei;

            text-decoration: none;
        }

        .sub_header .menu_list a:hover{
            color: #ff6700;
        }

        .sub_header .search{
            float: right;
        }
    </style>
</head>
<body>
<div class="header">
    <div class="container">
        <div class="menu">
            <a href="https://www.mi.com/">小米商城</a>
            <a href="https://www.mi.com/">MIUI</a>
            <a href="https://www.mi.com/">云服务</a>
            <a href="https://www.mi.com/">有品</a>
            <a href="https://www.mi.com/">开放平台</a>
        </div>
        <div class="account">
            <a href="https://www.mi.com/">登录</a>
            <a href="https://www.mi.com/">注册</a>
            <a href="https://www.mi.com/">消息通知</a>
        </div>
        <div style="clear: both"></div>
    </div>
</div>
<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.mi.com/">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">
            <a href="https://www.mi.com/">Xiaomi手机</a>
            <a href="https://www.mi.com/">Redmi红米</a>
            <a href="https://www.mi.com/">电视</a>
            <a href="https://www.mi.com/">笔记本</a>
            <a href="https://www.mi.com/">平板</a>
        </div>

        <div class="ht search"></div>
        <div style="clear: both;"></div>
    </div>
</div>
</body>
</html>
````

### 小结

- a标签是行内标签，行内标签的高度，内外边距默认无效。

- 垂直方向居中

  - 文本：```line-height```
  - 图片+边距

- a标签默认有下划线。去除:```text-decoration: none;```

- 鼠标放到某个标签上的效果：hover

  ````css
  .c1: hover{
      ...
  }
  
  a: hover{
      ...
  }
  ````


### 案例3

![image-20230107170446267](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107170446267.png)

#### 1. 划分区域

![image-20230107170711752](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107170711752.png)

#### 2. 搭建骨架

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0px;
        }

        img{
            width: 100%;
            height: 100%;
        }

        .header{
            background: #333;
        }

        .header .menu{
            float: left;
        }

        .container{
            width: 1226px;
            /*0贴紧上边， auto左右居中*/
            margin: 0 auto;
        }

        .header .account{
            float: right;
            color: white;
        }

        .header a{
            color: #b0b0b0;
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
            text-decoration: none;
        }

        .header a:hover{
            color: white;
        }

        .sub_header{
            height: 100px;
        }

        .sub_header .ht{
            height: 100px;
        }

        .sub_header .logo{
            width: 234px;
            float: left;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }

        .sub_header .menu_list{
            float: left;
            line-height: 100px;
        }

        .sub_header .menu_list a{
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            font-family: Microsoft Yahei;

            text-decoration: none;
        }

        .sub_header .menu_list a:hover{
            color: #ff6700;
        }

        .sub_header .search{
            float: right;
        }

        .slider .sd_image{
            width: 1226px;
            height: 460px;
        }


    </style>
</head>
<body>
<div class="header">
    <div class="container">
        <div class="menu">
            <a href="https://www.mi.com/">小米商城</a>
            <a href="https://www.mi.com/">MIUI</a>
            <a href="https://www.mi.com/">云服务</a>
            <a href="https://www.mi.com/">有品</a>
            <a href="https://www.mi.com/">开放平台</a>
        </div>
        <div class="account">
            <a href="https://www.mi.com/">登录</a>
            <a href="https://www.mi.com/">注册</a>
            <a href="https://www.mi.com/">消息通知</a>
        </div>
        <div style="clear: both"></div>
    </div>
</div>

<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.mi.com/">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">
            <a href="https://www.mi.com/">Xiaomi手机</a>
            <a href="https://www.mi.com/">Redmi红米</a>
            <a href="https://www.mi.com/">电视</a>
            <a href="https://www.mi.com/">笔记本</a>
            <a href="https://www.mi.com/">平板</a>
        </div>

        <div class="ht search"></div>
        <div style="clear: both;"></div>
    </div>
</div>

<div class="slider">
    <div class="container">
        <div class="sd_image">
            <img src="/images/b1.png" alt="">
        </div>
    </div>
</div>

<div class="news">
    <div class="container">
        <div class="channel"></div>
        <div class="list_item"></div>
        <div class="list_item"></div>
        <div class="list_item"></div>
    </div>
</div>
</body>
</html>
````

#### 3. 案例实现

![image-20230107175309585](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107175309585.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0px;
        }

        img{
            width: 100%;
            height: 100%;
        }

        .left{
            float: left;
        }

        .header{
            background: #333;
        }

        .header .menu{
            float: left;
        }

        .container{
            width: 1226px;
            /*0贴紧上边， auto左右居中*/
            margin: 0 auto;
        }

        .header .account{
            float: right;
            color: white;
        }

        .header a{
            color: #b0b0b0;
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
            text-decoration: none;
        }

        .header a:hover{
            color: white;
        }

        .sub_header{
            height: 100px;
        }

        .sub_header .ht{
            height: 100px;
        }

        .sub_header .logo{
            width: 234px;
            float: left;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }

        .sub_header .menu_list{
            float: left;
            line-height: 100px;
        }

        .sub_header .menu_list a{
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            font-family: Microsoft Yahei;

            text-decoration: none;
        }

        .sub_header .menu_list a:hover{
            color: #ff6700;
        }

        .sub_header .search{
            float: right;
        }

        .slider .sd_image{
            width: 1226px;
            height: 460px;
        }

        .news{
            margin-top: 14px;
        }

        .news .channel{
            width: 228px;
            height: 164px;
            padding: 3px;
            background-color: #5f5750;
        }

        .news .channel .item{
            width: 76px;
            height: 82px;
            float: left;
            text-align: center;
        }

        .news .channel .item a{
            display: inline-block;
            font-size: 12px;
            font-family: Microsoft-Yahei;
            padding-top: 18px;
            color: #fff;
            text-decoration: none;
            opacity: 0.7;
        }

        .news .channel .item a:hover{
            opacity: 1;
        }

        .news .channel .item img{
            width: 24px;
            height: 24px;
            display: block;
            margin: 0 auto 4px;
        }

        .news .list_item{
            width: 316px;
            height: 170px;
            margin-left: 14px;
        }

    </style>
</head>
<body>
<div class="header">
    <div class="container">
        <div class="menu">
            <a href="https://www.mi.com/">小米商城</a>
            <a href="https://www.mi.com/">MIUI</a>
            <a href="https://www.mi.com/">云服务</a>
            <a href="https://www.mi.com/">有品</a>
            <a href="https://www.mi.com/">开放平台</a>
        </div>
        <div class="account">
            <a href="https://www.mi.com/">登录</a>
            <a href="https://www.mi.com/">注册</a>
            <a href="https://www.mi.com/">消息通知</a>
        </div>
        <div style="clear: both"></div>
    </div>
</div>

<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.mi.com/">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">
            <a href="https://www.mi.com/">Xiaomi手机</a>
            <a href="https://www.mi.com/">Redmi红米</a>
            <a href="https://www.mi.com/">电视</a>
            <a href="https://www.mi.com/">笔记本</a>
            <a href="https://www.mi.com/">平板</a>
        </div>

        <div class="ht search"></div>
        <div style="clear: both;"></div>
    </div>
</div>

<div class="slider">
    <div class="container">
        <div class="sd_image">
            <img src="/images/b1.png" alt="">
        </div>
    </div>
</div>

<div class="news">
    <div class="container">
        <div class="channel left">
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div style="clear: both"></div>
        </div>
        <div class="list_item left">
            <img src="/images/item1.png" alt="">
        </div>
        <div class="list_item left">
            <img src="/images/item2.png" alt="">
        </div>
        <div class="list_item left">
            <img src="/images/item3.png" alt="">
        </div>
        <div style="clear: both"></div>
    </div>
</div>
</body>
</html>
````

### 小结

- 设置透明度```opacity: 0.7;```



### 3.5 CSS补充

#### 3.5.1 hover（伪类）

鼠标放到某个标签上改变标签样式

````css
/*改变字体大小，颜色，边框等*/
.c1{
    color: red;
    font-size: 18px;
    border: 3px solid red;
    height: 100px;
    width: 100px;
}

.c1: hover{
    color: green;
    font-size: 50px;
    border: 10px solid green;
}

/*改变子标签的样式，开始download不显示，鼠标移到app标签上，显示download*/
.download{
    display: none;
}

.app: hover .download{
    display: block;
}
````

#### 3.5.2 after（伪类）

在标签尾部追加

````css
.c1: after{
    content: "牛逼";
}
````

````html
<div class="c1">丁大佬</div>
````

**或者用于浮动回归文档流**

````css
.clearfix: after{
    /*空白div，所以content为空，显示为block*/
    content: "";
    display: block;
    clear: both;
}

.item{
    float: left;
}
````

````html
<div class="clearfix">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <!-- 省略 <div style="clear: both"></div> -->
</div>
````

#### 3.5.3 position

一直保持在页面某个位置，如下图右侧的菜单栏

![image-20230107181629617](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107181629617.png)

- fixed
- relative
- absolute

##### 1. fixed

固定在窗口的某个位置

###### 案例： 返回顶部

![image-20230107182213601](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107182213601.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .back{
            position: fixed;
            width: 60px;
            height: 60px;
            border: 1px solid red;

            right: 10px;
            bottom: 50px;
        }

    </style>
</head>
<body>
<div style="background-color: #b0b0b0; height: 1000px"></div>

<div class="back"></div>
</body>
</html>
````

###### 案例： 对话框

![image-20230107183710905](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107183710905.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0;
        }

        .dialog{
            position: fixed;
            height: 300px;
            width: 500px;
            background-color: white;

            /*两种皆可，一般用第二种*/
            /*left: 50%;*/
            /*margin-left: -250px;*/

            left: 0;
            right: 0;
            margin: 0 auto;
            top: 200px;
            /*z-index越大，越优先显示，1000>999，所以dialog浮在mask之上*/
            z-index: 1000;
        }

        .mask{
            background-color: black;
            position: fixed;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            opacity: 0.7;
            z-index: 999;
        }
    </style>
</head>
<body>
<div style="height: 1000px;">1213123123234568</div>
<div class="mask"></div>
<div class="dialog"></div>
</body>
</html>
````

##### 2. relative和absolute

相对于某个标签，其内部的标签在其中的位置。如果不加relative和absolute，则position默认对于整个body选择位置

![image-20230107184022113](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107184022113.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1{
            height: 300px;
            width: 500px;
            border: 1px solid red;
            margin: 100px;
            position: relative;
        }

        .c1 .c2{
            height: 59px;
            width: 59px;
            background-color: green;
            position: absolute;
            right: 0;
            top: 0;
        }
    </style>
</head>
<body>
<div class="c1">
    <div class="c2"></div>
</div>
</body>
</html>
````

###### 案例: 显示二维码

![image-20230107185631262](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107185631262.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            margin: 0px;
        }

        img{
            width: 100%;
            height: 100%;
        }

        .left{
            float: left;
        }

        .header{
            background: #333;
        }

        .header .menu{
            float: left;
        }

        .container{
            width: 1226px;
            /*0贴紧上边， auto左右居中*/
            margin: 0 auto;
        }

        .header .account{
            float: right;
            color: white;
        }

        .header a{
            color: #b0b0b0;
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
            text-decoration: none;
        }

         .header .download_app{
             position: relative
         }

        .header .download_app:hover .qcode{
            display: block;
        }

        .header .qcode{
            position: absolute;
            height: 100px;
            width: 100px;
            display: none;
        }

        .header a:hover{
            color: white;
        }

        .sub_header{
            height: 100px;
        }

        .sub_header .ht{
            height: 100px;
        }

        .sub_header .logo{
            width: 234px;
            float: left;
            /*line-heigt针对文字水平居中，对于图片没用*/
            /*line-height: 100px;*/
        }

        .sub_header .logo a{
            margin: 22px;
            display: inline-block
        }

        .sub_header .logo a img{
            height: 56px;
            width: 56px;
        }

        .sub_header .menu_list{
            float: left;
            line-height: 100px;
        }

        .sub_header .menu_list a{
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            font-family: Microsoft Yahei;

            text-decoration: none;
        }

        .sub_header .menu_list a:hover{
            color: #ff6700;
        }

        .sub_header .search{
            float: right;
        }

        .slider .sd_image{
            width: 1226px;
            height: 460px;
        }

        .news{
            margin-top: 14px;
        }

        .news .channel{
            width: 228px;
            height: 164px;
            padding: 3px;
            background-color: #5f5750;
        }

        .news .channel .item{
            width: 76px;
            height: 82px;
            float: left;
            text-align: center;
        }

        .news .channel .item a{
            display: inline-block;
            font-size: 12px;
            font-family: Microsoft-Yahei;
            padding-top: 18px;
            color: #fff;
            text-decoration: none;
            opacity: 0.7;
        }

        .news .channel .item a:hover{
            opacity: 1;
        }

        .news .channel .item img{
            width: 24px;
            height: 24px;
            display: block;
            margin: 0 auto 4px;
        }

        .news .list_item{
            width: 316px;
            height: 170px;
            margin-left: 14px;
        }

    </style>
</head>
<body>
<div class="header">
    <div class="container">
        <div class="menu">
            <a href="https://www.mi.com/">小米商城</a>
            <a href="https://www.mi.com/">MIUI</a>
            <a href="https://www.mi.com/">云服务</a>
            <a href="https://www.mi.com/">有品</a>
            <a href="https://www.mi.com/">开放平台</a>
            <a class="download_app" href="https://www.mi.com/">下载app
                <div class="qcode">
                    <img src="/static/kabi.jpg" alt="">
                </div>
            </a>
            <a href="https://www.mi.com/">MIUI</a>
            <a href="https://www.mi.com/">云服务</a>
            <a href="https://www.mi.com/">有品</a>
            <a href="https://www.mi.com/">开放平台</a>
        </div>
        <div class="account">
            <a href="https://www.mi.com/">登录</a>
            <a href="https://www.mi.com/">注册</a>
            <a href="https://www.mi.com/">消息通知</a>
        </div>
        <div style="clear: both"></div>
    </div>
</div>

<div class="sub_header">
    <div class="container">
        <div class="ht logo">
            <!--a标签为行内标签，不能设置高度和边距：变为行内或者变为行内块级（inline-block）-->
            <a href="https://www.mi.com/">
                <img src="/static/kabi.jpg" alt="">
            </a>
        </div>
        <div class="ht menu_list">
            <a href="https://www.mi.com/">Xiaomi手机</a>
            <a href="https://www.mi.com/">Redmi红米</a>
            <a href="https://www.mi.com/">电视</a>
            <a href="https://www.mi.com/">笔记本</a>
            <a href="https://www.mi.com/">平板</a>
        </div>

        <div class="ht search"></div>
        <div style="clear: both;"></div>
    </div>
</div>

<div class="slider">
    <div class="container">
        <div class="sd_image">
            <img src="/images/b1.png" alt="">
        </div>
    </div>
</div>

<div class="news">
    <div class="container">
        <div class="channel left">
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com/">
                    <img src="/static/kabi.jpg" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div style="clear: both"></div>
        </div>
        <div class="list_item left">
            <img src="/images/item1.png" alt="">
        </div>
        <div class="list_item left">
            <img src="/images/item2.png" alt="">
        </div>
        <div class="list_item left">
            <img src="/images/item3.png" alt="">
        </div>
        <div style="clear: both"></div>
    </div>
</div>
</body>
</html>
````

#### 3.5.4 border

![image-20230107190330881](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230107190330881.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1{
            height: 300px;
            width: 500px;
            margin: 100px;
            background-color: #b0b0b0;
            /*透明色，预先设置透明边框防止hover后位置变动*/
            border-left: 3px solid transparent;
        }

        .c1:hover{
            border-left: 3px solid red;
        }
    </style>
</head>
<body>
<div class="c1">菜单</div>
</body>
</html>
````

#### 3.5.5 背景色

````css
.c1{
    background-color: #5f5750;
}
````



## 4. BootStrap

别人已经帮我们写好的CSS样式，如果想要使用BootStrap: 

- 下载BootStrap
- 使用
  - 在页面上引入BootStrap
  - 编写HTML时，按照BootStrap的规定来编写+自定制

### 4.1 初试BootStrap

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--  开发版本  -->
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">

    <!--  压缩版本  -->
    <!--  <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.min.css">  -->
</head>
<body>
<input type="button" value="提交">
<input type="button" value="提交" class="btn btn-success">
<input type="button" value="提交" class="btn btn-primary">
<input type="button" value="提交" class="btn btn-danger">
<input type="button" value="提交" class="btn btn-danger btn-xs">
</body>
</html>
````

![image-20230108160030184](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108160030184.png)

### 4.2 导航

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }
    </style>
</head>
<body>

<div class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">中国联通</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                <li><a href="#">广西</a></li>
                <li><a href="#">上海</a></li>
                <li><a href="#">深圳</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">登录</a></li>
                <li><a href="#">注册</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</div>

</body>
</html>
````

### 4.3 栅格系统

- 把整个页面横向分为12个格子

- 分类

  非响应和sm，md用的多

  - 响应式：当页面足够宽时为水平排列，当页面不够宽则堆叠。lg到sm宽度阈值逐渐变小，大于该阈值栅格才生效，小于阈值则堆叠。

    ```.col-lg```： 1170px,

     ```.col-md```: 970px, 

    ```.col-sm```:750px.

  - 非响应式：永远随着浏览器宽度变化，总是水平排列

    ````html
    <div class="col-xs-2" style="background-color: red">1</div>
    <div class="col-xs-10" style="background-color: green ">2</div>
    ````

  - 列偏移

    用空白填充左侧的两格

    ````html
  <div class="col-sm-offset-2 col-sm-6" style="background-color: red">1</div>
    ````


### 4.4 container

全宽度container，无宽度限制平铺整个页面：

````html
<div class="container-fluid">
    <div class="col-sm-9">左边</div>
    <div class="col-sm-3">右边</div>
</div>
````

![image-20230108163619107](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108163619107.png)

固定宽度container：

````html
<div class="container">
    <div class="col-sm-9">左边</div>
    <div class="col-sm-3">右边</div>
</div>
````

![image-20230108163609676](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108163609676.png)

### 4.5 面板



### 案例：博客

![image-20230108162851518](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108162851518.png)

![image-20230108164433155](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108164433155.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }
    </style>
</head>
<body>

<div class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">中国联通</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                <li><a href="#">广西</a></li>
                <li><a href="#">上海</a></li>
                <li><a href="#">深圳</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">登录</a></li>
                <li><a href="#">注册</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</div>

<div class="container-fluid clearfix">
    <div class="col-sm-9">
        <div class="media">
            <div class="media-left">
                <a href="#">
                    <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                         src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODU5MzUxMjk0OSB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4NTkzNTEyOTQ5Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                         data-holder-rendered="true" style="width: 64px; height: 64px;">
                </a>
            </div>
            <div class="media-body">
                <h4 class="media-heading">Top aligned media</h4>
                <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo.
                    Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi
                    vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                    penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
            </div>
        </div>

        <div class="media">
            <div class="media-left">
                <a href="#">
                    <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                         src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODU5MzUxMjk0OSB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4NTkzNTEyOTQ5Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                         data-holder-rendered="true" style="width: 64px; height: 64px;">
                </a>
            </div>
            <div class="media-body">
                <h4 class="media-heading">Top aligned media</h4>
                <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo.
                    Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi
                    vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                    penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
            </div>
        </div>

        <div class="media">
            <div class="media-left">
                <a href="#">
                    <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                         src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODU5MzUxMjk0OSB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4NTkzNTEyOTQ5Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                         data-holder-rendered="true" style="width: 64px; height: 64px;">
                </a>
            </div>
            <div class="media-body">
                <h4 class="media-heading">Top aligned media</h4>
                <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo.
                    Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi
                    vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                    penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
            </div>
        </div>
        
        <ul class="pagination">
            <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">«</span></a></li>
            <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
            <li><a href="#">2</a></li>
            <li><a href="#">3</a></li>
            <li><a href="#">4</a></li>
            <li><a href="#">5</a></li>
            <li><a href="#" aria-label="Next"><span aria-hidden="true">»</span></a></li>
        </ul>
    </div>
    <div class="col-sm-3">
        <div class="panel panel-default">
            <div class="panel-heading">最新推荐</div>
            <div class="panel-body">
                Panel content
            </div>
        </div>

        <div class="panel panel-primary">
            <div class="panel-heading">Panel heading without title</div>
            <div class="panel-body">
                Panel content
            </div>
        </div>

        <div class="panel panel-danger">
            <div class="panel-heading">Panel heading without title</div>
            <div class="panel-body">
                Panel content
            </div>
        </div>
    </div>
</div>

</body>
</html>
````

### 案例：登录界面

![image-20230108170303535](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108170303535.png)

- 宽度+区域居中
- 内边距
- BootStrap表单

![image-20230108173234469](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108173234469.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .login-title {
            text-align: center;
            font-size: 25px;
            font-weight: 1000;
            margin-bottom: 10px;
        }

        .form {
            box-shadow: 10px 10px 10px #aaa;
            padding: 20px 40px;
            width: 500px;
            border: 1px solid #dddddd;
            border-radius: 5px;
            margin: 20px auto;
        }

        .form a {
            float: right;
        }
    </style>
</head>
<body>
<div class="container">
    <div class="form clearfix">
        <form>
            <div class="login-title">用户登录</div>
            <div class="form-group">
                <label for="exampleInputEmail1">用户名或手机号</label>
                <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Username">
            </div>
            <div class="form-group">
                <label for="exampleInputPassword1">密码</label>
                <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
            </div>
            <button type="submit" class="btn btn-primary">登录</button>
            <a href="https://www.baidu.com/">忘记密码？</a>
        </form>
    </div>

</div>
</body>
</html>
````

### 案例：后台管理

![image-20230108180241091](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108180241091.png)

- 导航
- 面板+按钮
- 面板+表格

![image-20230108182749746](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230108182749746.png)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }

        .data-area {
            margin-top: 20px;
        }
    </style>
</head>
<body>
<div class="navbar navbar-default">
    <div class="container">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">中国联通管理系统</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li><a href="#">广西</a></li>
                <li><a href="#">上海</a></li>
            </ul>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">登录</a></li>
                <li><a href="#">注册</a></li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</div>

<div class="container">
    <div class="panel panel-default">
        <div class="panel-heading">表单区域</div>
        <div class="panel-body">
            <form class="form-inline">
                <div class="form-group">
                    <label class="sr-only" for="exampleInputEmail3">Email address</label>
                    <input type="email" class="form-control" id="exampleInputEmail3" placeholder="Email">
                </div>
                <div class="form-group">
                    <label class="sr-only" for="exampleInputPassword3">Password</label>
                    <input type="password" class="form-control" id="exampleInputPassword3" placeholder="Password">
                </div>
                <button type="submit" class="btn btn-success">保 存</button>
            </form>
        </div>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading">数据列表</div>
        <!--    <div class="panel-body">
                    注意事项：xxxxxxx
                </div>                      -->
        <table class="table table-bordered table-striped table-hover">
            <thead>
            <tr>
                <th>#</th>
                <th>First Name</th>
                <th>Last Name</th>
                <th>操作</th>
            </tr>
            </thead>
            <tbody>
            <tr>
                <th scope="row">1</th>
                <td>Mark</td>
                <td>Otto</td>
                <td>
                    <a class="btn btn-primary btn-xs">编辑</a>
                    <a class="btn btn-danger btn-xs">删除</a>
                </td>
            </tr>
            <tr>
                <th scope="row">2</th>
                <td>Jacob</td>
                <td>Thornton</td>
                <td>
                    <a class="btn btn-primary btn-xs">编辑</a>
                    <a class="btn btn-danger btn-xs">删除</a>
                </td>
            </tr>
            <tr>
                <th scope="row">3</th>
                <td>Larry</td>
                <td>the Bird</td>
                <td>
                    <a class="btn btn-primary btn-xs">编辑</a>
                    <a class="btn btn-danger btn-xs">删除</a>
                </td>
            </tr>
            </tbody>
        </table>
    </div>

    <ul class="pagination">
            <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">«</span></a></li>
            <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
            <li><a href="#">2</a></li>
            <li><a href="#">3</a></li>
            <li><a href="#">4</a></li>
            <li><a href="#">5</a></li>
            <li><a href="#" aria-label="Next"><span aria-hidden="true">»</span></a></li>
        </ul>
</div>

</body>
</html>
````

### 4.6 图标

- BootStrap提供，但不多
- Font Awesome 4.7 https://fontawesome.com.cn/faicons/



## 5. JavaScript

- 运行在浏览器上的编程语言，浏览器就是JS的解释器。

- 让页面变得动态，给对象绑定一个事件

- BootStrap依赖JavaScript的类库：jQuery，第三方模块

  - 下载jQuery，在页面上应用jQuery

  - 在页面上应用BootStrap的JavaScript类库

    ````html
    <script src="static/js/jquery-3.6.3.min.js"></script>
    <script src="static/plugins/bootstrap-3.4.1/js/bootstrap.min.js"></script>
    ````

- DOM和BOM：相当于编程语言内置的模块
- JS的意义：让程序实现动态效果



### 5.1 JS基础

#### 5.1.1 代码位置

可以放在header里，也可以放在body的最尾部，一般放在body的最尾部。因为HTML是顺序执行，如果JS放在header中，将会在页面展示前执行。

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menus {
            width: 200px;
            border: 1px solid red;
        }

        .menus .header {
            background-color: gold;
            padding: 20px 10px;
        }
    </style>

    <!-- 位置1 -->
    <script type="text/javascript">
        // 编写JS代码
    </script>
</head>
<body>
<div class="menus">
    <div class="header" onclick="myFunc()">大标题</div>
    <div class="items">内容</div>
</div>

<!-- 位置2 -->
<script type="text/javascript">
    function myFunc() {
        confirm("是否要继续？")
    }
</script>
</body>
</html>
````

JS的存在形式：

- 直接写在当前HTML中

  ````html
  <script type="text/javascript">
      // 编写JS代码
  </script>
  ````

- 写在文件中进行导入

  ````html
  <script src="static/my.js"></script>
  <script type="text/javascript">
      function myFunc() {
          confirm("是否要继续？")
      }
  </script>
  ````

#### 5.1.2 变量

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    var name = "丁丁";
    // 输出到浏览器f12的console里
    console.log(name);
</script>
</body>
</html>
````

#### 5.1.3 字符串类型

````javascript
// 声明
var name = "丁丁";
var name = String("丁丁");

// 常见功能
var name = "1234";
var v1 = name.length;
var v2 = name[0];	// name.charAt(0)
var v3 = name.trim();	// 去除空白
var v4 = name.substring(0, 2);	// 前取后不取，输出‘12’
````

#### 案例：跑马灯

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<span id="txt">欢迎1234567</span>
<script>
    function show() {
        // 1.去HTML中找到某个标签并获取他的内容（DOM）
        var tag = document.getElementById("txt");
        var datastring = tag.innerText;

        // 2.动态起来， 把文本中的第一个字符放到字符串最后
        var firstChar = datastring[0];
        var otherString = datastring.substring(1, datastring.length);
        var newText = otherString + firstChar;

        // 3.在HTML标签中更新内容
        tag.innerText = newText;
    }

    // JS中的定时器，如每1s执行一次show函数
    setInterval(show, 1000);
</script>

</body>
</html>
````

#### 5.1.4 数组

````javascript
// 定义
var v1 = [11, 22, 33, 44];
var v2 = Array([11, 22, 33, 44]);


// 操作
var v1 = [11, 22, 33, 44];
var v1_1 = v1[1];
v1[0] = "丁丁";

v1.push("联通");			// 在尾部追加	[11, 22, 33, 44, "联通"]
v1.unshift("电信");		// 在头部追加	["电信", 11, 22, 33, 44]
v1.splice(1, 0, "中国");	// 在指定位置插入	splice(位置，削掉的数目:插入不削减，插入的项目)	[11, "中国"， 22， 33， 44]

v1.pop();			// 从尾部删除，并返回删除的元素
v1.shift();			// 从头部删除，并返回删除的元素
v1.splice(1, 0);	// 指定位置删除 splice(位置，削掉的数目:削掉1位)


// 遍历
var v1 = [11, 22, 33, 44];

for(var idx in v1){
    // idx为索引
    data = v1[idx];
}

for(var i = 0; i < v1.length; i++){
    
}
````

注意：break和continue

#### 案例：动态数据

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul id="city">
    <!-- 通过JS添加内容 -->
</ul>

<script type="text/javascript">

    // 发送网络请求，获取数据存到cityList
    var cityList = ["北京", "上海", "深圳"];
    for(var idx in cityList){
        var text = cityList[idx];
        // 创建<li></li>标签
        var tag = document.createElement('li');
        // 在li标签中写入内容
        tag.innerText = text;
        // 添加到id=city的标签中
        var parentTag = document.getElementById("city");
        parentTag.appendChild(tag);
    }
</script>
</body>
</html>
````

#### 5.1.5 对象

````javascript
// 定义
var info = {
    name: "丁丁"，
    age: 18
};

// 操作
var v1 = info.age;
info.name = "123";
delete info.name;

// 遍历
for(var key in info){
    data = info[key];
}
````

#### 案例：动态表格

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<table border="1">
    <thead>
    <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Age</th>
    </tr>
    </thead>
    <tbody id="body">

    </tbody>
</table>

<script type="text/javascript">
    // 网络请求获取数据
    var dataList = [
        {id: 1, name: "Ding", age: 19},
        {id: 2, name: "Ding", age: 19},
        {id: 3, name: "Ding", age: 19},
        {id: 4, name: "Ding", age: 19},
        {id: 5, name: "Ding", age: 19},
    ];
    for (var idx in dataList) {
        var info = dataList[idx];
        var trTag = document.createElement("tr");
        for (var key in info) {
            var text = info[key];
            var tdTag = document.createElement("td");
            tdTag.innerText = text;
            trTag.appendChild(tdTag);
        }
        var bodyTag = document.getElementById("body");
        bodyTag.appendChild(trTag);
    }

</script>
</body>
</html>
````

#### 5.1.6 条件语句

````javascript
if (...) {
    ...
} else {
    ...
}

if (...) {
    ...
} else if (...) {
    ...
} else {
    ...
}
````

#### 5.1.7 函数

````javascript
// 定义
function func() {
    ...
}
    
// 执行
func();
````



### 5.2 DOM

DOM是一个模块，可以对HTML页面中的标签进行操作

````javascript
// 根据ID获取标签
var tag = document.getElementById("tag");
// 获取并修改标签中的文本
tag.innerText = "123123123";

// 创建标签
var newTag = document.creatElement("div");
// 往标签里加子标签
tag.appendChild(newTag);
````

````html
<ul id="city">
    <!-- 通过JS添加内容 -->
</ul>

<script type="text/javascript">
    // 找到id为"city"的标签
    var tag = document.getElementById("city");
    // 创建要添加的子标签
    var newTag = document.creatElement("li");
    newTag.innerText = "123";
    // 添加子标签
    tag.appendChild(newTag);
</script>
````

#### 5.2.1 事件绑定

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" placeholder="请输入内容" id="txtUser">

<input type="button" value="点击添加" onclick="addCityInfo()">
<!-- ondbclick双击，onclick单击 -->
<ul id="city">

</ul>

<script type="text/javascript">
    function addCityInfo() {
        // 获取输入框中用户输入的数据
        var txtTag = document.getElementById("txtUser");
        // 获取用户输入的内容
        var content = txtTag.value;
        // 判断用户输入是否为空
        if (content.length == 0) {
            alert("输入不能为空")
            return;
        }
        // 创建子标签
        var newTag = document.createElement("li");
        newTag.innerText = content;
        // 添加子标签
        var parentTag = document.getElementById("city");
        parentTag.appendChild(newTag);
        // 将input框内容清空
        txtTag.value = "";
    }
</script>
</body>
</html>
````

![image-20230115170109134](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230115170109134.png)

DOM可以实现很多功能，但是比较繁琐。页面上的效果用jQuery，vue.js，react.js等来实现。



### 5.3 jQuery

是JS的第三方类库。

- 基于jQuery自己开发一个功能
- 现成的工具以来jQuery

#### 5.3.1 快速上手

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1 id="txt">中国联通</h1>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    // 找到id为txt的标签并修改内容。
    // 等同于document.getElementById("txt").innerText = "广西移动“
    $("#txt").text("广西移动");
</script>
</body>
</html>
````

#### 5.3.2 寻找标签（直接寻找）

- 基于ID寻找标签：ID选择器

  ````html
  <h1 id="txt">中国联通</h1>
  ````

  ````javascript
  $("#txt")
  ````

- 基于样式寻找标签：样式选择器

  ````html
  <h1 class="c1">中国联通1</h1>
  <h1 class="c1">中国联通2</h1>
  <h1 class="c2">中国联通3</h1>
  ````

  ````javascript
  $(".c1")
  ````

- 标签选择器

  ````html
  <h1 class="c1">中国联通1</h1>
  <div class="c1">中国联通2</div>
  <h1 class="c2">中国联通3</h1>
  ````

  ````javascript
  $("h1")
  ````

- 层级选择器

  ````html
  <h1 class="c1">中国联通1</h1>
  <h1 class="c1">
      <div class="c2">
          <span></span>
          <a></a>
      </div>
  </h1>
  <h1 class="c2">中国联通3</h1>
  ````

  ````javascript
  $(".c1 .c2 a")
  ````

- 多选择器

  ````html
  <h1 class="c1">中国联通1</h1>
  <h1 class="c1">
      <div class="c3">
          <span></span>
          <a></a>
      </div>
  </h1>
  <h1 class="c2">中国联通3</h1>
  <ul>
      <li>xx</li>
      <li>xx</li>
  </ul>
  ````

  ````javascript
  $(".c3, .c2, li")
  ````

- 属性选择器

  ````html
  <input type="text" name="n1" />
  <input type="text" name="n2" />
  <input type="text" name="n1" />
  ````

  ````javascript
  $("input[name='n1']")
  ````

#### 5.3.3 间接寻找

- 找到上，下一个兄弟

  ````html
  <div>
      <div>北京</div>
      <div id="c1">上海</div>
      <div>深圳</div>
      <div>广州</div>
  </div>
  ````

  ````javascript
  $("#c1").prev()			// 上一个
  $("#c1")
  $("#c1").next()			// 下一个
  $("#c1").next().next()	// 下一个的下一个
  $("#c1").siblings()		// 所有的兄弟，不包括自己
  ````

- 找父子

  ````html
  <div>
      <div>
          <div>北京</div>
          <div id="c1">
              上海
              <div class="p10">青浦区</div>
              <div>浦东新区</div>
          </div>
          <div>深圳</div>
          <div>广州</div>
      </div>
      <div>
          <div>陕西</div>
          <div>山西</div>
          <div>河北</div>
          <div>河南</div>
      </div>
  </div>
  ````

  ````javascript
  $("#c1").parent()			// 父亲
  $("#c1").parent().parent()	// 父亲的父亲
  
  $("#c1").children()			// 所有的孩子
  $("#c1").children(".p10")	// 所有的孩子中寻找class="p10"的标签
  
  $("#c1").find(".p10")				// 所有的子孙中寻找class="p10"的标签
  $("#c1").find("div")				// 所有的子孙中寻找div标签
  ````

#### 案例：菜单的切换

点击标签展开收起

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menus {
            height: 800px;
            width: 200px;
            border: 1px solid red;
        }

        .menus .header {
            background-color: gold;
            padding: 10px 5px;
            border-bottom: 1px dotted #dddddd;
            cursor: pointer;        /*光标移上去变成点击的图案*/
        }

        .menus .content a {
            display: block;
            padding: 5px 5px;
            border-bottom: 1px dotted #dddddd;
        }

        .hide {
            display: none;
        }
    </style>
</head>
<body>
<div class="menus">
    <div class="item">
        <div class="header" onclick="clickMe(this);">上海</div>
        <div class="content hide">
            <a href="">宝山区</a>
            <a href="">青浦区</a>
            <a href="">浦东区</a>
            <a href="">普陀区</a>
        </div>
    </div>

    <div class="item">
        <div class="header" onclick="clickMe(this);">北京</div>
        <div class="content hide">
            <a href="">海淀区</a>
            <a href="">朝阳区</a>
            <a href="">大兴区</a>
            <a href="">昌平区</a>
        </div>
    </div>
</div>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    function clickMe(self) {
        // 选择当前点击的标签，找到他的下一个兄弟标签，检查是否有hide
        var hasHide = $(self).next().hasClass("hide");
        if (hasHide) {
            // 移除一个样式
            $(self).next().removeClass("hide");
        } else {
            // 添加一个样式
            $(self).next().addClass("hide");
        }

    }
</script>
</body>
</html>
````

点击标签，展开自己收起其他所有标签

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menus {
            height: 800px;
            width: 200px;
            border: 1px solid red;
        }

        .menus .header {
            background-color: gold;
            padding: 10px 5px;
            border-bottom: 1px dotted #dddddd;
            cursor: pointer;        /*光标移上去变成点击的图案*/
        }

        .menus .content a {
            display: block;
            padding: 5px 5px;
            border-bottom: 1px dotted #dddddd;
        }

        .hide {
            display: none;
        }
    </style>
</head>
<body>
<div class="menus">
    <div class="item">
        <div class="header" onclick="clickMe(this);">上海</div>
        <div class="content">
            <a href="">宝山区</a>
            <a href="">青浦区</a>
            <a href="">浦东区</a>
            <a href="">普陀区</a>
        </div>
    </div>

    <div class="item">
        <div class="header" onclick="clickMe(this);">北京</div>
        <div class="content hide">
            <a href="">海淀区</a>
            <a href="">朝阳区</a>
            <a href="">大兴区</a>
            <a href="">昌平区</a>
        </div>
    </div>

    <div class="item">
        <div class="header" onclick="clickMe(this);">上海2</div>
        <div class="content hide">
            <a href="">宝山区</a>
            <a href="">青浦区</a>
            <a href="">浦东区</a>
            <a href="">普陀区</a>
        </div>
    </div>

    <div class="item">
        <div class="header" onclick="clickMe(this);">北京2</div>
        <div class="content hide">
            <a href="">海淀区</a>
            <a href="">朝阳区</a>
            <a href="">大兴区</a>
            <a href="">昌平区</a>
        </div>
    </div>
</div>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    function clickMe(self) {
        // 展示自己下面的标签
        $(self).next().removeClass("hide");
        // 找父亲的所有兄弟，再去每个子孙中找到content添加hide
        $(self).parent().siblings().find(".content").addClass("hide");
    }
</script>
</body>
</html>
````

#### 5.3.4 操作样式

- addClass
- removeClass
- hasClass

#### 5.3.5 值的操作

````html
<div id="c1">内容</div>
````

````javascript
$("#c1").text()				  // 获取文本内容
#("#c1").text("新内容")		// 设置文本内容
````

````html
<input type="text" id="c2" />
````

````javascript
$("#c2").val()			  // 获取用户输入的值
$("#c2").val("新内容")		// 设置值
````

#### 案例：获取输入的值

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" id="txtUser" placeholder="用户名" />
<input type="text" id="txtEmail" placeholder="邮箱" />
<input type="button" value="提交" onclick="getInfo()" />

<ul id="view">
</ul>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    function getInfo() {
        // 获取用户输入的用户名和密码
        var username = $("#txtUser").val();
        var email = $("#txtEmail").val();

        var dataString = username + "-" + email;

        // 创建li标签，在li的内部写入内容
        var newli = $("<li>").text(dataString);

        // 把新创建的li标签添加到ul里
        $("#view").append(newli);
    }
</script>
</body>
</html>
````

#### 5.3.6 绑定事件

点击并获取文本

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
</ul>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    // 选择所有li标签，绑定函数
    $("li").click(function () {
        // 选择当前点击的标签：this
        var text = $(this).text();
        console.log(text);
    });
</script>

</body>
</html>
````

在jQuery中可以在当前页面删除某个标签。

````html
<div id="c1">内容</div>
````

````javascript
$("#c1").remove;
````

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
</ul>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    $("li").click(function () {
        // 删除点击的标签
        $(this).remove();
    });
</script>

</body>
</html>
````

当页面框架加载完成后执行的代码：

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
    <!-- 加载很大的图片很费时间，想要在加载完成前就拥有点击删除的功能 -->
    <li>很大的图片</li>
</ul>

<script src="static/jquery-3.6.3.min.js"></script>
<script type="text/javascript">
    // 当页面的框架加载完成之后，自动执行
    $(function () {
        $("li").click(function () {
            $(this).remove();
        });
    });
</script>

</body>
</html>
````

#### 案例：表格操作

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<table border="1">
    <thead>
    <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>操作</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>1</td>
        <td>丁丁</td>
        <td>
            <input type="button" value="删除" class="delete"/>
        </td>
    </tr>
    <tr>
        <td>1</td>
        <td>丁丁</td>
        <td>
            <input type="button" value="删除" class="delete"/>
        </td>
    </tr>
    <tr>
        <td>1</td>
        <td>丁丁</td>
        <td>
            <input type="button" value="删除" class="delete"/>
        </td>
    </tr>
    <tr>
        <td>1</td>
        <td>丁丁</td>
        <td>
            <input type="button" value="删除" class="delete"/>
        </td>
    </tr>
    </tbody>
</table>

<script src="static/jquery-3.6.3.min.js"></script>
<script>
    $(function () {
        // 找到所有class="delete"的标签
        $(".delete").click(function () {
            // 删除当前行的数据
            $(this).parent().parent().remove();
        });
    });
</script>
</body>
</html>
````



## 6. 前端整合

- HTML
- CSS
- JavaScript, jQuery
- BootStrap



## 7. 初识网站

- 默认编写的网站是静态的效果

- 是用Web框架的功能让网页动态更新

  ![image-20230119161948716](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230119161948716.png)

数据存储：

- txt文件

- excel文件

- 数据库管理系统：MySQL，Oracle，SQLServer，DB2，Access

  ![image-20230119162609537](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230119162609537.png)



## 8. MySQL

- MySQL安装&配置
- MySQL的启动与关闭
- 指令（*）
- Python第三方模块，发送指令并获取MySQL返回的结果

### 8.1 启动MySQL

两种方式：

- 临时启动（不建议）

  ````
  .\mysqld.exe
  ````

- 制作成windows服务，服务来进行关闭和开启

  ````shell
  .\mysqld --install mysql57
  net start mysql57
  net stop mysql57
  ````

![image-20230206153746421](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230206153746421.png)

### 8.2 使用自带的工具连接MySQL

本机地址，端口号，用户名，密码

````shell
.\mysql -h 127.0.0.1 -P 3306 -u root -p
````

设置密码

````mysql
set password = password('password');
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
````

查看已有的数据库

````mysql
show databases;
````

退出

````mysql
exit
quit
\q
````

忘记密码：

- 关闭MySQL服务

- 修改配置文件，以无账号模式启动：在my.ini后添加一句

  ````ini
  skip-grant-tables=1
  ````

- 重新启动MySQL服务
- 再次登录MySQL，无需密码，然后设置密码
- 停止MySQL服务
- 删除配置文件中的无账号模式
- 重新启动MySQL服务

### 8.3 MySQL指令

#### 8.3.1 数据库管理

- 查看已有的数据库

  ````mysql
  show databases;
  ````

- 创建数据库

  ````mysql
  create database 数据库名字 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
  ````

- 删除数据库

  ````mysql
  drop database 数据库名字;
  ````

#### 8.3.2 数据表管理

- 进入数据库

  ````mysql
  use 数据库名字;
  ````

- 查看数据库下所有的表

  ````mysql
  show tables;
  ````

- 创建表

  ````mysql
  create table 表名称(
  	列名称 类型,
      列名称 类型,
      列名称 类型
  ) default charset=utf8;
  ````

  创建如下表格

  | id   | name | age  |
  | ---- | ---- | ---- |
  | 1    | asd  | 23   |

  ```` mysql
  create table tb1(
      id int, 
      name varchar(16), 
      age int
  ) default charset=utf8;
  ````

  ````mysql
  create table tb1(
      id int default 3,			-- 插入数据时，id列的值默认3 
      name varchar(16) not null, 	-- 不允许为空 
      age int null,				-- 允许为空（默认）
  ) default charset=utf8;
  ````

  ````mysql
  create table admin(
      id int not null auto_increment primary key, 		-- 设置为主键，不允许为空，不允许重复，内部维护id自增
      username varchar(64), 
      password varchar(64),
  ) default charset=utf8;
  ````

- 删除表

  ````mysql
  drop table 表名称;
  ````

- 查看表结构（description）

  ````mysql
  desc 表名称;
  ````

  ![image-20230208173300336](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230208173300336.png)

  

- 常用数据类型：

  - tinyint： 【默认】有符号（有正负）取值范围 -128 ~ 127，无符号（只有正数）取值范围 0 ~ 255。

    ````mysql
    create table tb1(
        id int not null auto_increment primary key,
        name varchar(16), 
        age tinyint unsigned			-- 设置为无符号
    ) default charset=utf8;
    ````

  - int：【默认】有符号取值范围 -2147483648 ~ 2147483647，无符号取值范围 0 ~ 4294967295

  - bigint：更大

  练习：

  ````mysql
  # 创建表
  create table tb2(
      id bigint not null auto_increment primary key,
      salary int, 
      age tinyint
  ) default charset=utf8;
  
  # 插入数据
  insert into tb2(salary, age) values(10000, 18);
  insert into tb2(salary, age) values(20000, 28);
  insert into tb2(salary, age) values(30000, 38), (40000, 48);
  
  # 查看表中的数据
  select * from tb2;
  ````

  - float：单精度小数部分只能精确到后面6位，加上小数点前的一位，即有效数字为7位， 单精度内存占4个字节。缺点: 但是当值很大或很小的时候，它将变得不精确。

  - doible：双精度小数部分能精确到小数点后的15位，加上小数点前的一位 有效位数为16位，, 双精度内存占8个字节。缺点: double 双精度是消耗内存的,并且是 float 单精度的两倍! ,double 的运算速度比 float 慢得多, 因为double 尾数比float 的尾数多。

  - decimal：float保存的值有可能是这个值的近似值，而不是这个值的真实值。如 0.1在二进制中是没有办法保存真实值的。所以通常用decimal

    ````mysql
    # 准确的小数值，m是数字总个数（负号不算）多于设定位数会报错，d是小数点后个数，多于设定位数会自动四舍五入。m最大值为65，d最大值为30。
    create table 表名称(
    	id int not null primary key auto_increment,
        salary decimal(m, d)
    ) default charset=utf8;
    
    # 插入
    insert into tb3(salary) values(1.28), (5.289), (6.282);
    ````

    ![image-20230208175346745](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230208175346745.png)

    因为小数点后只有两位，多于两位的被四舍五入

  - char：定长字符串，浪费空间，但查询速度快，最大可容纳255个字符

    ````mysql
    create table tb4(
    	id int not null primary key auto_increment,
        mobile char(11)				-- 固定用11个字符的长度进行存储
    ) default charset=utf8;
    ````

  - varchar：变长字符串，节省空间，查询速度慢，最大可容纳65535字节

    ````mysql
    create table tb5(
    	id int not null primary key auto_increment,
        mobile varchar(11)				-- 按照实际长度存储，但是上限是11位
    ) default charset=utf8;
    ````

  - text：长文本用。可存储65535（2^16-1）个字符

    ````mysql
    create table tb5(
    	id int not null primary key auto_increment,
        title varchar(128),
        content text
    ) default charset=utf8;
    ````

  - mediumtext：16777215（2^24-1）个字符

  - longtext：4294967295（2^32-1）个字符（4GB）

  - datetime：存储年月日时分秒：

    YYYY-MM-DD HH:MM:SS

  - date：存储年月日：

    YYYY-MM-DD

  练习：用户表

  ````mysql
  create table tb7(
  	id int not null primary key auto_increment,
      name varchar(64) not null,
      password char(64) not null,
      email varchar(64) not null,
      age tinyint,
      salary decimal(10, 2),
      ctime datetime
  ) default charset=utf8;
  
  insert into tb7(name, password, email, age, salary, ctime) values("Selene", "XXXXXXX", "XXX@xx.com", 19, 1000.20, "2011-11-11 11:11:00");
  ````

  ![image-20230208184002638](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230208184002638.png)

#### 8.3.3 数据行操作

- 新增数据

  ````mysql
  insert into 表名称(列1) values(值1);
  insert into 表名称(列1, 列2) values(值1, 值2), (值3, 值4);
  ````

- 删除数据

  ````mysql
  delete from 表名;					 		-- 删除该表的所有数据
  delete from 表名 where 条件;			   -- 删除表中符合条件的数据 
  
  delete from tb7 where id = 3;
  delete from tb7 where id = 3 or name = "Selene";
  delete from tb7 where id >= 3;
  delete from tb7 where id != 3;	
  delete from tb7 where id in (1, 5);		-- 删除表中id在列表中的数据
  ````

- 修改数据

  ````mysql
  update 表名 set 列=值;
  update 表名 set 列=值, 列=值;
  update 表名 set 列=值 where 条件;
  
  update tb7 set password="111";					-- 把表中所有的password列的值改成111
  update tb7 set password="111" where id = 3;		-- 把表中id=3的password列的值改成111
  update tb7 set age=age+10 where id = 3;
  ````

- 查询表中数据

  - 查看表中所有数据

    ````mysql
    select * from 表名;
    ````

    ![image-20230208174734410](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230208174734410.png)

  - 查看某些列某些行的数据

    ````mysql
    select 列1, 列2 from 表名;
    select 列1, 列2 from 表名 where 条件;
    
    select age, email from tb7 where id in (1,3);
    ````

### 8.4 Python连接MySQL

##### 案例：员工管理

- 使用MySQL内置工具：

  - 创建数据库

  - 创建表：admin

    | id               | username       | password       | mobile         |
    | ---------------- | -------------- | -------------- | -------------- |
    | 主键，自增，整型 | 字符串，不为空 | 字符串，不为空 | 字符串，不为空 |

- Python代码实现：

  - 添加数据

    ````python
    import pymysql
    
    # 1. 连接mysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    
    
    # 2. 发送指令
    cursor.execute("insert into admin(username, password, mobile) values('Selene', 'asd12311', '11122233333')")
    
    # 千万不要用字符串格式化去做SQL拼接，容易被SQL注入，不能用format
    sql = "insert into admin(username, password, mobile) values(%s, %s, %s)"
    cursor.execute(sql, ['Asd', "asd", "11122333333"])
    
    sql = "insert into admin(username, password, mobile) values(%(n1)s, %(n2)s, %(n3)s)"
    cursor.execute(sql, {"n1": "AAA", "n2":"aAA", "n3":"1233331112"})
    
    conn.commit()
    
    
    # 3. 关闭连接
    cursor.close()
    conn.close()
    ````

    根据用户输入录入数据库

    ````python
    import pymysql
    
    while True:
        user = input("用户名：")
        if user.upper() == "Q":
            break
        pwd = input("密码：")
        mobile = input("手机号：")
    
    # 1. 连接mysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    
    
    # 2. 发送指令
    sql = "insert into admin(username, password, mobile) values(%s, %s, %s)"
    cursor.execute(sql, [user, pwd, mobile])
    conn.commit()
    
    
    # 3. 关闭连接
    cursor.close()
    conn.close()
    ````

  - 删除用户

    ````python
    import pymysql
    
    # 1. 连接mysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    
    # 2. 执行删除指令
    sql = "delete from admin where id = %s"
    cursor.execute(sql, [3,])
    conn.commit()
    
    # 3. 关闭连接
    cursor.close()
    conn.close()
    ````
    
  - 查看数据
  
    ````python
    import pymysql
    
    # 1. 连接mysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    
    
    # 2. 执行查询的指令
    sql = "select * from admin where id >= %s"
    cursor.execute(sql, [1,])
    data_list = cursor.fetchall()           # 获取sql查询到的所有数据:[字典, 字典, ]
    # res = cursor.fetchone()                 只获取查询到的第一条数据
    for row_dict in data_list:
        print(row_dict)
    
    
    # 3. 关闭连接
    cursor.close()
    conn.close()
    ````
  
  - 修改数据
  
    ````mysql
    import pymysql
    
    # 1. 连接mysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    
    # 2. 执行修改指令
    sql = "update admin set mobile = %s where id = %s"
    cursor.execute(sql, ["12222333222", 4, ])
    conn.commit()
    
    # 3. 关闭连接
    cursor.close()
    conn.close()
    ````

**在进行增，删，改，操作时，一定要进行commit，不然数据库无法接收到数据。**

**在查询时，不需要commit，执行fetchall / fetchone**

**对于SQL语句不要用Python的字符串格式化进行拼接，会被SQL注入，一定要用execute+参数**



##### 案例：Flask+MySQL

- 新增用户

  ````python
  from flask import Flask, render_template, request
  import pymysql
  
  app = Flask(__name__)
  
  
  @app.route("/add/user", methods=["GET", "POST"])
  def add_user():
      if request.method == "GET":
          return render_template("add_user.html")
      username = request.form.get("user")
      pwd = request.form.get("pwd")
      mobile = request.form.get("mobile")
  
      # 1. 连接MySQL
      conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8", db="selenexx_test")
      cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
  
      # 2. 执行SQL
      sql = "insert into user_info(name, password, mobile) values (%s, %s, %s)"
      cursor.execute(sql, [username, pwd, mobile,])
      conn.commit()
  
      # 3. 关闭连接
      cursor.close()
      conn.close()
  
      return "成功"
  
  
  if __name__ == '__main__':
      app.run()
  ````

  ````html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <h1>添加用户</h1>
  <form method="post" action="/add/user">
      <input type="text" name="user" placeholder="用户名">
      <input type="password" name="pwd" placeholder="密码">
      <input type="text" name="mobile" placeholder="手机号">
      <input type="submit" value="提 交">
  </form>
  </body>
  </html>
  ````

- 查询所有用户

  ````python
  @app.route("/show/user")
  def show_user():
      # 1. 连接MySQL
      conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="asd12311", charset="utf8",
                             db="selenexx_test")
      cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
  
      # 2. 执行SQL
      sql = "select * from user_info"
      cursor.execute(sql)
      data_list = cursor.fetchall()
  
      # 3. 关闭连接
      cursor.close()
      conn.close()
  
      print(data_list)
  
      return render_template("show_user.html", data_list=data_list)
  ````

  ````html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <link rel="stylesheet" href="/static/plugins/bootstrap-3.4.1/css/bootstrap.css">
  </head>
  <body>
  <div class="container">
      <h1>用户列表</h1>
      <table class="table table-bordered">
          <thead>
          <tr>
              <th>ID</th>
              <th>姓名</th>
              <th>密码</th>
              <th>手机号</th>
          </tr>
          </thead>
          <tbody>
          {% for item in data_list %}
          <tr>
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.password }}</td>
              <td>{{ item.mobile }}</td>
          </tr>
          {% endfor %}
          </tbody>
      </table>
  </div>
  
  
  <script src="/static/js/jquery-3.6.3.min.js"></script>
  <script src="/static/plugins/bootstrap-3.4.1/js/bootstrap.js"></script>
  </body>
  </html>
  ````

![image-20230213190046465](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230213190046465.png)

## 9. Django

### 9.1 创建项目

- 命令行：创建的项目是标准的

  - 打开终端

  - 进入项目目录

  - 执行命令创建项目

    ````powershell
    "Python解释器目录\Scripts\django-admin.exe" startproject Project_name
    ````

- Pycharm直接创建：相比命令行创建多加了：

  1. templates目录
  2. setting.py增加了设置templates路径的语句

- 默认项目的文件介绍

  ````
  Project
  │  manage.py			[项目管理，启动项目，创建app，数据库管理]
  │
  └─djangoProject1
          asgi.py			[接受网络请求，异步]	
          settings.py		[配置文件]
          urls.py			[URL和函数的对应关系]
          wsgi.py			[接受网络请求，同步]
          __init__.py		
  ````

### 9.2 APP

当项目足够大的时候，可以将功能单独分成多个app进行开发

- 通过命令行创建App应用

  ````shell
  python .\manage.py startapp app01
  ````

  ![image-20230215183526576](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215183526576.png)

````
app01
│  admin.py				[提供了admin后台管理的功能]
│  apps.py				[app启动的类]
│  models.py			[对数据库操作]
│  tests.py				[单元测试]
│  views.py				[函数定义]
│  __init__.py
│
└─migrations			[数据库变更记录]
        __init__.py
````

### 9.3 运行Django

- 在配置文件注册新建的app：settings.py

  ![image-20230215184449408](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215184449408.png)

- 编写URL和视图函数的对应关系：urls.py

  ![image-20230215184716084](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215184716084.png)

- 编写视图函数：app01/views.py

  ![image-20230215184853199](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215184853199.png)

- 启动django项目

  - 命令行启动

    ````shell
    python manage.py runserver
    ````

  - Pycharm

### 9.4 templates模板

````python
def user_list(request):
    # 去app目录下的templates寻找html文件（根据app的注册顺序，逐一去所有app的templates目录中寻找）
    return render(request, "user_list.html")
````

![image-20230215190233861](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215190233861.png)

### 9.5 静态文件

在开发过程中一般将：

- 图片
- CSS
- Js

当作静态文件处理。

1. 在app目录下创建static文件夹

   ![image-20230215191504343](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230215191504343.png)

   2. 引入static

      ````html
      {% load static %}
      {# 载入静态文件目录 #}
      
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
          {#  拼接目录和文件路径，方便后期目录移动/改名  #}
          <link rel="stylesheet" href="{% static 'plugins/bootstrap-3.4.1/css/bootstrap.css' %}">
      </head>
      <body>
      <h1>用户列表</h1>
      
      <input type="text" class="btn btn-primary" value="新建">
      <img src="{% static 'img/kabi.jpg' %}" alt="">
      
      
      <script src="{% static 'plugins/bootstrap-3.4.1/js/bootstrap.js' %}"></script>
      <script src="{% static 'js/jquery-3.6.3.min.js' %}"></script>
      </body>
      </html>
      ````


### 9.6 模板语法

在HTML中写一些占位符，由数据对这些占位符进行替换

![image-20230224173805481](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230224173805481.png)

view函数的render内部：

- 读取含有模板语法的HTML文件
- 内部进行渲染（模板语法执行并替换数据），最终得到只包含HTML标签的字符串
- 将渲染完成的HTML文件返回给用户浏览器

````python
def tpl(request):
    name = "SeleneXX"
    roles = ["管理员", "CEO", "保安"]
    user_info = {"name": "Selene", "salary": 100000, "role": "CEO"}
    data_list = [
        {"name": "Selene1", "salary": 100000, "role": "CEO"},
        {"name": "Selene2", "salary": 200000, "role": "123"},
        {"name": "Selene3", "salary": 300000, "role": "345"}
    ]
    return render(request, "tpl.html", {"n1": name, "n2": roles, "n3": user_info, "n4": data_list})
````

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>模板语法的学习</h1>
<div>{{ n1 }}</div>
<div>{{ n2 }}</div>
<div>{{ n2.0 }}</div>
<div>{{ n2.1 }}</div>
<div>{{ n2.2 }}</div>

<div>
    {% for item in n2 %}
        <span>{{ item }}</span>
    {% endfor %}
</div>
<hr/>
<div>{{ n3 }}</div>
<div>{{ n3.name }}</div>
<div>{{ n3.salary }}</div>
<div>{{ n3.role }}</div>
<ul>
    {% for k, v in n3.items %}
        <li>{{ k }} = {{ v }}</li>
    {% endfor %}
</ul>

<hr/>
<div>{{ n4.0.name }}</div>
<div>{{ n4.0.role }}</div>
{% for item in n4 %}
    <div>{{ item.name }}</div>
    <div>{{ item.salary }}</div>
    <div>{{ item.role }}</div>
{% endfor %}
<hr/>

{% if n1 == "SeleneXX" %}
    <h1>12345</h1>
{% elif n1 == "12334" %}
    <h1>asdfg</h1>
{% else %}
    <h1>09876</h1>
{% endif %}

</body>
</html>
````

#### 案例： 新闻中心

````python
def news(req):
    # 1. 定义一些新闻
    # 向地址xxx发送请求爬取数据
    # 使用第三方模块requests
    url = "http://www.chinaunicom.com/api/article/NewsByIndex/2/2023/02/news"
    # UA反反爬
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36"}
    res = requests.get(url=url, headers=headers)
    data_list = res.json()

    return render(req, "news.html", {"newslist": data_list})
````

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>新闻中心</h1>
<ul>
    {%  for item in newslist %}
        <li>{{ item.news_title }} 时间：{{ item.post_time }}</li>
    {% endfor %}
</ul>
</body>
</html>
````

### 9.7 请求和响应

关于重定向

![image-20230224182635397](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230224182635397.png)

````python
def something (req):
    # req是一个对象，封装了用户发送过来的所有请求相关数据
    # 1. 获取请求方式 GET/POST
    print(req.method)

    # 2. 获取URL上传递的数据/something/?n1=123
    print(req.GET)

    # 3. 获取请求体中提交的数据
    print(req.POST)

    # 4. 响应：HttpResponse 返回字符串给请求者
    # return HttpResponse("返回内容")

    # 响应：render 读取HTML内容+渲染，返回给请求者
    # return render(req, "something.html", {"title": "来了"})

    # 响应: redirect 让请求者重定向到新页面
    return redirect("https://www.baidu.com")
````

#### 案例：用户登录

````python
def login(req):
    if req.method == "GET":
        # 访问该网址默认以GET访问，直接返回登陆界面
        return render(req, "login.html")
    # 收到点击按钮发送的POST请求
    print(req.POST)
    username = req.POST.get("user")
    password = req.POST.get("pwd")
    if username == "root@123.com" and password == "123":
        return redirect("http://www.baidu.com")
    # 登陆失败
    return render(req, "login.html", {"error_msg": "用户名或密码错误"})
````

````html
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="{% static 'plugins/bootstrap-3.4.1/css/bootstrap.css' %}">
    <style>
        .login-title {
            text-align: center;
            font-size: 25px;
            font-weight: 1000;
            margin-bottom: 10px;
        }

        .form {
            box-shadow: 10px 10px 10px #aaa;
            padding: 20px 40px;
            width: 500px;
            border: 1px solid #dddddd;
            border-radius: 5px;
            margin: 20px auto;
        }

        .form a {
            float: right;
        }

        .form span {
            color: red;
        }
    </style>
</head>
<body>
<div class="container">
    <div class="form clearfix">
        <form method="post" action="/login/">
            {#      一定要在表单中添加django自带的隐藏的校验数据：csrf_token      #}
            {% csrf_token %}
            <div class="login-title">用户登录</div>
            <div class="form-group">
                <label for="exampleInputEmail1">用户名或手机号</label>
                <input type="email" name="user" class="form-control" id="exampleInputEmail1" placeholder="Username">
            </div>
            <div class="form-group">
                <label for="exampleInputPassword1">密码</label>
                <input type="password" name="pwd" class="form-control" id="exampleInputPassword1" placeholder="Password">
            </div>
            <button type="submit" class="btn btn-primary">登录</button>
            <span>{{ error_msg }}</span>
            <a href="https://www.baidu.com/">忘记密码？</a>
        </form>
    </div>

</div>

<script src="{% static 'plugins/bootstrap-3.4.1/js/bootstrap.js' %}"></script>
<script src="{% static 'js/jquery-3.6.3.min.js' %}"></script>
</body>
</html>
````

### 9.8 数据库操作

- MySQL+pymysql
- Django开发引入orm框架，可以把代码翻译成数据库命令

![image-20230227183321685](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230227183321685.png)

ORM可以：

- 创建，修改，删除数据库中的表（不用写SQL语句）。无法创建数据库
- 操作表中的数据

#### 9.8.1 创建数据库

使用MySQL在终端创建数据库

````powershell
 create database Django_DB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
````

#### 9.8.2 Django连接数据库

在settings.py文件中进行配置和修改

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'dbname',
        'USER': 'root',
        'PASSWORD': 'xxx',
        'HOST': '',				# 主机IP
        'PORT': '',				# 端口号
    }
}
```

#### 9.8.3 Django操作表

- 创建表

  models.py

  ````python
  from django.db import models
  
  # Create your models here.
  class UserInfo(models.Model):
      name = models.CharField(max_length=32)
      password = models.CharField(max_length=64)
      age = models.IntegerField()
  
  # ORM将上述语句换成下面的SQL语句
  """
  create table app01_userinfo(
      id bigint auto_increment primary key,
      name varchar(32),
      password varchar(64),
      age int,
  )
  """
  ````

  在项目根目录终端执行命令：

  ````shell
  python manage.py make migrations
  python manage.py migrate
  ````

  注意：需要app已注册

  ![image-20230227190051091](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230227190051091.png)

  后面的表为Django默认需要的数据库

  直接删除这个类，然后再执行一遍migrate。

- 删除表

  直接删除models.py中需要删除的表对应的那个类，然后执行一遍migrate。

  删除数据同理，删除类中的一项然后执行migrate。

- 修改表

  新增列的时候，表中可能已有数据，所以新增列时要设置一个默认值或者允许为空。

  ````python
  class UserInfo(models.Model):
      name = models.CharField(max_length=32)
      password = models.CharField(max_length=64)
      age = models.IntegerField()
      size = models.IntegerField(default=2)
      data = models.IntegerFiled(null=True, blank=True)
  ````

#### 9.8.4 Django操作数据

- 增

  ````python
  def orm(req):
      # 测试ORM操作
      # 新建数据
      models.Department.objects.create(title="销售部")
      models.Department.objects.create(title="IT部")
      models.Department.objects.create(title="运营部")
      """
      insert into app01_department(title) values("销售部");
      """
      models.UserInfo.objects.create(name="Selene", password="123", age=24)
      models.UserInfo.objects.create(name="Dingding", password="123", age=24)
      return HttpResponse("成功")
  ````

- 删

  ````python
  # filter等价于条件语句where，删除id=3的行
  models.UserInfo.objects.filter(id=3).delete()
  # 全部删除
  models.Department.objects.all().delete()
  ````
  
- 改

  ````python
  # 更新数据
  # 把所有行的password列都改为999
  models.UserInfo.objects.all().update(password="999")
  # 选择id为2的行，更新各列的数据
  models.UserInfo.objects.filter(id=2).update(name="Ding", password="123", age=999)
  ````

- 查

  ````python
  # 查询数据，查询到的数据为QuerySet类型，列表中封装了一个一个对象代表一行
  data_list = models.UserInfo.objects.all()
  for obj in data_list:
      print(obj.id, obj.name, obj.password, obj.age)
  # 获取符合某个条件的第一个数据
  obj = models.UserInfo.objects.filter(id=1).first()
  print(obj.id, obj.name, obj.password, obj.age)
  ````

#### 案例：用户管理

##### 1. 展示用户列表

- url
- 函数
  - 获取所有的用户信息
  - HTML渲染

````python
def info_list(req):
    # 1. 获取数据库中所有的数据
    data_list = models.UserInfo.objects.all()

    return render(req, "info_list.html", {"data_list": data_list})
````

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Info列表</h1>

<a href="/info/add">添加</a>

<table border="1">
    <thead>
    <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>密码</th>
        <th>年龄</th>
    </tr>
    </thead>
    <tbody>
    {% for obj in data_list %}
        <tr>
            <td>{{ obj.id }}</td>
            <td>{{ obj.name }}</td>
            <td>{{ obj.password }}</td>
            <td>{{ obj.age }}</td>
        </tr>
    {% endfor %}
    </tbody>
</table>
</body>
</html>
````

##### 2. 添加用户

- url
- 函数
  - GET，看到页面，输入内容
  - POST， 提交，写入数据库

````python
def info_add(req):
    if req.method == "GET":
        return render(req, "info_add.html")

    # 获取用户提交的数据
    user = req.POST.get("user")
    pwd = req.POST.get("pwd")
    age = req.POST.get("age")

    # 添加到数据库
    models.UserInfo.objects.create(name=user, password=pwd, age=age)

    # 自动跳转
    return redirect("/info/list")
````

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>添加用户</h1>
{#  action可以不写，默认提交到当前页面  #}
<form action="/info/add/" method="post">
    {% csrf_token %}
    <input type="text" name="user" placeholder="用户名">
    <input type="password" name="pwd" placeholder="密码">
    <input type="text" name="age" placeholder="年龄">
    <input type="submit" value="提交">
</form>
</body>
</html>
````

##### 3. 删除用户

- url
- 函数
  - 获取GET请求中的数据然后删除

````python
def info_delete(req):
    nid = req.GET.get('nid')
    models.UserInfo.objects.filter(id=nid).delete()
    return redirect("/info/list/")
````

````html
<table border="1">
    <thead>
    <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>密码</th>
        <th>年龄</th>
        <th>操作</th>
    </tr>
    </thead>
    <tbody>
    {% for obj in data_list %}
        <tr>
            <td>{{ obj.id }}</td>
            <td>{{ obj.name }}</td>
            <td>{{ obj.password }}</td>
            <td>{{ obj.age }}</td>
            <td>
                <a href="/info/delete/?nid={{ obj.id }}">删除</a>
            </td>
        </tr>
    {% endfor %}
    </tbody>
</table>
````

![image-20230227210738738](C:\Users\SeleneXX\AppData\Roaming\Typora\typora-user-images\image-20230227210738738.png)

### 9.9 员工管理系统

- 创建项目：Pycharm创建项目时要删除settings里的templates目录

- 创建app并在settings中注册：新方法，pycharm--tools--run manage.py task...

  输入``` startapp app01```

#### 9.9.1 设计表结构

````python
class Department(models.Model):
    """部门表"""
    title = models.CharField(verbose_name='标题', max_length=32)

class UserInfor(models.Model):
    """员工表"""
    name = models.CharField(verbose_name="姓名", max_length=16)
    password = models.CharField(verbose_name="密码", max_length=64)
    age = models.IntegerField(verbose_name="年龄")
    salary = models.DecimalField(verbose_name="薪资", max_digits=10, decimal_places=2, default=0)
    create_time = models.DateTimeField(verbose_name="创建时间")
    # 通过外键关联
    # 级联删除
    depart = models.ForeignKey(to="Department", to_fields="title", on_delete=models.CASCADE)
    # 置空
    # depart = models.ForeignKey(to="Department", to_fields="title", on_delete=models.SET_NULL)
    # 通过django内部关联，查询该条目后使用.get_xxx_display()函数显示绑定的关系
    gender_choices = (
        (1, "男"),
        (1, "女"),
    )
    gender = models.SmallIntegerField(verbose_name="性别", choices=gender_choices)
````

#### 9.9.2 生成表

- 生成数据库

  ````shell
  create database Employer_Management DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

- 修改settings.py连接Mysql

- 使用django生成表

  - 使用两个migrate命令生成

  - 新方法，pycharm--tools--run manage.py task

    输入``` migrations```，``` migrate```

#### 9.9.3 部门管理

- 部门列表

- 新增部门

- 修改部门

  通过django3中的提交正则表达式功能，传递部门ID

  ````python
  # 定义正则表达式，中间必须传一个整型
  path("department/<int:nid>/edit/", views.department_edit),
  ````

  ````python
  def department_edit(request, nid):
      """ 修改部门 """
      # 根据nid获取数据
      row_obj = models.Department.objects.filter(id=nid).first()
      return render(request, "department_edit.html", {"title": row_obj.title})
  ````

  ````html
  <a class="btn btn-primary btn-xs" href="/department/{{ obj.id }}/edit/">编辑</a>
  ````


#### 9.9.4 模板继承

上面的3个html的导航条部分都相同，如果全部分开写，会导致大量重复。、

- 定义模板

  ````html
  {% load static %}
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      {% block css %}{% endblock %}
  </head>
  <body>
  
  <div>
      {% block content %}{% endblock %}
  </div>
  
  {% block js %}{% endblock %}
  </body>
  </html>
  ````

- 继承模板

  ````html
  {% extends 'layout.html' %}
  
  {% block css %}
      <link rel="stylesheet" href="...">
  	<style>...</style>
  {% endblock %}
  
  {% block content %}
      <div class="container">
          ...
      </div>
  {% endblock %}
  
  {% block js %}
      <script src="..."></script>
  	<script src="..."></script>
  {% endblock %}
  ````


#### 9.9.5 用户管理

- 用户列表

  前面在定义user_info表时，性别使用了choices去定义，想要获取性别的名称而不是关联的id，需要使用``` obj.get_gender_display()```来获取。

  模板语法中，所有函数都不能加括号。

  ````
  {{ obj.get_gender_display }}
  ````

  入职时间在数据库中定义为datetime格式，在python中想要正确显示datetime格式需要使用``` obj.create_time.strftime("%Y-%m-%d")```来将其格式化为字符串。但是在模板语法中函数不能有括号，所以需要写成：

  ````
  {{ obj.create_time|date:"Y-m-d" }}
  ````

  ````python
  def user_list(request):
      """ 用户管理 """
  
      # 获取所有的用户列表
      user_list = models.UserInfo.objects.all()
      return render(request, "user_list.html", {"user_list": user_list})
  ````

  ````html
  {% extends 'layout.html' %}
  
  {% block content %}
      <div class="container">
          <div style="margin-bottom: 10px">
              <a class="btn btn-success" href="" target="_blank">
                  <span class="glyphicon glyphicon-plus-sign" aria-hidden="true"></span>
                  新建员工
              </a>
  
          </div>
          <div class="panel panel-default">
              <!-- Default panel contents -->
              <div class="panel-heading">
                  <span class="glyphicon glyphicon-th-list" aria-hidden="true"></span>
                  用户列表
              </div>
              <!-- Table -->
              <table class="table table-bordered">
                  <thead>
                  <tr>
                      <th>ID</th>
                      <th>姓名</th>
                      <th>密码</th>
                      <th>年龄</th>
                      <th>薪资</th>
                      <th>入职时间</th>
                      <th>性别</th>
                      <th>所属部门</th>
                      <th>操作</th>
                  </tr>
                  </thead>
                  <tbody>
                  {% for obj in user_list %}
                      <tr>
                      <th>{{ obj.ID }}</th>
                      <th>{{ obj.name }}</th>
                      <th>{{ obj.password }}</th>
                      <th>{{ obj.age }}</th>
                      <th>{{ obj.salary }}</th>
                      {# 在python中使用obj.create_time.strftime("%Y-%m-%d")来输出时间，模板语法不允许出现括号 #}
                      <th>{{ obj.create_time|date:"Y-m-d" }}</th>
                      {# 使用get_xx_display()来直接显示choices的定义中key所对应的value值 #}
                      <th>{{ obj.get_gender_display }}</th>
                      <th>{{ obj.depart.title }}</th>
                      <td>
                          <a class="btn btn-primary btn-xs" href="">编辑</a>
                          <a class="btn btn-danger btn-xs" href="">删除</a>
                      </td>
                  {% endfor %}
                  </tr>
                  </tbody>
              </table>
          </div>
      </div>
  {% endblock %}
  ````

- 新建用户

  - 原始方法：

    ````python
    def user_add(request):
        """ 添加用户 （原始方式） """
        if request.method == "GET":
            context = {
                'gender_choices': models.UserInfo.gender_choices,
                'depart_list': models.Department.objects.all(),
            }
            return render(request, 'user_add.html', context)
        # 获取用户提交的数据
        user = request.POST.get('user')
        pwd = request.POST.get('pwd')
        age = request.POST.get('age')
        salary = request.POST.get('salary')
        c_time = request.POST.get('ctime')
        gender = request.POST.get('gender')
        department = request.POST.get('department')
    
        models.UserInfo.objects.create(name=user, password=pwd, age=age, salary=salary, create_time=c_time, gender=gender, depart_id=department)
    
        return redirect("/user/list/")
    ````

    - 数据需要校验，获取提交的数据很多，校验很复杂
    - 如果输入错误需要错误提示
    - 页面上每一个字段都需要根据数据库的列写一遍
    - 关联的数据需要手动获取并循环展示在页面中

  - Django组件

    - Form组件：针对其他情况

      可以改进原始方法的前3个缺点。

      1. views.py

         ````python
         class MyForm(Form):
             user = forms.CharField(widget=forms.Input)
             pwd = forms.CharField(widget=forms.Input)
             email = forms.CharField(widget=forms.Input)
         
         def user_add(request):
             # 添加用户 （原始方式）
             if request.method == "GET":
                 form = MyForm()
                 return render(request, 'user_add.html', {"form": form})
         ````

      2. user_add.html 不需要手写标签，直接调用form

         ````html
         <form method="post">
             {{ form.user }}
             {{ form.pwd }}
             {{ form.email }}
         </form>
         
         <form method="post">
             {% for field in form %}
             	{{ field }}
             {% endfor %}
         </form>
         ````

    - **ModelForm组件： 针对数据库**

      1. models.py

         ```python
         class UserInfo(models.Model):
             """员工表"""
             name = models.CharField(verbose_name="姓名", max_length=16)
             password = models.CharField(verbose_name="密码", max_length=64)
             age = models.IntegerField(verbose_name="年龄")
             salary = models.DecimalField(verbose_name="薪资", max_digits=10, decimal_places=2, default=0)
             create_time = models.DateTimeField(verbose_name="创建时间")
             # 通过外键关联
             # 级联删除
             depart = models.ForeignKey(to="Department", to_field="id", on_delete=models.CASCADE)
             # 置空
             # depart = models.ForeignKey(to="Department", to_fields="title", on_delete=models.SET_NULL)
             # 通过django内部关联，查询该条目后使用.get_xxx_display()函数显示绑定的关系
             gender_choices = (
                 (1, "男"),
                 (2, "女"),
             )
             gender = models.SmallIntegerField(verbose_name="性别", choices=gender_choices)
         ```

      2. views.py

         ````python
         class MyForm(ModelForm):
             xx = forms.CharField(widget=forms.Input)
             class Meta:
                 model = UserInfo
                 fields = ["name", "password", "age", "xx"]
                 
         def user_add(request):
             # 添加用户 （原始方式）
             if request.method == "GET":
                 form = MyForm()
                 return render(request, 'user_add.html', {"form": form})
         ````

      完整实现

      ```python
      class UserModelForm(forms.ModelForm):
          class Meta:
              model = models.UserInfo
              fields = ["name", "password", "age", "salary", "create_time", "gender", "depart"]
      
          # 重新定义init方法，面向对象改造
          def __init__(self, *args, **kwargs):
              # 保留父类的init方法
              super().__init__(*args, **kwargs)
      
              # 遍历所有的fields，修改widget，添加class="form-control"
              for name, field in self.fields.items():
                  # 隐藏密码
                  if name == "password":
                      field.widget.input_type = "password"
                  field.widget.attrs = {"class": "form-control", "placeholder": field.label}
      
      
      def user_modelform_add(request):
          # 添加用户 modelform
          if request.method == "GET":
              form = UserModelForm()
              return render(request, "user_modelform_add.html", {"form": form})
      
          # 数据校验, 使用Modelform获取request请求中的所有数据
          form = UserModelForm(data=request.POST)
          if form.is_valid():
              # 如果合法，保存到数据库
              form.save()
              return redirect("/user/list/")
      
          # 如果不合法（在页面上显示错误信息）
          print(form.errors)
      	# 返回包含提交字段和错误信息的form给前端    
          return render(request, "user_modelform_add.html", {"form": form})
      ```
      
      ````html
      {% extends 'layout.html' %}
      
      {% block content %}
          <div class="container">
              <div class="panel panel-default">
                  <div class="panel-heading">
                      <h3 class="panel-title"> 新建员工 </h3>
                  </div>
                  <div class="panel-body">
                      {# novalidate表示不需要浏览器进行验证 #}
                      <form method="post" novalidate>
                          {% csrf_token %}
                          {% for field in form %}
                              <div class="form-group">
                                  <label>{{ field.label }}</label>
                                  {{ field }}
                                  {# 如果出错，则显示这个字段 #}
                                  <span style="color: red;">{{ field.errors.0 }}</span>
                              </div>
                          {% endfor %}
      
                          <div class="form-group">
                              <button type="submit" class="btn btn-primary">提 交</button>
                          </div>
                      </form>
                  </div>
              </div>
      
          </div>
      {% endblock %}
      ````
  
- 编辑用户

  - 点击编辑，跳转到编辑页面（将编辑行的ID携带过去）
  - 编辑页面显示默认数据（根据ID获取并设置到页面中）
  - 提交时，做错误提示，数据校验，更新数据库

  ````python
  def user_edit(request, nid):
      # 根据ID去数据库获取目标行
      user_obj = models.UserInfo.objects.filter(id=nid).first()
      if request.method == "GET":
          # 默认传入行数据，自动写入input框
          form = UserModelForm(instance=user_obj)
          return render(request, "user_edit.html", {"form": form})
  
      # 需要多传一个instance告诉django这次需要更新的行而不是新增一行
      form = UserModelForm(data=request.POST, instance=user_obj)
      if form.is_valid():
          form.save()
          return redirect("/user/list/")
  
      return render(request, "user_modelform_add.html", {"form": form})
  ````

  ````html
  {% extends 'layout.html' %}
  
  {% block content %}
      <div class="container">
          <div class="panel panel-default">
              <div class="panel-heading">
                  <h3 class="panel-title"> 编辑员工 </h3>
              </div>
              <div class="panel-body">
                  {# novalidate表示不需要浏览器进行验证 #}
                  <form method="post" novalidate>
                      {% csrf_token %}
                      {% for field in form %}
                          <div class="form-group">
                              <label>{{ field.label }}</label>
                              {{ field }}
                              {# 如果出错，则显示这个字段 #}
                              <span style="color: red;">{{ field.errors.0 }}</span>
                          </div>
                      {% endfor %}
  
                      <div class="form-group">
                          <button type="submit" class="btn btn-primary">提 交</button>
                      </div>
                  </form>
              </div>
          </div>
      </div>
  
  {% endblock %}
  ````

  - 如果需要保存的数据中有其他不是用户输入的数据：

  ````python
  # 默认保存用户输入的所有数据
  form.save()
  
  # 如果有其他非用户输入的数据要保存
  form.instance.字段名 = 值
  form.save()
  ````


### 9.10 靓号管理

#### 9.10.1 数据库表结构

| ID   | mobile | level | status | price |
| ---- | ------ | ----- | ------ | ----- |
|      |        |       |        |       |

````python
class PrettyNum(models.Model):
    mobile = models.CharField(verbose_name="手机号", max_length=11)
    # 想要允许为空 null=True, blank=True
    price = models.IntegerField(verbose_name="价格", default=0)

    level_choices = (
        (1, "1级"),
        (2, "2级"),
        (3, "3级"),
        (4, "4级"),
    )
    level = models.SmallIntegerField(verbose_name="级别", choices=level_choices, default=1)

    status_choices = (
        (1, "已占用"),
        (2, "未占用"),
    )
    status = models.SmallIntegerField(verbose_name="状态", choices=status_choices, default=2)
````

#### 9.10.2 靓号列表

- URL

- 函数

  - 获取所有的靓号

    ````python
    def prettynum_list(request):
        # select * from 表 order by id asc; 升序排序
        # num_list = models.PrettyNum.objects.all().order_by("id")
        # select * from 表 order by id desc; 降序排序
        num_list = models.PrettyNum.objects.all().order_by("-level")
        return render(request, "prettynum_list.html", {"num_list": num_list})
    ````

    

  - 结合html+render将靓号罗列

    ````html
    {% extends 'layout.html' %}
    
    {% block content %}
        <div class="container">
            <div style="margin-bottom: 10px">
                <a class="btn btn-primary" href="#" target="_blank">
                    <span class="glyphicon glyphicon-plus-sign" aria-hidden="true"></span>
                    新建靓号
                </a>
    
            </div>
            <div class="panel panel-default">
                <!-- Default panel contents -->
                <div class="panel-heading">
                    <span class="glyphicon glyphicon-th-list" aria-hidden="true"></span>
                    靓号列表
                </div>
    
                <!-- Table -->
                <table class="table table-bordered">
                    <thead>
                    <tr>
                        <th>ID</th>
                        <th>手机号</th>
                        <th>价格</th>
                        <th>级别</th>
                        <th>状态</th>
                    </tr>
                    </thead>
                    <tbody>
                    {% for obj in num_list %}
                        <tr>
                        <th>{{ obj.id }}</th>
                        <th>{{ obj.mobile }}</th>
                        <th>{{ obj.price }}</th>
                        <th>{{ obj.get_level_display }}</th>
                        <th>{{ obj.get_status_display }}</th>
                        <td>
                            <a class="btn btn-primary btn-xs" href="#">编辑</a>
                            <a class="btn btn-danger btn-xs" href="#">删除</a>
                        </td>
                    {% endfor %}
                    </tr>
    
                    </tbody>
                </table>
            </div>
        </div>
    {% endblock %}
    ````

#### 9.10.3 新建靓号

- URL

- 函数

  - ModelForm
  - render传入html
  - 模板循环展示所有字段

- 点击提交

  - 数据校验

    ````python
    class PrettyModelForm(forms.ModelForm):
        # 验证：方式1，字段+正则
        mobile = forms.CharField(
            label="手机号",
            validators=[RegexValidator(r'^\d{11}$', '必须是11位数字')]
        )
    
        class Meta:
            model = models.PrettyNum
            # fields = ["mobile", "price", "level", "status"]
            # exclude = ["level"] 排除某个字段的剩余字段
            fields = "__all__"
    
        def __init__(self, *args, **kwargs):
            # 保留父类的init方法
            super().__init__(*args, **kwargs)
    
            # 遍历所有的fields，修改widget，添加class="form-control"
            for name, field in self.fields.items():
                field.widget.attrs = {"class": "form-control", "placeholder": field.label}
    
        # 验证：方式2，钩子方法
        def clean_mobile(self):
            # 取到当前form里instance传入的primary key即为id
            ID = self.instance.pk
            txt_mobile = self.cleaned_data["mobile"]
    
            exists = models.PrettyNum.objects.exclude(id=ID).filter(mobile=txt_mobile).exists()
            # 验证不通过
            if exists:
                raise ValidationError("手机号已存在")
            # 验证通过
            return txt_mobile
    ````
  
  - 保存到数据库
  
  - 返回列表

#### 9.10.4 编辑靓号

````python
class PrettyEditModelForm(PrettyModelForm):
    # 作为子类继承ModelForm
    # 显示mobile但是不可改
    mobile = forms.CharField(disabled=True, label="手机号")


def prettynum_edit(request, nid):
    num_obj = models.PrettyNum.objects.filter(id=nid).first()
    if request.method == "GET":
        form = PrettyEditModelForm(instance=num_obj)
        return render(request, "prettynum_edit.html", {"form": form})

    form = PrettyEditModelForm(data=request.POST, instance=num_obj)
    if form.is_valid():
        form.save()
        return redirect("/prettynum/list/")

    return render(request, "prettynum_edit.html", {"form": form})
````

- 不允许手机号重复

  - 添加：手机号不能存在

    ````python
    # True/False
    exists = models.PrettyNum.objects.filter(mobile="xxx").exists()

  - 编辑：因为编辑时手机号不能修改，所以永远当前手机号永远存在

    ````python
    exists = models.PrettyNum.objects.filter(mobile="xxx").exclude(id=xx).exists()
    ````

#### 9.10.5 查询手机号

````python
models.PrettyNum.objects.filter(mobile="12345678900", id=12)

# 可以按字典查找，使用**解包字典传入数据
data_dict = {"mobile": "12345678900", "id": 123}
models.PrettyNum.objects.filter(**data_dict)
````

````python
models.PrettyNum.objects.filter(id=12)				# 等于12
models.PrettyNum.objects.filter(id__gt=12)			# 大于12
models.PrettyNum.objects.filter(id__gte=12)			# 大于等于12
models.PrettyNum.objects.filter(id__lt=12)			# 小于12
models.PrettyNum.objects.filter(id__lte=12)			# 小于等于12
````

````python
models.PrettyNum.objects.filter(mobile__startswith="1234")		# 以1234开头
models.PrettyNum.objects.filter(mobile__endswith="1234")		# 以1234结尾
models.PrettyNum.objects.filter(mobile__contains="1234")		# 包含1234

# 也可以用解包传入数据
data_dict = {"mobile__startswith": "1234", "id__gte": 1}
models.PrettyNum.objects.filter(**data_dict)
````

````python
def prettynum_list(request):
    # 获取get请求传入的数据
    data_dict = {}
    # 防止拿到空值，传入搜索框会显示none
    search_data = request.GET.get("q", "")
    if search_data:
        data_dict["mobile__contains"] = search_data
    # 查询该数据
    num_list = models.PrettyNum.objects.filter(**data_dict).order_by("-level")
    return render(request, "prettynum_list.html", {"num_list": num_list, "search_data": search_data})
````

````html
<div style="float: right; width: 300px">
                <form method="get">
                    <div class="input-group">
                        <input type="text" class="form-control" name="q" placeholder="Search for..." value="{{ search_data }}">
                        <span class="input-group-btn">
                            <button class="btn btn-default" type="submit">
                                <span class="glyphicon glyphicon-search" aria-hidden="true"></span>
                            </button>
                        </span>
                    </div>
                </form>
            </div>
````

#### 9.10.6 分页

````python
query_set = models.PrettyNum.objects.all()
# 取前10条
query_set = models.PrettyNum.objects.all()[0:10]
````

````python
def prettynum_list(request):
    # for i in range(300):
    #     models.PrettyNum.objects.create(mobile="10001000100", price=10, level=1, status=1)
    data_dict = {}
    # 防止拿到空值，传入搜索框会显示none
    search_data = request.GET.get("q", "")
    if search_data:
        data_dict["mobile__contains"] = search_data

    # 根据用户访问的页面，获取数据
    # 获取page的值，默认为1
    page = int(request.GET.get("page", 1))
    page_size = 10
    start, end = (page - 1) * 10, page * 10
    count = models.PrettyNum.objects.filter(**data_dict).count()
    # 总页码
    page_count, mod = divmod(count, page_size)
    if mod:
        page_count += 1

    # select * from 表 order by id asc; 升序排序
    # num_list = models.PrettyNum.objects.all().order_by("id")
    # select * from 表 order by id desc; 降序排序
    num_list = models.PrettyNum.objects.filter(**data_dict).order_by("-level")[start: end]
    # 计算出当前应该显示的页面，前5页+后5页
    plus = 5
    if page_count <= 2 * plus + 1:
        # 总数据没有达到目标页数
        start_page = 1
        end_page = page_count
    else:
        start_page = max(1, min(page - plus, page_count - plus * 2))
        end_page = max(1 + plus * 2, min(page + plus, page_count))

    # 生成页码
    """
            <li><a href="?page=1">1</a></li>
            <li><a href="?page=2">2</a></li>
            <li><a href="?page=3">3</a></li>
            <li><a href="?page=4">4</a></li>
            <li><a href="?page=5">5</a></li>
    """
    page_str_list = []

    # 上一页和下一页
    prev = f'<li class="{"disabled" if page == 1 else ""}"><a href="?page={max(page - 1, 1)}" aria-label="Previous"><span aria-hidden="true">«</span></a></li>'
    page_str_list.append(prev)

    for i in range(start_page, end_page + 1):
        if i == page:
            elem = f'<li class="active"><a href="?page={i}">{i}</a></li>'
        else:
            elem = f'<li><a href="?page={i}">{i}</a></li>'
        page_str_list.append(elem)

    next = f'<li class="{"disabled" if page == page_count else ""}"><a href="?page={min(page + 1, page_count)}" aria-label="Next"><span aria-hidden="true">»</span></a></li>'
    page_str_list.append(next)
    page_string = mark_safe("".join(page_str_list))

    return render(request, "prettynum_list.html", {"num_list": num_list, "search_data": search_data, "page": page_string})
````

分页组件封装：

````python
import copy


class Pagination(object):
    """
    自定义分页组件
    """

    def __init__(self, request, query_set, page_size=10, page_param="page", plus=5):
        # 拷贝请求，使得请求可以修改
        get_object = copy.deepcopy(request.GET)
        get_object._mutable = True
        self.query_dict = get_object
        self.page_param = page_param
        page = self.query_dict.get(page_param, '1')
        if page.isdecimal():
            page = int(page)
        else:
            page = 1
        self.page = page
        self.page_size = page_size
        self.total_count = query_set.count()

        self.page_count, mod = divmod(self.total_count, self.page_size)
        if mod:
            self.page_count += 1
        self.page = max(1, min(self.page_count, self.page))

        self.start = (self.page - 1) * self.page_size
        self.end = self.page * self.page_size
        self.page_set = query_set[self.start: self.end]
        self.plus = plus

    def html(self):
        if self.page_count <= 2 * self.plus + 1:
            # 总数据没有达到目标页数
            start_page = 1
            end_page = self.page_count
        else:
            start_page = max(1, min(self.page - self.plus, self.page_count - self.plus * 2))
            end_page = max(1 + self.plus * 2, min(self.page + self.plus, self.page_count))
        self.page_str_list = []
		# 保持选择页面后，还有搜索条件，即再原有get请求数据的基础上加入分页信息，需要用到第一步的mutable=true
        self.query_dict.setlist(self.page_param, [max(self.page - 1, 1)])

        # 上一页和下一页
        prev = f'<li class="{"disabled" if self.page == 1 else ""}"><a href="?{self.query_dict.urlencode()}" aria-label="Previous"><span aria-hidden="true">«</span></a></li>'
        self.page_str_list.append(prev)

        for i in range(start_page, end_page + 1):
            self.query_dict.setlist(self.page_param, [i])
            if i == self.page:
                elem = f'<li class="active"><a href="?{self.query_dict.urlencode()}">{i}</a></li>'
            else:
                elem = f'<li><a href="?{self.query_dict.urlencode()}">{i}</a></li>'
            self.page_str_list.append(elem)

        self.query_dict.setlist(self.page_param, [min(self.page + 1, self.page_count)])
        next = f'<li class="{"disabled" if self.page == self.page_count else ""}"><a href="?{self.query_dict.urlencode()}" aria-label="Next"><span aria-hidden="true">»</span></a></li>'
        self.page_str_list.append(next)
        return ''.join(self.page_str_list)

````



#### 9.10.7 时间插件

引入datepicker插件

````html
{% block css %}
    <link rel="stylesheet" href="{% static 'plugins/bootstrap-datepicker/css/bootstrap-datepicker.css' %}">
{% endblock %}

{% block js %}
    <script src="{% static 'plugins/bootstrap-datepicker/js/bootstrap-datepicker.js' %}"></script>
    <script src="{% static 'plugins/bootstrap-datepicker/locales/bootstrap-datepicker.zh-CN.min.js' %}"></script>

    <script>
        $(function () {
            {# ModelForm自动生成的element的id为"id_元素名称" #}
            $("#id_create_time").datepicker({
                format: 'yyyy-mm-dd',
                {# startDate: '0', #}
                language: 'zh-CN',
                autoclose: true,
            });
        })
    </script>
{% endblock %}
````

#### 9.10.8 ModelForm和Bootstrap

- ModelForm可以帮助我们生产HTML标签，但是没有bootstrap样式

  - 可以通过定义插件来添加样式（比较繁琐）

    ````python
    class UserModelForm(forms.ModelForm):
        class Meta:
            model = models.UserInfo
            fields = ["name", "password"]
            widgets = {
                "name": forms.TextInput(attrs={"class": "form-control"}),
                "name": forms.PasswordInput(attrs={"class": "form-control"}),
            }
    ````

    ````python
    class UserModelForm(forms.ModelForm):
        name = forms.CharField(
        	min_lenth=3,
            label="用户名"，
            widget=forms.TextInput(attrs={"class": "form-control"})
        )
        class Meta:
            model = models.UserInfo
            fields = ["name", "password"]
    ````

  - 重新定义方法，批量设置

    ````python
    class UserModelForm(forms.ModelForm):
        class Meta:
            model = models.UserInfo
            fields = ["name", "password"]
    
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            # 循环ModelForm中的所有字段，给每个字段的插件设置，但是这样会直接覆盖掉原来attrs中的属性
            for name, field in self.fields.items():
                field.widget.attrs = {
                    "class": "form-control", 
                    "placeholder": field.label, 
                }
    ````

    优化：

    ````python
    class UserModelForm(forms.ModelForm):
        class Meta:
            model = models.UserInfo
            fields = ["name", "password"]
    
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            # 循环ModelForm中的所有字段，给每个字段的插件设置
            for name, field in self.fields.items():
                # 如果本来attr中有值，那么就新增值，没有值就覆盖
                if field.widget.attrs:
                   	field.widget.attrs["class"] = "form-control"
                    field.widget.attrs["placeholder"] = field.label
                else:
                    field.widget.attrs = {
                        "class": "form-control", 
                        "placeholder": field.label, 
                    }
    ````

    但是这样每新写一个类，都要重新定义init。

  - 自定义继承ModelForm的类，面向对象

    ````python
    class BootStrapModelForm(forms.ModelForm):
    
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            for name, field in self.fields.items():
                if field.widget.attrs:
                   	field.widget.attrs["class"] = "form-control"
                    field.widget.attrs["placeholder"] = field.label
                else:
                    field.widget.attrs = {
                        "class": "form-control", 
                        "placeholder": field.label, 
                    }
    ````

    ````python
    class UserModelForm(BootStrapModelForm):
        class Meta:
            model = models.UserInfo
            fields = ["name", "password"]
    ````

### 9.11 管理员操作

#### 9.11.1 管理员表结构

| ID   | username | password |
| ---- | -------- | -------- |
|      |          |          |

#### 9.11.2 管理员列表

````python
def admin_list(request):
    """ 管理员列表 """
    data_dict = {}
    # 获取查询要求
    search_data = request.GET.get("q", "")
    if search_data:
        data_dict["username__contains"] = search_data
    admin_list = models.Admin.objects.filter(**data_dict)
	# 分页
    page_obj = Pagination(request, query_set=admin_list, page_size=2)
    context = {
        "admin_list": page_obj.page_set,
        "page": mark_safe(page_obj.html()),
        "search_data": search_data
    }
    return render(request, 'admin_list.html', context)
````

#### 9.11.3 管理员新建

````python
class AdminModelForm(BootStrapModelForm):
    confirm_password = forms.CharField(
        label="确认密码",
        widget=forms.PasswordInput
    )

    class Meta:
        model = models.Admin
        fields = ["username", "password"]
        widgets = {
            # 加入render_value=True就会在提交后保留框内的值
            "password": forms.PasswordInput(render_value=True)
        }

    def clean_password(self):
        # 使用md5加密密码
        pwd = self.cleaned_data.get("password")
        return md5(pwd)

    def clean_confirm_password(self):
        # 校验两次输入的密码是否相同
        pwd = self.cleaned_data.get("password")
        confirm = self.cleaned_data.get("confirm_password")
        if pwd != md5(confirm):
            raise ValidationError("密码不一致")
        # 返回确认后字段的值，保存到cleaned_data中
        return confirm
    
# 把之前的添加页面封装到change.html以便复用
def admin_add(request):
    """ 添加管理员 """
    title = "新建管理员"
    if request.method == "GET":
        form = AdminModelForm()
        return render(request, "change.html", {"title": title, "form": form})

    form = AdminModelForm(data=request.POST)
    if form.is_valid():
        form.save()
        return redirect("/admin/list/")

    return render(request, "change.html", {"title": title, "form": form})
````

#### 9.11.4 管理员编辑和删除

````python
def admin_edit(request, nid):
    row_obj = models.Admin.objects.filter(id=nid).first()
    if not row_obj:
        return render(request, "error.html", {"msg": "数据不存在"})

    title = "编辑管理员"
    if request.method == "GET":
        form = AdminModelForm(instance=row_obj)
        return render(request, "change.html", {"title": title, "form": form})

    form = AdminModelForm(data=request.POST, instance=row_obj)
    if form.is_valid():
        form.save()
        return redirect('/admin/list/')

    return render(request, 'change.html', {"form": form, "title": title})


def admin_delete(request, nid):
    """ 删除管理员 """
    models.Admin.objects.filter(id=nid).delete()
    return redirect('/admin/list/')

````

#### 9.11.5 重置用户密码

````python
class AdminResetModelForm(AdminModelForm):
    """ 继承AdminModelForm类 """
    class Meta:
        model = models.Admin
        # 只显示password字段
        fields = ["password"]
        widgets = {
            # 加入render_value=True就会在提交后保留框内的值
            "password": forms.PasswordInput(render_value=True)
        }

    def clean_password(self):
        pwd = self.cleaned_data.get("password")
        md5_pwd = md5(pwd)
        # 判断当前密码与新输入密码是否一致
        if models.Admin.objects.filter(id=self.instance.pk, password=md5_pwd).exists():
            raise ValidationError("密码不能与之前的相同")
        return md5_pwd


def admin_reset(request, nid):
    """ 重置密码 """
    row_object = models.Admin.objects.filter(id=nid).first()
    if not row_object:
        return redirect('/admin/list/')

    title = f"重置密码-{row_object.username}"
    if request.method == "GET":
        form = AdminResetModelForm()
        return render(request, 'change.html', {"title": title, "form": form})
    form = AdminResetModelForm(instance=row_object, data=request.POST)
    if form.is_valid():
        form.save()
        return redirect("/admin/list/")
    return render(request, 'change.html', {"title": title, "form": form})
````

### 9.12 用户登录

#### 9.12.1 Cookie和Session

浏览器向服务器发送的请求是http或者https请求。http请求是无状态的短连接。一次请求响应结束后，下一次再请求，服务端不会记录你上一次的状态。为了能够保持原来的状态，引入了cookie和session。

Cookie是保存在浏览器上的键值对。

第一次访时，服务端再响应时，会传cookie给浏览器（cookie位于响应头中）。cookie相当于字典，下一次浏览器访问网站时，会自动带这个cookie去请求服务端，供服务端识别。

Session是保存在服务端的。服务端在生成cookie的同时，生成session以保存访问的浏览器的信息。Session的存储形式有很多种：数据库，redis，文件。

![image-20230320232928172](C:\Users\Selene\AppData\Roaming\Typora\typora-user-images\image-20230320232928172.png)

#### 9.12.2 登录界面和逻辑

因为只有两行输入，所以不需要使用modelform来帮助生成信息，直接使用form。

````python
class LoginForm(BootStrap, forms.Form):
    username = forms.CharField(
        label="用户名",
        widget=forms.TextInput,
        required=True
    )
    password = forms.CharField(
        label="密码",
        widget=forms.PasswordInput,
        required=True
    )

    def clean_password(self):
        pwd = self.cleaned_data.get("password")
        return md5(pwd)


def login(request):
    """ 登录 """
    if request.method == "GET":
        form = LoginForm()
        context = {"form": form}
        return render(request, 'login.html', context)

    form = LoginForm(data=request.POST)
    context = {"form": form}
    if form.is_valid():
        # 验证成功获取到的用户名和密码并校验
        admin_obj = models.Admin.objects.filter(**form.cleaned_data).first()

        if not admin_obj:
            # 因为这里是手动校验数据库，如果出错，form不会自动生成错误信息，需要自己手动加
            form.add_error("password", "用户名或密码错误")
            return render(request, 'login.html', context)
        # 如果正确
        # 网站生成随机字符串，写到用户浏览器的cookie中，再写到session中
        request.session["info"] = {
            "name": admin_obj.username,
            "id": admin_obj.id
        }
        return redirect("/admin/list/")
    return render(request, 'login.html', context)

````

````html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href={% static 'plugins/bootstrap-3.4.1/css/bootstrap.min.css' %}>
    <style>
        .login-title {
            text-align: center;
            font-size: 25px;
            font-weight: 1000;
            margin-bottom: 10px;
        }

        .form {
            box-shadow: 10px 10px 10px #aaa;
            padding: 20px 40px;
            width: 500px;
            border: 1px solid #dddddd;
            border-radius: 5px;
            margin: 20px auto;
        }

        .form a {
            float: right;
        }

        .form span{
            color: red;
        }
    </style>
</head>
<body>
<div class="container" style="margin-top: 300px">
    <div class="form clearfix">
        <form method="post" novalidate>
            {% csrf_token %}
            <div class="login-title">用户登录</div>
            {% for field in form %}
                <div class="form-group">
                    <label>{{ field.label }}</label>
                    {{ field }}
                    <span>{{ field.errors.0 }}</span>
                </div>
            {% endfor %}
            <button type="submit" class="btn btn-primary">登录</button>
        </form>
    </div>

</div>
</body>
</html>
````

#### 9.12.3 登录验证（中间件）

用户如果没有登陆，不能让用户访问任何页面。

````python
info = request.session.get("info")
if not info:
    return redirect("/login/")
````

需要给views中每一个视图函数前都加上，非常麻烦。也可以使用装饰器，可以让其他函数在不需要做任何代码变动的前提下增加额外功能。

````python
def check(func):
	def inner():
		...
		
	return inner

@check
def xxx(request):
	...
	
# 相当于 check(xxx(request))
````

若使用装饰器，则还是需要给每个视图函数前关联装饰器。

Django的中间件，是在执行视图函数前，请求经过的类。如果请求在中间件中被否决，那么不会传到视图函数，而是直接返回。执行顺序类似stack，后进先出。

![image-20230323194100601](C:\Users\Selene\AppData\Roaming\Typora\typora-user-images\image-20230323194100601.png)

- 定义中间件

  ````python
  from django.utils.deprecation import MiddlewareMixin
  class M1(MiddlewareMixin):
      """ 中间件1 """
  
      def process_request(self, request):
          # 如果方法返回None，则继续向后走进入下一个中间件
          # 如果返回Httpresponse，render， redirect，则不向后走，直接回头
          pass
  
      def process_response(self,request, response):
          return response
  ````

- 注册中间件，在settings.py里加上定义的中间件

  ````python
  MIDDLEWARE = [
      "django.middleware.security.SecurityMiddleware",
      "django.contrib.sessions.middleware.SessionMiddleware",
      "django.middleware.common.CommonMiddleware",
      "django.middleware.csrf.CsrfViewMiddleware",
      "django.contrib.auth.middleware.AuthenticationMiddleware",
      "django.contrib.messages.middleware.MessageMiddleware",
      "django.middleware.clickjacking.XFrameOptionsMiddleware",
      "app01.middleware.auth.M1",
  ]
  ````

编写中间件：

````python
from django.utils.deprecation import MiddlewareMixin
from django.shortcuts import HttpResponse, redirect


class AuthMiddleware(MiddlewareMixin):
    """ 中间件1 """

    def process_request(self, request):
        # 如果方法返回None，则继续向后走进入下一个中间件
        # 如果返回Httpresponse，render， redirect，则不向后走，直接回头

        # 0. 排除不需要登录就能访问的页面
        # 获取当前用户请求的url
        if request.path_info == "/login/":
            return

        # 1. 读取当前访问的用户的session信息，如果能读到，说明已经登录，继续向后执行
        # 自动对比用户发送的cookie和缓存中对应的session
        info = request.session.get("info")
        if info:
            return

        # 2. 没登陆过
        return redirect("/login/")

    def process_response(self,request, response):
        return response
````

#### 9.12.4 用户注销

````python
def logout(request):
    """ 注销 """
    request.session.clear()
    return redirect("/login/")
````

#### 9.12.5 修改layout.html

每次登录显示登录用户的用户名，使用request.session.info来获取当前登录的用户信息。

```request.session: {'id': xx, 'name': 'xxx'}```

````html
<li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">{{ request.session.info.name }} <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">个人资料</a></li>
                        <li><a href="#">我的信息</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="/logout/">注销</a></li>
                    </ul>
                </li>
````

#### 9.12.6 图片验证码

防止暴力破解

- 生成图片验证码

  使用pillow库：

  ````python
  from PIL import Image, ImageDraw, ImageFont
  
  # 创建大小为120*30，通道为RGB，颜色为白色的图片
  img = Image.new(mode="RGB", size=(120, 30), color=(255, 255, 255))
  # 在img图像上创建画笔，模式为RGB
  draw = ImageDraw.Draw(imgh, mode='RGB')
  # 使用字体Monaco.ttf
  font = ImageFont.truetype("Monaco.ttf", 28)
  # 在img上画出文本，从0， 0像素位置开始画，要画的文本为python，颜色为红色，字体为font
  draw.text([0,0], "python", "red"， font=font)
  
  with open("xxx.png", "wb") as f:
      img.save(f, format="png")
  ````

  实现：

  ````python
  import random
  from PIL import Image, ImageDraw, ImageFont, ImageFilter
  
  
  def check_code(width=120, height=30, char_length=5, font_file='C:/Users/Selene/PycharmProjects/EmployerManageSystem/font/kumo.ttf', font_size=28):
      code = []
      img = Image.new(mode='RGB', size=(width, height), color=(255, 255, 255))
      draw = ImageDraw.Draw(img, mode='RGB')
  
      def rndChar():
          """
          生成随机字母
          :return:
          """
          return chr(random.randint(65, 90))
  
      def rndColor():
          """
          生成随机颜色
          :return:
          """
          return (random.randint(0, 255), random.randint(10, 255), random.randint(64, 255))
  
      # 写文字
      font = ImageFont.truetype(font_file, font_size)
      for i in range(char_length):
          char = rndChar()
          code.append(char)
          h = random.randint(0, 4)
          draw.text([i * width / char_length, h], char, font=font, fill=rndColor())
  
      # 写干扰点
      for i in range(40):
          draw.point([random.randint(0, width), random.randint(0, height)], fill=rndColor())
  
      # 写干扰圆圈
      for i in range(40):
          draw.point([random.randint(0, width), random.randint(0, height)], fill=rndColor())
          x = random.randint(0, width)
          y = random.randint(0, height)
          draw.arc((x, y, x + 4, y + 4), 0, 90, fill=rndColor())
  
      # 画干扰线
      for i in range(5):
          x1 = random.randint(0, width)
          y1 = random.randint(0, height)
          x2 = random.randint(0, width)
          y2 = random.randint(0, height)
  
          draw.line((x1, y1, x2, y2), fill=rndColor())
  
      img = img.filter(ImageFilter.EDGE_ENHANCE_MORE)
      return img, ''.join(code)
  
  
  def image_code(request):
      """ 生成图片验证码 """
      img, code_string = check_code()
  
      # 将文件写入内存中
      stream = BytesIO()
      img.save(stream, 'png')
  
      # 把验证码写入session中，与用户对应
      request.session['image_code'] = code_string
      # 给session设置60秒超时，即验证码60秒过期
      request.session.set_expiry(60)
  
      return HttpResponse(stream.getvalue())
  ````

- 校验

  从session中读取记录的值，并且获取cleaned_data中用户输入的值，使用pop以剔除cleaned_data中验证码的值，使后续用户名密码校验不会出错。
  
  ````python
  def login(request):
      """ 登录 """
      if request.method == "GET":
          form = LoginForm()
          context = {"form": form}
          return render(request, 'login.html', context)
  
      form = LoginForm(data=request.POST)
      context = {"form": form}
      if form.is_valid():
          # 验证成功获取到的用户名和密码并校验
  
          # 剔除cleaned_data中的code，以免影响用户名密码校验
          user_input_code = form.cleaned_data.pop('code')
          code = request.session.get('image_code', "")
          if code.upper() != user_input_code.upper():
              form.add_error("code", "验证码错误")
              return render(request, 'login.html', context)
          admin_obj = models.Admin.objects.filter(**form.cleaned_data).first()
  
          if not admin_obj:
              # 因为这里是手动校验数据库，如果出错，form不会自动生成错误信息，需要自己手动加
              form.add_error("password", "用户名或密码错误")
              return render(request, 'login.html', context)
          # 如果正确
          # 网站生成随机字符串，写到用户浏览器的cookie中，在写到session中
          request.session["info"] = {
              "name": admin_obj.username,
              "id": admin_obj.id
          }
          # 重新设置session过期时间
          request.session.set_expiry(60 * 60 * 24 * 7)
          return redirect("/admin/list/")
      return render(request, 'login.html', context)
  ````
  
  html中，使用onclick绑定事件，每次点击验证码图片，就给图片网页生成一个get请求输入，用于点击刷新验证码
  
  ````html
  <img id="image_code" src="/image/code/" onclick="this.setAttribute('src', '/image/code/?random='+Math.random())">
  ````

### 9.13 Ajax请求

浏览器向网站发送请求：URL和表单的形式提交。特点是提交后页面会刷新。

- GET
- POST

除此之外，基于Ajax向后台发送请求。页面不会刷新：

- 依赖jQuery

- 编写ajax代码：

  - 基于DOM绑定

    ````javascript
    $.ajax({
        url:"发送的地址",
        type:"请求的方式(get, post)",
        data:{
            n1: 123,
            n2: 456
        },
        success: function(res){
            // 获取后端返回的值
            console.log(res);
        }
    })
    ````

  - 基于jQuery绑定：

    ````javascript
    $(function () {
        // 页面框架加载完成后代码自动执行
        bindBtn1Event()
    })
    
    function bindBtn1Event() {
        $("#btn1").click(function () {
            $.ajax({
                url: '/task/ajax/',
                type: "get",
                data: {
                    n1: 123,
                    n2: 456
                },
                success: function (res) {
                    console.log(res)
                }
            })
        })
    }
    ````

#### 9.13.1 使用ajax发送get请求

````javascript
function clickme() {
    $.ajax({
        url: '/task/ajax/',
        type: "get",
        data: {
            n1: 123,
            n2: 456
        },
        success: function (res) {
            console.log(res)
        }
    })
}
````

````python
def task_ajax(request):
    print(request.GET)
    return HttpResponse("成功了")
````

#### 9.13.2 使用ajax发送post请求

由于django中对post请求发送的表单要求csrftoken，所以发送post请求会报错。可以从session和请求头中获取csrftoken并携带过去，也可以免除csrftoken。

使用装饰器，将目标方法传入django内建的方法免除csrf认证的方法：

````python
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def task_ajax(request):
    print(request.GET)
    return HttpResponse("成功了")
````

#### 9.13.3 ajax的返回值

一般不会返回页面，返回json。前端可以拿到json的数据进行操作

````python
import json
@csrf_exempt
def task_ajax(request):
    print(request.GET)
    data_dict = {'status': True, 'data': [11, 22, 33, 44]}
    json_string = json.dumps(data_dict)
    return HttpResponse(json_string)
````

也可以直接使用django内封装的jsonresponse函数，不需要自己将字典转成json字符串。

````python
from django.http import JsonResponse
@csrf_exempt
def task_ajax(request):
    print(request.GET)
    data_dict = {'status': True, 'data': [11, 22, 33, 44]}
    return JsonResponse(data_dict)
````

前端代码加上dataType设置可以直接解包json数据。

````javascript
function bindBtn1Event() {
            $("#btn1").click(function () {
                $.ajax({
                    url: '/task/ajax/',
                    type: "get",
                    data: {
                        n1: 123,
                        n2: 456
                    },
                    dataType: "JSON",
                    success: function (res) {
                        console.log(res.status)
                        console.log(res.data)
                    }
                })
            })
        }
````

也可以通过form标签框住所有的输入框，使用jQuery中的serialize方法自动将所有输入转化为字典。

````html
<h3>示例3</h3>
<form id="form3">
    <input type="text" name="name" placeholder="姓名">
    <input type="text" name="age" placeholder="年龄">
    <input type="text" name="email" placeholder="邮箱">
    <input type="text" name="more" placeholder="其他">
</form>
<input type="button" id="btn3" class="btn btn-primary" value="点击"/>
````

````javascript
function bindBtn3Event() {
    $("#btn3").click(function () {
        $.ajax({
            url: '/task/ajax/',
            type: "post",
            data: $("#form3").serialize(),
            dataType: "JSON",
            success: function (res) {
                console.log(res)
            }
        })
    })
}
````

#### 9.13.4 案例

使用ajax和modelform，实现无刷新提交表单数据。并返回错误信息。

由于校验完成后并不能通过返回render的方式将包含错误信息的modelform传给前端，只能将错误信息包装为json传递给前端，再由javascript完成页面的替换。可以直接使用responsejson包装直接包装forms.error，也可以直接调用ErrorDict类中的as_json()方法直接将errors转化为json：

````python
import django.forms.utils.ErrorDict
def xx(request):
    form = xxx
    return HttpResponse(form.errors.as_json())
````

完整实现：

````python
class TaskModelForm(BootStrapModelForm):
    class Meta:
        model = models.Task
        fields = '__all__'
        widgets = {
            "detail": forms.TextInput,
        }

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        for name, field in self.fields.items():
            if name == "user":
                # 设置默认空选项的名字
                field.empty_label = "请选择"
            if field.widget.attrs:
                field.widget.attrs["class"] = "form-control"
                field.widget.attrs["placeholder"] = field.label
            else:
                field.widget.attrs = {
                    "class": "form-control",
                    "placeholder": field.label,
                }
                
@csrf_exempt
def task_add(request):
    data_dict = request.POST
    print(data_dict)

    # 1. 用户发送过来的数据进行校验，ModelForm进行校验
    form = TaskModelForm(data=data_dict)
    if form.is_valid():
        form.save()
        response = {"status": True}
        return JsonResponse(response)

    response = {"status": False, "error": form.errors}
    return JsonResponse(response)
````

JS代码

````javascript
function bindBtnAddEvent() {
    $("#btnAdd").click(function () {
        // 新提交后清空上一次传入的错误信息
        $(".error-msg").empty()

        // 使用ajax请求发送form里的数据
        $.ajax({
            url: '/task/add/',
            type: "post",
            data: $("#formAdd").serialize(),
            dataType: "JSON",
            success: function (res) {
                if (res.status) {
                    // 添加成功弹窗提示
                    alert("添加成功")
                } else {
                    // $.each自动遍历第一个参数中的所有元素，类似python的enumerate，生成键值对，第二个参数为对应执行的方法
                    $.each(res.error, function (name, data) {
                        // 设置错误信息，由于Modelform生成的标签自带的id为id_xxx
                        // 所以使用jQuery的id选择器，将对应名称和id_拼接后选定所需要的标签，设置该标签下方的错误信息
                        $("#id_" + name).next().text(data[0])
                    })
                }
            }
        })
    })
}
````

如果添加成功需要刷新页面，则使用JS语句```location.reload()```：

````javascript
success: function (res) {
    if (res.status) {
        // 添加成功弹窗提示并刷新页面
        alert("添加成功")
        location.reload()
    }
}
````

### 9.14  订单管理

#### 9.14.1  订单表结构

| ID   | 订单号 | 商品名称 | 价格 | 状态          | 用户ID      |
| ---- | ------ | -------- | ---- | ------------- | ----------- |
|      |        |          |      | 已支付/待支付 | foreign key |

#### 9.14.2 订单列表模态框添加

使用模态框进行新建。绑定模态框弹出有两种方式：

- 使用bootstrap写好的方式，直接在input中添加attributes: ```data-toggle="modal" data-target="#myModal"```

  ````html
  <button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal">
  	Launch demo modal
  </button>
  ````

- 使用JS绑定事件

  ````javascript
  function bindBtnAddEvent() {
      $("#btnAdd").click(function () {
          // 点击新建按钮
          $("#myModal").modal('show')
      })
  }
  ````

通过ajax请求发送输入的数据：

````javascript
function bindBtnSaveEvent() {
    $("#btnSave").click(function () {

        // 清除错误信息
        $(".error-msg").empty()
		// 发送数据
        $.ajax({
            url: "/order/add/",
            type: "post",
            data: $("#formAdd").serialize(),
            dataType: "JSON",
            success: function (res) {
                if (res.status) {
                    alert("创建成功")
                } else {
                    $.each(res.error, function (name, errorList) {
                        $("#id_" + name).next().text(errorList[0])
                    })
                }
            }
        })
    })
}
````

在后端页面，希望数据库中的订单号oid为系统默认生成，不让用户自己输入：

````python
class OrderModelForm(BootStrapModelForm):
    class Meta:
        model = models.Order
        # 排除oid字段
        exclude = ["oid"]
````

排除oid后，在后面的提交数据并保存时，需要自己生成该字段。

````python
@csrf_exempt
def order_add(request):
    """ 新建订单 """
    form = OrderModelForm(data=request.POST)
    if form.is_valid():
        # 额外增加一些不是用户输入的值 (自己计算) 获取当前时间加上任意随机数
        form.instance.oid = datetime.now().strftime("%Y%m%d%H%M%S") + str(random.randint(1000, 9999))
        form.save()
        return JsonResponse({"status": True})
    return JsonResponse({"status": False, "error": form.errors})
````

#### 9.14.3 订单删除

首先定义一个全局变量，当点击操作中的删除按钮时，将该行的id赋值给该全局变量。

然后再模态框中点击删除按钮，根据这个全局变量的值，发送数据给服务器。

删除完毕，可以不重新刷新页面，而是手动删除页面上对应的数据。再数据库较大，并发较多时，节省数据库性能。

````javascript
var DELETE_ID;

function bindBtnDeleteEvent() {
    $(".btn-delete").click(function () {
        // 显示删除对话框
        $("#deleteModal").modal("show")

        // 获取当前行的ID获取全局变量
        DELETE_ID = $(this).attr("uid")
    })
}

function bindBtnConfirmDelete() {
    $('#btnConfirmDelete').click(function () {
        // 将全局变量中设置的要删除的id发送到后台
        $.ajax({
            url: "/order/delete/",
            type: "GET",
            data: {
                uid: DELETE_ID
            },
            dataType: "JSON",
            success: function (res) {
                if (res.status) {
                    // 删除成功
                    // 手动在页面寻找到对应的数据，通过js删除
                    $("#deleteModal").modal("hide")
                    $("tr[uid='" + DELETE_ID + "']").remove()
                } else {
                    // 删除失败
                    alert(res.error)
                }
            },
        })
    })
}
````

````html
<div class="modal fade" id="deleteModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog" role="document">

        <div class="alert alert-danger alert-dismissible fade in" role="alert">
            <h4>是否确定删除？</h4>
            <p style="margin: 10px 0;">删除后所有关联的相关数据都会被删除</p>
            <p style="text-align: right">
                <button id="btnConfirmDelete" type="button" class="btn btn-danger" control-id="ControlID-60">确定
                </button>
                <button type="button" class="btn btn-default" control-id="ControlID-61" data-dismiss="modal">取消
                </button>
            </p>
        </div>
    </div>
</div>
````

````python
def order_delete(request):
    """ 删除订单 """
    uid = request.GET.get("uid")
    if not models.Order.objects.filter(id=uid).exists():
        return JsonResponse({'status': False, 'error': "数据不存在"})
    models.Order.objects.filter(id=uid).delete()
    return JsonResponse({'status': True})
````

#### 9.14.4 订单编辑

取数据库获取对象/字典：

````python
# 对象，当前行所有数据
row_obj = models.Order.objects.filter(id=uid).first()

# 对应字段的字典 {"id": xx, "title": xx}
row_dict = models.Order.objects.filter(id=uid).values("id", "title").first()

# queryset，所有符合条件的数据组成的集合 [obj, obj, obj]
query_set = models.Order.objects.filter(id=uid).all()

# queryset，所有符合条件的对应字段的字典的集合 [{"id": xx, "title": xx}, {"id": xx, "title": xx}]
row_dict = models.Order.objects.filter(id=uid).values("id", "title").all()

# queryset，所有符合条件的对应字段的值和idx的元组的集合 [(1, "xx"), (2, "xx")]
row_dict = models.Order.objects.filter(id=uid).values_list("id", "title").all()
````

## 10. Json Web Token

JWT, 一般用于前后端分离项目的用户认证

### 10.1 Token

传统token方法，使用uuid生成随机字符串，作为token发送给用户，并存在对应用户的数据库。用户发请求的时候，带上token，去数据库进行校验

```python
import uuid

class LoginView(APIView):
    
    def post(self, request, *args, **kwargs):
        user = request.data.get("username")
        pwd = request.data.get('pwd')
        
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({'code': 1000, 'error': '用户名或密码错误'})
        # 使用uuid生成随机字符串，存入数据库并传给用户
        random_string = str(uuid.uuid4())
        user_object.token = random_string
        user_object.save()
        return Response({'code': 1001, 'data': random_string})    
    
    
class OrderView(APIView):
    
    def get(self, request, *args. **kwargs):
        # 用户带着token来访问
        token = request.query_params.get("token")
        if not token:
            return Response({'code': 2000, 'error': "请登录"})
        user_object = models.UserInfo.objects.filter(token=token).first()
        if not user_object:
            return Response({'code': 2000, 'error': 'token无效'})
       	return Response('订单列表')
```

### 10.2 JWT

服务端给用户返回token，服务端不需要保存。客户端携带token来访问服务端，服务端进行校验。

优势：

- 无需在服务端保存

实现原理:

- 用户提交用户名和密码给服务端，服务端使用jwt创建一个token，返回给客户端。jwt生成的token，是一个由`.`拼接起来的3个字符串：

  - 第一段字符串Header：

    - 加密算法（默认SHA256）：`"alg": "HS256"`

    - token类型：`"typ": "JWT"`

    由json格式转化为字符串，然后做base64url编码，可以被解码

  - 第二段字符串Payload，自定义，一般可以是:

    - `"id": "123123"`
    - `"name": "Selene"`
    - 超时时间`"exp": 123412`

    由json格式转化为字符串，然后做base64url编码，可以被解码（不要返回敏感信息：密码等）

  - 第三段字符串为：

    - 编码后的第一，二段字符串用`.`拼接后
    - 对这个得到的字符串进行第一段Header中设定的算法的加密（SHA256）。加密前需要对字符串加盐（可以使用django.settings.Secret_Key），防止用户自行加密得到合法token。
    - 对加密后的字符串，进行一次base64url编码

- 以后用户再来访问，需要携带token，后端对token进行校验

  - 获取token
  - 对token按`.`分割
  - 对第二段进行base64url解码，获取payload信息并检测是否超时
  - 第1，2段拼接加盐再次进行SHA256加密，得到的密文和第3段进行base64url解码结果对比。如果相等，认证通过。

```python
import jwt
import datetime
from jwt import exceptions
from django.conf import settings

class JwtLoginView(APIView):
    
    def post(self, request, *args, **kwargs):
        user = request.data.get("username")
        pwd = request.data.get('pwd')
        
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({'code': 1000, 'error': '用户名或密码错误'})
        # 构造header
        headerrs = {
            'alg': 'HS256',
            'typ': 'jwt'
        }
        # 构造payload
        payload = {
            'user_id': user_object.pk,
            'user_name': user,
            'exp': datetime.datetime.utcnow() + datetime.timedelta(minutes=5)		# 超时时间设置
        }
        salt = settings.SECRET_KEY
        token = jwt.encode(payload=payload, key=salt, algorithm="HS256", headers=headers).encode('utf-8')
        return Response({'code': 1001, 'data': token})
    
    
class JwtOrderView(APIView):
    
    def get(self, request, *args. **kwargs):
        # 获取token并判断合法性
        token = request.query_params.get("token")
        # 1. 切割
        # 2. 解密第二段/判断过期
        # 3. 验证第三段合法性
        salt = settings.SECRET_KEY
        try:
        	verified_paylaod = jwt.decode(token, salt, True)
            return verified_payload
        except exceptions.ExpiredSignatureError:
            msg = 'token已失效'
        except jwt.DecodeError:
            msg = 'token认证失败'
        except jwt.InvalidTokenError:
            msg = '非法的token'
        if not payload:
            return Response({'code': 1003, 'error': msg})
        return Response('订单列表')
```

### 10.3 进阶方法

将加密解密功能解耦。

加密：

```python
import datetime
import jwt
from django.conf import settings

def create_token(payload, timeout=1):
	salt = settings.SECRET_KEY
    headerrs = {
        'alg': 'HS256',
        'typ': 'jwt'
    }
    payload['exp'] = datetime.datetime.utcnow() + datetime.timedelta(minutes=timeout)		# 超时时间设置
    token = jwt.encode(payload=payload, key=salt, algorithm="HS256", headers=headers).encode('utf-8')
    return token
```

之后，直接在生成token时调用这个方法

```python
class JwtLoginView(APIView):
    
    def post(self, request, *args, **kwargs):
        user = request.data.get("username")
        pwd = request.data.get('pwd')
        
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({'code': 1000, 'error': '用户名或密码错误'})
         payload = {
            'user_id': user_object.pk,
            'user_name': user
        }
        token = creat_token(payload)
        return Response({'code': 1001, 'data': token})
```

解密：

```python
from rest_framework.authentication import BaseAuthentication
from django.conf import settings
from rest_framework.exceptions import AuthenticationFailed
import jwt

class JwtQueryParamsAuthentication(BaseAuthentication):
    
    def authenticate(self, request):
        token = request.query_params.get("token")
        salt = settings.SECRET_KEY
        try:
        	paylaod = jwt.decode(token, salt, True)
        # 如果验证不通过，抛出异常
        except exceptions.ExpiredSignatureError:
            raise AuthenticationFailed({'code': 1003, 'error': 'token已失效'})
        except jwt.DecodeError:
            raise AuthenticationFailed({'code': 1003, 'error': 'token认证失败'})
        except jwt.InvalidTokenError:
            raise AuthenticationFailed({'code': 1003, 'error': '非法的token'})
		# 验证通过，返回payload和token
        return (paylaod, token)
```

之后，可以在定义视图类时，定义类变量，效果类似中间件，如果抛出异常，则不会进入页面渲染阶段

```python
class JwtOrderView(APIView):
    
    authentication_classes = [JwtQueryParamsAuthentication, ]
    
    def get(self, request, *args. **kwargs):
        
        return Response('订单列表')
```

也可以不定义类变量。在setting.py中加上一段，设置默认验证类为自定义类。这样，APIView类中的默认验证，就会变成自定义的方法。

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": ['api.extensions.auth.JwtQueryParamsAuthentication', ]
}
```

但是，需要排除登陆页面，覆盖类变量为空.

```python
class JwtLoginView(APIView):
    
    authentication_classes = []
    
    def post(self, request, *args, **kwargs):
        user = request.data.get("username")
        pwd = request.data.get('pwd')
        
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({'code': 1000, 'error': '用户名或密码错误'})
         payload = {
            'user_id': user_object.pk,
            'user_name': user
        }
        token = creat_token(payload)
        return Response({'code': 1001, 'data': token})
```

### 10.4 应用

```python
pip install pyjwt
pyjwt.encode		# 生成token
pyjwt.decode		# 解密token
```

djangorestframework-jwt也是调用pyjwt实现。
