
A private group messaging system enables members of a secret group to securely communicate with each other. This includes the ability to create, then add or remove user(s) to a private group, exchange message visible only to the members and hide as much information as well as metadata relating to any group activity from the outside. 

## Background

1. Privacy Preserving Attribute Based Credentials

    Identity of a user is linked to a set of *attributes*. For example, a user could have age, marital status, phone-number as attributes. A user is able to authenticate for possessing (being an owner of) some kind of identity. For example, a user could authenticate with a voter card, which certifies his eligibility by ensuring his attribute (say, age & citizenship) assume acceptable values. Here, the voter card is called a *credential*, which is issued and signed by an authority, certifies the user (or holder) of having necessary attributes and can be validated by a verifying authority when necessary. As such, a credential is a set of attributes digitally issued by an *issuer* and later verified of its authenticity & validity by a *verifier*. 

    A user can assume different identities and as such a different set of attributes in different contexts. We need privacy preserving property to ensure that only necessary information is presented to the verifier is not provided through a credential and also to avoid credential presentation by the user on different places from being linked together. This anonymity and multi-show unlinkability is safeguarded by technologies such as zero-knowledge proofs. 

    Further reading: [Concepts Around Privacy-Preserving Attribute-Based Credentials](https://link.springer.com/chapter/10.1007/978-3-642-55137-6_4)

2. Keyed-Verification Anonymous Credentials (KVAC) and Algebraic MAC

    Credentials like digital signature signed by an authorized entity (issuer) makes it publicy verifiable i.e. any user with access to issuer's public-key will be able to verify that the credential is valid. In cases where the issuer and the verifier is the same entity (or hold a shared secret), then symmetric key primitives can be used to construct *keyed-verification credentials* through the application of [Message Authentication Codes](https://en.wikipedia.org/wiki/Message_authentication_code) (MAC) which makes these schemes more efficient than their public-key counterparts.

    A MAC is an encrypted checksum generated on the underlying message and tagged along with it to ensure integrity and authenticity of the transferred message. It is computed using a secret value, which is shared between sender and recepient(s), and the message to be transferred. The MAC generated and sent by the sender is received and compared with a freshly generated MAC calculated using the same message and shared secret. In order to integrate with zero-knowledge proofs (needed to construct anonymous credentials), we require an algebraic MAC,which is constructed using group operations.

    A construction of algebraic MAC involves the following steps:

    - KeyGen: choose random scalars as shared secret i.e. sk = (x0, x1)
    - MAC(sk, m): choose random generator point U, then tag = (U, U<sup>x0 + x1 * m</sup>). Here, 'm' is the message which is converted to a group element.
    - Verify(sk, tag, m): recalculate new tag using U, sk and m. Verification passes if new tag equals transferred tag.

    Further reading: [Algebraic MACs and Keyed-Verification Anonymous Credentials](https://eprint.iacr.org/2013/516)

3. Sigma Protocol

    Sigma Protocol enables proving knowledge of some secret value x such that g<sup>x</sup> = y without discolsing the value of x.
    It consists of the following steps in non-interactive setting:
    - Commitment: Prover generates a random value r and creates a commitment t = g <sup>r</sup>
    - Challenge: Prover hashes g, t and y to get the challenge c = Hash(g, y, t)
    - Response: Prover creates response to the challenge as s = r + c * x. The prover sends (c, s) to the verifier. 
    - Reconstruct Commitment: Verifier reconstructs t using the equation gˢ = y<sup>c</sup>.t. Thus t = g<sup>s</sup>y<sup>-c</sup>.
    - Reconstruct Challenge: Verifier reconstructs challenge c using c = Hash(g, y, t). If this reconstructed challenge c equals the one sent by the prover, the verifier is convinced of prover’s knowledge of x.

    Further reading: [Zero Knowledge Proofs with Sigma Protocols](https://medium.com/@loveshharchandani/zero-knowledge-proofs-with-sigma-protocols-91e94858a1fb)

4. Verifiable Encryption

5. Ristreetto Points
_____________________

## Signal Private Group System
Signal's [paper](https://eprint.iacr.org/2019/1416) on private group system provides a comprehensive explanation on how a private group is set up for message exchanges later.

### Procedure to setup a group

1. Setup Credentials

    *What are credentials?*

    A user possesses a globally unique public identifier (UID), which he can use as an attribute to fetch an *AuthCredential* by requesting the server through a mutually authenticated channel. A user a also possesses ProfileKey, which encodes user's public profile data used to make user's identity more friendly. This ProfileKey can be shared with trusted users, who can then use the ProfileKey as an attribute to fetch *ProfileKeyCredential*. This *ProfileKeyCredential* is used to invite the corresponding user to a private group. As such, each user hold their own *AuthCredential* while they can also hold other users' *ProfileKeyCredential* to serve as a token of the relation between the owner and the holder of *ProfileKeyCredential*. 

    *How are AuthCredentials issued?*

    A credential is a MAC on the attributes that is issued and verified by the server. To get an *AuthCredential*, a user submits his UID, the server then adds validity period of the credential as another attribute of this credential, calculates a MAC tag for these attributes along with a zero-knowledge proof that the computation was done correctly. (ZKP hides the secret server parameters used to calculate the MAC tag) The user can then fetch this credential and verify the proof to ensure the credential is valid. 
    
    *How are ProfileKeyCredentials issued?*

    Getting a *ProfileKeyCredential* is slightly different. *ProfileKey* contains attributes which are not known to the server and as such should not be submitted in the clear. As such, these attributes that need to be hidden are encrypted with ElGammal encryption. But the server is able to issue this credential only if it can verify that the encrypted ProfileKey submitted is not fake. The credential requester proves the veracity of the request by generating a proof that the ProfileKey's hash matches the commitment already placed upon the server by the owner of the ProfileKey. After verifying proof of the request for *ProfileKeyCredential*, the server calculates MAC tag on these encrypted attributes. This MAC tag can, later, be decrypted by the user himself to obtain the clean MAC tag as credential. This procedure of issuing credentials on encrypted attributes is termed *Blind Issuance*. 

2. Registering a group

    A user creates a new group by first creating a random *GroupMasterKey*, used for secure communication between group members only. Public parameters associated to this key is registered on the server. A newly added user uses the *GroupMasterKey* to download and decrypt data relating to the group from the server.

3. Adding a user to the group

    *How to prove that a user can be added to a group?*

    A member who has the permission to add other user(s) has possession of its own *AuthCredentials* and *ProfileKeyCredential* of the user it intends to add. So a user proves that another user can be added to a group by submitting a zero knowledge proof to the verifier (here, server) that it holds *ProfileKeyCredential* for the user. To verify the credential, the server will require information on the attribute but submitting the attribute values in clear will break anonymity. So the request submitted will include ciphertexts of UID and ProfileKey along with a proof that the ciphertext is a deterministic encryption of the required attributes.

    *How is the added user able to access the group?*

    The server takes the credentials mentioned above, verifies the proof and adds the user's UID Ciphertext, ProfileKey Ciphertext and Role to a group. These ciphertexts were encrypted by *GroupMasterKey* which gets passed to the added user privately by the adding user. The new user will now be able to authenticate using his *AuthCredentials* for being part of the group, download necessary information and decrypt them with the symmetric key. 


4. Exchanging Messages
    Despite having a shared group secret, group messages are exchanged such that each user sends encrypted message to every other member of the group individually with end-to-end encryption. Having the group secret enables the sender-receiver pair to access public keys necessary to decipher messages.

5. Removing users
    Following is an excerpt from the Signal's private group [paper](https://eprint.iacr.org/2019/1416) on removing users.
   > A user who has acquired a group’s GroupMasterKey and then leaves the group (or is deleted) retains the ability to collude with the server to encrypt and decrypt group entries. In the current system a new group would have to be created, excluding the removed user(s). An automated or more sophisticated re-keying strategy could also be added as a future extension.
   
   Difficulties of re-keying strategy has been mentioned [here](https://github.com/manishbista28/study/blob/main/key_agreement.md).

### Features
- multi-show unlinkability; how has this been useful ?
- blind-issuance of credentials with ZKP
- privacy preserving with ElGammal Encryption
- unforgeability
- multi-attribute type (scalar,vector or multi-attribute)
- efficient for its group elements size and type (same issuer-verifier)


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




Using prime order groups also means the credential system can use the same elliptic curve as
is used for key agreement and signatures



When using generalized Schnorr proofs in a cyclic group, the types of statements that can be
proven about attributes in G are limited, but we will be able to prove statements about
the plaintext encrypted by an Elgamal-like ciphertext.
- uses curve25519 with Risetto Group
- uses algebraic MAC, KVAC scheme; how does this help ?

