# dbox

文件快递柜：匿名储存文件，通过取件码获取文件。

此处为网页端源码，实际效果请访问 [演示站点](https://dbox.cf/)
项目效果可以直接用 https://dbox.cf ，底部有项目链接，下面说下有什么不一样：

1. 无需数据库

FileCodeBox 用了 sqlite，我的第一版改用了 redis，不过后来发现其实不用外部数据库也可以，所以当前第二版完全取消外部数据库，

2. 无文件大小上传限制

通过分片上传实现了无文件大小限制，但目前一些地方为了方便写死了 100MB，需要后面增加可配置选项。

3. 无记名会员卡

通过 jwt 密钥和图种的原理实现了无记名会员卡，在上传的时候选择包含密钥的 会员卡.jpg 文件并点击使用，就可以升级为会员了。

会员上传限制从 100MB 增加到了 1G，这个也是为了方便，目前写死了 1G 限制和 10 次有效期，需要后面增加可配置选项。


目前项目仍然是处于非常早期的状态，算是提前发布吧，不建议正式使用，目前主要还是在找 bug、完善阶段。


使用方法：

虽然不建议正式使用，但对于想要测试的人，这里写下简单的部署方法。项目分为前端 dbox 和后端 fbox（d 在 f 前面 ）。

1. 下载部署前端文件

从 dbox 项目 release 中下载  dbox.tar.gz ，解压放在网站根目录。

2. docker 部署后端

放下镜像地址：

docker pull ghcr.io/veoco/fbox:latest


懂得都懂，就不细写了，目前只有一个 SECRET_KEY 环境变量是必须的，可以是任何字符串。

默认文件保存在 /app/data 和 /app/logs。

3. 配置反代

需要将 /api/ 反代到后端，nginx 示例：

location /api/ {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_pass http://127.0.0.1;
}


完成。
