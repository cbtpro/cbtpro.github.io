总结下一个建议的devops搭建流程，帮助理解DevOps。

选择jenkins，虽然他比较老牌，也有很多的替代品，正因为他是一个老牌工具，且还在持续更新中，社区版本还免费。

使用中文官网 https://www.jenkins.io/zh/

阅读安装文档 https://www.jenkins.io/zh/doc/book/installing/

选择社区版本安装

```shell
docker run \
  -u root \
  --rm \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v $HOME/jenkins:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean \
  --name jenkins-blueocean
```

```shell
docker run \
  -u root \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v $HOME/jenkins:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins-blueocean \
  jenkinsci/blueocean
```



```shell
docker exec -it jenkins-blueocean bash
```

```
docker logs jenkins-blueocean
```

```
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

588c208f36094d6aa361701d330914d0

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
```

稍等一会访问http://localhost:8080/等待初始化后从docker镜像里获取上面的密钥填入页面中。

可以先按推荐插件安装，后续进入系统可以手动安装。

登录上去后立即修改密码。

实例配置设置Jenkins URL，因为jenkins要给其他的服务使用到，所以最好设置成域名，如果本机IP固定，也可以设置成IP，其他服务都在本机，就可以设置成localhost。因为我马上就要关机回家了，所以我暂时先不修改。



新建流水线

https://github.com/settings/tokens/new



```
docker start jenkins-blueocean
docker stop jenkins-blueocean
docker restart jenkins-blueocean
```

