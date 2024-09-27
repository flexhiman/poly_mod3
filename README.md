# zkSNARK Logical Gate Circuit

This project implements a zkSNARK circuit using the **Circom** programming language. The circuit performs logical operations, where the goal is to prove knowledge of inputs **A = 0** and **B = 1** that yield a **0** output.

## Project Overview

The circuit implements the following logical operations:
- **AND** gate
- **NOT** gate
- **OR** gate

These gates combine to ensure that the output **Q** is `0` when inputs are **A = 0** and **B = 1**.

## Tools Used
- **Circom 2.0.0**: For writing and compiling the zkSNARK circuit.
- **Hardhat**: To compile, deploy, and test the Solidity verifier contract.
- **Ethers.js**: For generating and verifying proofs.
- **Sepolia/Mumbai Testnet**: For deploying the Solidity verifier.

## Circuit Design

The circuit logic is designed using AND, NOT, and OR gates. Here's the `circuit.circom` code:

```circom
pragma circom 2.0.0;

// This circuit checks that the output Q is a result of the logical gates AND, NOT, and OR
template node () {  
    signal input a;  
    signal input b; 

    signal x;  
    signal y; 

    signal output Q; 
    
    component andGate = AND();
    component notGate = NOT();
    component orGate = OR();

    andGate.A <== a;
    andGate.B <== b;
    x <== andGate.out;
  
    notGate.A <== b;
    y <== notGate.out;

    orGate.A <==x;
    orGate.B <==y;
    Q <== orGate.out; 
}

template AND() {
  signal input A;
  signal input B;
  signal output out;

  out <== A * B;
}

template NOT() {
  signal input A;
  signal output out;

  out <== 1 + A - 2 * A;
}

template OR() {
  signal input A;
  signal input B;
  signal output out;

  out <== A + B - A * B;
}

component main = node();
```

install these:
```npm install circom```
```npm install dotenv```
to deploy and verify: ```npx hardhat run scripts/deploy.ts --network sepolia```

Now copy the address provided after deploying and it balance to verify the transaction.
