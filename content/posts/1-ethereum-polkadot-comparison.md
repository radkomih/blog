---
title: "Comparison between Ethereum and Polkadot's node architecture"
date: 2023-06-10T00:00:00+02:00
draft: false
--- 

Ethereum, a decentralized, open-source blockchain protocol, featuring smart contract functionality, and Polkadot, a multi-chain platform enabling different blockchains to interoperate, both play significant roles in the blockchain space. However, their node architectures present unique benefits and challenges, impacting network resilience, efficiency, and adaptability. So let's explore the design decisions behind the two and discuss each approach. 

*One clarification, all Polkadot comparisons relate to a single chain - relay-chain, or single para-chain, not the whole network of chains which would be a more accurate comparison to Ethereum's network of nodes.*

In contrast to how Ethereum nodes are designed, where each node is composed of two clients, **Execution Client** and **Consensus Client**, Polkadot specification describes the nodes also divided into two - **Host** and **Runtime**, but the **Runtime** in some sense, incorporates both the execution and consensus logic. It is responsible for the business logic of the protocol, while the **Host** provides the necessary resources to the **Runtime** like memory, storage, io, networking, etc. Some important distinctions between the two approaches originate from these facts.

In Ethereum, multiple implementations can work simultaneously. This feature minimizes the risk of protocol-wide bugs affecting the whole network. However, it also means that upgrades to the protocol and clients require extra coordination and effort.

On the other hand, the Polkadot protocol is upgradeable via the consensus mechanism, and the **Runtime** (Wasm blob) is stored on-chain, so there is only one "active" **Runtime** implementation at a time, thus, having the benefit of supporting forkless upgrades, but also being a single point of failure. Its design does not allow multiple **Runtime** implementations to work at the same time, so if there is a bug in the **Runtime**, the whole network is affected. Moreover, depending on the bug, if it is in the block production logic, the upgrade/fix needs to happen manually in a coordinated fashion. 

Another drawback is that even though the **Host** is considered to be mostly static, in practice, there is still a need for coordination between different **Host** implementations to account for changes in the current **Runtime**.

Currently, in Ethereum, there are multiple **Execution** and **Consensus** implementations working. In Polkadot, there are Rust, C++, and Go **Host** implementations, but only Rust's implementation is used in production. For the **Runtime**, there is only one written in Rust. Using different **Host** implementations helps, but having a single **Runtime** implementation, where is most of the business logic, compared to Ethereum's multiple implementations, makes the Polkadot network more vulnerable to bugs and exploits. 

What adds even more to the complexity of supporting a diverse set of alternative **Runtime** implementations, is the choice to use WebAssembly as the target for the **Runtime** compared to Ethereum, which implements its own simple, stack-based, bytecode virtual machine (EVM), aligned towards its specific requirements and goals. Instead of using a general-purpose virtual machine like Webassemblly, the EVM is optimized for the security and reliability of smart contracts.

In contrast, WebAssembly is a low-level bytecode instruction format for a typed stack-based virtual machine that is designed to be portable and general-purpose, but Polkadot targets Webassembly MVP (no bulk memory operations, reference types, and multi-value returns) that is not well-supported by many languages and most of the time, languages compile to Webassemblly depending on some custom APIs.
It has single, contiguous, byte-addressable linear memory, accessed by all memory operators, great for languages with manual, reference counting, or ownership memory model (C++, Rust), but GC-managed languages require a bit more work to port it. 
It has an implicit stack with no stack introspection to scan the roots, from where it starts the scanning for references, which is problematic for garbage-collected languages like Go, Java, etc. It requires to use mirrored shadow stack in the linear memory, pushed/popped along with the machine stack, thus making it less efficient.

Another pain point is that, by specification, the **Host** is expected to manage the memory of the **Runtime** by providing a memory allocator for all heap allocations. In case, one wishes to write the **Runtime** in a language that manages its memory by itself via a garbage collector, the GC needs to be aware of the **Host** memory allocator and cooperate with it, so all allocations are tracked correctly and there are no memory collisions, which is a non-trivial task and requires a lot of work to be done in the GC implementation and the compiler.

In summary, Ethereum's and Polkadot's node architectures both offer distinct benefits and challenges; Ethereum's multiple implementations foster resiliency, but complicate upgrades, while Polkadot's on-chain **Runtime** enables seamless upgrades, but introduces potential single points of failure and coordination complexities.
