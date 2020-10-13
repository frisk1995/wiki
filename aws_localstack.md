# AWSのサーバレスサービスをローカルで検証できる環境を構築する。
- https://qiita.com/kojiisd/items/8d7b98fb7f3c8f871f96
- https://dev.classmethod.jp/cloud/aws/localstack-lambda/
- localstackを利用する

### 手順
#### Docker Install
```
$ yum-config-manager --enable rhel-7-server-extras-rpms
$ yum install container-selinux
$ yum install -y yum-utils device-mapper-persistent-data lvm2
$ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ yum makecache fast
$ yum install docker-ce -y
$ yum-config-manager --disable rhel-7-server-extras-rpms
$ systemctl start docker
$ systemctl enable docker
```
### aws cli Install
```
$ curl -kL https://bootstrap.pypa.io/get-pip.py | python
$ pip install awscli --upgrade --user
```
```
$ vi ~/.bash_profile
export PATH=~/.local/bin:$PATH
```
```
$ source ~/.bash_profile
```

### aws cli configure
```
$ aws configure --profile localstack
AWS Access Key ID [None]: dummy
AWS Secret Access Key [None]: dummy
Default region name [None]: ap-northeast-1
Default output format [None]: json
 
$ cat ~/.aws/credentials
[localstack]
aws_access_key_id = dummy
aws_secret_access_key = dummy
 
$ cat ~/.aws/config
[profile localstack]
region = ap-northeast-1
output = json
```
### git install
```
$ yum install -y git
```

### Docker proxy configure
```
$ mkdir -p /etc/systemd/system/docker.service.d
$ echo -e "[Service]\nEnvironment=\"HTTP_PROXY=http://proxy.ns-sol.co.jp:8000/\"" | sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf
$ systemctl daemon-reload
$ systemctl restart docker
```

### LocalStack install
```
$ docker pull localstack/localstack
```
### Start docker localstack
```
$ docker run -it -p 4567-4582:4567-4582 -p 8080:8080 localstack/localstack
```
## Blowser
```
http://localhost:8080/
```
