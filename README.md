# plutus-cborhex-automation

This is an open-source project supported by [Gimbalabs](https://discord.gg/2Qz73CjSxz) to create RESTful API Endpoint for Compiling Plutus Scripts.

## Why we need this?

In Cardano's eUTxO model, parameterized script is heavily used for real scalable smart contract. With different parameters supplied to the same validator function, it would return multiple plutus `Validator` and generate multiple `cborHex`. With different `cborHex` created, different script addresses could be derived. It is a best practice to split users who never interact with each others into different places, which avoid multiple issues such as [double satifaction](https://plutus.readthedocs.io/en/latest/reference/writing-scripts/common-weaknesses/double-satisfaction.html) etc.

However, in the available education resources most script `cborHex`'s are created through the `Utils.hs` function inside Haskell library. In the real Dapp production, user cannot get the script calculated easily on the frontend. Therefore, currently there is still gap implementing such Plutus best practice. This project is trying to reduce the pain in between, building a module or service which could help Plutus developers to easily convert their Plutus cborHex generating functions into RESTful API endpoints, which are easily interacted with the client side.

In the long-term, an API for compiling plutus scripts can be served via Dandelion. In addition to being an essential tool, this project can serve as an example of how we can extend Dandelion.

## Quick Start

1. Enter your `nix-shell` of your `plutus-apps` using checkout tag `97b4c1da03faf9bc35f348802fb7927231657e75`

- Inside `plutus-apps`

```
git pull
git checkout 97b4c1da03faf9bc35f348802fb7927231657e75
nix-shell
```

2. Clone this repository says at Desktop, go to the example folder and run:

```
cd ~
```

```
git clone https://github.com/SIDANWhatever/plutus-cborhex-automation.git
cd plutus-cborhex-automation/example
cabal run sidan-plutus-server
```

3. Allow for a while of build time, you could then see the local server is up for example validator.

   > Spock is running on port 8080

4. Try the endpoint!

```
curl -X POST http://localhost:8080/testing-validator -H "Content-Type: application/json" -d '{"testNumber": 245324,"testPpkh": {"unPaymentPubKeyHash": {"getPubKeyHash": "8f2ac4b2a57a90feb7717c7361c7043af6c3646e9db2b0e616482f73"}}}'
```

## API Documentation

For full API documentation please find it [here](./documentation/APIDoc.md)

## Deployment

You can deploy this service as any other typical backend servers. We have one [example](./documentation/Deployment.md) demonstrating how to deploy on AWS EC2. Onwards we would develop scripts for streamlining deployment process.

## Milestones

### Milestone 0: Study the problem

- Exploring different alternatives (done)
- Testing out different Haskell backend libraries and dockerize functions for simple function to see if there is any major blocker.
- Set the workflow for the rest of the research. (done)

### Milestone 1:

Developers can use a Docker image to create a REST endpoint that returns CBOR and Address for any pre-defined, parameterized Plutus Validator.
Upon delivery of Docker image, code repo and documentation.

#### Deliverables:

- Open source repository (this repo)
- Docker Image (on halt)
- Developer-Facing Documentation for the following steps:

1. How to add an arbitrary contract to Docker image (on halt)
2. How to specify Validator Parameters related to that Contract (done)
3. How to use local endpoint that takes Parameters and returns CBOR + Address (done)

#### Updated 15 Nov, 2022:

- Upon further research, as we face blocker in wrapping plutus code as docker, we shift the focus to be building a module for easy implementation of any other Plutus project. The goal is specify at [Issue 4](https://github.com/SIDANWhatever/plutus-cborhex-automation/issues/4), which upon completion this would be wrapped as a public module publish to [Hackage](https://hackage.haskell.org/packages/upload).

### Milestone 2:

Outcomes:

- Use the library to build exposed endpoints for creating GBTE Instances
- Add this example microservice to Dandelion

#### Deliverables:

- Public endpoint for creating GBTE Contract Instance

## What's next

- Backend server design choice. Currently we develop based on Spock. Later on we would explore options.
- Unnecessary packages clean up.
- More `plutus-apps` tags support upon community suggestions.
- Deployment streamlining like using Terraform.

## Further cooperation with Gimbalabs

We can take the final step of using this service as a first step to putting reference scripts on-chain. This will also serve as a hands-on example that can be used in upcoming Dandelion PBL course.

## Contribution

We also welcome any contributors on any suggestions and thoughts to make this infrastructure more accessible, and better to the community.

## Other Resources / List of Other Related Projects

1. [Helios](https://github.com/Hyperion-BT/Helios)
2. [demeter.run](https://demeter.run/)
3. [Cardano Syllabus](https://johnshearing.github.io/cardano_syllabus/)
