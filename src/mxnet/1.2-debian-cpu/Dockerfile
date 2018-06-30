FROM debian:stretch-slim

ENV MXNET_VERSION="v1.2.0"

# Change USE_ALT_REPO to set the configuration url for an alternative repo
# - HINT: for china users, try this one: mirrors.ustc.edu.cn/repogen/conf/debian-http-4-stretch

RUN USE_OPENBLAS="true" ; \
	USE_OPENCV="true" ; \
	USE_ALT_REPO="" ; \
	cp /etc/apt/sources.list /tmp/ ; \
	apt-get update ; \
	if [ "x${USE_ALT_REPO}x" != "xx" ]; then \
		apt-get install -y wget ; \
		wget -O- "${USE_ALT_REPO}" > /etc/apt/sources.list ; \
		apt-get update ; \
	fi; \
	apt-get install -y build-essential git \
		libatlas-base-dev libopencv-dev libcurl4-openssl-dev libgtest-dev \
		cmake wget unzip \
		python-opencv ; \
	if [ "${USE_OPENBLAS}" = "true" ]; then \
		apt-get install -y libopenblas-dev liblapack-dev ; \
	fi ; \
	cd /usr/src/gtest ; \
	cmake CMakeLists.txt ; \
	make ; \
	cp *.a /usr/lib ; \
	cd / ; \
	git clone --branch "${MXNET_VERSION}" --recursive https://github.com/dmlc/mxnet ; \
	cd /mxnet ; \
	cp make/config.mk . ; \
	if [ "${USE_OPENBLAS}" = "true" ]; then \
		echo 'USE_BLAS = openblas' >> config.mk ; \
	fi ; \
	if [ "${USE_OPENCV}" = "true" ]; then \
		echo 'USE_OPENCV = 1' >> config.mk ; \
	fi ; \
	make -j$(nproc) ; \
	rm -r build ; \
	apt-get purge -y --auto-remove build-essential git \
		libatlas-base-dev libopencv-dev libcurl4-openssl-dev libgtest-dev \
		cmake wget unzip ; \
	if [ "${USE_OPENBLAS}" = "true" ]; then \
		apt-get purge -y --auto-remove libopenblas-dev liblapack-dev ; \
	fi ; \
	if [ "${USE_OPENCV}" = "true" ]; then \
		apt-get install -y libopencv-videostab2.4v5 libopencv-ts2.4v5 libopencv-superres2.4v5 \
			libopencv-stitching2.4v5 libopencv-ocl2.4v5 libopencv-gpu2.4v5 \
			libopencv-core2.4v5 libopencv-ml2.4v5 ; \
		apt-get install -y libsm6 ; \
	fi ; \
	apt-get autoremove ; \
	mv /tmp/sources.list /etc/apt/ ; \
	rm -rf /var/cache/apt/archives/* ; \
	rm -rf /var/lib/apt/lists/* ; \
	find /mxnet -maxdepth 1 -type d -name "[^lp]*" ! -path /mxnet -exec rm -rf {} \; ; \
	find /mxnet -maxdepth 1 -type f -name "[^lp]*" -exec rm -rf {} \; ; \
	find / -depth -type d -name tests -exec rm -rf {} \; ; \
	find / -depth -type d -name test -exec rm -rf {} \; ; \
	find / -depth -type d -name \__pycache__ -exec rm -rf {} \; ; \
	rm /mxnet/lib/libmxnet.a

# To remove the following error caused by opencv
#    libdc1394 error: Failed to initialize libdc1394"
CMD sh -c 'ln -s /dev/null /dev/raw1394'; bash
