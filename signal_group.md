
A private group messaging service should have
- ability to add and remove users securely
- exchange message visible only to users within the group
- outside members or the server should not be able to draw information from group messages, which can include
which group the user is in, who else is there, is it a group message or a peer-to-peer message

Let's see how Signal does it

| FrontEnd | Backend |
| -------- | ------- |
| GroupCreation: A user creates a group | Creates group visible only to the user and its members |
| GroupInvitation: A user invites another user to the group | Share GroupMasterKey securely |
| Inivited user can now see group information | Download and decrypt with groupMasterKey |
| SecureMessageExchange | one to many message exchanges |
| RemoveUser | GroupKey is not updated |

Let's establish background knowledge of related terms

Background Knowledge Topics

- ElGammal Encryption and its Homomorphic property
- Schnorr Identification Proofs
- Multi-show unlinkability
- Key-Exchange mechanisms and pre-existing secure channels
- KVAC
- Algebraic MACs, credentials (t, U, V)
- Anonymous credential systems (Issue, Verify, Revoke, credential, attributes)

_____________________

## Implementation Details:

Signal's Private Group has the following features:

- multi-show unlinkability; how has this been useful ?
- blind-issuance of credentials with ZKP
- privacy preserving with ElGammal Encryption
- susceptible to collusion from removed members
- issuer and verifier should be same or should share secret
- credential includes server specified params like expiry period of credential
- some data is encrypted with GroupParams to ensure it's visible only to group members
- verifier needs to generate hash from encrypted data to ensure it matches the commitment

multi show unlinkability
credential issuer and verifier - who ? KVAC 
time limited creds
applications of profile keys ?
Algebraic MAC ? certain algebraic MACs can be used to construct an efficient type of anonymous credential.

In a group, membership info is available to all group members but hidden from the service provider or anyone outside of the group. In
the proposed solution, a central server stores the group membership in the form of encrypted entries. Members of the group authenticate to the server in a way that reveals only that they correspond to some encrypted entry, then read and write the encrypted entries.

keyed verification anonymous credential that uses algebraic MAC.
This enables us to encrypt group
data using an efficient Elgamal-like encryption scheme, and to prove in zero-knowledge
that the encrypted data is certified by a credential.

In our new approach, group members encrypt the membership list using a shared
key and store the encrypted entries on a server. This means clients can acquire an up-to-
date view of group membership by simply querying the server, and the server can apply
access-control rules to all group updates

A group member anonymously authenticates by proving ownership of encrypted UID

As part of authentication, users prove to the server
that their encrypted entry is a correct deterministic encryption of some UID

it(the solution) is compatible with the efficient
zero-knowledge proof system we use for credential presentation, allowing us to prove that
ciphertexts are well-formed with respect to a public commitment of the key, and that the
plaintext is an attribute from a credential

A user who has acquired a group’s GroupMasterKey and then leaves the group (or is
deleted) retains the ability to collude with the server to encrypt and decrypt group entries.
In the current system a new group would have to be created, excluding the removed user(s).
An automated or more sophisticated re-keying strategy could also be added as a future
extension


Using prime order groups also means the credential system can use the same elliptic curve as
is used for key agreement and signatures

When users in a Signal group send encrypted messages to the group, they encrypt
and send the message to each group member, individually, with end-to-end encryption.
The server is given no explicit indication of the difference between group and non-group
encrypted messages, apart from traffic analysis.


When using generalized Schnorr proofs in a cyclic group, the types of statements that can be
proven about attributes in G are limited, but we will be able to prove statements about
the plaintext encrypted by an Elgamal-like ciphertext.
- uses curve25519 with Risetto Group
- uses algebraic MAC, KVAC scheme; how does this help ?

List down the steps:

- Setup Parameters
    - System Params
    - ServerSecretParams and ServerPublicParams: issuer key and parameters separate for profile key and authcredentials
- AuthCredential's
    - Attributes contain Hash and Encodings of UID and redemption date (m1, m2 and m3)
    - Response contains an algebraic MAC for the credential, which requester verifies by deriving m1 and m2 from his UID
- ProfileKeyCredential
    - Attributes contain Hash and Encoding of UID and ProfileKey

BlindIssuance
- attributes of profileKeyCredential are not all known to the server
- PFKeyCredRequest will contain encryption of private attributes and a proof that these values match a ProfileKeyCommitment
- PFKeyCommitment is a triple of values (G_j1 M3, ..M4, G..) - Pedersen Commitment for group elements
- server uses homomorphic property to create encrypted MAC from message attributes and pass it to the user (along with proof of correctness)
- user decrypts the MAC to recover PKeyCreds
- User generates ElGammalKeyPair (y, Y), chooses random values r1 and r2, creates output credential (t, U, V) using y

GroupSecretParams: a1, a2, b1, b2
GroupPublicParams: A, B


