# 使用docker-compose部署带有认证的ELK

## 准备环境

* Docker
* Docker-compose 

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

**先启动elasticsearch**

```
cd elk-compose
docker-compose up -d elasticsearch
# 此时先启动elasticsearch，然后设置认证密码。
```

**设置认证密码**

```
docker exec -ti elasticsearch /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
# 记住设置的密码，建议全部设置一样
```

**修改kibana配置中的密码**

```
# 修改配置中的密码为刚才设置的密码
sed -i "s/123456/yourpassword/g" kibana_config/kibana.yml
```

**修改logstash配置中的密码**

```
# 修改配置中的密码为刚才设置的密码
sed -i "s/123456/yourpassword/g" logstash_config/logstash.conf
sed -i "s/123456/yourpassword/g" logstash_config/logstash.yml
```

**修改Logstash输入输出规则**（选做）

```
vim logstash_config/logstash.conf
```

**启动所有容器**

```
docker-compose up -d
# 刚才Elasticsearch已经启动，执行此命令后会把Logstash和Kibana容器也启动。
```

**访问服务**

```
http://宿主机IP:5601
```

---

## 项目说明

* 所有ELK组件使用官方的7.17.1版本
* 项目中的三个文件夹中的配置会挂载到容器中
* `ES_JAVA_OPTS`默认设置为`-Xms1024m -Xmx1024m`，可按需修改
* 如果容器无法启动，提示目录权限问题，请调整容器数据持久化目录的权限

