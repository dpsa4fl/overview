# Differential Privacy for Federated Learning
The goal of this project is to create a system for federated machine learning where privacy of an individual client's data can be guaranteed.
It is applicable in situations where there are multiple parties interested in the learning task. As long as at least one of the parties does not
collude with others, none of them have access to individual client submissions.

## Setup
![overview](./dpsa-overview-2.svg)

The following entities participate:
1. The **clients** are interested in preserving the privacy of their data.
2. The **aggregation servers** are run by parties that are interested
   in protecting the client data from the owners of the other aggregation servers,
   thereby establishing a situation where no one except the client has access to its submitted data.
3. The **ML server** is run by a party interested in only training on valid client responses.

## How it works
1. The ML server distributes its current model to the clients.
2. Each client locally computes the gradient vector for that model based on its data.
3. Each client splits its gradient vector into *gradient shares* and submits
   a share to each aggregation server.
4. The aggregation servers verify that the submitted vectors are well-formed (clipped, with L2 norm less than 1).
   This is done in a distributed way, without any knowledge being gained about the values of the clients' submissions.
5. Each aggregation server adds noise to the clients' shares to provide pre-established privacy guarantees.
5. The aggregation servers compute the *aggregate gradient* as a sum of all client gradients. It contains noise from all the
   aggregation servers and is sent to the ML server.
6. The ML server updates its model and can initiate a new training round. 

## Implementation
For aggregation of gradient vectors we use [prio-rs](https://github.com/divviup/libprio-rs).
Its mechanism for zero-knowledge proofs on secret-shared data (which is used for verifying that gradient vectors are bounded) is described [here](https://crypto.stanford.edu/prio) and [here](https://eprint.iacr.org/2019/188). We plan to use the [flower](https://github.com/adap/flower) framework for federation.
