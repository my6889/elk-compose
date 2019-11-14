# 使用docker-compose部署带有认证的ELK

## 准备环境

* [Docker]( [https://docs.foofish.cn/2019/05/06/Docker%E5%B0%8F%E6%8A%80%E5%B7%A7/](https://docs.foofish.cn/2019/05/06/Docker小技巧/) )
* [Docker-compose]( [https://docs.foofish.cn/2019/05/06/Docker%E5%B0%8F%E6%8A%80%E5%B7%A7/](https://docs.foofish.cn/2019/05/06/Docker小技巧/) )
* Ubuntu 18.04 或 CentOS7 (推荐)

---

## 部署指南

**克隆项目**

```
git clone https://github.com/my6889/elk-compose.git
```

**设置宿主机内核参数**

```
echo vm.max_map_count=655360 >> /etc/sysctl.conf
sysctl -p
```

**第一次启动**

```
cd elk-compose
docker-compose up
# 此时可以先忽略kibana、logstash连接ES的一些报错信息，然后再打开一个窗口设置认证密码。
```

**设置认证密码**

```
docker exec -ti elasticsearch /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
# 记住设置的密码，建议全部设置一样
```

**修改kibana配置中的密码**

```
sed -i "s/123456/yourpassword/g" kibana/config/kibana.yml
```

**修改logstash配置中的密码**

```
sed -i "s/123456/yourpassword/g" logstash/config/logstash.conf
sed -i "s/123456/yourpassword/g" logstash/config/logstash.yml
```

**修改Logstash输入输出规则**（选做）

```
vim logstash/config/logstash.conf
```

**重新创建容器**

```
docker-compose down && docker-compose up -d 
```

**访问服务**

```
http://宿主机IP:5601
```

<font color=#FF0000 >**完全移除**</font> 

```
# 慎重操作
docker-compose down -v 
rm -r elk-compose
```

---

## 项目说明

* 所有ELK组件使用官方的7.4.2版本
* 项目中的三个文件夹中的配置会挂载到容器中
* ES的数据卷挂载到了volume，可以通过`docker volume ls`命令看到

