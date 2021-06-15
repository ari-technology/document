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

## _Setting new item project
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

## Plugins

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:

```sh
node app
```

Second Tab:

```sh
gulp watch
```

(optional) Third:

```sh
karma test
```

#### Building for source

For production release:

```sh
gulp build --prod
```

Generating pre-built zip archives for distribution:

```sh
gulp build dist --prod
```

## Docker

Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the
Dockerfile if necessary. When ready, simply use the Dockerfile to
build the image.

```sh
cd dillinger
docker build -t <youruser>/dillinger:${package.json.version} .
```

This will create the dillinger image and pull in the necessary dependencies.
Be sure to swap out `${package.json.version}` with the actual
version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on
your host. In this example, we simply map port 8000 of the host to
port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart=always --cap-add=SYS_ADMIN --name=dillinger <youruser>/dillinger:${package.json.version}
```

> Note: `--capt-add=SYS-ADMIN` is required for PDF rendering.

Verify the deployment by navigating to your server address in
your preferred browser.

```sh
127.0.0.1:8000
```

## License

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
