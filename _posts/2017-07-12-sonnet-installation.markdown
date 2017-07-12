---
title: "Sonnet Installation Instructions"
layout: post
date: 2017-07-12 01:00
tag: Sonnet, Tensorflow, AWS, Ubuntu
headerImage: false
projects: true
hidden: true
---

Sonnet Installation Instructions on AWS Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-835b4efa
---

1. We start by updating the packages of Ubuntu:
       $ sudo apt-get update && sudo apt-get -y upgrade
2. Install pip and virtualenv by issuing one of the following commands:
       $ sudo apt-get install python-pip python-dev python-virtualenv # for Python 2.7
       $ sudo apt-get install python3-pip python3-dev python-virtualenv # for Python 3.n
3. Update pip:
       $ pip install --upgrade pip
4. Create a virtualenv environment by issuing one of the following commands:
       $ virtualenv --system-site-packages ~/tensorflow # for Python 2.7
       $ virtualenv --system-site-packages -p python3 ~/tensorflow # for Python 3.n
   where ~/tensorflow is a target directory that specifies the top of the virtualenv tree. We may choose any directory.
5. Activate the virtualenv environment by issuing one of the following commands:
       $ source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh
       $ source ~/tensorflow/bin/activate.csh  # csh or tcsh
   The preceding source command should change your prompt to the following:
        (tensorflow)$ 
6. Issue one of the following commands to install TensorFlow in the active virtualenv environment:
       (tensorflow)$ pip install --upgrade tensorflow      # for Python 2.7
       (tensorflow)$ pip3 install --upgrade tensorflow     # for Python 3.n
       (tensorflow)$ pip install --upgrade tensorflow-gpu  # for Python 2.7 and GPU
       (tensorflow)$ pip3 install --upgrade tensorflow-gpu # for Python 3.n and GPU
7. Install JDK 8 by using:
       $ sudo apt-get install openjdk-8-jdk
8. Add Bazel distribution URI as a package source (one time setup):
       $ echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
       $ curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
9. Install and update Bazel:
       $ sudo apt-get update && sudo apt-get install bazel
   Once installed, you can upgrade to a newer version of Bazel with:
       $ sudo apt-get upgrade bazel
10. Activate virtualenv again:
        $ source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh
        $ source ~/tensorflow/bin/activate.csh  # csh or tcsh
11. First clone the Sonnet source code with TensorFlow as a submodule:
        $ git clone --recursive https://github.com/deepmind/sonnet
    and then configure Tensorflow headers:
        $ cd sonnet/tensorflow
        $ ./configure
        $ cd ../
    You can choose the suggested defaults during the TensorFlow configuration. Note: This will not modify your existing installation of TensorFlow. This step is necessary so that Sonnet can build against the TensorFlow headers.
12. Build and run the installer:
        $ mkdir /tmp/sonnet
        $ bazel build --config=opt --copt="-D_GLIBCXX_USE_CXX11_ABI=0" :install
        $ ./bazel-bin/install /tmp/sonnet
    pip install the generated wheel file:
        $ pip install /tmp/sonnet/*.whl
13. You can verify that Sonnet has been successfully installed by, for example, trying out the resampler op:
        $ cd ~/
        $ python
        >>> import sonnet as snt
        >>> import tensorflow as tf
        >>> snt.resampler(tf.constant([0.]), tf.constant([0.]))
    The expected output should be:
        <tf.Tensor 'resampler/Resampler:0' shape=(1,) dtype=float32>



[1] https://www.tensorflow.org/install/install_linux#installing_with_virtualenv

[2] https://docs.bazel.build/versions/master/install-ubuntu.html

[3] https://github.com/deepmind/sonnet


