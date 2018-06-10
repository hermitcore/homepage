---
layout:     post
title:      A Rust-based Unikernel
author:     Stefan Lankes
tags: 	    rust
subtitle:   First version of a Rust-based libOS
---

Rust is an extremely interesting language for the development of system software.
It promises a secure memory handling enabled by its principle of zero-cost abstraction.
Projects like [Redox](https://www.redox-os.org) and this [blog posts](https://os.phil-opp.com/second-edition/) from Philipp Oppermann show the feasibility of writing an OS kernel with Rust in an expressive, high level way without worrying about undefined behavior or memory safety.
Rust features like iterators, closures, pattern matching, option and result, string formatting, and the ownership system are still usable for a kernel developers. 

This was the motivation to evaluate Rust for HermitCore and to develop an [experimental version of our libOS in Rust](https://github.com/hermitcore/libhermit-rs).
Components like the IP stack and *uhyve* (our unikernel hypervisor) are still written in C.
In addition, the user applications are still compiled by our cross-compiler, which is based on gcc and supports C, C++, Fortran, and Go.
The core of the kernel, however, is now written in Rust and published at [GitHub](https://github.com/hermitcore/libhermit-rs).
Our experiences so far are really good and we are looking into possibly new Rust activities, e.g., the support for Rust’s userland.

The current version is experimental and does not provide support for all the features of our [C version](https://github.com/hermitcore/libhermit).
Furthermore, it is also only tested on Ubuntu 18.04.
If you use Ubuntu 18.04, you can install the packages as follows:

```bash
$ echo "deb [trusted=yes] https://dl.bintray.com/hermitcore/ubuntu bionic main" | sudo tee -a /etc/apt/sources.list
$ sudo apt-get -qq update
$ sudo apt-get install binutils-hermit newlib-hermit pte-hermit-rs gcc-hermit libhermit-rs
```

If you use another operating system, you may use our Docker image to test the kernel:

```bash
$ docker pull rwthos/hermitcore-rs
$ docker run -it rwthos/hermitcore-rs:latest
```

After you have installed HermitCore successfully, you can test the system by running the stream benchmark:

```bash
HERMIT_ISLE=qemu HERMIT_KVM=0 /opt/hermit/bin/proxy /opt/hermit/x86_64-hermit/extra/benchmarks/stream
```

If your system supports KVM, you can use *uhyve* to accelerate the boot time:

```bash
$ HERMIT_ISLE=uhyve /opt/hermit/bin/proxy /opt/hermit/x86_64-hermit/extra/benchmarks/stream
```

The current version is an experimental version and definitely not stable.
But it is a good starting point...
