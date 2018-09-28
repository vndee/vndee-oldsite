---
published: true
title: Cài đặt Microsoft SQL Server trên Ubuntu 18.04 LTS
date: '2018-09-24 02:57:18 +0700'
comments: true
categories:
  - database
tags: database
author: Duy V. Huynh
lang: VI
description: >-
  Cài đặt Microsoft SQL Server trên hệ điều hành Ubuntu 18.04 LTS Bionic Beaver
  Linux.
mathjax: true
---

Microsoft SQL Server là hệ thống database được phát triển bởi Microsoft và đừng sử dụng rộng rãi. Microsoft SQL Server được open-source và năm 2016. Bài viết này sẽ hướng dẫn cách cài đặt Microsoft SQL Server trên hệ điều hành Linux 18.04 LTS Bionic Beaver Linux. Do đến thời điểm của bài viết này, Microsoft vẫn chưa chính thức tung ra bản Microsoft SQL Server cho Ubuntu 18.04 cho nên chúng ta sẽ dùng bản dành cho Ubuntu 16.04

### Tạo bộ cài đặt 
Tạo folder để chứa bộ cài đặt của Microsoft SQL Sever:
```shell
cd ${HOME} && mkdir -p tmp/mssql/newpkg/DEBIAN/ && cd tmp/mssql
```
Tải xuống phiên bản mới nhất của SQL Server dpkg (dpkg SQLServer date: 20-Jun-2018 18:03):
```shell
wget https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb
```
Phân giải các dpkg package:
```shell
dpkg-deb -x mssql-server_14.0.3029.16-1_amd64.deb newpkg/
dpkg-deb -e mssql-server_14.0.3029.16-1_amd64.deb newpkg/DEBIAN/
```
Bước tiếp theo là thay đổi phiên bản OpenSSL có sắn trên Ubuntu để tránh đụng độ với Microsoft SQL trong quá trình cài đặt:
```shell
sed -i -e 's#openssl (<= 1.1.0)#openssl (<= 1.1.0g-2ubuntu4.1)#g' newpkg/DEBIAN/control
cat newpkg/DEBIAN/control | grep openssl
```
Cuối cùng là đóng gói bộ cài đặt thành file `*.deb`:
```shell
sudo dpkg-deb -b newpkg/ 18.04-mssql-server_14.0.3029.16-1_amd64.deb
```  

Sau bước này, chúng ta có thể dùng file `18.04-mssql-server_14.0.3029.16-1_amd64.deb` để thực hiện cài đặt. Tuy nhiên vẫn có thể xảy ra lỗi do thiếu dependencies.

### Kiểm tra và cài đặt các dependencies bị thiếu
Cách dễ nhất để kiểm tra xem dependency nào bị thiếu đó là thử cài đặt file `*.deb`:
```shell 
sudo dpkg -i 18.04-mssql-server_14.0.3029.16-1_amd64.deb
```
Sau khi thực hiện lệnh trên, output sẽ chỉ ra những dependency nào bị thiếu. Ví dụ:
```shell
Selecting previously unselected package mssql-server.
(Reading database ... 368673 files and directories currently installed.)
Preparing to unpack 18.04-mssql-server_14.0.3029.16-1_amd64.deb ...
Unpacking mssql-server (14.0.3029.16-1) ...
dpkg: dependency problems prevent configuration of mssql-server:
 mssql-server depends on libjemalloc1; however:
  Package libjemalloc1 is not installed.
 mssql-server depends on libc++1; however:
  Package libc++1 is not installed.
 mssql-server depends on libsss-nss-idmap0; however:
  Package libsss-nss-idmap0 is not installed.
 mssql-server depends on gawk; however:
  Package gawk is not installed.

dpkg: error processing package mssql-server (--install):
 dependency problems - leaving unconfigured
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for man-db (2.8.3-2) ...
Errors were encountered while processing:
 mssql-server
```

Bây giờ để fix lỗi đó, ta có thể dùng câu lệnh:
```shell
sudo apt install -f
```
Câu lệnh này sẽ cố gắng cài tất cả dependency được yêu cầu của bộ cài đặt nhưng vẫn sẽ có khả năng một số dependency bị bỏ qua. Do đó chúng ta vẫn cần xác định những dependency còn lại bị thiếu đề cài cho đủ. Sau khi chạy lệnh trên, ta thử chạy lại file `*.deb` để xem bây giờ đã đủ dependency hay chưa. Nếu chưa thì cần phải cài đặt thủ công cho từng dependency.

### Cấu hình sau khi cài đặt
Khi cài đặt thành công, ta sẽ nhận được thông báo:
```shell
(Reading database ... 368947 files and directories currently installed.)
Preparing to unpack 18.04-mssql-server_14.0.3029.16-1_amd64.deb ...
Unpacking mssql-server (14.0.3029.16-1) over (14.0.3029.16-1) ...
Setting up mssql-server (14.0.3029.16-1) ...
Locale en_GB not supported. Using en_US.

+--------------------------------------------------------------+
Please run 'sudo /opt/mssql/bin/mssql-conf setup'
to complete the setup of Microsoft SQL Server
+--------------------------------------------------------------+

Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for man-db (2.8.3-2) ...
```
Để hoàn thành việc câu hình SQL Server, chạy lệnh và làm theo hướng dẫn của chương trình:
```shell
sudo /opt/mssql/bin/mssql-conf setup
```

Thêm đường dẫn đến thư mục bin của SQL server vào PATH variable:
```shell
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
```

### Cài đặt MS SQL tools và unixODBC plugin
Để tiện dụng ta có thể cài đặt mssql-tool:
```shell
sudo apt-get update 
sudo apt-get install mssql-tools unixodbc-dev
```
Để các ứng dụng và tool kết nối được với MS SQL, ta cần ở port 1433:
```shell
sudo ufw allow 1433/tcp
sudo ufw allow 1433/udp
```
Dùng lệnh sau để kiểm tra xem port 1433 đã được mở thành công hay chưa:
```shell
netstat -na | grep 1433
```
Sau khi cài đặt xong, để kiểm tra xem sql service đã chạy hay chưa, ta thực hiện lệnh:
```shell
systemctl status mssql-server
```

### Cài đặt Azure Data Studio
Nếu như trong Windows ta có SQL Server Management Studio thì trong Linux cũng có Azure Data Studio với những chức năng tương tự. Để cài đặt Azure Data Studio, trước tiên ta cần cài đặt một số gói cần thiết:
```shell
sudo apt-get install gconf-service gconf-service-backend gconf2-common libgconf-2-4
```
Download gói cài đặt Azure Data Studio tại đây: [Azure Data Studio released note]( https://docs.microsoft.com/en-gb/sql/azure-data-studio/download?view=sql-server-2017)
Màn hình làm việc của Azure Data Studio
![Azure Data Studio](https://lh3.googleusercontent.com/_A45Q8irYdlR-ThTdsPtNdPwMLwUpO3rBztgOz0yKNYjXtU2AWEUFzMGtBnnPHH8BFr7EXeewzs7gBrwe7ID27LbvrUmDfv-TtQXrkETE2YbLvNvk0qGvN3UTVGi0RhlwMoIG0UzhSCeHLOWx1S9fUEX3LkN_Zlz7a2w2KMT8MJAcQytOEOoMnMx7KP3EaBR1HSlr1x2QAXYmo_jDi6afKaK3U6nT9WuWeAug0ECJaJCRR0CWQn9ewzLGB1WxZcHvzLLPMYU_yjFpa4juNGihNN8CSjCbtXD8YCZowShV7kEx9ZeopjaV9Y7Mk1SRzmbc1d8wIn2SfV4XBs6AFnAr7jeEXhmVVQ-x34T8lX4iCzSC4rwyA_FchoUqQmWB6NWbPxObEqDeuJA29PSWElF7gcNjAbYw5Rhrlx_lMT5QfK7WbIAntsG9vYJPaBuYoJcgNsFKrdqXxD_dIMDuNscLk2r3QKAXxhcGrNzJzkGR9I-2Doxp5clRnKEl8LwseTN8zcZCtjd8VnBMhO9Z3A4gpSdZyVlBavJHEcTx9TsC4g0J_M8MIdtVSmINkbEp2ZC-RPNN1Ir-b9ZDD2-jke9-etdj3LBE-ldPHHgHOK9NnPwXYNZ0BHNYDPwm0xcxQSu4qWwzsSW3RHcv_7icnXxt7385VtGe8bO_aRZ9N4HI3BhOaSQJjKRVuxR=w1190-h669-no)
