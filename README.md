使用Jenkins官方dokerfile进行更改，更换war包源地址默认为LTS
更改启动用户

build时，需指定版本号
JENKINS_VERSION=jenkins版本号，注LTS版
JENKINS_URL 可指定完整的url

Use Jenkins official dokerfile to change, replace the war package source address default to LTS
Change the startup user
When building, you need to specify the version number.
JENKINS_VERSION=jenkins version number, note LTS version
JENKINS_URL = specify the full url

docker build --build-arg JENKINS_VERSION=2.176.2 -t jenkins:2.176.2 .

docker run --name jenkins -p 9090:8080 -p 50000:50000 -v /var/docker/jenkins:/var/jenkins_home jenkins:2.176.2
