
## https://github.com/chaiyd/jenkins.git

使用Jenkins官方dokerfile进行更改，更换war包源地址默认为LTS

build时，需指定版本号
JENKINS_VERSION=jenkins版本号，注LTS版本号
JENKINS_URL 可指定完整的url
maven 变量为maven包名

docker build --build-arg JENKINS_VERSION=2.176.2 -t jenkins:2.176.2 .

docker run --name jenkins -p 9090:8080 -p 50000:50000 -v /var/docker/jenkins:/var/jenkins_home jenkins:2.176.2
