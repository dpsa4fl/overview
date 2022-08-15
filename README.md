# Differential Privacy for Federated Learning
The goal of this project is to create a system for federated machine learning where differential privacy of any individual client's data can be guaranteed, using [global differential privacy](https://desfontain.es/privacy/local-global-differential-privacy.html).

## Setup
![overview](./dpsa-overview-2.svg)

The following entities participate:
1. The **clients** are interested in preserving the privacy of their data. They may be malicious.
2. At least two **aggregation servers** participate, of which at least one must be honest (but can be curious). All other servers may be malicious.
3. The **ML server** may be malicious.

If the properties stated above are met by the participants, the clients' **anonymity** (no adversary can tell which client submitted which data value), **privacy** (no adversary learns anything about the clients' data values except their aggregate), and even [**differential privacy**](https://en.wikipedia.org/wiki/Differential_privacy) can be guaranteed.

## How it works
1. The ML server distributes its current model to the clients.
2. Each client locally computes the gradient vector for that model based on its data.
3. Each client splits its gradient vector into *gradient shares* and submits
   a share to each aggregation server.
4. The aggregation servers verify that the submitted vectors are well-formed (clipped, with L2 norm less than 1).
   This is done in a distributed way, without any knowledge being gained about the values of the clients' submissions.
5. Each aggregation server adds noise to the clients' shares to provide pre-established privacy guarantees.
5. The aggregation servers compute the *aggregate gradient* as a sum of all client gradients, again in a distributed fashion. The aggregate contains noise from all the
   aggregation servers and is sent to the ML server.
6. The ML server updates its model and can initiate a new training round.

## Implementation
For aggregation of gradient vectors we use [prio-rs](https://github.com/divviup/libprio-rs).
Its mechanism for zero-knowledge proofs on secret-shared data (which is used for verifying that gradient vectors are bounded) is described [here](https://crypto.stanford.edu/prio) and [here](https://eprint.iacr.org/2019/188).
We plan to use the [flower](https://github.com/adap/flower) framework for federation.
