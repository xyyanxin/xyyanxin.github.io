---
layout: post
title: chrome调试
description: Chrome浏览器不仅可以调试页面、JS、请求、资源、cookie，还可以模拟手机进行调试。自从使用了Chrome，我就离不开它了。
category: blog
jd_id: 242891127
---
1 flask_sqlalchemy和sqlalchemy区别
    1   It aims to simplify using SQLAlchemy with Flask by providing useful defaults and extra helpers that make it easier to accomplish common tasks. (官网原文)
    2 看源码,只不过简单封装了一下

2 configs.py
    生成了db/lm(?)/attments对象
    默认设置.测试环境设置.生产环境设置

3 application.py
    create_app():1 生成app对象
                2 app对象设置 先加载默认设置,再加载传进来的参数,最后加载环境变量设置;
                3 设置db对象,db.init_app(app)
                4 login设置.@lm.user_loader?
                            @lm.unauthorized_handler?
                5 注册蓝图:app.register_blueprint(blue, url_prefix=url_prefix)
                    blue为instance对象
                    如果为开发环境,注册mod_test.instance对象

4 helpers.py
    1   checkACL装饰器:
            检查modFuncName
            找出来之后查mod_id
            检查公司有无购买这个模块
            列出这个人的所有角色
            检查角色中有allow的,只要有一个,就通过
            ?怎么用?
    2   attachStorage:
            拿到list:uploaded_files = req.files.getlist(form_name)
            判断是否allowed
            存储filename = attachments.save(item)
            拿路径filepath = os.path.join(current_app.root_path,
                                attachments.path(filename))
            存到model中
            db.session.flush()??::::session是有一级缓存的,目的是为了减少查询数据库的时间,一级缓存的的
                周期和session是一样的.session.flush()和session.clear()是针对session的一级缓存的处理.
                session.flush作用是将session的缓存中的数据与数据库同步.
                session.clear作用是清除session中的缓存数据
                一般用在保存大量数据的时候,可以每固定数量的时候,flush,再clear,因为把大量的session对象缓存中会浪费大量的内容空间

            如何生成session?::db对象的一个属性.

    3   op_log():
        每个视图函数都要调用一次op_log函数



5 sysModel.py
    ModRegisterModel 模块(name,desc)
    ModFuncRegisterModel 模块的方法(mod_id,function_name,function_desc)
    CompanyModModel 公司(mod_id,company_id,status)
    缺一个角色对应的func的model.
    AttachmentModel 附件(company_id,user_id,ext,filename,filepath)
    OpLogModel 操作日志(company_id,user_id,op_mod,op_interface,related_id,opdesc,extra)
        其中:op_mod为(project,fund)
        如何区别增删改查在哪里体现.??



6 注意拿用户id:
    from flask_login import current_user,直接调用current_user.id

7 sysview.py:
    self.is_active???
    self.is_authenticated??
    from flask_login import login_required, login_user, logout_user

8 cookie后端是怎么带过去的?
    没有用set cookie
    可能是flask_login模块

9 下载有道chrome

10  sql语句导出
    mysqldump -u yanxin -p even(数据库名)>even.sql(文件名)
    sql语句导入
    source even.sql

11  如何安装deb安装包
    dpkg -i *.deb
    如果确实依赖
        apt-get -f -y install

12  源码
    ~/.virtualenv/even/lib/python3.4/site-packagets

13 基金模块开始

    负责人(user_id)

    缺募资信息 : 人--投资金额 ---状态


14 decimal
    首先，对于精度比较高的东西，比如money，我会用decimal类型，不会考虑float,double,因为他们容易产生误差，

    DECIMAL列的声明语法是DECIMAL(M,D)。在MySQL 5.1中，参量的取值范围如下：

    ·         M是数字的最大数（精度）。其范围为1～65（在较旧的MySQL版本中，允许的范围是1～254）。

    ·         D是小数点右侧数字的数目（标度）。其范围是0～30，但不得超过M。

    说明：float占4个字节，double占8个字节，decimail(M,D)占M+2个字节。

15 查看系统版本
    lsb_release -a
    uname -a

16  set格式取并集:
    set(a)|set(b)
    或者 set(a).add()

17 在test中print(resp)

18 #TODO currentuser.company_id
        current_user
        from flask_login import current_user

19  #REMEMBER 返回的时候多返回一些数据,例如完一个基金,也返回它的id

20  #DONE project_name 和 fund_name 应该为唯一   unique=True

21  #TODO 查询该机构所有基金

22  decimal单位如何存?
        from decimal import Decimal

23  #TODO  判断输入参数的更好解决方式,除了set之外

24 find . -name 'youdao*'

25  age-get remove
    pip uninstall

26 查看电脑已安装的所有程序  usr/share/application
    打开文件夹，点击计算机
    打开usr 打开share 打开applications
    所有的软件图标都在这里（自带和自己安装的linux软件）
    然后把鼠标放在在屏幕左上角点击书签 添加为书签，下次可以从文件夹直接打开。
    可以右键图标复制到桌面，就可以在桌面创建快捷方式了


27 Unimplemented method 'GET'
    未实现的get方法

28 关于set更多内容联系
    http://blog.csdn.net/business122/article/details/7541486

29 Decimal('1000000.00000000') is not JSON serializable
    Decimal 格式不允许序列化
        to: 转为字符串:str方法

30  如何比较列表和set格式
    答案:无法比较

31 阅读:
    但是Python 将坐稳数据分析和 AI 第一语言的位置;
    Python正在迅速成为全球大中小学编程入门课程的首选教学语言;
    由于全局解释器锁（GIL）的限制，单个Python 程序无法在多核上并发执行；
    网络攻防的第一黑客语言，正在成为编程入门教学的第一语言，云计算系统管理第一语言。
    Python 也早就成为Web 开发、游戏脚本、计算机视觉、物联网管理和机器人开发的主流语言之一


======================================2.6==================================

1   Entity '<class 'basesite.models.modLPModel.LPModel'>' has no property 'fund_id'
    Entity:条目

2    连表查询的时候,filter/filter_by 需要制定哪个个表的哪个字段
        filter_by的参数不用加表名
        filter的参数要加表名

3   filter_by(fund_id=1)
        第一个参数不能为FundLPModel.fund_id,否则报错

        filter则可以;

4   连表后不到一定要非要用with_entities.返回的对象继续可以用todict()[存疑]

5   pip install -r requrements.txt

6   flask_sqlalchemy 只是sqlalchemy 的一个简单封装,最终还是要查询sqlalchemy
        所以先玩熟sqlalchemy

7   聚合查询:
        qur = db.session.query(func.sum(FundLPModel.amount))\
                .filter_by(fund_id=int(data['fund_id']))\
                .filter_by(status='已打款')\
                .one()
            ret = {'QueryFunderRaisingProgress':str(qur[0])}

8   unbuntu命令上删除到.号处
        未找到

9   http://gs.amac.org.cn/amac-infodisc/res/pof/manager/managerList.html?nsukey=yXa8y1I%2Bccjh589%2FNMzLrsKZjoT4tdriVSmaz3xcpR3jXBIar2hOo34TI5q2Io3L3wQ1BWvzcDXckmltUlOc0hehqYVM5nKnpBWVALDZAB4h0B0rjz30015YXSI51x70IfKk9luQ%2FJQpQGrgTEZGxDg4KkMnIC5LhXfzYQ2wQqHtji39IlSXR5qxt2r%2F40vM

    rand加密

10   上传文件(已完成 在helps.py中)

11  unique两个字段????
    __table_args__ = (UniqueConstraint('customer_id', 'location_code', name='_customer_location_uc'),)

12  修改mergerequest错误
        删除unique
        status改成中文[todo]
        todict的时候映射成中文

14 修改展示机构成员,userid根据
    待完成

15  文件上传再包一层[todo]

===================================================2.6===================

1 为什么文件夹 __pytche__

2 lp_type改成英文[已完成]

3   login模块学习:
        #第一步 导入
        from flask_login import LoginManager
        lm = LoginManager()
        lm.init(db)
        #第二部 配置
        # 1函数接受一个用户id参数,返回一个User对象,Flask Login 使用这个函数来检查给定的id是否对应了一个正确的用户对象
        @lm.user_loader
        def load_user(user_id):
            user = User.query.get(user_id)
            return user
        # 2
        lg.session_protection = 'strong'
        lg.login_view = 'api/login'

        #第三部
        def is_authenticated :表示是否登录
        def is_active :表示用户是否通过了某种激活程序
        def is_anonymou :表示是否为匿名用户


4 test的执行顺序

5   基金池要order_by
        filter_by(company_id)

6 list 时候;
    拼sql

7   判断一个set是否另外一个set的子集
    set学习:
        set是无序的,无法通过下标来获取;
        并集 |
        差集 -
        交集 &
        对称差集 ^(对称差集的含义为)

        a.issubset(b)  与 <= 一致

8 [修复] set判断子集为<=

9 @login_requied 没有登录如何捕获错误

10 参数的中英文转换


11 git diff  工作区 与 暂存区
    git diff --cached  暂存区 与 版本库
    git diff HEAD 工作区 与 版本库

版本库 与 远程分支


12 [修复] 所有的返回后效数据 加 errcode 加errmsg
            创建成功也返回一点东西

    [文件上传]

    [修复login]

13 增加 查询机构成员 函数



==================================2.7===============================

1 完成以前未完成任务
    [正价查询机构成员]


==================================2.10================================

1 gunicorn

    gunicorn [OPTIONS] APP_MODULE

    gunicorn -b 0.0.0.0:8080 basesite.application:create_app

    独角兽是一个高效的python WSGI SERVER 用来运行 WSGI application

2 create database even default charset utf8 collate utf8_general_ci
    其中collate为校准规则,还有另外一种校准规则:urf8_unicode_ci 两者主要是对俄罗斯语的校准标准不一致.但是general更快,ci表示大小写敏感.


3 文件上传

    request.files 格式为  ImmutableMultiDict([])

    ImmutableMultiDict([('user_avater_new', <FileStorage: '海绵宝宝0.jpeg' ('image/jpeg')>)])



===============================2.11======================================

1 如何用postman模拟表单提交? [失败]

    tip1:
        header标签不要填 content_type



2 用户参与的项目
    .update 只能更新一个字典
    dict(dictA,**dictB)


3 数据库锁住了
    show processlist
    kill id

4   文件上传

    request.files:
     ImmutableMultiDict([('user_file_new', <FileStorage: 'file-项目字段梳理+说明.txt' ('text/plain')>)])

    request.form
    一个包含解析过的从 POST 或 PUT 请求发送的表单对象的 MultiDict 。请注意上传的文件不会在这里，而是在 files 属性中

    用get方法来获取,或者用getlist('')[0]


5   三表查询:
    注意三表的位置关系:
        temp = AttachmentModel.query\
            .with_entities(AttachmentModel.id,CompanyUserDetailModel.chinese_name)\
            .join(ProjectFileModel,ProjectFileModel.file_id==AttachmentModel.id)\
            .join(CompanyUserDetailModel,AttachmentModel.user_id==CompanyUserDetailModel.user_id)\
            .all()
''

6   未完成的任务
    project_id的name 做成字典
    user_id 的name 做成字典
    for i in result_data : i.update({})


7 如何使用.gitignore
    __pycache__/  忽略这个文件目录
    *.py[cod]     忽略pyc,pyo,pyd文件
    basesite/attachments/

8 增加task的评论还有附件功能

9 db.session.query功能可不可以用
    db.session.query(ProjectTaskModel).count() 可以用
    db.session.query(ProjectTaskModel.project_id).all()
        结果为:[(1,), (1,), (2,), (2,)]
    from sqlalchemy import distinct
    db.session.query(distinct(ProjectTaskModel.project_id)).all()


    from sqlalchemy import func
    db.session.query(func.count(ProjectTaskModel.project_id)).all()

    group_by
    db.session.query()

    from sqlalchemy.sql import label
    db.session.query(ProjectTaskCommemtModel.task_id.label('task_id'),
                        func.count(ProjectTaskCommemtModel.task_id).label('comment_count'))\
                        .group_by(ProjectTaskCommemtModel.task_id)\
                        .all():

10 请求的时候,有{}和没有{} 结果是一样的

11 登录之后返回他的roleid,role_name

12 数据库  一个用户在写,另外一个用户在读,怎么办?

13 锁是什么原因


=================================2.12===================================

1 创建项目任务评论 以及 附件


2 bug: 主页位置查询项目assign_id 现在只有一个人
        创建任务时候,返回的id,为最后一个assign_id 的task_id

3 两个列表取并集
  list(set(lista).union(set(listb)))


5 设置为public 其他机构可以看见吗?

6 listproject的时候如果有参数,参数里面的值不能为空

7 一个字典中,想要有对应的key,并且对应的value不为空,那么用get方法

8 编辑项目资本资料的时候,只有admin才可以编辑吗?还是只要是项目成员都可以编辑?


10 修改的时候:ProjectModel.query.filter_by(id=remain_parames['project_id']).update(project_params)
        不是.all()再进行修改

11 如何修改commit log



=============================2.13=======================================

1 db.session.delete() 只能删除单个对象

2
    db.session.query(CompanyUserRoleModel.role_id.label('role_id'),CompanyRoleModel.role_name.label('role_name'))\
                          .join(CompanyRoleModel,CompanyUserRoleModel.role_id==CompanyRoleModel.id)\
                          .filter(CompanyUserRoleModel.user_id==current_user.id).all()



4 添加任务附件

5如何写类的装饰器 暂且没有

6 .IntegrityError 完整性错误,具体原因要看 错误的类型

7 create table yanxintest001 (name varchar(20),id int not null auto_increment, primary key(id))

8 insert into yanxintest001 values('yanxin','test')


10 default可以为null?

11 select * form xxx   where id=7  \g

12
ProjectCompanyInfoModel.query\
                            .filter_by(project_id=26)\
                            .filter_by(company_id=1).first()


13 nodejs V6.XX

14sudo dpkg -l nodejs ?




16 常见项目admin_id如果不是自己,那么自己的角色为member;


17 查询的时候,rollback,这是正常的,隐式事务
    21.3.1 隐式事务
隐式事务是SQL Server为你而做的事务.隐式事务又称自动提交事务.如果运行一条
I N S E RT语句,SQL Server将把它包装到事务中,如果此I N S E RT语句失败,SQL Server将回滚
或取消这个事务.每条S Q L语句均被视为一个自身的事务.例如在程序清单2 1 - 2中,有四条
I N S E RT语句.第一,二,四条是有效的,第三条语句是无效的.因为它违反了该表中有关作
者标识必须唯一的约束.当程序运行时,第一,二,四条语句执行成功并插入表中.第三条
第2 1学时SQL Serv e r编程2 0 5
下载
语句失败并回滚.
程序清单21-2 隐式事务
在日常操作中,你可能依赖于隐式事务.在第三方应用程序中,这些应用程序的开发人
员则可能使用显式事务.
21.3.2 显式事务
显示事务是一种由你自己指定的事务.这种事务允许你自己决定哪批工作必须成功完成,
否则所有部分都不完成.为了给自己的事务定界,可以使用关键字BEGIN TRANSACTION和
ROLLBACK TRANSACTION或COMMIT TRANSACTION.
BEGIN TRANSACTION—这个关键词用来通知SQL Server一个事务就要开始了.
BEGIN TRANSACTION后面发生的每一条S Q L语句都是同一个事务中的一部分.
ROLLBACK TRANSACTION—这个关键词用来通知SQL Server自BEGIN TRANSACTION
后的所有工作都应取消,对数据库中任何数据的改变都被还原,任何已经创建或删除的对
象被清除或恢复.
COMMIT TRANSACTION—这个关键词用来通知SQL Server自BEGIN TRANSACTION
后的全部工作都要完成并成为数据库的一个永久性部分.在同一个事务中,你不能同时
使用ROLLBACK TRANSACTION和COMMIT TRANSACTION.
你必须意识到,即使你的脚本中有错误,而你又让SQL Server提交事务,该事务也将执
行.如果你打算依赖于现实事务保证数据完整性,必须在脚本中建立错误检查机制.程序清
单2 1 - 3中的代码显示了运用显式事务来回滚对e m p l o y e e s表的改动.

18  能不能改只取决于是不是项目成员
    能不能看取决于是不是公开,以及是不是项目成员
    能不能创建取决于是不是项目成员



===========================================2.14=======================================


"[{'name':'刘青','职位':'CEO','phone':'2343454','email':'23434545@qq.com'}{'name':'刘青','职位':'CEO','phone':'2343454','email':'23434545@qq.com'}]"




数据库CompanyUserDetailModel extra

注册资本为 decimal

2.15======================================2.15=====================================


视图模式  shift+v : s/FROM/TO/g

删除创始人
删除管理人
删除角色




current_app.logger.exception(e)
current_app.logger.info(e)



选中当前单词  yaw
复制后覆盖  yw cw esc p
取消搜索后的高亮  /noh
跳转到相同的函数  gd



5 WSGI协议
    从应有程序角度思考:
        python程序必须实现了__call__方法  可调用
        python 接受(environ,star_response), 并在返回 iterable前调用了star_response
                star_response接受两个参数(status ,response_headers)
                这两个参数由http server 提供
    从WSGI角度思考
        调用了python程序,传给了它两个参数,

6 Falsk流程
启动:
    1 app.run()
        调用了 werkzeug.runsimple(,,self..)
        开始监听端口
        有请求来了
        调用app去执行对应的逻辑(application_iter = app(environ, start_response))
            里面这两个参数为包含了环境变量的字典,

    2 既然要调用app,那么就需要定义了__call__方法
        __call__方法return了wsgi_app(environ, start_response)
                    路由分发,找正确的处理函数
                    返回函数处理后的结果response = self.full_dispatch_request()
                                            rv = self.dispatch_request() 这个返回就是字符串或json数据
                                            return self.finalize_request(rv) 这个把数据包装成response对象
                                    response(environ, start_response)




7 cache 读完缓存  (因为从硬盘读到内存中很慢,所以读完后会在内存中缓存下来,及时程序结束,cache也不会自动释放
            所以当你大量读文件操作的时候,内存使用率会上去)

buffer 写的时候,写到buffer里面,然后buffer写到硬盘里面.

swap 因为内存不够用了,所以用硬盘的一部分当内存用,
    swap out 内存到硬盘
    swap in 硬盘到内存



8 Flask路由规则

    装饰器实际上调用的还是add_url_rule方法,

    def add_url_rule(self, rule, endpoint=None, view_func=None, **options):
    """Connects a URL rule.  Works exactly like the :meth:`route`
    decorator.  If a view_func is provided it will be registered with the
    endpoint.
    """

    methods = options.pop('methods', None)

    rule = self.url_rule_class(rule, methods=methods, **options)
    self.url_map.add(rule)
    if view_func is not None:
        old_func = self.view_functions.get(endpoint)
        if old_func is not None and old_func != view_func:
            raise AssertionError('View function mapping is overwriting an '
                                 'existing endpoint function: %s' % endpoint)
        self.view_functions[endpoint] = view_func
    这段代码实际上就是更新self.url_map变量
                    self.view_functions变量 一个字典




===================================2.16=============================

1 idea  : art of computer
 是什么 : 计算机艺术网站
 有什么: 看 (最简介的编程,编程的视觉化设计,VR油画)
        玩 (项目:cosplay 油画)
    实质是做内容

    这个项目带来了什么 ? 小众用户,广告
    这个项目的人群 :
    为什么不会被模仿:???

2 idea : 教育培训,计算机英语
    是什么:面对程序员而做的英语学习
    有什么:视频?翻译?字典?
    凭什么吸引人: 程序员的学习性
    为什么不会被模仿: ????

3 rmdir只能删除空文件夹
    rm -r 可以删除里面有内容的文件夹

4 关于时间进度,
    第一步,有工程师参与,确定产品方向
    第二步,确定如果delay的话,模块的优先级

5 什么叫上下文
    很多程序都有很多外部变量,只有像是add这种简单的函数才是没有外部变量的,一旦你的程序有了外部变量,你的程序就是不完整的,不能独立运行,
    为了使他们运行,你要把所有的外部变量一个个加进去,这些值的集合就叫做上下文

    比如在flask中,试图函数需要知道他的请求方式,url参数等


6   vim快捷键
    "{" 是上一个分段处，"{{" 就是两个段了。

    "]]" ，下一个函数块开始位置
    这个命令看源码的时候特别方便
7 vim如何整体锁紧
    进入试图模式,然后<或者>

8 vim复制全部
    yy
    "*y
    G


LPmodle 增加 manager_id

=================================================2.17===================================================
1 关于delete 因为有太多外键关联,所以不能直接删除,底层的status


2.18==============================================2.18==============================================

1翻墙软件装不上
    原因: connnetct failed
        原因 :配置 ip,port,password,加密方式
                103,240,183,54  8989  yan0312   aescfb
                    端口号换一个 否定
                    加密方式不支持 否定

                    log:Please check the connection's profile and your firewall settings.
                    这个软件有问题
                    退出后重新进又连接通了
                    但是latency test failed

        换一个软件连接
            如何安装github上的软件

    [解决]:
        原因 : shadowserver 根本没有打开,可以根据下面的命令来看是不是打开
        参考的帖子为:
            https://www.qiyunss.com/blog/70.htm
            https://www.xiaoz.me/archives/5643


2 netstat -tunpl
    查看qt5状态


3 如何发送cookie




================================================2.20==================================================
今日计划: 上午学习fabric 下午开始补充权限体系和oplog
用户name

1 完成已知的仍缺少的视图函数

2 handing signal winch是什么意思
    Gracefully shutdown the worker processes when Gunicorn is daemonized.
    优雅的关闭工作进程
    handing signal int 快速关闭


3 为什么只有虚拟环境下pip install 才不会出差

    我配置的豆瓣源地址为~/.pip/pip/conf 按理说跟环境没有关系

4 安装fabric的时候,paramiko(ssh协议库),cryptography,pyasn1,idna 也安装了

5 flask中session cookie 的security属性
    1 secure属性
当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输，也就是只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被窃取到Cookie 的具体内容。
2 HttpOnly属性
如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击。



6 ansible 是什么?
    与fabric一样,都是做了这么个事——批量的在远程服务器上执行命令 。

那么fabric和ansible有什么差别呢？简单来说fabric像是一个工具箱，提供了很多好用的工具，用来在Remote执行命令，而Ansible则是提供了一套简单的流程，你要按照它的流程来做，就能轻松完成任务。这就像是库和框架的关系一样。

当然，它们之间也是有共同点的——都是基于 paramiko 开发的。这个paramiko是什么呢？它是一个纯Python实现的ssh协议库。因此fabric和ansible还有一个共同点就是不需要在远程主机上安装client/agents，因为它们是基于ssh来和远程主机通讯的。

7 [宝剑]没有构成问题之前,先不要优化它
    命名规则主要看团队要求

8 postman点击后没有反应listlis
    有可能是路由的问题,造成了超时
    所有把地址改为localhost

9 current_user.detail是个什么鬼?
是一个列表 可以这样用:current_user.detail[0].chinese_name

10 request.data
    这是一个字符串,
    而request.get_json()是一个字典

11 搞定还款

12  oplog要按顺序一个一个来



            op_log(op_mod='project',
                op_interface='CreateProjectView',
                related_id=project.id,
                op_desc='{username}创建了项目{project_name}'.format(username=current_user.detail[0].chinese_name,project_name=project_params['project_name']),
                extra=request.get_data()
                )




13 related_id 应该设置为项目的id,因为会根据项目id查动态;
        desc尽量带多点信息



14 birthday present
    flower 预定

15 只有管理员才可以添加项目成员,删除项目成员,

=======================================================2.21=============================================

1 sqlalchemy中scalar()调用one()方法，并且成功时返回第一列数据

            query.scalar()

2 首页完成我的任务. 搞定


4 基金的募资信息

5 下载东西 是filepath还是什么构造字符串
    /attachment/<int:attachment_id>.<ext>

6 git cm -m'' 如何换行
    '
    1
    2  '


8 知乎图片 模糊效果
    Progressive JPEG
    具体看http://blog.jobbole.com/44038/
    在photoshop中有“存储为web所用格式”，打开后选择“连续”就是渐进式JPEG

9住房公积金 手机号更换 未换成功


=====================================2.22===================================

1 提交前 自己审查一遍  手滑太多

2 上午任务: 任务加oplog功能
        并且查询任务动态


3 tower清单


5 翻墙出现异常
    通过netstat -punal 判断
    本机没有监听1080端口,系统配置有问题
    最后发现qt5没有开,尴尬.......

6 chrome://settings/fonts

7 架构知识:
    系统降级 组件可替换
        如何进行数据的迁移


8 禁止登录?
    二级菜单


11 了解现有的消息系统
    1消息的几种分类:公告(站内用户).提醒(关注的问题).私信(一对一)
    2消息的获取方式:push .pull
                push:推的比较常见，需要针对某一个问题维护着一张关注者的列表，每当触发这个问题推送的条件时（例如有人回答问题），就把这个通知发送给每个关注者。
                软件pull:每个用户都有一张关注问题的列表，每当用户上线的时候，对每个问题进行轮询，当问题的事件列表出现了比我原本时间戳大的信息就进行拉取。

    3而我们则根据消息的不同分类采用不同的获取方式：
         通告和提醒，适合使用拉取的方式，消息产生之后，会存在消息表中，用户在某一特定的时间根据自己关注问题的表进行消息的拉取，然后添加到自己的消息队列中，

        信息，适合使用推的方式，在发送者建立一条信息之后，同时指定接收者，把消息添加到接收者的消息队列中。
    4根据提醒使用拉取的方式，需要维护一个关注某一事物的列表。这种行为，我们称之为：「订阅」Subscribe

        有空看下篇

12 什么是feed流
    1. Feed 是一种数据格式，用于给（订阅的）用户提供持续更新的内容；
    2. 不是 Push ，实质是用户自己主动选择多个订阅源，展示内容汇总的聚合器（典型代表是RSS）主动向服务器请求内容，再以时间顺序呈现到聚合器，是一种典型的 Pull Technology

13 什么是amqp
    高级消息队列协议（AMQP1）是一个异步消息传递所使用的应用层协议规范

14 推荐3.4还是3.5 ????


15  ChangeProjectTaskView中oplog未测试

16 挂号



=====================================2.23

1 ubuntu系统上划词翻译的软件

2 SQLALCHEMY_TRACK_MODIFICATIONS:如果设置成 True (默认情况)，Flask-SQLAlchemy 将会追踪对象的修改并且发送信号

3 sqlalchemy的优化
    1 session.query(cls).count()
        相比select count(*) from XXX 慢很多
            因为先遍历查询了一遍,然后再count了一次
            数据量很大的时候是很恐怖的.
        优化:    1 直接使用func.count()
                2 一种粗暴的办法就是使用memcache，直接用查询的sql的hash做key来保存结果


    2

4 limit=0,offset=20

5 orm的查询日志保存在了哪里

7 系统监控

6 loging模块

7  .tar
　　解包：tar zxvf FileName.tar
　　打包：tar czvf FileName.tar DirName

8  测试CreateProjectMilestonView oplog



9 查询mysql里面的用户

 SELECT User, Host, Password FROM mysql.user;

 10 tower GV 修改为别人的时候,前段验证

11 listprojectuser的时候要进行分组

12 添加项目成员的时候设为member





13 重要:sql GROUP_CONCAT,聚合对字符串的拼接

a = db.session.query(ProjectUserModel.user_id,func.group_concat(ProjectUserModel.role)).group_by(ProjectUserModel.user_id).all()


14 字符串变list
    str.split(',')

    列表变字符串
    ','.join(alist)

===========================================2.24======================================

1 任务动态,创建任务的时候给Project挂一个op_log
            其他的时候挂在任务身上

2 文件大小要不要单独加一个字段
    这个字段怎么拿?
        上网查,

3 快递,药,焦虑症


4 ubuntu半天连不上网
    enable wifi


5 ssh证书
    cat ~/.ssh/id_rsa.pub | ssh bill@128.199.209.242 'mkdir -p .ssh && cat - >> ~/.ssh/authorized_keys'

    >>代表什么意思
    > is used to write to a file and >> is used to append to a file.
    "|" (pipe)這個呢則是把前面的stdout 接到後面程式的 stdin
7 任务相关的oplog

8 版本控制

=======================================2.25================================

1 电脑装系统

2 ssh如何退出
    ~ + ctrl+Z
3  ssh No route to host


5 node学习

6   no default or ui configuration directive found


======================================2.27================================

1爬虫课件

2ChangeProjectPrivacyView

3 宝剑的文章
    {'periodically':定期的}
    {'approximate':大概的}
    {'The original underlying function is accessible through the __wrapped__ attribute':'原始的底层函数'}
    dispat

4 flask的并发
    这段程序只是一个APP服务的一个路由。你的APP服务又被用WSGI接入到HTTP服务器，就是你说的nginx。这个问题跟nginx无关，因为这里它只是一个通道。跟Flask也无关，因为它只是负责根据Request产生Response。有关的就是WSGI server。通常来讲WSGI server是通过进程（pre-fork)来并发的。这样并发就取决与进程数。如果WSGI server用了gevent, eventlet等 green thread技术，就可以支持更多并发。

    gunicorn app -w 2 -b :8000 gunicorn是通过多进程来进行并发

    flask可以和gevent共用的，并发会好很多。 用gevent monkey patch，可以尽可能少的修改当前代码。


5 一个项目中有多个管理员,那么多个管理员能不能被删除

6 Getting text from the clipboard on this platform requires the python3-tk package.

    sudo apt-get install python3-tk


8 文件上传413错误
    请求实体过长
        nginx 中修改配置
    414错误 请求参数过长

9 清明节安排
    4.4日


12 单独改状体 项目状态

13 linux rar解压工具
    unrar e 文件名

===================================2.28=========================================

1 掘金文章分享 https
    https://showme.codes/2017-02-20/understand-https/
    http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html

    要想https安全,必须使用对称加密算法,
        但是协商对称加密算法的过程,需要使用非对称加密算法来保证安全
            直接使用非对称加密会有漏洞,比如中间人篡改公钥的可能性
                所以不使用公钥,而是使用证书来保证非对称加密过程的安全


2 继续权限相关

3
    op_log(op_mod='fund',
                op_interface='CreateProjectView',
                related_id=fund.id,
                op_desc='{username}创建了基金{fund_name}'.format(username=current_user.detail[0].chinese_name,fund_name=params['fund_name']),
                extra=request.get_data()
                )

8

一 首先在传输中,为了加快加密和解密的速度,我们一般选用对称加密
    在AtoB模型中,对称加密足够了
二 但是具体到BS模型中,以为是一对多关系,所以S对A的加密算法被B轻松拿到.
    所以S针对不同不用采用不同加密算法
    也就是说,在真正加密之前要协商具体用哪种加密算法,这个过程叫做:协商
三 如果保证的协商的安全,如果继续加密又会产生鸡生蛋蛋生鸡的问题,所以要靠非对称加密来解决
    非对称加密: 服务器是私钥,用户拿的是公钥,私钥加密的所有东西公钥都可以解开,公钥加密的东西只有私钥可以解开.

四 那么如何让客户端安全的拿到公钥?
    我们不能直接将服务器的公钥传递给客户端(中间人掉包又产生鸡蛋问题)，而是第三方机构使用它的私钥对我们的公钥进行加密后，再传给客户端。客户端再使用第三方机构的公钥进行解密。
    第一版“数字证书”，证书中只有服务器交给第三方机构的公钥，而且这个公钥被第三方机构的私钥加密了：如果能解密，就说明这个公钥没有被中间人调包。因为如果中间人使用自己的私钥加密后的东西传给客户端，客户端是无法使用第三方的公钥进行解密的。



9 405错误
    方法不被允许

10 linux学习  第14章
    查看电脑上的用户名/哪用户的uid  grep 'xy' /etc/password  /id xy

    linux 启动过程
         boot ---init进程---运行级别-----rc脚本-----6个终端----login
                祖宗进程     守护进程



12 media和dev

13 >>> import datetime

>>> utc = “2014-09-18T10:42:16.126Z”
>>> local = “2014-09-18 10:42:16”

>>> UTC_FORMAT = “%Y-%m-%dT%H:%M:%S.%fZ”
>>> LOCAL_FORMAT = “%Y-%m-%d %H:%M:%S”

>>> datetime.datetime.strptime(utc, UTC_FORMAT)
datetime.datetime(2014, 9, 18, 10, 42, 16, 126000)

>>> datetime.datetime.strptime(local, LOCAL_FORMAT)
datetime.datetime(2014, 9, 18, 10, 42, 16)
"2013-12-07T16:00:00.000Z"



14

运行shell的两种方法
    . test/sh
    /bin/sh test/sh



================================================3.1==============================================

1 机械键盘

2 PostgreSQL入门学习

3 utc时间更改
    1 datetime.utcnow()
    2 展示的时候也是

4 default =now


5 关于时间格式  如果是日志系统的,用后端的时间,无所谓了
    如果入口在用户那,那么接受什么,存什么,返回什么,保证我不改用户所存



10 如何对字典的value格式进行排序
        def sort_by_value(adict):
            backdict = {adict[i]:i for i in adict}
            return backdict

11 字典生成式 常用



优化:
13 默认角色叫 超级管理员
17 必填字段后端来卡 优先级低
18 修改用户角色
机构里面是要默认给VP 角色的,

=================================================3.2======================================

1 微信定时

2 责任GP 为机构角色


3 创始人




==================================3.3=================================================

2 逐个排查删除接口
    删除评论附件

3 项目标签的增删改查


关于项目标签
新增一个标签:createtag 传入tag_name 我返回一个tag_id
创建项目标签: createproject 把tag_id放入到tag_id_list参数中
删除标签 只需要在编辑项目中把tag_id_list中要删除的tag_id删掉点保存即可


4 添加投资信息后bug


6 movie
    OK

7  dpkg -i XXX.deb
    之后如果确实依赖
    sudo apt-get -y -f install
    或者
    dpkg-checkbuilddeps

8掘金文章 :
    http://www.jianshu.com/p/4568c2ff3e6d
    什么是数据库的一致性问题:
        你支付了两笔订单,但是数据库中只有一个订单


    数据库的使用可以分为三个层面
        基本使用
        orm使用
        分布式使用
    关于sqlalchemy的几个特点
        自动同步model和元数据
            比如你在model中新加了一个字段,会自动反应在数据表中的元数据中,
        与web框架无缝结合
            每个请求会产生了一个数据库连接,每个请求被封装在一个transaction中
        Eager loading
            尽量不要在for循环中执行查询
            session.query(Order).options(joinedload_all('items.product')).get(order_id)
        提供钩子,用户可以自定制

    分布式数据库应用
    暂时先理解为分库分表
        如果为分库分别,那么对我们的程序有什么影响呢
            同一个用户的数据只存在一个表中,不能多到两个表
                所以要对数据的key进行hash取模,通过一个公式来计算存储在哪个表中
            另外设立get_table()这个表,如果不存在就创建一个表

    什么是词云
        词云的原理是对输入的文本数据进行词频统计，根据词汇出现频率的不同，按不同比例显示出词汇，生成图片。频率高的词汇显示的大，频率低的词汇显示的小


9 corrsion
    要求：

    统计一部英文小说里单词的出现次数（忽略大小写）
    按出现次数显示最高的 100 个单词
    【附加题】多统计几个不同作家的作品，挑选一些特征词汇的次数画在图表上，展示不同作家的风格区别。

思路
    一:
    存到dict{'word':'count'}
    按value排序
    二:
    存到树结构




========================================3.4==========================

1 listrole list_admin:1


2 没有角色不让登录


3 删除用户角色,角色地下的人不删除 [宝剑确认]

4 103.240.183.54
    127.0.0.1
    1080
    AES-256-CFB

5   权限更改
    后台制作
    爬虫学习


6 从产品角度与vcsaas vc800区别
点击等待的动态效果
返回的数据格式


7 gitk
干什么用的 git图形化工具

8 flask_principal

9 对于返回数据格式的理解

10 重新安装shadowsockqt5


11 shadowsock failed
    把迅雷的端口删掉就ok

12 supervisor

13 apt list --installed | grep xxxx

14 manager admin 同样权限

============================3.6======新的一周===========================

1 这周任务
    上线一个博客系统
    今天把数据库建立好
    项目名称 XY
f


2 nginx启动的时候端口被占用?
    看谁在占用
    lsof -i:80

3 如何启动nginx
    cd /usr
    sudo sbin/nginx

    或者
    sudo /etc/init.d/nginx start

4 配置文件到底放哪里


5 sudo service nginx reload
    listen 监听的6789
    8081转到node服务器
    8080转到我的服务器

    sudo ln -s /etc/nginx/sites-available/even.conf . 在本地创建一个快捷方式

6 如何避免循环导入
    引入但不执行
    或者把import语句换个位置再试



7manager是如何runserver的




8 pyspider



6 19:08:48,630 INFO sqlalchemy.engine.base.Engine ROLLBACK
--------------------------------------------------------------------------------
ERROR in project_view [/home/xy/workspace/gitRespority/basesite/blueprints/mod_project/project_view.py:3678]:
Cannot convert None to Decimal
--------------------------------------------------------------------------------
Traceback (most recent call last):
  File "/home/xy/workspace/gitRespority/basesite/blueprints/mod_project/project_view.py", line 3664, in post
    'amount': Decimal(comany_fund['amount']),
  File "/usr/lib/python3.4/decimal.py", line 710, in __new__
    raise TypeError("Cannot convert %r to Decimal" % value)
TypeError: Cannot convert None to Decimal
192.168.1.249 - - [06/Mar/2017 19:08:48] "POST /api/changeprojectinvestmentstage HTTP/1.0" 200 -




======================================3.7====================================
1 deleterole 需要前段确认

2  pip下载错误
    pip  install  -i  https://pypi.doubanio.com/simple/  --trusted-host pypi.doubanio.com


3 Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-hgyI4P/pycurl/
    sudo pip install --upgrade setuptools

4 明天单元测试

5 管理员 ,查看全部隐私项目, 如何同意

6all
如果iterable的所有元素不为0、''、False或者iterable为空，all(iterable)返回True
可以if not all(params.values)

7 any

8basestring 和 string的区别
    basestring() 抽象工厂函数,其作用仅仅是为 str 和 unicode 函数提供父类，所以不能被
实例化，也不能被调用



=========================================3.7real=============================================

1下载pyspider

bug:
    pip  install  -i  https://pypi.doubanio.com/simple/  pyspider
Collecting pyspider
/home/xy/.virtualenvs/spy/local/lib/python2.7/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:318: SNIMissingWarning: An HTTPS request has been made, but the SNI (Subject Name Indication) extension to TLS is not available on this platform. This may cause the server to present an incorrect TLS certificate, which can cause validation failures. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.io/en/latest/security.html#snimissingwarning.
  SNIMissingWarning
/home/xy/.virtualenvs/spy/local/lib/python2.7/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:122: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.io/en/latest/security.html#insecureplatformwarning.
  InsecurePlatformWarning
  Certificate did not match expected hostname: pypi.doubanio.com. Certificate: {'notAfter': 'Jun 11 23:59:59 2018 GMT', 'subjectAltName': (('DNS', '*.ucdl.pp.uc.cn'),), 'subject': ((('countryName', u'CN'),), (('stateOrProvinceName', u'Guangdong'),), (('localityName', u'Guangzhou'),), (('organizationName', u'\u5e7f\u5dde\u5e02\u52a8\u666f\u8ba1\u7b97\u673a\u79d1\u6280\u6709\u9650\u516c\u53f8'),), (('organizationalUnitName', u'IT DEPT'),), (('commonName', u'*.ucdl.pp.uc.cn'),))}
  Could not fetch URL https://pypi.doubanio.com/simple/pyspider/: There was a problem confirming the ssl certificate: hostname 'pypi.doubanio.com' doesn't match '*.ucdl.pp.uc.cn' - skipping
  Could not find a version that satisfies the requirement pyspider (from versions: )
No matching distribution found for pyspider
缺少SNI 你可以用最新python来解决
或者pip install pyopenssl ndg-httpsclient pyasn1   fromhttp://stackoverflow.com/questions/35535566/installing-pip-using-get-pip-py-snimissingwarning





2 安装python3的虚拟环境

which python3 #Output: /usr/bin/python3
mkvirtualenv --python=/usr/bin/python3 nameOfEnvironment



3 pip安装制定版本
    pip install XXX==XXX

4 pip install bug
    100% |████████████████████████████████| 184kB 117kB/s
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "/tmp/pip-build-6yxbtleh/pycurl/setup.py", line 103, in configure_unix
        stdout=subprocess.PIPE, stderr=subprocess.PIPE)
      File "/usr/lib/python3.4/subprocess.py", line 859, in __init__
        restore_signals, start_new_session)
      File "/usr/lib/python3.4/subprocess.py", line 1457, in _execute_child
        raise child_exception_type(errno_num, err_msg)
    FileNotFoundError: [Errno 2] No such file or directory: 'curl-config'

    During handling of the above exception, another exception occurred:

    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-build-6yxbtleh/pycurl/setup.py", line 823, in <module>
        ext = get_extension(sys.argv, split_extension_source=split_extension_source)
      File "/tmp/pip-build-6yxbtleh/pycurl/setup.py", line 497, in get_extension
        ext_config = ExtensionConfiguration(argv)
      File "/tmp/pip-build-6yxbtleh/pycurl/setup.py", line 71, in __init__
        self.configure()
      File "/tmp/pip-build-6yxbtleh/pycurl/setup.py", line 107, in configure_unix
        raise ConfigurationError(msg)
    __main__.ConfigurationError: Could not run curl-config: [Errno 2] No such file or directory: 'curl-config'

    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-6yxbtleh/pycurl/

Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-_jstrvgd/pycurl/

解决方式 :sudo apt-get install libcurl4-openssl-dev



5 pyspider有什么优缺点



6  globals 是包含当前全局符号表的名字的字典


7 安装mongodb




8 re 用户名 数字字母下划线 \w
    \s 制表符 空白符


9 w3m上一个单词
        W

10 安装 virtualenv
    pip install virtualenvwrapper
    source /usr/local/bin/virtualenvwrapper.sh
    或者在/user/bin/virual.sh

11
设置 PATH
为了我们能够方便的使用Python，我们需要设置系统变量或者建立 软连接将新版本的 Python
加入到 path 对应的目录 ：
export PATH="/usr/local/bin:$PATH"
or
ln -s /usr/local/bin/python2.7  /usr/bin/python
# 检查
[root@dbmasterxxx ~]# python -V
Python 2.7.8
[root@dbmasterxxx ~]# which python
/usr/bin/python




12 data = {
            'username': 'test',
            'password': '123456',
        }
        self.client.post('/api/login',
                      data=json.dumps(data),
                      content_type='application/json')



13 装新电脑



14 list接口示意


{
"code":"0000",
"msg":"",
"data":{
"totalItem":2,"data":[{"id":10558,"name":"drop database vcsaas","logo":null,"field":4,"stage":1,"financing_stage":1,"priority":null,"group":null,"seq":2,"flowInfo":null,"funds":[]},{"id":10554,"name":"滴滴大人","logo":"//src.vcsaas.cn/production/10765/1/10554/logo/10554_1488456580583.jpg","field":2,"stage":0,"financing_stage":0,"priority":3,"group":null,"seq":1,"flowInfo":{"_id":"58ba68ed695e7b02d43012f5"},"funds":[]}]
}
}


15 部署的时候 su app 来拉代码
    进入到/etc/supervisor 运行


=============================================3.8=======================================

1 没事man一下 包括nginx

2 优雅退出ssh ~.
    有时候不管用用 logout

3 今天任务      中文提示
             单元测试
            pyspider



4   how to catch error duplicatte
        except IntegrityError



5   {'assign_user_id_list': [None], 'content': '', 'project_id': 1, 'title': '严新要测试'}

6   values返回字典中的值，itervalues已经不被python3支持。

7   创建我的笔记必填字段还没有限制


8   NoneType' object has no attribute 'strftime'

    if isinstance(value, Decimal) and (value is not None) :

    if isinstance(value, Decimal) and (value is not None) :


9   bug 进入项目的时候 速度慢  然后multiresult
        查到特定接口,定位数据库,最后查原因,发现以前的接口还在用



10 dict.pop(a,{'a':"sdf"})
        The pop method of dicts (like self.data, i.e. {'a':'aaa','b':'bbb','c':'ccc'}, here) takes two arguments -- see the docs

        The second argument, default, is what pop returns if the first argument, key, is absent. (If you call pop with just one argument, key, it raises an exception if that key's absent).

        In your example, print b.pop('a',{'b':'bbb'}), this is irrelevant because 'a' is a key in b.data. But if you repeat that line...:

        b=a()
        print b.pop('a',{'b':'bbb'})
        print b.pop('a',{'b':'bbb'})
        print b.data

11 pdf阅读软件



12 git rebase -i HEAD~5
    pick：正常选中
    reword：选中，并且修改提交信息；
    edit：选中，rebase时会暂停，允许你修改这个commit（参考这里）
    squash：选中，会将当前commit与上一个commit合并
    fixup：与squash相同，但不会保存当前commit的提交信息
    exec：执行其他shell命令

13 git stash
    git stash


14 错误502 本地调试断网的时候

15 重构原因 1权限的冲突
        2  OPLOG放前端
        3 接口 再封装一层底层的sqlalchemy

16 更改配置文件的时候:
    尽量放在注释的位置
改之前cp 一个 bak
改制后 diff 两个文件

17 安装python 2.7
    make altinstall
        这样不会覆盖


18

In [5]: ['status','project_id'] == ['project_id', 'status']
Out[5]: False

In [6]: set(['status','project_id']) == set(['project_id', 'status'])
Out[6]: True



====================================3.9===============================

1   localhost  是一个域名 指向了127.0.0.1 在etc/host里面可以看到

     本机有三个网卡  一个是lo 虚拟网卡
                    一个是有线网卡 etho
                    一个是无线网卡 wlan

2   今天下午任务
    一些单元测试
    爬虫


3   数据库更改 用 alter
    加项目头像字段


6   项目池 抽象成函数


7
    更改表结构
    alter table XXX add column_name XXX datatype
    alter table XXX drop column XXX
    alter tales XXX modify column XXX datatype

8 alter table default ?
    ALTER TABLE Persons
    ALTER City SET DEFAULT 'SANDNES'

9  vim复制到电脑
    The "* and "+ registers are for the system's clipboard


10





文件b


=======================================================


/api/attachment/id/filename(jpg)
返回加url



===================================================
/api/changedetail'
'/api/changefund'
'/api/changefundlp'
'/api/changelp'
'/api/changemember'


'/api/changeproject'
'/api/changeprojectcompanyinfo'
'/api/changeprojectintro'
'/api/changeprojectinvestmentstage'
'/api/changeprojectprivacy'
'/api/changeprojectreleventuser'
'/api/changeprojecttask'
'/api/changeprojectuserrole'
'/api/changetaskstatus'



'/api/changepwd'
'/api/changeroleacl'


/api/changedetail  直接返回+data

====================================================
'/api/deletefundfile'
'/api/deletelp'
'/api/deletemember'
'/api/deleteproject'
'/api/deleteprojectfile'
'/api/deleteprojectinvesthistory'
'/api/deleteprojectinvestmentstage'
'/api/deleteprojectmilestone'
'/api/deleteprojectnote'
'/api/deleteprojecttask'
'/api/deleteprojectuser'
'/api/deleterole'
'/api/deletetaskcomment'

一次删除多个?




======================================================
'/api/createfund'
'/api/createfundlp'
'/api/createlp'
'/api/createmember'
'/api/createproject'
'/api/createprojectinvesthistory'
'/api/createprojectinvestmentstage'
'/api/createprojectmileston'
'/api/createprojectnote'
'/api/createprojecttask'
'/api/createprojectuser'
'/api/createrole'
'/api/createtag'
'/api/createtaskcomment'

验证前移  接口合并  ?


=============================================================


'/api/queryfunddetail'
'/api/queryproject'
'/api/queryprojectcompanyinfo'
'/api/queryprojecttask'
'/api/queryroleacl'
'/api/quitproject'

'/api/listacl'
'/api/listcategory'
'/api/listfundfile'
'/api/listlp'
'/api/listmember'
'/api/listprojectfile'
'/api/listprojectinvesthistory'
'/api/listprojectinvestmentstage'
'/api/listprojectmilestone'
'/api/listprojecttask'
'/api/listprojectuser'
'/api/listrole'
'/api/listuserpopularproject'
'/api/listuserproject'
'/api/listusertask'


=============================================

'/api/uploadavatar'
'/api/uploadcommentfile'
'/api/uploadprojectfile'
'/api/uploadtaskfile'


helper文件更改


==============================================
'/api/getacl'
'/api/login'
'/api/logout'

xsrf+


=====================
'/api/attachment/'
'

'/api/followprojecttask'



=========================================





admin/manager 移除的时候判断count


=================================================3.9晚上线======================================
101.200.130.122
su dev
screen  R/r
ctrl+a ctrl+c
ctrl+a *2
============
apt- update

install git fail2ben
===============
adduser
passwd -l XXX

pip env
===============
mkvirtualenv -python=/
workon XXX
pip instal -r require
curl nvm (google)
npm run build
==============
apt-get supervisor
cd /etc/superivisor/config.d
ln -s XXXX   .
supervisorctl
updata
reread

==============
.bashrc
$APP_Config=""
写到.bashrc

source /usr/local/bin/virtualenvwrapper.sh
export APP_CONFIG="/home/app/config/Even.cfg"



==================================3.10=======================================

FO in user_view [/home/xy/workspace/gitRespority/basesite/blueprints/system_bp/user_view.py:1212]:
(pymysql.err.IntegrityError) (1451, 'Cannot delete or update a parent row: a foreign key constraint fails (`even`.`lp_model`, CONSTRAINT `lp_model_ibfk_2` FOREIGN KEY (`manager_id`) REFERENCES `company_user_model` (`id`))') [SQL: 'DELETE FROM company_user_model WHERE company_user_model.id = %(id)s'] [parameters: {'id': 3}]
--------------------------------------------------------------------------------
2017-03-10 12:46:57,226 INFO sqlalch

bug: when we delete a member if the member is a manger in LP




===============================3.11================================
1 资料迁移

2   source
    .
    ./
    三者区别,第一个在当前进程执行,可以通过$$查看pid
    剩下的另开一个进程

3   export用于设置环境变量,但仅用于本次修改
        所以还要加到bashrc当中
4
