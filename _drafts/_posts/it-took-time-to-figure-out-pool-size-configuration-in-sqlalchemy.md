---
title: It took time to figure out pool size configuration in SQLalchemy
undefined: Description
created_date: 2018-01-17 00:00:00 +0530
date: 2018-01-17 22:00:52 +0000
---
Pool size in sqlalchmey can be configured while creating engine, or if you are encountering below errors ?

>     in _do_get#012    (self.size(), self.overflow(), self._timeout))#012TimeoutError: QueuePool limit of size 5 overflow 10 reached, connection timed out, timeout 30
>     

All you have to do is

>     engine = create_engine('postgresql://me@localhost/mydb',pool_size=20, max_overflow=0)
>     

But what if you are creating engine using config ? (pyramid)

>     config = Configurator(settings=settings)
>     engine = engine_from_config(settings, prefix='sqlalchemy.')
>     config.registry.dbmaker = sessionmaker(bind=engine)
>     

    https://github.com/zzzeek/sqlalchemy/blob/master/lib/sqlalchemy/engine/init.py#L376
    

in your development.ini

>     sqlalchemy.url = postgresql://postgres:password@localhost/your_db
>     sqlalchemy.pool_size = 10
>     sqlalchemy.max_overflow = 15
>     