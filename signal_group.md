
A private group messaging system enables members of a secret group to securely communicate with each other. This includes the ability to create, then add or remove user(s) to a private group, exchange message visible only to the members and hide as much information as well as metadata relating to any group activity from the outside. 

## Background

### 1. Privacy Preserving Attribute Based Credentials

Identity of a user is linked to a set of *attributes*. For example, a user could have age, marital status, phone-number as attributes. A user is able to authenticate for possessing (being an owner of) some kind of identity. For example, a user could authenticate with a voter card, which certifies his eligibility by ensuring his attribute (say, age & citizenship) assume acceptable values. Here, the voter card is called a *credential*, which is issued and signed by an authority, certifies the user (or holder) of having necessary attributes and can be validated by a verifying authority when necessary. As such, a credential is a set of attributes digitally issued by an *issuer* and later verified of its authenticity & validity by a *verifier*. 

A user can assume different identities and as such a different set of attributes in different contexts. We need privacy preserving property to ensure that only necessary information is presented to the verifier is not provided through a credential and also to avoid credential presentation by the user on different places from being linked together. This anonymity and multi-show unlinkability is safeguarded by technologies such as zero-knowledge proofs. 

Further reading: [Concepts Around Privacy-Preserving Attribute-Based Credentials](https://link.springer.com/chapter/10.1007/978-3-642-55137-6_4)

### 2. Keyed-Verification Anonymous Credentials (KVAC) and Algebraic MAC
Credentials like digital signature signed by an authorized entity (issuer) makes it publicy verifiable i.e. any user with access to issuer's public-key will be able to verify that the credential is valid. In cases where the issuer and the verifier is the same entity (or hold a shared secret), then symmetric key primitives can be used to construct *keyed-verification credentials* through the application of [Message Authentication Codes](https://en.wikipedia.org/wiki/Message_authentication_code) (MAC) which makes these schemes more efficient than their public-key counterparts.

A MAC is an encrypted checksum generated on the underlying message and tagged along with it to ensure integrity and authenticity of the transferred message. It is computed using a secret value, which is shared between sender and recepient(s), and the message to be transferred. The MAC generated and sent by the sender is received and compared with a freshly generated MAC calculated using the same message and shared secret. In order to integrate with zero-knowledge proofs (needed to construct anonymous credentials), we require an algebraic MAC,which is constructed using group operations.

A construction of algebraic MAC involves the following steps:

- KeyGen: choose random scalars as shared secret i.e. sk = (x0, x1)
- MAC(sk, m): choose random generator point U, then tag = (U, U<sup>x0 + x1 * m</sup>). Here, 'm' is the message which is converted to a group element.
- Verify(sk, tag, m): recalculate new tag using U, sk and m. Verification passes if new tag equals transferred tag.

Further reading: [Algebraic MACs and Keyed-Verification Anonymous Credentials](https://eprint.iacr.org/2013/516)

### 3. Sigma Protocol
Sigma Protocol enables proving knowledge of some secret value x such that g<sup>x</sup> = y without discolsing the value of x.
It consists of the following steps in non-interactive setting:
- Commitment: Prover generates a random value r and creates a commitment t = g <sup>r</sup>
- Challenge: Prover hashes g, t and y to get the challenge c = Hash(g, y, t)
- Response: Prover creates response to the challenge as s = r + c * x. The prover sends (c, s) to the verifier. 
- Reconstruct Commitment: Verifier reconstructs t using the equation gˢ = y<sup>c</sup>.t. Thus t = g<sup>s</sup>y<sup>-c</sup>.
- Reconstruct Challenge: Verifier reconstructs challenge c using c = Hash(g, y, t). If this reconstructed challenge c equals the one sent by the prover, the verifier is convinced of prover’s knowledge of x.

Further reading: [Zero Knowledge Proofs with Sigma Protocols](https://medium.com/@loveshharchandani/zero-knowledge-proofs-with-sigma-protocols-91e94858a1fb)

### 4. Verifiable Encryption
### 5. Ristreetto Points
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


