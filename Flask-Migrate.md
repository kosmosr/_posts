---
title: Flask-Migrate 实现数据库更新
date: 2018-04-11 10:09:50
tags:
- flask
---
## Flask-Migrate 实现数据库更新

- 当我们在flask中更新了模型时需要及时更新到数据库中，除了手动添加更改，还可以使用flask-migrate来帮助我们实现。

- flask-migrate能跟踪数据库模式的变化，然后增量式的把变化应用到数据库中


1. 在虚拟环境中安装flask-migrate:
	`pipenv install flask-migrate`
	
2. 在项目中的启动类上初始化flask-migrate
```pyton
from flask_script import Manager, Shell
from flask_migrate import Migrate, MigrateCommand
...
manager = Manager(app)
migrate = Migrate(app, db)
manager.add_command("db", MigrateCommand)
```

第一次使用时，使用init命令创建迁移仓库:
`python <启动类>.py db init`

当我们更改了模型时，可以使用migrate自动创建迁移脚本
`python <启动类>.py db migrate -m "记录"`

使用db upgrade命令应用到数据库中或者db downgrade将改动删除
`python <启动类>.py db upgrade`