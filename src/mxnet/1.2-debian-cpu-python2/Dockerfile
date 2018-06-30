FROM dockerguys/mxnet:1.2-debian-cpu

ENV PYTHONPATH=/mxnet/python

# Change USE_ALT_REPO to set the configuration url for an alternative repo
# - HINT: for china users, try this one: mirrors.ustc.edu.cn/repogen/conf/debian-http-4-stretch

RUN USE_ALT_REPO="" ; \
	apt-get update ; \
	cp /etc/apt/sources.list /tmp/ ; \
	if [ "x${USE_ALT_REPO}x" != "xx" ]; then \
		apt-get install -y wget ; \
		wget -O- "${USE_ALT_REPO}" > /etc/apt/sources.list ; \
		apt-get update ; \
	fi; \
	apt-get update ; \
	cd /tmp ; \
	wget https://bootstrap.pypa.io/get-pip.py ; \
	apt-get install -y wget python-dev ; \
	python2 get-pip.py ; \
	pip2 install nose pylint numpy nose-timer requests Pillow ; \
	rm get-pip.py ; \
	apt-get autoremove ; \
	mv /tmp/sources.list /etc/apt/ ; \
	find / -depth -type d -name tests -exec rm -rf {} \; ; \
	find / -depth -type d -name test -exec rm -rf {} \; ; \
	find / -depth -type d -name \__pycache__ -exec rm -rf {} \; ; \
	rm -rf /var/cache/apt/archives/* ; \
	rm -rf /var/lib/apt/lists/*

# To remove the following error caused by opencv
#    libdc1394 error: Failed to initialize libdc1394"
CMD sh -c 'ln -s /dev/null /dev/raw1394'; bash

