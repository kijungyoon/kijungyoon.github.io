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

<ol><li>
<p>We start by updating the packages of Ubuntu:</p>
<pre><code>
$ sudo apt-get update &amp;&amp; sudo apt-get -y upgrade
</code></pre>
</li>
<li>
<p>Install pip and virtualenv by issuing one of the following commands:</p>
<pre><code>
$ sudo apt-get install python-pip python-dev python-virtualenv # for Python 2.7
$ sudo apt-get install python3-pip python3-dev python-virtualenv # for Python 3.n
</code></pre>
</li>
<li>
<p>Update pip:</p>
<pre><code>
$ pip install --upgrade pip
</code></pre>
</li>
<li>
<p>Create a virtualenv environment by issuing one of the following commands:</p>
<pre><code>
$ virtualenv --system-site-packages ~/tensorflow # for Python 2.7
$ virtualenv --system-site-packages -p python3 ~/tensorflow # for Python 3.n
</code></pre>
<p>where <code>~/tensorflow</code> is a target directory that specifies the top of the virtualenv tree. We may choose any directory.</p>
</li>
<li>
<p>Activate the virtualenv environment by issuing one of the following commands:</p>
<pre><code>
$ source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh
$ source ~/tensorflow/bin/activate.csh  # csh or tcsh
</code></pre>
<p>The preceding <code>source</code> command should change your prompt to the following:</p>
<pre><code>
 (tensorflow)$ 
</code></pre>
</li>
<li>
<p>Issue one of the following commands to install TensorFlow in the active virtualenv environment:</p>
<pre><code>
(tensorflow)$ pip install --upgrade tensorflow      # for Python 2.7
(tensorflow)$ pip3 install --upgrade tensorflow     # for Python 3.n
(tensorflow)$ pip install --upgrade tensorflow-gpu  # for Python 2.7 and GPU
(tensorflow)$ pip3 install --upgrade tensorflow-gpu # for Python 3.n and GPU
</code></pre>
</li>
<li>
<p>Install JDK 8 by using:</p>
<pre><code>
$ sudo apt-get install openjdk-8-jdk
</code></pre>
</li>
<li>
<p>Add Bazel distribution URI as a package source (one time setup):</p>
<pre><code>
$ echo &quot;deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8&quot; | sudo tee /etc/apt/sources.list.d/bazel.list
$ curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
</code></pre>
</li>
<li>
<p>Install and update Bazel:</p>
<pre><code>
$ sudo apt-get update &amp;&amp; sudo apt-get install bazel
</code></pre>
<p>Once installed, you can upgrade to a newer version of Bazel with:</p>
<pre><code>
$ sudo apt-get upgrade bazel
</code></pre>
</li>
<li>
<p>Activate virtualenv again:</p>
<pre><code>
$ source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh
$ source ~/tensorflow/bin/activate.csh  # csh or tcsh
</code></pre>
</li>
<li>
<p>First clone the Sonnet source code with TensorFlow as a submodule:</p>
<pre><code>
$ git clone --recursive https://github.com/deepmind/sonnet
</code></pre>
<p>and then configure Tensorflow headers:</p>
<pre><code>
$ cd sonnet/tensorflow
$ ./configure
$ cd ../
</code></pre>
<p>You can choose the suggested defaults during the TensorFlow configuration. Note: This will not modify your existing installation of TensorFlow. This step is necessary so that Sonnet can build against the TensorFlow headers.</p>
</li>
<li>
<p>Build and run the installer:</p>
<pre><code>
$ mkdir /tmp/sonnet
$ bazel build --config=opt --copt=&quot;-D_GLIBCXX_USE_CXX11_ABI=0&quot; :install
$ ./bazel-bin/install /tmp/sonnet
</code></pre>
<p><code>pip install</code> the generated wheel file:</p>
<pre><code>
$ pip install /tmp/sonnet/*.whl
</code></pre>
</li>
<li>
<p>You can verify that Sonnet has been successfully installed by, for example, trying out the resampler op:</p>
<pre><code>
$ cd ~/
$ python
&gt;&gt;&gt; import sonnet as snt
&gt;&gt;&gt; import tensorflow as tf
&gt;&gt;&gt; snt.resampler(tf.constant([0.]), tf.constant([0.]))
</code></pre>
<p>The expected output should be:</p>
<pre><code>
&lt;tf.Tensor &#39;resampler/Resampler:0&#39; shape=(1,) dtype=float32&gt;
</code></pre>
</li>
</ol>
<p></p>
<p>[1] <a href='https://www.tensorflow.org/install/install_linux#installing_with_virtualenv' target='_blank' >https://www.tensorflow.org/install/install_linux#installing_with_virtualenv</a></p>
<p>[2] <a href='https://docs.bazel.build/versions/master/install-ubuntu.html' target='_blank' >https://docs.bazel.build/versions/master/install-ubuntu.html</a></p>
<p>[3] <a href='https://github.com/deepmind/sonnet' target='_blank' >https://github.com/deepmind/sonnet</a></p>
