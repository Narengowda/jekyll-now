---
layout: Better pyramid logging with python and log configurations
title: Better pyramid logging with python and log configurationsYou're up and running!
date: 2014-03-03 05:30:00 +0530
---
We often log like below mentioned code snippet, but there is a better way to do it !!

    import logging
    import logging.handlers
    
    LOG_FILENAME = 'testing.log'
    
    # Set up a specific logger with our desired output level
    my_logger = logging.getLogger('agentlogger')
    
    # Add the log message handler to the logger
    handler = logging.handlers.RotatingFileHandler(LOG_FILENAME, maxBytes=2000, backupCount=100)
    
    # create a logging format
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    handler.setFormatter(formatter)
    
    my_logger.addHandler(handler)
    
    my_logger.debug('debug message')
    my_logger.info('info message')
    my_logger.warn('warn message')
    my_logger.error('error message')
    my_logger.critical('critical message')

What if you have many modules/ crons. where in you need to log them separately ?

the simple way is [https://docs.python.org/2/library/logging.config.html#logging.config.fileConfig](https://docs.python.org/2/library/logging.config.html#logging.config.fileConfig "https://docs.python.org/2/library/logging.config.html#logging.config.fileConfig")

Read this [https://docs.python.org/2/library/logging.config.html#logging-config-fileformat](https://docs.python.org/2/library/logging.config.html#logging-config-fileformat "https://docs.python.org/2/library/logging.config.html#logging-config-fileformat") to know logging configuration

If your web framework is pyramid, you can use development.ini file itself

Edit: Learnt that you can log method/ function entry exit actions using decorators also

eg: [http://www.wellho.net/resources/ex.php4?item=y212/dec01](http://www.wellho.net/resources/ex.php4?item=y212/dec01 "http://www.wellho.net/resources/ex.php4?item=y212/dec01")

simillary use class level decoration to log method entry exit actions

Eg: logger section in my pyramid app

    [logger_alembic]
    level = INFO
    handlers = console
    qualname = alembic
    
    [logger_eta_c]
    level = DEBUG
    handlers = eta
    qualname = eta_c
    
    [logger_vp_cron]
    level = DEBUG
    handlers = vpc
    qualname = vp_cron
    
    [logger_rp_cron]
    level = DEBUG
    handlers = rpc
    qualname = rp_cron
    
    [handler_console]
    class = StreamHandler
    args = (sys.stderr,)
    level = NOTSET
    formatter = generic
    
    [handler_eta]
    class = handlers.TimedRotatingFileHandler
    args = ('logs/eta.log', 'midnight', 1, 5, 'utf-8')
    level = DEBUG
    formatter = generic
    
    [handler_vpc]
    class = handlers.TimedRotatingFileHandler
    args = ('logs/vp_cron.log', 'midnight', 1, 5, 'utf-8')
    level = DEBUG
    formatter = generic
    
    [handler_rpc]
    class = handlers.TimedRotatingFileHandler
    args = ('logs/route_path_cron.log', 'midnight', 1, 5, 'utf-8')
    level = DEBUG
    formatter = generic
    
    [handler_filelog]
    class = handlers.TimedRotatingFileHandler
    args = ('logs/zta_console.log', 'midnight', 1, 5, 'utf-8')
    level = DEBUG
    formatter = generic
    
    [formatter_generic]
    format = %(asctime)s %(levelname)-5.5s [%(name)s][%(threadName)s] %(message)s