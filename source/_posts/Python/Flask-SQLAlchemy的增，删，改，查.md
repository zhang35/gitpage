### Flask-SQLAlchemy的使用：

**ORM的好处：可以让我们操作数据库跟操作对象是一样的，非常方便，因为一个表就抽象成一个类，一条数据就抽象成该类的一个对象**

1.初始化和设置数据库配置信息

- 使用flask_sqlalchemy中的SQLAlchemy进行初始化：

  ```python
  from flask_sqlalchemy import SQLAlchemy
  app = Flask(__name__)
  db = SQLAlchemy(app) # 通过类`SQLAlchemy`来连接数据库
  ```

- 设置配置信息：在`config.py`文件中添加以下的配置信息：

  

  ```python
  DB_URI = 'mysql+mysqldb://{}:{}@{}/{}.format(USERNAME，PASSWORD，HOSTNAME，PORT，DATABASE)'
  # 创建数据库引擎
  engine = create_engine(DB_URI)
  ```

  首先从sqlalchemy中导入`create_engine`,用这个函数来创建引擎，然后用`engine.connect()`来连接数据库，其中一个比较重要的点是，通过create_engine函数的时候，需要传递一个满足某种**格式**的字符串，对这个字符串的格式来进行解释：

  

  ```cpp
  dialect+driver://username:password@host:port/database
  ```

  dialect是数据库的实现，比如MySQL,PostgreSQL,SQLite,并且转换为小写，driver是python对应的驱动，如果不指定，会选择默认的驱动，比如MySQL的默认驱动是Mysqldb,username是数据库的用户名，password是连接数据库的密码，host是连接数据的域名，port是数据库监听的端口号，database是连接哪个数据库的名字。

- 在主`app`文件中，添加配置文件：

  

  ```python
  app = Flask(__name__)
  app.config.from_object(config)
  db = SQLAlchemy(app)
  ```

- 测试，看有没有问题：

  

  ```python
  db.create_all()
  ```

  如果没有报错，说明配置没有问题，如果有错误，可以根据错误进行修改

**Python语言不像Java, C,C++会有`main()`函数，Python是脚本语言，只会由上往下执行。**

### Flask-SQLAlchemy的使用

1. 初始化和设置数据库配置信息

- 使用flask_sqlalchemy中的SQLAlchemy经行初始化：



```python
from flask_sqlalchemy import SQLAlchemy
app = Flask(__name__)
db = SQLAlchemy(app)
```

1. 设置配置信息：`config.py`文件中添加配置信息

2. 在主`app`文件中，添加配置文件：

   

   ```python
   app = Flask(__name__)
   app.config.from_object(config)
   db = SQLAlchemy(app)
   ```

3. 做测试，看有没有问题

   

   ```python
   db.create_all()
   ```



```python
 class Article(db.Model):
   id = db.Column(db.Integer)
```

### 使用Flask-SQLAlchemy创建模型与表的映射：

1. 模型需要继承自`db.Model`,然后需要映射到表中的属性，必须写成`db.Column`的数据类型。
2. 数据类型：
   - `db.Integer`代表的是整形.
   - `db.String`代表的是`varchar`,需要指定最长的长度.
   - `db.Text`代表的是`text`.
3. 其他参数：
   - `primary_key`:代表的是将这个字段设置为主键。
   - `autoincrement`:代表的是这个主键为自增长的。
   - `nullable`:代表的是这个字段是否为空，默认可以为空，可以将这个值设置为`False`,在数据库中，这个值就不能为空了。
4. 最后需要调用`db.create_all`来将模型真正地创建到数据库中。



```python
from flask import Flask
import pymysql
import config
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config.from_object(config)
db = SQLAlchemy(app)

class Article(db.Model):
    __tablename__ = 'article' #如果不指定表名，会默认以这个类名的小写为表名
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    title = db.Column(db.String(100),nullable = False)
    content = db.Column(db.Text,nullable = False)

db.create_all()

@app.route('/')
def index():
    return 'index'

if __name__ == '__main__':
    app.run(debug=True)
```

### Flask-SQLAlchemy的增，删，改，查

1. 增：

   

   ```python
   # 增加：
    article1 = Article(title = u'aaa',content = u'bbb')
    db.session.add(article1)
    # 事务
    db.session.commit()
   ```

2. 查：

   

   ```python
   # select * from article where article.title = 'aaa';
   article1 = Article.query,filter(Article.title == 'aaa').first()
   print('title:%s' % article1.title)
   print('content:%s' % article1.content)
   ```

3. 改：

   

   ```python
   # 改：
    # 1.先把你要更改的数据查找出来
    article1 = Article.query.filter(Article.title == 'aaa').first()
    # 2.把这条数据，你需要修改的地方进行修改
    article1.title = 'New title'
    # 3.做事物的提交
    db.session.commit()
   ```

4. 删：

   

   ```python
   # 删：
    # 1.把需要删除的数据查找出来
    article1 = Article.query.filter(Article.title == 'New title').first()
    # 2.把这条数据删除掉
    db.session.delete(article1)
    # 3.做事物的提交
    db.session.commit()
   ```

### Flask-Script的介绍与安装

1. `Flask-Script`的作用是可以通过命令行的形式来操作Flask.例如通过命令跑一个开发版本的服务器、设置数据库、定时任务等。
2. 如果直接在主`manager.py`中写命令，那么在终端就只需要`python manager.py command_name`就可以了。
3. 如果把一些命令集中在一个文件中，那么在终端就需要输入一个父命令，比如`python manager.py db init`
4. 例子：flask_script_demo.py 和manager.py



```python
# flask_script_demo.py
from flask import Flask

app = Flask(__name__)

db.create_all()

@app.route('/')
def hello_world():
    return 'hello_world'

if __name__ == '__main__':
    app.run(debug=True)
```



```python
# 主manager.py
from flask_script import Manager
from flask_script_demo import app
from db_script import DBmanager

manager = Manager(app)

@manager.command
def runserver():
    print('服务器跑起来了')

manager.add_command('db',DBmanager)


if __name__ == '__main__':
    manager.run()
```

1. 有子命令的例子：db_script.py

```python
from flask_script import Manager

DBmanager = Manager() # 这个为什么不是Manager(app)。因为这个不是主文件，所以不需要在Manager()里面写入'app'

@DBmanager.command
def init():
    print('数据库初始化完成')

@DBmanager.command
def migrate():
    print('数据表重新迁移成功')

# 这个没有下面的代码是因为这个不是主的manager.py。它只是子的文件，所以不需要有下面的代码。
'''
if __name__ == '__main__':
    manager.run()
'''
```

有关Flask-Script还是有很多的不理解,例如代码当中的解释器`@DBmanager.command`这个可以Flask-Script的官网。[Flask-Script](https://flask-script.readthedocs.io/en/latest/)

### 分开models和解决循环引用

1. 分开Models的目的：是为了让代码更加方便地管理。

2. 什么是循环引用？

   **[循环引用] 当一个单元格内的公式直接或间接地应用了这个公式本身所在的单元格时，就称为循环引用。**

   为什么会报错？因为结果还没出来，却又要去调用它。所以会报错！
    假设有如下的文件结构：

   

   ```css
   models_sep.py 
   models.py 
   ```

   

   ```python
   # models_sep.py 
   from flask import Flask
   from flask_sqlalchemy import SQLAlchemy
   from models import Article
   
   app = Flask(__name__)
   db = SQLAlchemy(app)
   
   db.create_all()
   
   @app.route('/')
   def hello_world():
       return 'hello_world'
   
   if __name__ == '__main__':
       app.run(debug=True)
   ```

   

   ```python
   # models.py 
   from models_sep import db 
   
   class Article(db.Model):
       __tablename__ = 'article'
       id = db.Column(db.Integer,primary_key = True,autoincrement = True)
       title = db.Column(db.String(100),nullable = False) 
   ```

上面的代码会造成一个循环引用：

![img](https:////upload-images.jianshu.io/upload_images/2459618-9e5ac8143f149b5d.png?imageMogr2/auto-orient/strip|imageView2/2/w/442/format/webp)

循环引用

主文件app引入models文件中的Article.但models文件要引入app中的db,才可以生成，做成了循环引用。

1. 如何解决循环引用：把`db`放在一个单独的文件中，切断循环引用的线条就可以
    假设有如下的文件结构：

   

   ```css
   app.py 
   exts.py 
   models.py 
   ```

   

   ```python
   # app.py 
   from flask import Flask
   from models import Article
   from exts import db
   import config
   
   app = Flask(__name__)
   app.config.from_object(config)
   db.init_app(app) # init_app()可以用来解决循环引用的问题
   
   db.create_all()
   if __name__ == '__main__':
     app.run(debug = True)
   
   # exts.py 
   from flask_sqlalchemy import SQLAlchemy
   db = SQLAlchemy()
   
   # models.py 
   from exts import db
   
   class Article(db.Model):
     __tablename__ = 'article'
     id = db.Column(db.Integer,primary_key = True,autoincrement = True)
     title = db.Column(db.String(100),nullable = False)
    
   ```

### Flask-SQLAlchemy的外键约束

1. 外键：

   

   ```python
   class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    title = db.Column(db.String(100),nullable = False)
    content = db.Column(db.Text,nullable = False)
    author_id = db.Column(db.Integer,db.ForeignKey('user.id')) # user这个是表的名字，而不是类的名字。注意
    author = db.relationship('User',backref = db.backref('articles')) # User这个是类的名字，注意
                                                                      # relations：会在Article表中寻找外键‘user.id’然后关联到User表
   ```

2. ```
   author = db.relationship('User',backref = db.backref('articles'))
   ```

   解释：

   - 给`Article`这个模型添加一个`author`属性，可以访问这篇文章的作者的数据，像访问普通模型一样。
   - `backref`是定义反向引用的，可以通过`User.articles`访问这个模型所写的所有文章

*这里有一个知识点，当`db.create_all()`运行了后，数据库的表已经创建好了。若要再对表(类)修改，添加外键，关系等。将不会生效。因为表，已经在数据库中创建好了。*
 例子代码：



```python
from flask import Flask
import pymysql
import config
from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)
app.config.from_object(config)
db = SQLAlchemy(app)

# 用户表
class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    username = db.Column(db.String(100),nullable = False)

# 文章表
class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    title = db.Column(db.String(100),nullable = False)
    content = db.Column(db.Text,nullable = False)
    author_id = db.Column(db.Integer,db.ForeignKey('user.id')) # user这个是表的名字，而不是类的名字。注意
    author = db.relationship('User',backref = db.backref('articles')) # User这个是类的名字，注意
                                                                      # relations：会在Article表中寻找外键‘user.id’然后关联到User表


db.create_all()

@app.route('/')
def index():
    #想要添加一篇文章，因为文章必须依赖用户而存在，所以首先的话，先添加一个用户
    #user1 = User(username = 'zhiliaoketang')
    #db.session.add(user1)
    #db.session.commit()
    
    #article1 = Article(title = 'aaa',content = 'This is an Article',author_id=1)
    #db.session.add(article1)
    #db.session.commit()

    #article = Article.query.filter(Article.title == u'aaa').first()
    #author_id = article.author_id
    #user = User.query.filter(User.id == author_id).first()
    #print("username:%s" %(user.username))
    #return 'Hello World'

    #article = Article(title = 'aaa',content = 'This is an Article')
    #article = Article(title = 'bbb',content = 'This is an Article2')
    #article.author = User.query.filter(User.id == 1).first()
    #db.session.add(article) 
    #db.session.commit()

    article = Article.query.filter(Article.content == 'This is an Article2').first()
    print("author is %s" %(article.author.username))
    user = User.query.filter(User.id == 1).first()
    print("The Articles are:")
    for arti in user.articles:
        print('--------')
        print(arti.title)
    
    return 'Hello World!'

if __name__ == '__main__':
    app.run(debug=True)
```

1. 表多对多的关系：

- 多对多的关系，需要通过一个中间表进行关联。

- 中间表，不能通过`class`的方式实现，只能通过`db.Table`的方式实现

- 设置关联：
   `tags = db.relationship('Tag',secondary = article_tag,backref = db.backref('articles'))`需要使用一个关键字参数`secondary=中间表`

- 数据的添加与访问：

  - 数据添加：

  

  ```python
  
  ```



~~~go
article1 = Article(title = 'aaa')
article2 = Article(title = 'bbb')

tag1 = Tag(name = '999')
tag2 = Tag(name = '888')

article1.tags.append(tag1) # 留意可以用append()来添加数据
article1.tags.append(tag2)
article2.tags.append(tag1)
article2.tags.append(tag2)

db.session.add(article1)
db.session.add(article2)

db.session.add(tag1)
db.session.add(tag2)

db.session.commit()
```

- 数据访问：

```python 
article1 = Article.query.filter(Article.title == 'aaa').first()
tags = article1.tags

for tag in tags:
    print('---------')
    print('Tag: %s' %(tag.name))

tag1 = Tag.query.filter(Tag.name == '999').first()
articles = tag1.articles

for article in articles:
    print('********')
    print('article: %s' %(article.title))
```
~~~

例子代码：



```python
from flask import Flask
import pymysql
import config
from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)
app.config.from_object(config)
db = SQLAlchemy(app)

article_tag = db.Table('article_tag',
              db.Column('article_id',db.Integer,db.ForeignKey('article.id'),primary_key = True),
              db.Column('tag_id',db.Integer,db.ForeignKey('tag.id'),primary_key = True),
              )

class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    title = db.Column(db.String(100),nullable = False)
    tags = db.relationship('Tag',secondary = article_tag,backref = db.backref('articles'))

class Tag(db.Model):
    __tablename__ = 'tag'
    id = db.Column(db.Integer,primary_key = True,autoincrement = True)
    name = db.Column(db.String(100),nullable = False)

db.create_all()

@app.route('/')
def index():
    ''' article1 = Article(title = 'aaa')
    article2 = Article(title = 'bbb')

    tag1 = Tag(name = '999')
    tag2 = Tag(name = '888')

    article1.tags.append(tag1)
    article1.tags.append(tag2)
    article2.tags.append(tag1)
    article2.tags.append(tag2)


    db.session.add(article1)
    db.session.add(article2)

    db.session.add(tag1)
    db.session.add(tag2)
    
    db.session.commit() '''

    article1 = Article.query.filter(Article.title == 'aaa').first()
    tags = article1.tags

    for tag in tags:
        print('---------')
        print('Tag: %s' %(tag.name))

    tag1 = Tag.query.filter(Tag.name == '999').first()
    articles = tag1.articles

    for article in articles:
        print('********')
        print('article: %s' %(article.title))

    return 'Hello World!'

if __name__ == '__main__':
    app.run(debug=True)
```

### Flask-Migrate的介绍与安装：

1. 介绍：因为采用`db.create_all`在后期修改字段的时候，不会自动的映射到数据库中，必须删除表，然后重新运行`db.create_all`才会重新映射，这样不符合实际的工作要求。因此flask-migrate就是为了解决这个问题，它可以在每次修改模型后，可以将修改的东西映射到数据库中。

2. 使用`flask_migrate`必须借助`flask_scripts`,这个包的`MigrateCommand`中包含了所有和数据库相关的命令。

3. ```
   flask_migrate
   ```

   相关的命令：

   - `python manager.py db init`:初始化一个迁移脚本的环境，只需要执行一次
   - `python manager.py db migrate`:将模型生成迁移文件，只要模型更改了，就需要执行一遍这个命令
   - `python manager.py db upgrade`:将迁移文件真正映射到数据库中，每次运行`migrate`命令后，记得要运行这个命令
   - 注意点：需要将你想要的映射到数据库中的模型，都要导入到`manager.py`文件中，如果没有导入进去，就不会映射到数据库中。



```python
from flask import Flask
from exts import db
import config
from models import Article

app = Flask(__name__)
app.config.from_object(config)
db.init_app(app)

# 新建一个article模型，采用models分开的方式
with app.app_context():  # 手动将app推到app栈顶
    db.create_all()

@app.route('/')
def hello_world():
    return 'hello_world'

if __name__ == '__main__':
    app.run(debug=True)
```

### Flask-SQLAlchemy模型的理解



```python
class User(db.Model):
    id = db.Column(db.Integer(), primary_key=True)
    username = db.Column(db.String(255))
    password = db.Column(db.String(255))

    def __init__(self, username):
        self.username = username

    def __repr__(self):
        return "<User `{}`>".format(self.username)
```

这段代码做了什么呢？实际上，我们已经得到了`User`模型，它基于一个`user`表，该表拥有3个字段。当我们继承`db.Model`时，与数据库连接和通信的工作已经自动完成了。`User`的某些类的属性值是`db.Column`类的实例，每个这样的属性都代表了数据库表里的一个字段。在`db.Column`的构造函数里，第1个参数是可选的，通过这个参数我们可以指定该属性在数据库中的字段名。如果没有指定，则SQLAlchemy会认为字段名与这个属性的名字是一样的。如果要指定这个可选参数，则可以这样写：

```
username = db.Column('user_name', db.String(255))
```

传给`db.Column`的第2个参数会告诉SQLAlchemy，应该把这个字段作为什么类型来处理。我们在书中将会用到的主要类型有:



```python
db.String
db.Text
db.Integer
db.Float
db.Boolean
db.Date
db.DateTime
db.Time`
```

每种类型的含义都很简单。`String`和`Text`类型会接收`Python`的字符串，并且把它们转为`varchar`和`text`类型的字段。`Integer`和`Float`类型则会接收`Python`的任意数值类型，把它们分别转换为对应的正确类型，再插入数据库中。`Boolean`类型会接收`Python`的布尔值转换成`Boolean`类型的字段；如果数据库不支持`Boolean`类型的字段，则`SQLAlchemy`会自动把Python的布尔值转换为0和1保存在数据库中。`Date`,`DateTime`和Time类型使用了Python的`datetime`原生包中的同名类，并把它们转换后保存到数据库中。`String`,`Integer`和`Float`类型都会接收一个额外的参数，来告诉SQLAlchemy该字段的存储长度限制。

`primary_key`参数会告诉`SQLAlchemy`，这个字段需要做主键索引。每个`SQLAlchemy`模型类型都必须有一个主键才能正常工作。

`SQLAlchemy`会假设你的表名就模型类型的小写版本。但是，如果你想给表起个别的名字，你可以给类添加`__tablename__`的类属性。另外，通过采用这种方式，你也可以使用在数据库中已经存在的表，只需把表名设为该属性的值：



```python
class User(db.Model):
    __tablename__ = 'user_table_name'

   id = db.Column(db.Integer(), primary_key=True)
    username = db.Column(db.String(255))
    password = db.Column(db.String(255))
```

我们不需要定义`__init__`或`__repr__`方法，如果我们没有定义，则SQLAlchemy会自动创建`__init__`方法。你定义的所有字段名将会成为此方法所接收的关键字的参数名。

[参考原文](https://blog.csdn.net/happyanger6/article/details/53947162)

自己当初在用`Flask`来创建用户角色时，对下面的代码的代码中的`role = Role(name = r)思考了很久。



```python
class Role(db.Model):
    __tablename__ = 'roles'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    default = db.Column(db.Boolean,default=False,index=True)
    permissions = db.Column(db.Integer)
    users = db.relationship('User', backref='role', lazy='dynamic')

    @staticmethod
    def insert_roles():
        roles = {
            'User':[Permission.FOLLOW,Permission.COMMENT,Permission.WRITE],
            'Moderator':[Permission.FOLLOW,Permission.COMMENT,Permission.WRITE,Permission.MODERATE],
            'Administrator':[Permission.FOLLOW,Permission.COMMENT,Permission.WRITE,Permission.MODERATE,Permission.ADMIN]
        }
        default_role = 'User'
        for r in roles:
            role = Role.query.filter_by(name = r).first()
            if role is None:
                role = Role(name=r)
            role.reset_permissions()
            for perm in roles[r]:
                role.add_permission(perm)
            role.default = (role.name == default_role)
            db.session.add(role)
        db.session.commit()
```

角色`Role`模型明明有好几个字段，为什么但一个关键字参数`name = r`就可以用来创建一个实例？知道在SQLAlchemy中，不需要定义`__init__`或`__repr__`方法，如果没有定义，则SQLAlchemy会自动创建`__init__`方法。定义过的所有字段名将会成为此方法所接收的关键字的参数名。

当若作为`__init__()`构造函数的参数，不是要一次性全部赋值的吗？为什么就只有'name=r'作为关键字传入就可以了？
 知道看到用户`User`模型中的构造模型。发现到了关键字参数`**kwargs`。一切都突然明白了。还是自己python基础不太熟悉。要找找源代码来看一看。



```python
class User(UserMixin, db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(64), unique=True, index=True)
    username = db.Column(db.String(64), unique=True, index=True)
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))
    password_hash = db.Column(db.String(128))
    confirmed = db.Column(db.Boolean,default = False)
    name = db.Column(db.String(64))
    location = db.Column(db.String(64))
    about_me = db.Column(db.Text())
    member_since = db.Column(db.DateTime(),default = datetime.utcnow)
    last_seen = db.Column(db.DateTime(),default = datetime.utcnow)
    avatar_hash = db.Column(db.String(32))
    posts = db.relationship('Post',backref = 'author',lazy = 'dynamic')
    followed = db.relationship('Follow',
                                foreign_keys=[Follow.follower_id],
                                backref=db.backref('follower',lazy='joined'),
                                lazy='dynamic',r.
                                cascade='all,delete-orphan')
    followers = db.relationship('Follow',
                                 foreign_keys=[Follow.followed_id],
                                 backref=db.backref('followed',lazy='joined'),
                                 lazy='dynamic',
                                 cascade = 'all,delete-orphan')


    def __init__(self,**kwargs):
        super(User,self).__init__(**kwargs)
        if self.role is None:
            if self.email == current_app.config['FLASKY_ADMIN']:
                self.role = Role.query.filter_by(name = 'Administrator').first()
            if self.role is None:
                self.role = Role.query.filter_by(default = True).first()
        if self.email is not None and self.avatar_hash is None:
            self.avatar_hash = self.gravatar_hash()
```

还有就是对为什么在用户的`confirm`函数中，只有`db.session.add(self)`
 而没有db.session.commit().



```php
    def confirm(self,token):
        s = Serializer(current_app.config['SECRET_KEY'])
        try:
            data = s.loads(token)
        except:
            return False
        if data.get('confirm')!=self.id:
            return False
        self.confirmed = True
        db.session.add(self)
        return True
```

这个这是模型，真正实现`db.session.commit()`的是在视图函数里面。



```kotlin
@auth.route('/confirm/<token>')
@login_required
def confirm(token):
    if current_user.confirmed:
        return redirect('main.index')
    if current_user.confirm(token):
        db.session.commit()
        flash('You have confirmed your account.Thanks')
    else:
        flash('The confirmation link is invalid or has expired.')
    return redirect(url_for('main.index'))
```



作者：Dozing
链接：https://www.jianshu.com/p/b729e84fae4f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。