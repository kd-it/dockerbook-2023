# syntax=docker/dockerfile:1

FROM ubuntu:jammy
ENV DEBIAN_FRONTEND=nointeractive
ENV LANG=ja_JP.UTF-8
RUN --mount=type=cache,target=/var/lib/apt  --mount=type=cache,target=/var/cache/apt \
	echo Asia/Tokyo > /etc/timezone; \
	apt-get update; apt-get install -y tzdata
RUN --mount=type=cache,target=/var/lib/apt  --mount=type=cache,target=/var/cache/apt \
	apt-get update; apt-get install -y \
	sudo \
	git \
	python3-pip \
	python3
RUN --mount=type=cache,target=/root/.cache/pip \
	pip install sphinx jupyter-book sphinx-autobuild
WORKDIR /workspace
ARG USER=worker
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd --gid ${USER_GID} ${USER}; \
	useradd -m --uid ${USER_UID} --gid ${USER_GID} -s /bin/bash ${USER}; \
	echo "${USER} ALL= (ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USER}; \
	chmod 0400  /etc/sudoers.d/${USER}
RUN chown ${USER}:${USER} /workspace
USER worker
ENV PATH=/home/worker/.local/bin:$PATH
