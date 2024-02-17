
**Abstract**
Solana having fast execution enviroment should be accessible by every state machine.A vision on having chain agnostic application is being shaped by having an application benefiting from best of the worlds.

This implementation is a proof of concept demonstrating how we can use solana state machine as an execution hub for different XCM instructions from polkadot parachains via communication channel ( IBC/ ISMP ).

**Terminologies**
* *[XCVM](https://wiki.polkadot.network/docs/learn-xcvm)* : Cross Consensus Virtual Machine responsible for executing pre-defined set of composed instructions.
*  *[XCM](https://github.com/paritytech/xcm-format)* : Set of predefined instructions upon which they are executed based on the implementation of XCVM.
*  *[IBC](https://github.com/cosmos/ibc-rs)* : The Inter-Blockchain Communication Protocol (IBC) is an end-to-end, connection-oriented, stateful protocol for reliable, ordered, and authenticated communication between heterogeneous blockchains arranged in an unknown and dynamic topology
*  *[ISMP](https://docs.hyperbridge.network/protocol/ismp/)* : GET and POST based communication protocol between consensus machines. This protocol depends on consensus and state proofs of the communicated state machine.
*  *Weight*: Analogus to Gas, It is the amount of computation measured in picosecond.
*  *Call* : A fucntion to be dispatched.
*  *Location* : Indentifier for the consensus mashine in reference to the origin consensus mashine for the sent XCM instructions. 
---
**Architecture**

![SolanaXCVM](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_2e99729c654a907069e89def7669be03.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1708169321&Signature=GDhFnFqPGX2FS2+KbnE95zHL1wU%3D)

---

**XCVM**

This is the main contract which will be responsible for intepreting and executing pre-defined composed XCM instructions and applying the state changes. This implementation will be a POC as opposed to a fully fledged implementation in Polkadot as it is used for parachain communication. 

The XCVM implementation in Solana will consist of the following parts;
* Registers

    These are responsible for keeping track the origin of the instructions being executed, account signer, the state of execution, error handling and resolution, barriers for execution.
    
            1.   Programme
            2.   Programme Counter
            3.   Error
            4.   Error Handler
            5.   Origin
            6.   Refunded Weight
            7.   Surplus Weight


* Pre-defined XCM instructions

    All the supported instructions to be executed. Instructions while executing they manipulate the registers at each stage of their execution. 
    
            1.   Transact = { origin_kind: OriginKind, require_weight_at_most: Weight, call: DoubleEncoded<Call> }
            2.   QueryResponse = {query_id: Compact<QueryId>, response: Response, max_weight: Weight, querier: Option<Location> }
            3.   SetErrorHandler = (Xcm<Call>)
            4.   ExportMessage = 38 { network: NetworkId, destination: InteriorLocation, xcm: Xcm<()> }
            5.   DescendOrigin = 11 (InteriorLocation)
            
    
    
    

**Encoding & Decoding Scheme**

Solana uses Borsh encoding and decoding scheme, but the messages originating from Parachain will be [scale encoded](https://github.com/paritytech/parity-scale-codec). The XCVM will only understand scale encoded messages, so for this to work, messages will be scale encoded in outer level and every data obtained will be borsh encoded.

**Communication channels and Relayers**

//TODO!

**Contract Structure**

//TODO!

**User Flow**

//TODO!

**Assumptions**

//TODO!
