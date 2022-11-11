# Implementation
This projects involves many repositories, the dependencies between them are as follows:

1. We define a [prio3](https://github.com/divviup/libprio-rs) type with the purpose of
   securely aggregating gradient vectors from clients. This is done directly in the [prio crate]().
2. We provide the necessary plumbing code for our new type into the [janus aggregator server]() project.
3. Our rust crate [dpsa4fl]() depends on janus, it contains the core of our project: Code necessary to interact with
   janus servers specific for our use-case of federated learning.
4. The rust-python project [dpsa4fl-bindings.py]() is, as the name implies, a wrapper around dpsa4fl, and is released
   as a python package that can be downloaded from [PyPi]().
5. The [dpsa4fl-example-project]() repo is (going to contain) a fully working example of how to use dpsa4fl (via our python bindings) with the
   flower framework for differentially private federated learning.
