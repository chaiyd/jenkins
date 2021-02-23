## https://github.com/chaiyd/jenkins.git

## jenkins Weekly
- 使用Jenkins官方dokerfile进行更改
- 默认使用jenkins 用户.
- 镜像中包含maven-3.6.3
- 默认已安装npm,cnpm

---
- Use Jenkins official dokerfile to change
- Use jenkins user by default.
- When building, you need to specify the version number,The default is the latest version of lts version
- The default will install npm cnpm
---

## build & run
- docker build --build-arg JENKINS_VERSION=2.249.2 -t jenkins:2.280 .
- docker-compose up -d