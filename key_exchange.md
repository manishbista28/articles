In public key cryptography, each user holds a key pair (public and private key) which he can use in unison with the recepient's public key to exchange messages. Such exchanges involve an initial step called a key agreement protocol (like Diffie-Hellman protocol) where the parties agree upon a shared key that can, then, be used for message encryption and decryption. This is the basic definition of key agreement protocol. Seemingly simple yet the value of this key agreement step, its variations and use cases implore a closer look to realize its importance.

## Terms of variance
This key exchange is done in such a way that none of the involved parties have an unequal influence upon the final shared key generated.
But what makes this worth a deeper look Modern communication systems use variations on top of this approach based upon cryptographic security guarantees, message throughput, liveness of communicating endpoints, practical usecases and compatibility of the protocol with other parts of the system. For example, 
1. symmetric keys are useful when payloads can be arbitrarily long and the computational cost of encryption/decryption has to be low
2. asymmetric cryptography is directly used without shared keys when public verifiability and security is necessary even at the cost of high computational cost
3. asynchronous key exchange protocols like X3DH are useful when both ends of the party may not be available for key agreement at the same time
4. for systems like blockchains where the encrypted messages are publicly readable, protocols enabling forward secrecy and post compromise security may be essential
5. for systems where message has to be broadcasted to a group, using a shared key to send a message that's readable by all recepients in the group is more efficient than performing multiple one to one exchanges with separate keys.

## X3DH
X3DH (or Extended Triple Diffie Hellman) protocol is a key agreement protocol used by secure messaging applications like Signal. It is designed for asynchronous settings where one user (“Bob”) is offline but has published some information to a server. Another user (“Alice”) wants to use that information to send encrypted data to Bob, and also establish a shared secret key for future communication. Compared to standard DH key agreement protocol, it uses a set of keys (identity key, ephemeral key and one time prekeys).
Before or after key agreement, the parties must find a way to authenticate each other. This can be done by providing an authentication based credential encrypted with a shared key during the key agreement. 
After the shared key is established, secure payload transfers can be done with protocols like Double Ratchet algorithm.
The prekey bundle saved on the server acts as an invitation for key exchange. 
Its resilience against key compromise is attractive. Can be used even for anonymous voting.

## Messaging Layer Security
used to establish shared key in a group
handles group management
key sharing is not something you do just once, has to be repeated for safety or group management

## Authentication with ABC, NFT and ZKP
Attribte Based Credential
Account Abstraction
Token gated
Zero Knowledge Proofs 
