# syntax=docker/dockerfile:1

FROM ubuntu:jammy
ENV DEBIAN_FRONTEND=nointeractive
ENV LANG=ja_JP.UTF-8

# 必要なパッケージの準備
RUN --mount=type=cache,target=/var/lib/apt  --mount=type=cache,target=/var/cache/apt \
	echo Asia/Tokyo > /etc/timezone; \
	apt-get update; apt-get install -y tzdata
RUN --mount=type=cache,target=/var/lib/apt  --mount=type=cache,target=/var/cache/apt \
	apt-get update; apt-get install -y \
	sudo \
	curl \
	git \
	python3-pip \
	python3

# SphinxおよびJupyter book環境
RUN --mount=type=cache,target=/root/.cache/pip \
	pip install sphinx jupyter-book sphinx-autobuild

# TeX環境(PDF生成)
RUN mkdir /tmp/texlive-installer
COPY install-tl-unx.tar.gz /tmp/texlive-installer
COPY texlive.profile /tmp/texlive-installer
RUN cd /tmp/texlive-installer; \
	tar xvzf install-tl-unx.tar.gz; \
	cd install-tl-*; \
	./install-tl --profile /tmp/texlive-installer/texlive.profile
RUN tlmgr install \
	latexmk \
	jsclasses \
	tex-gyre \
	float \
	wrapfig \
	capt-of \
	framed \
	fancyvrb \
	upquote \
	needspace \
	tabulary \
	varwidth \
	parskip \
	titlesec \
	cmap

# アカウントとアクセス権
WORKDIR /workspace
ARG USER=worker
ARG USER_UID=1000
ARG USER_GID=1000
RUN grep -q ":$USER_GID:" /etc/group || groupadd --gid ${USER_GID} ${USER}
RUN useradd -m --uid ${USER_UID} --gid ${USER_GID} -s /bin/bash ${USER}
RUN echo "${USER} ALL= (ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USER}; chmod 0400  /etc/sudoers.d/${USER}
RUN chown ${USER_UID}:${USER_GID} /workspace
USER worker
ENV PATH=/opt/texlive/bin/x86_64-linux/:/home/worker/.local/bin:$PATH
