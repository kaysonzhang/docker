FROM nginx:1.19.1
MAINTAINER goozp "313569855@qq.com"
#set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN usermod -u 1000 www-data