# NOTE: this example is taken from the default Dockerfile for the official nginx Docker Hub Repo
# https://hub.docker.com/_/nginx/
# NOTE: This file is slightly different than the video, because nginx versions have been updated 
#       to match the latest standards from docker hub... but it's doing the same thing as the video
#       describes
FROM arm64v8/debian:buster
# all images must have a FROM
# usually from a minimal Linux distribution like debian or (even better) alpine
# if you truly want to start with an empty container, use FROM scratch

# ENV NGINX_VERSION 1.13.6-1~stretch
# ENV NJS_VERSION   1.13.6.0.1.14-1~stretch

#http://nginx.org/en/linux_packages.html#Debian

ENV NGINX_VERSION 1.21.4-1~buster
ENV NJS_VERSION   1.18.0-6.1

# optional environment variable that's used in later lines and set as envvar when container is running

RUN apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
	&& \
	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
	apt-get --no-install-recommends --no-install-suggests -y install curl gnupg2 ca-certificates lsb-release debian-archive-keyring \
	&& curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
	| tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null \
	# && gpg --dry-run --quiet --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg \
	&& echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \ 
	http://nginx.org/packages/debian `lsb_release -cs` nginx" \
	| tee /etc/apt/sources.list.d/nginx.list \
	&& apt-get update \
	&& apt-get --no-install-recommends --no-install-suggests -y install nginx
# optional commands to run at shell inside container at build time
# this one adds package repo for nginx from nginx.org and installs it

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log
# forward request and error logs to docker log collector

EXPOSE 80 443
# expose these ports on the docker virtual network
# you still need to use -p or -P to open/forward these ports on host

CMD ["nginx", "-g", "daemon off;"]
# required: run this command when container is launched
# only one CMD allowed, so if there are multiple, last one wins
