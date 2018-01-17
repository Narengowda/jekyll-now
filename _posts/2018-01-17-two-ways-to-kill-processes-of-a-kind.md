---
title: Two ways to kill processes of a kind
undefined: 'Handy solution to grep and kill process by process name.  When Celery
  workers are not properly shutdown their will be background celery workers running
  and they needs to be killed. Eg:'
created_date: 2018-01-17 00:00:00 +0530
date: 2018-01-17 21:58:55 +0000
---
Handy solution to grep and kill process by process name.

When Celery workers are not properly shutdown their will be background celery workers running and they needs to be killed. Eg:

    ps -ef | grep celery | awk {'print $2'} | xargs kill

Or you can install “htop”

> sudo yum install htop
>
> sudo apt-get install htop

Along with procesess you can now see memory/cpu/pid information press F4 to filter processess based on keyword, then press Enter and press F9 to kill.