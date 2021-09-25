FROM ubuntu:20.04

# argument required by tzdata installation from software-properties-common
ARG DEBIAN_FRONTEND=noninteractive
ARG PYTHON_VERSION
ARG IS_CI

RUN set -x; \
	apt update; \
	apt install -y git \
		wget; \
	if [ "${PYTHON_VERSION}" != "2.7" ]; then \
		# clean install Python
		apt install -y build-essential \
			zlib1g-dev \
			libncurses5-dev \
			libgdbm-dev \
			libnss3-dev \
			libssl-dev \
			libreadline-dev \
			libffi-dev \
			libsqlite3-dev \
			libbz2-dev; \
		\
		case "${PYTHON_VERSION}" in \
			"3.6") \
			PYTHON_TGZ_TAG="3.6.9" \
			;; \
			"3.7") \
			PYTHON_TGZ_TAG="3.7.9" \
			;; \
			"3.8") \
			PYTHON_TGZ_TAG="3.8.12" \
			;; \
			"3.9") \
			PYTHON_TGZ_TAG="3.9.7" \
			;; \
		esac; \
		wget https://www.python.org/ftp/python/$PYTHON_TGZ_TAG/Python-$PYTHON_TGZ_TAG.tgz; \
		tar -xf Python-$PYTHON_TGZ_TAG.tgz; \
		( \
			cd Python-$PYTHON_TGZ_TAG || exit; \
			./configure --enable-optimizations; \
			make -j 12; \
			make altinstall; \
		); \
		rm -rf Python-$PYTHON_TGZ_TAG.tgz Python-$PYTHON_TGZ_TAG; \
		# install pip
		curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; \
		python3.9 get-pip.py; \
		rm -rf get-pip.py; \
	fi; \
	if [ "${IS_CI}" = "true" ]; then \
		apt install -y graphviz \
				curl \
				jq; \
		if [ "${PYTHON_VERSION}" = "2.7" ]; then \
			apt install -y python2-minimal; \
			curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -k --output get-pip.py; \
			python2 get-pip.py; \
		fi; \
		if [ "${PYTHON_VERSION}" = "3.9" ]; then \
			pip install flake8 \
				packaging; \
		fi; \
	else \
		pip install twine; \
	fi;