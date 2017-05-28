# rongyun_binding
帮你在帐号系统中集成融云, 主要是将融云token和帐号绑定的逻辑

# 用法
前提, 你得用Sqlalchemy作为数据库操作的Orm, 初始化的时候也是注入你项目的的Sqlalchemy环境到包里, 你要是用别的Orm可以尝试自己fork自己的版本

## 1. 安装
    
    pip install rongyun_binding

## 2. 注入Sqlalchemy的环境

    engine = create_engine('postgresql+psycopg2://xxx:@localhost/rongbing', convert_unicode=True, echo=True)
    db = scoped_session(sessionmaker(autocommit=False, autoflush=False, bind=engine))
    class DeclaredBase(object):
        id = Column(Integer, primary_key=True, autoincrement=True)
    Base = declarative_base(cls=DeclaredBase)
    #主要是下面两行, 上面的都是你自己项目的, 也许会有很大区别
    from rongyun_binding import RongService, bind_models
    bind_models(Base, db)
    
## 3. 初始化service, 注入融云app_key和app_secret

    RongService.initialize('key', 'sec')
    
## 4. 调用更新融云token

    rong_id, rong_token = RongService.instance().update_token(1, 233, '123123', user_name="test123123")
