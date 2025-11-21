☆☆☆所有命令请复制【】中全部内容。所有{}请替换为本机展现内容☆☆☆
☆☆☆所有命令请复制【】中全部内容。所有{}请替换为本机展现内容☆☆☆
☆☆☆所有命令请复制【】中全部内容。所有{}请替换为本机展现内容☆☆☆

一、拉取镜像：【docker pull saintvamp/auto_rss:latest】
二、创建容器：【docker run -d --network host \
--name=auto_rss \
-e TZ=Asia/Shanghai \
-v /{RSS的父目录地址}/auto_rss/log:/auto_rss/log \
-v /{RSS的父目录地址}/auto_rss/config:/auto_rss/config \
-v /{RSS的父目录地址}/auto_rss/database:/auto_rss/database \
saintvamp/auto_rss:latest】
#自行修改-v中冒号前的文件夹地址后，执行一次，此时程序未运行，只是生成容器。
三、修改base_config中config.toml中的内容。
四、源站点选择RSS，直接生成，查询对应的passkey字段



Q&A：
Q1：config里的model-config.toml是干什么的？
A1：这个是最全的配置文件模版。在第一次创建容器后，需要复制model-config.toml一份并改名为config.toml，然后配置config中的必备信息。
另外每次更新model后，重启时程序会自动更新model-config.toml，并将config.toml缺少的配置项补全，但仍需要人工配置此类配置项的值。

Q2：当前支持哪些源站，哪些目标站？
A2：源站支持【观众、季节、憨憨、家园、杜比、HDV、朋友、猫、织梦、优堡、reelflix、FL】
目标站支持【末日、象岛、优堡、麒麟、劳改所、织梦、柠檬、南工大】

Q3：配置中的RSS链接怎么填。
A3：从你想要发布的网站选择RSS后生成的链接中【是生成的链接中！！！不是控制面板的！！！】，只需要找到passkey对应的内容，比如原链接可能为【https://www.sv.com/rss?passkey=123321&cat=1&dl=true】那么passkey对应的就是【123321】


API说明：
PS:如果是curl命令要把调用方法整体引号引起，浏览器则无影响。
如：curl "http://127.0.0.1:56789/sv/rss/api/checkDetail?url=https://audiences.me/details.php?id=306417"

1.验证页面详情接口：checkDetail
功能：检查页面解析内容是否正常，主要修复BUG用
调用方法：http://127.0.0.1:56789/sv/rss/api/checkDetail?url={源站种子详情页}
源站种子详情页：https://audiences.me/details.php?id=306417

2.获取源种存储信息接口：getHashInfo
功能：查看源种存储内容是否正确，主要修复BUG用
调用方法：http://127.0.0.1:56789/sv/rss/api/getHashInfo?h={QB种子信息中的信息哈希值}

3.手工新增源站种子接口：addByUrl
功能：人工增加RSS队列中的种子资源，供DTU下载使用
调用方法：http://127.0.0.1:56789/sv/rss/api/addByUrl?h={QB种子信息中的信息哈希值}&url={源站种子详情页}&hr={源网站此种子的HR标记}&sf={站点架构}
源站种子详情页：https://audiences.me/details.php?id=306417
源网站此种子的HR标记：有标记写1，没标记写0
站点架构：目前支持两类，选一个填写：np|u3d

3.手工删除报错种子接口：delByUrl、delByHash
功能：人工增加RSS队列中的种子资源，供DTU下载使用
调用方法：http://127.0.0.1:56789/sv/rss/api/delByUrl?u={源站种子详情页}
调用方法：http://127.0.0.1:56789/sv/rss/api/delByHash?h={QB种子信息中的信息哈希值}
源站种子详情页：https://audiences.me/details.php?id=306417

4.本地简介信息相关接口：addDoubanM、selDouban、delDouban
新增接口调用方法：http://127.0.0.1:56789/sv/rss/api/addDoubanM，post传json数据{"douban":"链接","imdb":"链接","name":"中文名"}
查询接口调用方法：http://127.0.0.1:56789/sv/rss/api/selDouban?d={豆瓣或IMDB链接}
删除接口调用方法：http://127.0.0.1:56789/sv/rss/api/delDouban，post传json数据{"douban":"链接","imdb":"链接"}

5.查询待发布资源的名称和PTGEN对应情况接口：getNearlyInfo
调用方法：http://127.0.0.1:56789/sv/rss/api/getNearlyInfo?t={指定的时间范围}
指定的时间范围：填任意数字，单位小时

6.强制更新待发布资源的PTGEN信息接口：forceUpdateIntro
调用方法：http://127.0.0.1:56789/sv/rss/api/forceUpdateIntro?h={QB种子信息中的信息哈希值}