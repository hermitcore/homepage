---
layout:     post
title:      A Rust-based Unikernel
author:     Stefan Lankes
tags: 	    rust
subtitle:   First version of a Rust-based libOS
---

Rust is an extremely interesting language to develop system software.
It promises a secure memory handling by zero-cost abstraction.
Projects like [Redox](https://www.redox-os.org) and the [blog posts from Philipp Oppermann](https://os.phil-opp.com/second-edition/) shows that it possible to write with Rust a kernel in a very expressive, high level way without worrying about undefined behavior or memory safety.
Rust features like iterators, closures, pattern matching, option and result, string formatting, and the ownership system are still usable for kernel developers. 

This was our motivation to evaluate Rust as programming language for HermitCore and to develop an [experimental version of our libOS in Rust](https://github.com/hermitcore/libhermit-rs).
However, components like the IP stack and our unikernel hypervisor *uhyve* are still written in C.
In addition, the user applications are still compiled by our cross-compiler, which is based on gcc and supports C, C++, Fortran and Go.
But the core of the kernel is now written in Rust and published at [GitHub](https://github.com/hermitcore/libhermit-rs).
Currently, our experiences are really good and we think about an extension of our activities.
For instance, a support of Rust’s userland is currently missing.

The current version is experimental and doesn’t support all features of our [C version](https://github.com/hermitcore/libhermit).
In addition, it is only tested on Ubuntu 18.04.
If you use Ubuntu 18.04 and want to test our kernel, you could install the packages as follows:

```bash
$ echo "deb [trusted=yes] https://dl.bintray.com/hermitcore/ubuntu bionic main" | sudo tee -a /etc/apt/sources.list
$ sudo apt-get -qq update
$ sudo apt-get install binutils-hermit newlib-hermit pte-hermit-rs gcc-hermit libhermit-rs
```

If you use another operating system, you are able to use our Docker image to test the kernel:

```bash
$ docker pull rwthos/hermitcore-rs
$ docker run -it rwthos/hermitcore-rs:latest
```

If you are able to install HermitCore on Ubuntu 18.04 or to run our docker container, you can test the system for instance by running the stream benchmark.

```bash
HERMIT_ISLE=qemu HERMIT_KVM=0 /opt/hermit/bin/proxy /opt/hermit/x86_64-hermit/extra/benchmarks/stream
```

If your system supports KVM, you could use *uhyve* to accelerate the boot time:

```bash
$ HERMIT_ISLE=uhyve /opt/hermit/bin/proxy /opt/hermit/x86_64-hermit/extra/benchmarks/stream
```

The current version is an experimental version and definitely not stable.
But it is a good starting point...
