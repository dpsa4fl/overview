# Differential Privacy for Federated Learning
The goal of this project is to create a system for federated machine learning where privacy of an individual client's data can be guaranteed.
It is applicable in situations where there are multiple parties interested in the learning task. As long as at least one of the parties does not
collude with others, none of them have access to individual client submissions.

## Setup
![overview](./dpsa-overview-2.svg)

The following entities participate:
1. The **clients** are interested in preserving the privacy of their data.
2. The **aggregation servers** are run by parties that are interested
   in protecting the client data from the owners of the other aggregation servers.
3. The **ML server** is run by a party interested in only training on valid client responses.

## How it works
1. The ML server distributes its current model to the clients.
2. The clients locally compute update gradients for that model.


