FROM node:latest
ADD sources.list /etc/apt/
RUN apt-get update \ 
&& apt-get install -y git \ 
&& npm install -g cnpm --registry=https://registry.npm.taobao.org \ 
&& npm install -g express \ 
&& npm install -g webpack \ 
&& npm install --global vue-cli