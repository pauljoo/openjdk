FROM centos:7

MAINTAINER from xpanda.info by pauljoo (jianghuiyao@outlook.com)

RUN yum -y groupinstall "Development Tools"

RUN yum -y install unzip libXtst-devel libXt-devel libXrender-devel cups-devel freetype-devel alsa-lib-devel which ccache

ADD jdk-7u80-linux-x64.tar.gz /root/
ADD openjdk-8-src-b132-03_mar_2014.tar.gz /root/

RUN chmod 777 /root/openjdk/configure

ENTRYPOINT ["tail", "-F", "/dev/null"]