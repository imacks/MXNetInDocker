MXNET in DOCKER
===============
This is the repo for my MXNet Dockerfiles. Everything is compiled from source.

Compared with the official image:
- Rebased on debian-stretch-slim
- Removed build files and unnecessary files - output file is about 400mb (from 1.6gb)
- separated python runtimes from MXNet library
