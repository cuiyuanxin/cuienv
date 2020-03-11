FROM node:latest
ADD sources.list /etc/apt/
RUN apt-get update
RUN apt-get install -y git

RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
RUN npm install -g express
RUN npm install -g webpack
RUN npm install --global vue-cli