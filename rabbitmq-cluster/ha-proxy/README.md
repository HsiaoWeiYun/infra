# HAProxy

### 關閉防火牆
1. 關防火牆方便測試: systemctl stop firewalld.service
***
### 安裝工具
1. 安裝好用工具: yum -y update && yum -y install nano vim wget net-tools
***
### 設定/ect/hosts
1. 設定剛剛安裝好的2台rabbitmq集群host (rabbitmq1 rabbitmq2): vim /ect/hosts
2. ping rabbitmq1
3. ping rabbitmq2
***
### 安裝編譯依賴
1. yum -y install gcc pcre-static pcre-devel openssl-devel systemd-devel
***
### 下載並編譯haproxy
1. 下載haproxy: wget http://www.haproxy.org/download/2.0/src/haproxy-2.0.14.tar.gz && tar -zxvf haproxy-2.0.14.tar.gz
2. 進入haproxy-2.0.14資料夾, 裡面應該會有個makefile
3. 編譯: make TARGET=linux-glibc USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1 USE_CRYPT_H=1 USE_LIBCRYPT=1 USE_SYSTEMD=1
4. 安裝: make install
***
### 設定haproxy
1. 建立資料夾: mkdir -p /etc/haproxy /var/lib/haproxy
2. 建立軟連結: ln -s /usr/local/sbin/haproxy /usr/sbin/haproxy
4. 建立daemon服務: cp examples/haproxy.init /etc/init.d/haproxy && chmod 755 /etc/init.d/haproxy
5. 載入daemon配置文件: systemctl daemon-reload
6. 預設開啟daemon: chkconfig haproxy on
7. 複製haproxy.cfg 至 /etc/haproxy/ 路徑下
8. 檢查配置文件是否有誤: haproxy -c -f haproxy.cfg
9. 啟動haproxy: systemctl start haproxy
10. 訪問haproxy介面查看是否成功啟動
11. 查看是否成功能訪問到rabbitmq管理網頁
***
### 開啟防火牆
1. ***開啟port 5672 15672 1936***
2. ***僅允許特定內網ip區段連線***
3. ***MQ Cluster 以及與AP Server 之間的連線請走內網***
***
### 生產環境應有一套管理log的機制
