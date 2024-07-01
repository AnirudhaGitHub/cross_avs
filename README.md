# Cross AVS

The Cross-Chain Messaging AVS is a protocol designed to facilitate secure and verifiable messaging across multiple blockchain networks. Currently crossAVS will allow you to transfer message from Movement blockchain to other EVM network like arbitrum. It ensures reliable communication between different blockchain environments, enabling actions such as asset transfers, data sharing, and decentralized application interactions.

![image](https://github.com/AnirudhaGitHub/cross_avs/assets/167628180/65d59d99-f99c-4e80-b69f-3181a9d17fc1)

## Integrations:
The project integrates Movement labs and eigan layer.

## Architecture of the AVS
- **`AVS contract:`**:
Source Chain  and destination chain will have same EVS Contract deployed on both chains. In our case destination and source chain can be movement chain and arbitrum. In this Contract user / dapp contract will initiate the task on source chain's avs contract. Operator will listen to this task event and operator will send the message and sign it and send it to destination chain via aggregator.

- **`Operators:`**
Sends the message sent by chain A crossmessagecontract to chain B, sign it, and send it to the aggregator.

- **`Aggregator:`**
aggregates BLS signatures from operators and posts the aggregated response to the task manager
For this simple demo, the aggregator is not an operator, and thus does not need to register with eigenlayer or the AVS contract. It's IP address is simply hardcoded into the operators' config.

- **`Task Generator:`**
in a real world scenario, this could be a separate entity, but for this simple demo, the aggregator also acts as the task generator

## Dependencies

You will need [foundry](https://book.getfoundry.sh/getting-started/installation) and [zap-pretty](https://github.com/maoueh/zap-pretty) and docker to run the examples below.
```
curl -L https://foundry.paradigm.xyz | bash
foundryup
go install github.com/maoueh/zap-pretty@latest
```
You will also need to [install docker](https://docs.docker.com/get-docker/), and build the contracts:
```
make build-contracts
```

## Running via make

This simple session illustrates the basic flow of the AVS. The makefile commands are hardcoded for a single operator, but it's however easy to create new operator config files, and start more operators manually (see the actual commands that the makefile calls).

Start anvil in a separate terminal:

```bash
make start-anvil-chain-with-el-and-avs-deployed
```

The above command starts a local anvil chain from a [saved state](./tests/anvil/avs-and-eigenlayer-deployed-anvil-state.json) with eigenlayer and incredible-squaring contracts already deployed (but no operator registered).

Start the aggregator:

```bash
make start-aggregator
```

Register the operator with eigenlayer and incredible-squaring, and then start the process:

```bash
make start-operator
```

> By default, the `start-operator` command will also setup the operator (see `register_operator_on_startup` flag in `config-files/operator.anvil.yaml`). To disable this, set `register_operator_on_startup` to false, and run `make cli-setup-operator` before running `start-operator`.



