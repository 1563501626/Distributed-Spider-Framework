# Distributed-Spider-Framework

<ul><li><i>manager/auto_update.py</i>：该文件为自动更新文件，需要在<i>config.py</i>文件中配置相应的参数，直接运行此文件会从数据库查询即将更新的脚本并运行。</li></ul>
<ul><li><i>manager/engine.py</i>：该文件为引擎文件，负责控制数据的走向，与其他组件保持低耦合度，具有良好的扩展性，只需将扩展程序放入main函数即可注册到项目流程中。</li></ul>
<ul><li><i>manager/run.py</i>：该文件为项目启动文件，（1）通过命令行运行：<strong>python run.py path [m/w] 1</strong>；（2）通过配置该文件中的相应的config参数运行；path：需要运行的相对路径，m：开启生产模式，w：开启消费模式，async_num：本次爬虫开启的并发数。</li></ul>
<ul><li><i>spider</i>：该文件夹下为编写的爬虫脚本。</li></ul>
<ul><li><i>sub/db.py</i>：入库组件，负责对数据的入库、更新和查询。</li></ul>
<ul><li><i>sub/pipeline.py</i>：rabbitmq组件，负责对生产的<i>message</i>进行持久化存储以供下次消费。</li></ul>
<ul><li><i>sub/spiders.py</i>：下载组件，负责发起请求下载并生成<i>response</i>对象。</li></ul>
<ul><li><i>tools</i>：该文件夹下为各种工具脚本。</li></ul>
环境配置：python==3.8, pika==1.1.0

***
**2020-11-25更新**  

- 修复pika多个线程共用一个rabbitmq连接而导致'pop from an empty deque'的问题
- 修复windows下aiohttp不支持https代理的问题
- 因业务需要，本框架支持scrapy下的原生爬虫代码运行，只需要替换继承的基类，以及启动方式有所改变，还在持续更新中...，详见[SpiderFrame/spider_code/example](./spider_code/example)
- 启动方式更新，详见[SpiderFrame/spider_code/example](./spider_code/example)
- 重写scrapy.spiderloader.SpiderLoader，每个爬虫name属性绑定，当前目录下的所有文件name为唯一，不得出现重复name
- 增加优先级队列，通过priority_queue=True属性开启优先级队列，每个请求通过priority指定当前优先级，详见[SpiderFrame/spider_code/example/priorityExample.py](./spider_code/example/priorityExample.py)
- 当队列消费完后，程序不会自动停止，新增自动删除空队列的程序，详见[SpiderFrame/tools/rabbitmq_client_test.py](./tools/rabbitmq_client_test.py)