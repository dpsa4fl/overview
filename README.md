# Differential Privacy for Federated Learning
The goal of this project is to create a system for federated machine learning where differential privacy of any individual client's data can be guaranteed, using [global differential privacy](https://desfontain.es/privacy/local-global-differential-privacy.html) without requiring trust in a single aggregator.

## Setup
![overview](./dpsa-overview-2.svg)

The following entities participate:
1. The **clients** hold sensitive data on which the machine learning task is supposed to be executed.
2. The **aggregation servers** perform gradient aggregation without seeing plaintext submissions.
3. The **ML server** holds the current model, updates according to gradient aggregates, and distributes the update.

We aim to provide the following privacy guarantees:
- **Anonymity** (no adversary can tell which client submitted which data value) and **privacy** (no adversary learns anything about an honest clients' data values except the aggregate) an be guaranteed if
   - all clients are malicious
   - at least one aggregation server is honest but curious, the remaining ones are malicious
   - the ML server is malicious
- [**Differential privacy**](https://en.wikipedia.org/wiki/Differential_privacy) can be guaranteed if
   - all clients are malicious
   - all aggregation servers are honest but curious
   - the ML server is malicious

Correctness of the result of the learning procedure requires honesty of all participants. As differential privacy persists even for malicious clients, the learning result is guaranteed to at least be robust towards data poisoning in that case.


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
For aggregation of gradient vectors we use [prio-rs](https://github.com/divviup/libprio-rs) with [fixed-point vectors](https://github.com/dpsa-project/libprio-rs).
Its mechanism for zero-knowledge proofs on secret-shared data (which is used for verifying that gradient vectors are bounded) is described [here](https://crypto.stanford.edu/prio) and [here](https://eprint.iacr.org/2019/188).
We plan to use the [flower](https://github.com/adap/flower) framework for federation.
