# 一键部署带有认证的ELK

## 部署指南

**克隆项目**

```
git clone https://github.com/my6889/elk-compose.git
```

**第一次启动**

```
cd elk-compose
docker-compose up
```

**设置认证密码（建议全部一样）**

```
docker exec -ti elasticsearch /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
# 记住设置的密码
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

---



## 项目说明

* 所有ELK组件使用官方的7.4.2版本
* 项目中的三个文件夹中的配置会挂载到容器中
* ES的数据卷挂载到了volume，可以通过`docker volume ls`命令看到

