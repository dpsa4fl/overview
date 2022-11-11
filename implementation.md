# Implementation
This projects involves many repositories, the dependencies between them are as follows.

*Please note that not everything is implemented yet.*

1. **libprio-rs**: We define a prio3 type with the purpose of
   securely aggregating gradient vectors from clients. This code is integrated upstream in the [libprio-rs](https://github.com/divviup/libprio-rs) repository.
2. **janus**: We add the necessary plumbing code for our new type to [janus](https://github.com/divviup/janus).
3. **dpsa4fl**: Our [rust crate](https://github.com/dpsa-project/dpsa4fl) depends on janus, it contains the core of our project: code necessary to interact with
   janus servers specific to our use-case of federated learning.
4. **dpsa4fl-bindings.py**: [This](https://github.com/dpsa-project/dpsa4fl-bindings.py) is a wrapper around dpsa4fl, and is released
   as a python package that can be downloaded from [PyPi](https://pypi.org/project/dpsa4fl-bindings/).
5. **dpsa4fl-example-project**: [This](https://github.com/dpsa-project/dpsa4fl-example-project) is
   a fully working example of how to use dpsa4fl (via our python bindings) with the flower framework for differentially private federated learning.
