## DEVOPS BEGINNER

| STT | CONTENT |
| ------ | ------ |
| 1 |Create Ec2 run jenkins machine|
| 2 |Install nginx in ec2 linux|
| 3 |Install git|
| 4 |Install jenkins| 
| 5 |Setting webhook on github|
| 6 |Setting new item project| 
| 7 |Setting ssh from jenkins ec2 to webserver ec2|
| 8 |Setting nginx mapping domain|
| 9 |Install  SSL| 
| 10 |Setting Route53|
| 11 |Create AMIs|  

# Architecture CI/CD (Continuous Integration, Continuous Delivery)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_19.png)

# Jenkins Install
## _Create Ec2 run jenkins machine_
> Note: `--open port 8080` in security group.

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_1.png)

## _Install nginx in ec2 linux_
```sh
Install nginx: <https://www.youtube.com/watch?v=leCZ7htfB_g>
$sudo su -
$sudo yum update
$sudo nano /etc/yum.repos.d/nginx.repo
add to file nginx.repo :
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
$sudo yum update
$sudo yum install nginx
$sudo service nginx start
```
## _Install git_
```sh
$sudo yum install git
$git --version
```
## _Install jenkins_:
https://www.youtube.com/watch?v=v7tLaDJ-uqg&list=PLjCpH2Qpki-vDvSxypCxOgfjuVHaXxcaa&index=2
```sh
1. Java installation
$ yum install -y java-1.8.0-openjdk-devel.x86_64
$ alternatives --config java -> input 1 -> enter
$ java -version
2. Jenkins install
$ wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
$ rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
$ yum install -y jenkins
3. Start Jenkins
$ systemctl start jenkins
4. Setting password
$ cat /var/lib/jenkins/secrets/initialAdminPassword
5. login
user : admin
pass : admin
6. http://54.169.68.172:8080/
```
Install jenkins at http://54.169.68.172:8080/
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_2.jpg)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_3.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_4.jpg)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_5.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_6.jpg)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_7.jpg)

## _Setting webhook on github_
```sh
http://18.139.163.216:8080/github-webhook/
```
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_8.png)

## _Setting new item project_
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_9.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_10.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_11.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_12.png)
## Command
```sh
ssh -T -i /home/jenkins/web-server.pem -o StrictHostKeyChecking=no ec2-user@3.1.145.148 << EOF
sudo su -
cd /usr/share/ari-core/angular_uat/ARI-CORE-ANGULAR
sudo git pull origin deployment/uat-webapp-vn
sudo npm i
sudo rm -rf dist
sudo ng build
EOF
```
## _Setting ssh from jenkins ec2 to webserver ec2_
```sh
- Create folder jenkins
$ sudo mkdir /home/jenkins
$ cd /home/jenkins
$ sudo nano web-server.pem
- copy content private key ec2 web-server in web-server.pem
$ chmod 777 web-server.pem
```

## _Install ec2 webapp & Setting nginx mapping domain_
### Install node & npm
```sh
- curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash - 
- sudo yum install nodejs
- node --version
- npm --version
```
### Install angular
```sh
- sudo npm install -g @angular/cli@9.0.4
```
### Config with http
```sh
server {
    listen       80;
    server_name  _;

    location / {
         root /usr/share/ari-core/demo-ari;
        index  index.html index.htm;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}
```
### Git clone 
```sh
ssh-keygen
add public key to github
ssh-agent bash -c 'ssh-add /root/.ssh/id_rsa; git clone --branch deployment/staging git@github.com:ari-technology/demo-webapp-angular.git'
npm i
ng build
```
### Config with https
```sh
$ sudo nano /etc/nginx/conf.d/default.conf
server {
        listen 80 default_server;

        server_name _;
        return 301 https://$host$request_uri;

}
server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/webserver.cert;
        ssl_certificate_key /etc/nginx/cert/webserver.key;

        server_name ari-webserver.click;

        location / {
                root /usr/share/ari-core/angular/ARI-CORE-ANGULAR/dist/Corev2;
                try_files $uri $uri/ /index.html?/$request_uri;
        }
}

server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/uat.cert;
        ssl_certificate_key /etc/nginx/cert/uat.key;

        server_name shop-thermomix-sg.click;

        location / {
                root /usr/share/ari-core/angular-uat/ARI-CORE-ANGULAR/dist/Corev2;
                try_files $uri $uri/ /index.html?/$request_uri;
        }
}
```
## _SSL là gì_
SSL là viết tắt của Secure Sockets Layer, một công nghệ tiêu chuẩn cho phép thiết lập kết nối được mã hóa an toàn giữa máy chủ web (host) và trình duyệt web (client).
Kết nối này đảm bảo rằng dữ liệu được truyền giữa host và client được duy trì một cách riêng tư, đáng tin cậy.
## _Tầm quan trọng của SSL_
SSL giúp bảo vệ những thông tin nhạy cảm khi chúng được truyền qua các mạng máy tính trên thế giới. Nó cung cấp sự riêng tư, bảo mật nghiêm ngặt và toàn vẹn cho dữ liệu của cả trang web và thông tin cá nhân của người truy cập.
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_20.png)
## _Install  SSL_: https://www.youtube.com/watch?v=ciFW75XtUOc&t=96s

```sh
Step1: Cài đặt mod ssl cho Apache
yum -y install mod_ssl

Step2:Cài đặt Cài đặt openssl
yum install openssl

Step3: Tạo private key
 mkdir -p /usr/share/nginx/conf/ssl
cd /usr/share/nginx/conf/ssl
openssl genrsa > server.key

Step4: Tiếp theo, tạo file CSR, file CSR này được sử dụng khi cơ quan cấp chứng chỉ cấp chứng chỉ cho máy chủ
openssl req -new -key server.key > server.csr

Step5: Gửi file server.csr ở trên cho nhà cung cấp SSL(nơi bạn muốn mua SSL) bạn sẽ nhận được các file cerfiticate
ví dụn mình dùng sslforfree để thực hành bài này : https://manage.sslforfree.com/

Step6: Copy các file cerfiticate nhận được vào thư mục và change quyền cho các file này
```
```sh
server {
        listen 80 default_server;

        server_name _;
        return 301 https://$host$request_uri;

}
server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/demo.cert;
        ssl_certificate_key /etc/nginx/cert/demo.key;

        server_name api-web-info-quan5-stag2a.com;

        location / {
                root /usr/share/ari-core/demo-web-ari/demo-webapp-angular/dist/Corev2/;
                try_files $uri $uri/ /index.html?/$request_uri;
        }
}
```

## _Setting Route53_
Manager domain and mapping beween domain with nginx.

1. Register new domain
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_13.png)
2. Create record
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_14.png)
3. Check result
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_15.png)

## _Create AMIs_
Create image to backup ec2.
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_17.png)
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/img_18.png)

## _Install PM2_
https://pm2.keymetrics.io/docs/usage/quick-start/

```sh
$npm install pm2@latest -g
$sudo pm2 install typescript
$sudo pm2 install @types/node
```

## _Update nginx_
```sh
upstream customer_be_8888 {
        ip_hash;
        server 127.0.0.1:8888;
}

server {
        listen 80 default_server;

        server_name _;
        return 301 https://$host$request_uri;

}
server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/webserver.cert;
        ssl_certificate_key /etc/nginx/cert/webserver.key;

        server_name ari-webserver.click;

        location / {
                root /usr/share/ari-core/angular/ARI-CORE-ANGULAR/dist/Corev2;
                try_files $uri $uri/ /index.html?/$request_uri;
        }
}

server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/uat.cert;
        ssl_certificate_key /etc/nginx/cert/uat.key;

        server_name shop-thermomix-sg.click;

        location / {
                root /usr/share/ari-core/angular-uat/ARI-CORE-ANGULAR/dist/Corev2;
                try_files $uri $uri/ /index.html?/$request_uri;
        }
}

server {
        listen 443 ssl;

        ssl on;
        ssl_certificate /etc/nginx/cert/webserver.cert;
        ssl_certificate_key /etc/nginx/cert/webserver.key;

        server_name api-ari-webserver.click;

        location / {
                # trailing slash is key
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_pass http://customer_be_8888;
        }


}
```

### _Config jenkins deploy backend_
```sh
ssh -T -i /home/jenkins/web-server.pem -o StrictHostKeyChecking=no ec2-user@3.1.145.148 << EOF
sudo su -
cd /usr/share/ari-core/backend/ARI-CORE-NESTJS/
git pull origin deployment/stag2a
npm i
export NODE_ENV=local
pm2 kill
pm2 start npm --name "nestjs" -- start
```

### _Install postgre database in nginx_






## License
Version 1.0
**Trung.nb!**
