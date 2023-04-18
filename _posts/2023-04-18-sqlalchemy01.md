---

layout: post
title: "sqlalchemy 详解 - Python操作数据库"
date: 2023-04-18
description: "数据库"

---

```
# -*- coding: utf-8 -*-
"""
Created on Wed Mar 22 14:29:19 2023

@author: h1501
"""

# =============================================================================
# # 例子1

# # 首先创建了一个引擎对象，该对象指定连接到example.db数据库。
# # 然后，它使用该引擎创建了一个会话工厂，该工厂可以用于创建会话对象。
# # 接下来，它定义了一个User类来映射users表，并使用该类的元数据创建了该表。
# # 然后，它创建了一个会话对象，并使用该对象插入了一些数据。
# # 最后，它查询了所有用户，并将其打印到控制台上。
# 
# # 注意，该脚本中的echo=True参数将打印出所有生成的SQL语句，以便您可以看到SQLAlchemy在后台执行的实际查询。
# # 如果您不想看到这些语句，请将该参数设置为False。
# 
# =============================================================================
# 版本检查
import sqlalchemy
sqlalchemy.__version__

from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 创建一个数据库连接引擎
engine = create_engine('sqlite:///example.db', echo=True)
# engine = create_engine("sqlite+pysqlite:///:memory:", echo=True)
# 创建一个会话工厂
Session = sessionmaker(bind=engine)

# 创建一个基类
Base = declarative_base()

# 创建一个User类来映射users表
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)

# 创建表
Base.metadata.create_all(engine)

# 创建一个会话对象
session = Session()

# 插入一些数据
session.add_all([
    User(name='Alice'),
    User(name='Bob'),
    User(name='Charlie')
])
session.commit()

# 查询数据
users = session.query(User).all()
for user in users:
    print(user.id, user.name)

# 关闭会话
session.close()

# =============================================================================
# # 解释session的含义以及其常用函数的功能和参数，举例子说明
# 
# # 在SQLAlchemy中，Session是一个与数据库的交互会话，它提供了一组方法来执行数据库操作，
# # 例如插入、更新和删除数据。
# # Session负责管理与数据库的连接、事务和对象状态，
# # 以确保在应用程序中执行的所有数据库操作都是原子的，可靠的和一致的。
# # 以下是Session的一些常用函数及其功能和参数：
# =============================================================================

# 1、add(obj)：将一个对象添加到会话中。
from sqlalchemy.orm import Session
from models import User
session = Session()
user = User(name="Alice")
session.add(user)
session.commit()

# 2、add_all(objs)：将多个对象添加到会话中。
users = [User(name="Alice"), User(name="Bob"), User(name="Charlie")]
session.add_all(users)
session.commit()

# 3、delete(obj)：从会话中删除一个对象。
user = session.query(User).filter_by(name="Alice").first()
session.delete(user)
session.commit()

# 4、query(model)：创建一个查询对象，可以用来执行复杂的查询操作。
users = session.query(User).filter(User.name.like('%A%')).all()

# 5、commit()：提交当前会话中的所有更改。
session.commit()

# 6、rollback()：撤销当前会话中的所有更改。
session.rollback()

# 7、flush()：将所有未提交的更改发送到数据库，但不提交事务。
session.flush()

# 8、以上是Session的一些常用函数及其功能和参数的简要说明。
# 需要注意的是，在使用Session时，通常会将会话对象作为上下文管理器使用，以确保自动提交或回滚所有更改。

with Session() as session:
    user = User(name="Alice")
    session.add(user)
    session.commit()
# 这样，在with块的末尾，会话会自动提交所有更改或回滚它们，以便在出现异常或错误的情况下保持数据的一致性。



# =============================================================================
# # SQLAlchemy是一个用于Python编程语言的开源SQL工具包，它提供了一种基于Python对象的方法来管理关系型数据库。
# # 在SQLAlchemy中，可以使用ORM（对象关系映射）来创建、更新、查询和删除数据库表格。
# # 举例来说，假设我们要创建一个博客应用，其中包括文章（Post）和评论（Comment）两个表格。
# # 可以使用SQLAlchemy来定义这两个表格及它们之间的关系：
# =============================================================================

from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, backref
from sqlalchemy.ext.declarative import declarative_base

engine = create_engine('sqlite:///blog.db', echo=True)
Base = declarative_base()

class Post(Base):
    __tablename__ = 'posts'

    id = Column(Integer, primary_key=True)
    title = Column(String)
    content = Column(String)

    comments = relationship("Comment", backref=backref('post', uselist=False))

class Comment(Base):
    __tablename__ = 'comments'

    id = Column(Integer, primary_key=True)
    content = Column(String)
    post_id = Column(Integer, ForeignKey('posts.id'))

    post = relationship(Post, backref=backref('comments', uselist=True))

Base.metadata.create_all(engine)

# 在上述代码中，我们首先创建了一个SQLAlchemy引擎，并使用declarative_base()函数创建了一个基类（Base）。
# 然后，我们定义了两个表格类：Post和Comment，它们都继承自基类。
# 在这些类中，我们使用Column定义了表格中的列，并使用relationship定义了表格之间的关系。
# 例如，Post类中的comments属性是一个relationship，它表示一个文章可以有多个评论，而每个评论都对应于一个文章。
# 我们使用backref参数来定义一个反向引用，这样我们可以从评论中访问到对应的文章。
# 最后，我们使用Base.metadata.create_all(engine)来创建这两个表格。
# 这个函数会在数据库中自动创建两个表格，并确保它们之间的关系被正确地建立。

# =============================================================================
# ###############################################################################
# # 动态生成表格的例子
# # 假设我们有一个应用程序，需要存储用户数据和他们发布的文章数据。
# # 我们可以使用SQLAlchemy来管理这些数据，使用两个不同的表格：一个用户表格和一个文章表格。
# # 这里提供一个示例代码：
# =============================================================================

# 需要导入SQLAlchemy库，并创建一个数据库引擎和一个会话。在本例中，我们将使用SQLite作为我们的数据库引擎。
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine('sqlite:///mydatabase.db')
Session = sessionmaker(bind=engine)
session = Session()

# 创建一个用户表格，其中包含用户名和电子邮件地址。我们将使用SQLAlchemy的ORM功能来定义模型类，以便可以轻松地将Python对象映射到数据库表格。
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    username = Column(String)
    email = Column(String)

# 创建一个文章表格，其中包含文章标题、内容和作者。我们还将使用外键将文章表格链接到用户表格中的作者
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class Article(Base):
    __tablename__ = 'articles'

    id = Column(Integer, primary_key=True)
    title = Column(String)
    content = Column(String)
    author_id = Column(Integer, ForeignKey('users.id'))
    author = relationship('User', backref='articles')
# 现在我们已经定义了用户表格和文章表格，可以使用下面的代码创建表格
Base.metadata.create_all(engine)

# 创建用户
user1 = User(username='Alice', email='alice@example.com')
user2 = User(username='Bob', email='bob@example.com')
session.add_all([user1, user2])
session.commit()

# 创建文章
article1 = Article(title='First article', content='This is my first article', author=user1)
article2 = Article(title='Second article', content='This is my second article', author=user2)
session.add_all([article1, article2])
session.commit()


# 获取所有用户
users = session.query(User).all()
for user in users:
    print(user.username)

# 获取所有文章
articles = session.query(Article).all()
for article in articles:
    print(article.title)

# 修改文章
article2.title = 'Updated title'
session.commit()

# 删除文章
session.delete(article1)
session.commit()


# =============================================================================
# # relationship()的功能、参数
# # relationship() 是 SQLAlchemy 中一个非常重要的函数，可以定义不同表之间的关系，包括一对一、一对多和多对多关系。
# =============================================================================
# 功能
# - relationship() 用于在 SQLAlchemy 中定义关系，它可以告诉 SQLAlchemy 如何在两个表之间建立关联。一般来说，这个函数是在定义 SQLAlchemy 中的模型类时使用的。通过使用 relationship()，我们可以让 SQLAlchemy 了解到不同表之间的关系，使我们能够轻松地从一个表中访问另一个表的数据。

# 参数
# relationship() 函数有多个参数，下面是一些常用的参数：
# - secondary: 对于多对多关系，这个参数用于指定中间表的名称。
# - backref: 反向引用，可以让 SQLAlchemy 自动为关系中的另一个表生成一个引用属性。
# - lazy: 定义关系如何进行加载。通常有两个选项：select 和 joined，默认为 select。
# - uselist: 定义关系是否是一对多还是一对一的关系。
# - primaryjoin 和 secondaryjoin: 用于指定关系的连接条件。

from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, backref
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    addresses = relationship("Address", backref="user")

class Address(Base):
    __tablename__ = 'addresses'

    id = Column(Integer, primary_key=True)
    email_address = Column(String, nullable=False)
    user_id = Column(Integer, ForeignKey('users.id'))

engine = create_engine('sqlite:///example.db')
Base.metadata.create_all(engine)

# 在上面的代码中，我们定义了两个表，users 和 addresses，并通过 relationship() 定义了这两个表之间的关系。
# 在 User 模型中，我们定义了一个 addresses 属性，用于关联到 Address 模型。
# 这个属性的类型是 relationship()，我们将它设置为 "Address"，表示它关联到 Address 模型。
# 在 Address 模型中，我们定义了一个 user_id 属性，它关联到 User 模型中的 id 属性。
# 使用 backref 参数，我们还定义了一个反向引用，将 Address 模型中的 user 属性关联到 User 模型中的 addresses 属性。
# 这样，我们就可以在一个模型中轻松地访问另一个模型的数据了。

# =============================================================================
# SQLAlchemy 如何连接数据库
# SQLAlchemy 提供了一种灵活的方式来连接各种类型的数据库，例如 MySQL、PostgreSQL、Oracle、Microsoft SQL Server 等等。
# 下面是一个基本的连接示例
# =============================================================================
from sqlalchemy import create_engine

# 创建连接引擎
engine = create_engine('database_type://user:password@host:port/database_name')

# 创建连接对象
conn = engine.connect()

# 执行 SQL 查询
result = conn.execute('SELECT * FROM mytable')

# 打印结果
for row in result:
    print(row)

# 关闭连接
conn.close()

# create_engine() 函数用于创建一个连接引擎，它接受一个字符串参数，用于指定数据库的连接信息，包括数据库类型、用户名、密码、主机名、端口号和数据库名称。
# 这个字符串的格式取决于你使用的数据库类型和驱动程序。
# 例如，如果你要连接到名为 mydatabase 的 MySQL 数据库，用户名为 myuser，密码为 mypassword，主机名为 localhost，端口号为 3306，则可以这样创建连接引擎
engine = create_engine('mysql://myuser:mypassword@localhost:3306/mydatabase')

# =============================================================================
# # SQLAlchemy 连接数据库 - 使用连接池来管理连接、处理异常等等
# =============================================================================
# 使用 SQLAlchemy 连接池可以处理以下几种情况：

# 高并发场景：在高并发场景下，连接池可以提高数据库的并发性能，减少等待时间和死锁的风险。
# 例如，在 Web 应用程序中，每个请求都需要连接数据库来获取数据。如果每个请求都创建一个新
# 的数据库连接，这将导致数据库连接的开销非常大，而且可能会超出数据库连接的最大限制。使用
# 连接池可以重用现有连接，避免了频繁创建和销毁连接的开销，提高了数据库的性能。

# 处理异常：连接池还可以处理数据库连接中的异常。例如，在使用 SQLAlchemy 连接数据库时，
# 可能会出现连接断开、超时、错误等异常。如果没有连接池，这些异常将会导致应用程序崩溃。
# 使用连接池可以通过重新尝试连接来处理这些异常，避免应用程序的崩溃。

# 节省资源：连接池还可以节省数据库资源。例如，在使用 SQLAlchemy 连接数据库时，每个连接
# 都需要消耗一定的内存和 CPU 资源。如果没有连接池，应用程序将会消耗大量的资源来创建和销毁连接。
# 使用连接池可以重用现有连接，减少了资源的消耗。

from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

# 创建连接引擎
engine = create_engine('database_type://user:password@host:port/database_name',
                       poolclass=QueuePool,
                       pool_size=10,
                       max_overflow=20)

# 创建连接对象
with engine.connect() as conn:
    try:
        # 执行 SQL 查询
        result = conn.execute('SELECT * FROM mytable')

        # 打印结果
        for row in result:
            print(row)

    except Exception as e:
        # 处理异常
        print(f"Error: {e}")

# 上述代码，使用 QueuePool 类作为连接池的实现。
# pool_size 参数设置连接池的初始大小，max_overflow 参数设置连接池可以增长的最大数量。
# 这意味着当连接池中的所有连接都被占用时，最多可以创建 max_overflow 个额外的连接。
# 请注意，这些选项的最佳值取决于你的应用程序的特定需求和资源限制。
# 在使用连接对象时，我们使用了 with 语句块，这样可以确保在完成操作后关闭连接。
# 我们还使用了 try/except 语句块来处理可能发生的异常。

# =============================================================================
# SQLAlchemy 连接数据库 - 连接泄露、连接超时、连接错误、连接池大小  举例说明解决方法
# =============================================================================

# 1、连接泄漏：使用 ConnectionProxy 对象来确保连接在使用后被正确地释放，或者使用 ScopedSession 来自动管理连接的生命周期。例如：
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine('mysql://user:password@host/database', pool_size=5, max_overflow=10)
Session = sessionmaker(bind=engine)

class ConnectionProxy(object):
    def __init__(self, conn):
        self._conn = conn

    def __getattr__(self, name):
        return getattr(self._conn, name)

    def close(self):
        Session.remove()

def get_connection():
    session = Session()
    conn = session.connection()
    return ConnectionProxy(conn)

conn = get_connection()
# Use the connection
conn.execute("SELECT * FROM my_table")
conn.close()

# 2、连接超时：使用 pool_pre_ping 参数来定期测试连接的可用性，并确保连接池中只包含可用的连接。例如：
engine = create_engine('mysql://user:password@host/database', pool_size=5, max_overflow=10, pool_pre_ping=True)

# 3、连接错误：使用 retry 或 fallback 策略来重试连接，或者使用 fallback_on_failure 参数来在连接失败时自动切换到备用数据库
engine = create_engine('mysql://user:password@host/database', pool_size=5, max_overflow=10, connect_args={'connect_timeout': 10})

# Retry up to 5 times if the connection fails
from sqlalchemy.exc import OperationalError
from sqlalchemy import event

@event.listens_for(engine, 'engine_connect')
def retry_connect(dbapi_conn, connection_record):
    tries = 0
    max_tries = 5
    while True:
        try:
            dbapi_conn.ping(reconnect=True)
            break
        except OperationalError as ex:
            tries += 1
            if tries > max_tries:
                raise ex

# Use a fallback database if the primary database fails
fallback_engine = create_engine('mysql://user:password@backup_host/backup_database')
engine = create_engine('mysql://user:password@host/database', 
                       pool_size=5, 
                       max_overflow=10, 
                       pool_recycle=3600, 
                       connect_args={'connect_timeout': 10,'fallback_on_failure': fallback_engine})

# 4、连接池大小：根据应用程序的负载和数据库的规模来调整连接池的大小。
engine = create_engine('mysql://user:password@host/database', 
                       pool_size=20, 
                       max_overflow=30)

# =============================================================================
# # SQLAlchemy动态生成表格时，如何避免对已有表格冲突，且能够使用已有表格。有哪几种方法，举例说明。
# =============================================================================
# 1、使用表格名称前缀或后缀：为了避免与已有的表格名称冲突，可以为新创建的表格名称添加前缀或后缀。例如，如果已有一个名为“users”的表格，可以为新表格添加前缀“new_”或后缀“_v2”，创建名为“new_users”或“users_v2”的新表格。
# 2、检查表格是否已存在：在创建新表格之前，可以使用SQLAlchemy提供的API检查表格是否已存在。如果表格已存在，则可以选择使用已有表格，而不是创建一个新的表格。例如，可以使用以下代码检查名为“users”的表格是否存在
from sqlalchemy import create_engine, MetaData, Table

engine = create_engine('sqlite:///example.db')
metadata = MetaData(bind=engine)

if not engine.dialect.has_table(engine, 'users'):
    Table('users', metadata, ...)
else:
    Table('users', metadata, autoload=True)

# 在这个例子中，如果名为“users”的表格不存在，则创建一个新表格；否则，使用已有的表格。autoload=True参数将自动从数据库中加载已有的表格结构。
# 3、明确指定表格结构：在创建新表格时，可以明确指定表格结构，例如列名、列类型、索引等。如果已有表格结构与新表格结构相同，则可以使用已有的表格。例如，可以使用以下代码指定名为“users”的表格结构：
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

engine = create_engine('sqlite:///example.db')
metadata = MetaData(bind=engine)

users_table = Table('users', metadata,
                    Column('id', Integer, primary_key=True),
                    Column('name', String),
                    Column('email', String),
                    ...)

if not engine.dialect.has_table(engine, 'users'):
    users_table.create()
else:
    existing_users_table = Table('users', metadata, autoload=True)
    if existing_users_table.columns == users_table.columns:
        users_table = existing_users_table
    else:
        raise ValueError('Table structure mismatch')

# 在这个例子中，如果名为“users”的表格不存在，则创建一个新表格；否则，检查已有表格结构是否与指定的结构相同，如果相同，则使用已有表格；否则，抛出ValueError异常。
# 通过上述方法，可以在动态生成表格时避免对已有表格冲突，同时能够使用已有表格。



```