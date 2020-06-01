# RabbitMQ-Cluster

## 分別設定Host (兩台rabbitmq分別設定)
1. hostnamectl set-hostname rabbitmq1
2. hostnamectl set-hostname rabbitmq2
3. 分別編輯/etc/hosts 把rabbitmq1 rabbitmq2 的host都加進去
***
## 關閉防火牆 (方便測試)
systemctl stop firewalld.service

## 下載並安裝erlang and RabbitMQ (rabbitmq1 rabbitmq2)
1. 新增erlang倉庫與依賴: curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
2. 新增rabbitmq倉庫與依賴: curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
3. 安裝: yum -y install erlang && yum -y install rabbitmq-server
4. 將rabbit設為daemon service: chkconfig rabbitmq-server on
***
## 啟用管理網頁插件 (rabbitmq1 rabbitmq2)
1. 進入 /sbin路徑下, ll rabbitmq* 可看到mq相關工具
2. 啟動MQ: systemctl start rabbitmq-server.service
3. 啟用plugin: rabbitmq-plugins enable rabbitmq_management
4. 重啟MQ: systemctl restart rabbitmq-server.service
5. 新增管理帳號: rabbitmqctl add_user <帳號> <密碼>
6. 新增管理帳號權限 (default vhost所有讀寫權限): rabbitmqctl set_permissions -p "/" <管理者帳號> ".*" ".*" ".*"
7. 設定管理員標籤: rabbitmqctl set_user_tags <管理者帳號> administrator
8. 結束後可訪問管理後台: http://<ip>:15672/
***
## 設定rabbitmq cluster
1. 查詢rabbitmq1的erlang.cookie: cat /var/lib/rabbitmq/.erlang.cookie
2. 關閉rabbitmq2: systemctl stop rabbitmq-server.service
3. 將剛剛查到的.erlang.cookie 覆蓋rabbitmq2那台的 /var/lib/rabbitmq/.erlang.cookie
4. 重啟rabbitmq2: systemctl start rabbitmq-server.service
5. 在rabbitmq2那台初始化集群: 先進入/sbin/
6. 停止rabbitmq2 app: rabbitmqctl stop_app
7. 重設rabbitmq2: rabbitmqctl reset
8. 加入集群: rabbitmqctl join_cluster rabbit@rabbitmq1
9. Stop the entire RMQ server: rabbitmqctl stop
10. 重啟rabbitmq2: systemctl start rabbitmq-server.service
11. 進入管理網頁首頁查看集群節點狀態
***
## 設定rabbitmq.conf
1. 將rabbitmq.conf分別放入/etc/rabbitmq/
2. 分別重啟
***
## 開啟防火牆
1. **4369 - 內部服務發現使用**
2. **5672, 5671 - AMQP 0-9-1 and 1.0**
3. **25672, 35672~35682 - 內部節點, CLI Tool溝通**
4. **15672 - HTTP API clients, management UI and rabbitmqadmin (only if the management plugin is enabled)**
5. 61613, 61614 - STOMP clients (only if the STOMP plugin is enabled)
6. 1883, 8883 - MQTT clients
7. 15674 - STOMP-over-WebSockets clients (only if the Web STOMP plugin is enabled)
8. 15675 - MQTT-over-WebSockets clients (only if the Web MQTT plugin is enabled)
9. 15692 - Prometheus metrics
10. ***MQ Cluster以及與HA Proxy之間的溝通請一律走內網***
***
## HA Proxy 附載均衡
1. 請參照HA Proxy章節
***
## 生產環境應有一套管理log的機制

#### log path: /var/log/rabbitmq
#### log path: /var/log/messages
