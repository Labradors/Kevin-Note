# Django 学习笔记一
1. Django建议，将你的代码放倒用户个人目录下，而不是/var/www下，为了网站的安全考虑
2. 新建django项目，目录下的各个文件的意义：
3. manager.py:一个命令行工具，用于命令行控制Django
4. setting.py:项目的配置文件。
5. urls.py:项目中的url配置。
6. wsgi.py：web服务器入口。
7. python manage.py migrate:
syncdb command is deprecated in django 1.7. Use the python manage.py migrate instead.
8. 重新添加模块后需要重新构建，`python manage.py makemigrations`
9. 然后再刷新数据`python manage.py migrate `
10. 
