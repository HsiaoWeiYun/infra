# Cache-Redis

## 編譯&安裝Redis
1. 安裝一些好用的工具
    yum -y update && yum -y install nano vim wget net-tools
2. 從官方網站下載原始碼
    wget http://download.redis.io/releases/redis-5.0.8.tar.gz && tar -zxvf redis-5.0.8.tar.gz
3. redis資料夾下安裝編譯相依包 <br>
    yum -y install cpp binutils glibc glibc-kernheaders glibc-common glibc-devel gcc make tcl <br>
4. 編譯: make MALLOC=libc <br>
5. 測試: make test <br>
6. 安裝: make install
7. 檢查/usr/local/bin 下是否有 redis-* 工具, 若有則安裝完成.
***
## 複製並編輯redis.conf
1. 複製redis.config檔案至/usr/local/bin 路徑下
2. 將bind字段改為 bind 127.0.0.1 <欲監聽網卡的ip, 通常會是區網那張>   (表示127.0.0.1以及區網可以連線)
3. 將requirepass改為redis想要的密碼, 如requirepass 123123
4. 設定redis最多可以吃多少記憶體, maxmemory 6.5g 代表最多吃6.5g (這邊需要預留一些記憶體給os用)
***
## 啟動redis
redis-server ./redis.config
