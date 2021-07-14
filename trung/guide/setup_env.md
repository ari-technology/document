## DEVOPS BEGINNER

| Plugin | README |
| ------ | ------ |
| Create Ec2 run jenkins machine | [https://aws.amazon.com/ec2/getting-started/] |
| Install nginx in ec2 linux | [plugins/github/README.md][PlGh] |
| Install git | [plugins/googledrive/README.md][PlGd] |
| Install jenkins | [plugins/onedrive/README.md][PlOd] |
| Setting webhook on github | [plugins/medium/README.md][PlMe] |
| Setting new item project | [plugins/googleanalytics/README.md][PlGa] |
| Setting ssh from jenkins ec2 to webserver ec2 | [plugins/onedrive/README.md][PlOd] |
| Setting nginx mapping domain | [plugins/onedrive/README.md][PlOd] |
| Install  SSL | [plugins/onedrive/README.md][PlOd] |
| Setting Route53 | [plugins/onedrive/README.md][PlOd] |
| Create AMIs | [plugins/onedrive/README.md][PlOd] |

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
$ less /var/lib/jenkins/secrets/initialAdminPassword
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

## _Setting nginx mapping domain_
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
