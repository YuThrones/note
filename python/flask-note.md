###Install virtualenv and use
	$ sudo apt-get install python-virtualenv
###User
	$ git clone https://github.com/miguelgrinberg/flasky.git
	$ cd flasky
	$ git checkout 1a
	$ virtualenv venv
	$ source venv/bin/activate

###Install flask
	$ pip install flask

###hello.py
	from flask import Flask   
	app = Flask(__name__)
	@app.route('/')  
	def index():  
	    return '<h1>Hello World!</h1>'
	@app.route('/user/<name>')
	def user(name):
	    return '<h1>Hello, %s!</h1>' % name
	if __name__ == '__main__':
 	   app.run(debug=True)

#request
	from flask import request
	@app.route('/')
	def index():
    	user_agent = request.headers.get('User-Agent')
    	return '<p>Your browser is %s</p>' % user_agent

###
请求钩子使用修饰器实现。Flask 支持以下4 种钩子。 
 
* before\_first\_request：注册一个函数，在处理第一个请求之前运行。
* before\_request：注册一个函数，在每次请求之前运行。
* after\_request：注册一个函数，如果没有未处理的异常抛出，在每次请求之后运行。
* teardown\_request：注册一个函数，即使有未处理的异常抛出，也在每次请求之后运行。

###response
	from flask import make_response
	@app.route('/')
	def index():
 	   response = make_response('<h1>This document carries a cookie!</h1>')
 	   response.set_cookie('answer', '42')
 	   return response

###flask拓展
`(venv) $ pip install flask-script`

	from flask.ext.script import Manager
	manager = Manager(app)
	# ...
	if __name__ == '__main__':
    	manager.run()  

`python hello.py runserver`

###渲染模板
	from flask import Flask, render_template
	# ...
	@app.route('/')
	def index():
	    return render_template('index.html')
	@app.route('/user/<name>')
	def user(name):
	    return render_template('user.html', name=name）

###过滤器
Hello, {{ name|capitalize }}

###模板控制结构
#控制
	{% if user %}
	    Hello, {{ user }}!
	{% else %}
	    Hello, Stranger!
	{% endif %}
#一组元素
	<ul>
	    {% for comment in comments %}
	        <li>{{ comment }}</li>
	    {% endfor %}
	</ul>
#宏（类似函数）
	{% macro render_comment(comment) %}
	    <li>{{ comment }}</li>
	{% endmacro %}
	<ul>
	    {% for comment in comments %}
	        {{ render_comment(comment) }}
	    {% endfor %}
	</ul>
#在模板中导入文件中的宏
	{% import 'macros.html' as macros %}
	<ul>
	    {% for comment in comments %}
	        {{ macros.render_comment(comment) }}
	    {% endfor %}
	</ul>

###模板继承
#base.html
	<html>
	<head>
	    {% block head %}
	    <title>{% block title %}{% endblock %} - My Application</title>
	    {% endblock %}
	</head>
	<body>
	    {% block body %}
	    {% endblock %}
	</body>
	</html>
#衍生模板
	{% extends "base.html" %}
	{% block title %}Index{% endblock %}
	{% block head %}
	    {{ super() }}
	    <style>
	    </style>
	{% endblock %}
	{% block body %}
	<h1>Hello, World!</h1>
	{% endblock %}

###Flask-Bootstrap
	(venv) $ pip install flask-bootstrap
###初始化
	from flask.ext.bootstrap import Bootstrap
	# ...
	bootstrap = Bootstrap(app)
###templates/user.html：使用Flask-Bootstrap 的模板
	{% extends "bootstrap/base.html" %}
	{% block title %}Flasky{% endblock %}
	{% block navbar %}
	<div class="navbar navbar-inverse" role="navigation">
	    <div class="container">
		<div class="navbar-header">
	    		<button type="button" class="navbar-toggle"
	  		data-toggle="collapse" data-target=".navbar-collapse">
				<span class="sr-only">Toggle navigation</span>
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
			</button>
			<a class="navbar-brand" href="/">Flasky</a>
		</div>
		<div class="navbar-collapse collapse">
	    		<ul class="nav navbar-nav">
				<li><a href="/">Home</a></li>
	    		</ul>
		</div>
	    </div>
	</div>
	{% endblock %}
	{% block content %}
	<div class="container">
	    <div class="page-header">
		<h1>Hello, {{ name }}!</h1>
	    </div>
	</div>
	{% endblock %}

###自定义错误页面
	@app.errorhandler(404)
	def page_not_found(e):
    	return render_template('404.html'), 404

###链接
使用url\_for() 生成动态地址时， 将动态部分作为关键字参数传入。例如，url\_for
('user', name='john', \_external=True) 的返回结果是http://localhost:5000/user/john。  
传入url\_for() 的关键字参数不仅限于动态路由中的参数。函数能将任何额外参数添加到
查询字符串中。例如，url\_for('index', page=2) 的返回结果是/?page=2。

###静态文件
调用url_for('static', filename='css/styles.css', _external=True) 得到的结果是http://
localhost:5000/static/css/styles.css。
默认设置下，Flask 在程序根目录中名为static 的子目录中寻找静态文件。如果需要，可在
static 文件夹中使用子文件夹存放文件。服务器收到前面那个URL 后，会生成一个响应，
包含文件系统中static/css/styles.css 文件的内容。

###templates/base.html：定义收藏夹图标
	{% block head %}
	{{ super() }}
	<link rel="shortcut icon" href="{{ url_for('static', filename = 'favicon.ico') }}"
    type="image/x-icon">
	<link rel="icon" href="{{ url_for('static', filename = 'favicon.ico') }}"
    type="image/x-icon">
	{% endblock %}

###使用Flask-Moment本地化日期和时间
	(venv) $ pip install flask-moment
###初始化Flask-Moment
	from flask.ext.moment import Moment
	moment = Moment(app)

###templates/base.html：引入moment.js 库
	{% block scripts %}
	{{ super() }}
	{{ moment.include_moment() }}
	{% endblock %}

###hello.py：加入一个datetime 变量
	from datetime import datetime
	@app.route('/')
	def index():
	    return render_template('index.html',
	    current_time=datetime.utcnow())

###templates/index.html：使用Flask-Moment 渲染时间戳
	<p>The local date and time is {{ moment(current_time).format('LLL') }}.</p>
	<p>That was {{ moment(current_time).fromNow(refresh=True) }}</p>

##4.Web表单

###Flask-WTF安装
	(venv) $ pip install flask-wtf
###设置秘钥  
hello.py：设置Flask-WTF  

	app = Flask(__name__)
	app.config['SECRET_KEY'] = 'hard to guess string'

###hello.py：定义表单类
	from flask.ext.wtf import Form
	from wtforms import StringField, SubmitField
	from wtforms.validators import Required
	class NameForm(Form):
		name = StringField('What is your name?', validators=[Required()])
		submit = SubmitField('Submit')

###hello.py：路由方法
	@app.route('/', methods=['GET', 'POST'])
	def index():
		name = None
		form = NameForm()
		if form.validate_on_submit():
			name = form.name.data
			form.name.data = ''
		return render_template('index.html', form=form, name=name)
###
	@app.route('/', methods=['GET', 'POST'])
	def index():
		form = NameForm()
		if form.validate_on_submit():
			session['name'] = form.name.data
			return redirect(url_for('index'))
		return render_template('index.html', form=form, name=session.get('name'))

###hello.py：Flash 消息
	from flask import Flask, render_template, session, redirect, url_for, flash
	@app.route('/', methods=['GET', 'POST'])
	def index():
		form = NameForm()
		if form.validate_on_submit():
			old_name = session.get('name')
			if old_name is not None and old_name != form.name.data:
				flash('Looks like you have changed your name!')
			session['name'] = form.name.data
			return redirect(url_for('index'))
		return render_template('index.html',
			form = form, name = session.get('name'))

###，Flask-SQLAlchemy 也使用pip 安装：
`(venv) $ pip install flask-sqlalchemy`  
mysql的url:  **mysql://username:password@hostname/databas**

###hello.py：配置数据库
	from flask.ext.sqlalchemy import SQLAlchemy
	basedir = os.path.abspath(os.path.dirname(__file__))
	app = Flask(__name__)
	app.config['SQLALCHEMY_DATABASE_URI'] =\
		'sqlite:///' + os.path.join(basedir, 'data.sqlite')
	app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
	db = SQLAlchemy(app)

###　hello.py：定义Role 和User 模型
	class Role(db.Model):
		__tablename__ = 'roles'
		id = db.Column(db.Integer, primary_key=True)
		name = db.Column(db.String(64), unique=True)
		def __repr__(self):
			return '<Role %r>' % self.name
	class User(db.Model):
		__tablename__ = 'users'
		id = db.Column(db.Integer, primary_key=True)
		username = db.Column(db.String(64), unique=True, index=True)
		def __repr__(self):
			return '<User %r>' % self.username

###hello.py：关系
	class Role(db.Model):
		# ...
		users = db.relationship('User', backref='role')
	class User(db.Model):
		# ...
		role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

###插入行
	>>> from hello import Role, User
	>>> admin_role = Role(name='Admin')
	>>> user_john = User(username='john', role=admin_role)
	>>> db.session.add(admin_role)     ###提交全部>>> db.session.add_all([admin_role, mod_role, user_role,user_john, user_susan, user_david])
	>>> db.session.commit()

###删除行
	>>> db.session.delete(mod_role)
	>>> db.session.commit()

###查询行
	>>> Role.query.all()
	>>> User.query.filter_by(role=user_role).all()
	#转化为原生SQL语句
	>>> str(User.query.filter_by(role=user_role))

###重新加载python数据库对象
	>>> user_role = Role.query.filter_by(name='User').first()

###我们修改了关系的设置，加入了lazy = 'dynamic' 参数，从而禁止自动执行查询。
	class Role(db.Model):
		# ...
		users = db.relationship('User', backref='role', lazy='dynamic')

###　hello.py：为shell 命令添加一个上下文
	from flask.ext.script import Shell
	def make_shell_context():
		return dict(app=app, db=db, User=User, Role=Role)
	manager.add_command("shell", Shell(make_context=make_shell_context))
####make\_shell\_context() 函数注册了程序、数据库实例以及模型，因此这些对象能直接导入shell

###虚拟环境中安装Flask-Migrate：
	(venv) $ pip install flask-migrate

###hello.py：配置Flask-Migrate
	from flask.ext.migrate import Migrate, MigrateCommand
	# ...
	migrate = Migrate(app, db)
	manager.add_command('db', MigrateCommand)

#在维护数据库迁移之前，要使用init 子命令创建迁移仓库：
	(venv) $ python hello.py db init

#migrate 子命令用来自动创建迁移脚本：
	(venv) $ python hello.py db migrate -m "initial migration"

###使用pip 安装Flask-Mail：
	(venv) $ pip install flask-mail

###　hello.py：配置Flask-Mail 使用Gmail
	import os
	# ...
	app.config['MAIL_SERVER'] = 'smtp.googlemail.com'
	app.config['MAIL_PORT'] = 587
	app.config['MAIL_USE_TLS'] = True
	app.config['MAIL_USERNAME'] = os.environ.get('MAIL_USERNAME')
	app.config['MAIL_PASSWORD'] = os.environ.get('MAIL_PASSWORD')

###hello.py：初始化Flask-Mail
	from flask.ext.mail import Mail
	mail = Mail(app)

###hello.py：异步发送电子邮件
	from threading import Thread
	def send_async_email(app, msg):
		with app.app_context():
			mail.send(msg)
	def send_email(to, subject, template, **kwargs):
		msg = Message(app.config['FLASKY_MAIL_SUBJECT_PREFIX'] + subject,
			sender=app.config['FLASKY_MAIL_SENDER'], recipients=[to])
		msg.body = render_template(template + '.txt', **kwargs)
		msg.html = render_template(template + '.html', **kwargs)
		thr = Thread(target=send_async_email, args=[app, msg])
		thr.start()
		return thr

###工厂函数创建程序实例
	bootstrap = Bootstrap()
	mail = Mail()
	moment = Moment()
	db = SQLAlchemy()
	def create_app(config_name):
		app = Flask(__name__)
		app.config.from_object(config[config_name])
		config[config_name].init_app(app)
		bootstrap.init_app(app)
		mail.init_app(app)
		moment.init_app(app)
		db.init_app(app)
		return app

###　app/main/__init__.py：创建蓝本
	from flask import Blueprint
	main = Blueprint('main', __name__)
	from . import views, errors

###app/_init_.py：注册蓝本
	def create_app(config_name):
		# ...
		from .main import main as main_blueprint
		app.register_blueprint(main_blueprint)
		return app

###app/main/errors.py：蓝本中的错误处理程序
	from flask import render_template
	from . import main
	@main.app_errorhandler(404)
	def page_not_found(e):
		return render_template('404.html'), 404	
	@main.app_errorhandler(500)
	def internal_server_error(e):
		return render_template('500.html'), 500
在蓝本中编写错误处理程序稍有不同，如果使用errorhandler 修饰器，那么只有蓝本中的
错误才能触发处理程序。要想注册程序全局的错误处理程序，必须使用app_errorhandler。

###
在蓝本中编写视图函数主要有两点不同：第一，和前面的错误处理程序一样，路由修饰器
由蓝本提供；第二，url\_for() 函数的用法不同。你可能还记得，url\_for() 函数的第一
个参数是路由的端点名，在程序的路由中，默认为视图函数的名字。例如，在单脚本程序
中，index() 视图函数的URL 可使用url\_for('index') 获取。

###
程序中必须包含一个requirements.txt 文件，用于记录所有依赖包及其精确的版本号。如果
要在另一台电脑上重新生成虚拟环境，这个文件的重要性就体现出来了，例如部署程序时
使用的电脑。pip 可以使用如下命令自动生成这个文件：
	(venv) $ pip freeze >requirements.txt

###
如果你要创建这个虚拟环境的完全副本，可以创建一个新的虚拟环境，并在其上运行以下
命令：  

	(venv) $ pip install -r requirements.txt

###manage.py：启动单元测试的命令
	@manager.command
	def test():
		"""Run the unit tests."""
		import unittest
		tests = unittest.TestLoader().discover('tests')
		unittest.TextTestRunner(verbosity=2).run(tests)
`(venv) $ python manage.py test`

###
不管从哪里获取数据库URL，都要在新数据库中创建数据表。如果使用Flask-Migrate 跟
踪迁移，可使用如下命令创建数据表或者升级到最新修订版本：

	(venv) $ python manage.py db upgrade

* generate\_password\_hash(password, method=pbkdf2:sha1, salt\_length=8)：这个函数将原始密码作为输入，以字符串形式输出密码的散列值，输出的值可保存在用户数据库中。
method 和salt\_length 的默认值就能满足大多数需求。
* check\_password\_hash(hash, password)：这个函数的参数是从数据库中取回的密码散列
值和用户输入的密码。返回值为True 表明密码正确。

###生成令牌
	from manage import app
	>>> from itsdangerous import TimedJSONWebSignatureSerializer as Serializer
	用户认证 ｜ 91
	>>> s = Serializer(app.config['SECRET_KEY'], expires_in = 3600)
	>>> token = s.dumps({ 'confirm': 23 })
	>>> token
	'eyJhbGciOiJIUzI1NiIsImV4cCI6MTM4MTcxODU1OCwiaWF0IjoxMzgxNzE0OTU4fQ.ey ...'
	>>> data = s.loads(token)
	>>> data
	{u'confirm': 23}

###
	@auth.before_app_request
	def before_request():
		if current_user.is_authenticated() \
			and not current_user.confirmed \
			and request.endpoint[:5] != 'auth.':
			and request.endpoint != 'static':
		return redirect(url_for('auth.unconfirmed'))

###有多个Python 包可用于生成虚拟信息，其中功能相对完善的是ForgeryPy，可以使用pip 进行安装：
	(venv) $ pip install forgerypy

