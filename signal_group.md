
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

    With [verifiable encryption](https://link.springer.com/chapter/10.1007/3-540-44448-3_25), a user is able to prove that an encrypted message possesses a certain attribute. Message attributes can include data that needs to be hidden while issuing and/or presenting credentials. However, the issuer and verifier still needs to be able to verify that the ciphertext is well formed. This is guaranteed with verifiable encryption.

5. Ristretto Groups

    Signal uses Curve25519 for elliptic curve operations, which is not readily usable with ZK Proofs. We need Ristretto groups for this compatibility.
    Excerpt from [ristretto group](https://ristretto.group/)
    > This allows an existing Curve25519 library to implement a prime-order group with only a thin abstraction layer, and makes it possible for systems using Ed25519 signatures to be safely extended with zero-knowledge protocols, with no additional cryptographic assumptions and minimal code changes.

_____________________

## Signal Private Group System
Signal's [paper](https://eprint.iacr.org/2019/1416) on private group system provides a comprehensive explanation on how a private group is set up for message exchanges later.

### Procedure to setup a group

1. Setup Credentials

    *What are credentials?*

    A user possesses a globally unique public identifier (UID), which he can use as an attribute to fetch an *AuthCredential* by requesting the server through a mutually authenticated channel. A user a also possesses ProfileKey, which encodes user's public profile data used to make user's identity more friendly. This ProfileKey can be shared with trusted users, who can then use the ProfileKey as an attribute to fetch *ProfileKeyCredential*. This *ProfileKeyCredential* is used to invite the corresponding user to a private group. As such, each user hold their own *AuthCredential* while they can also hold other users' *ProfileKeyCredential* to serve as a token of the relation between the owner and the holder of *ProfileKeyCredential*. 

    *How are AuthCredentials issued?*

    A credential is a MAC on the attributes that is issued and verified by the server. To get an *AuthCredential*, a user submits his UID, the server then adds validity period of the credential as another attribute of this credential, calculates a MAC tag for these attributes along with a zero-knowledge proof that the computation was done correctly. The user can then fetch this credential and verify the proof to ensure the credential is valid. 
    
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

5. Removing users from the group

    Following is an excerpt from the Signal's private group [paper](https://eprint.iacr.org/2019/1416) on removing users.
   > A user who has acquired a group’s GroupMasterKey and then leaves the group (or is deleted) retains the ability to collude with the server to encrypt and decrypt group entries. In the current system a new group would have to be created, excluding the removed user(s). An automated or more sophisticated re-keying strategy could also be added as a future extension.
   
   Some of the difficulties of re-keying strategy has also been mentioned [here](https://github.com/manishbista28/study/blob/main/key_agreement.md).

### Features

1. Anonymity

    With blind issuance of credentials, the server will not be able to obtain and use any identifying information present in profile data like name and photo. During credential presentation, submitting encrypted ciphertexts of UID and ProfileKeys both for credential verification and record-keeping ensures raw information is not accessible to the server. Furthermore, the MAC tag values are also modified such that the server can not correlate credentials during issuance and verification to extract any information. 

2. Unforgeability

    Algebraic MAC tag values are constructed such that the verifier can confirm that the issued credential was not modified in any way. This ensures an adversary can not create a valid proof for a statement not satisfied by the credential he was issued.

3. Multi-show Unlinkability

    Multi-show unlinkability ensures that the credential you present more than once either on same or different contexts can not be linked together. The credential values (MAC tag) can be modified by a different random parameter (z in the paper) in each submissions making the values presented to the verifier different at each instances. However, the verifier can link different credential presentations if some parameters like attribute encrypted with group key is submitted. As such separate credential presentations are unlinkable unless a separate common value has been repeatedly submitted.

4. Transferrability

    Credentials are transferrable. A different user in possession of the credential and ciphertexts of UID and ProfileKeys can also make the presentation. This ensures users can not be censored based on the origin of presentation requests.

5. Multi-attribute type

    Attributes encoded on MAC can be hidden or plain text making the underlying KVAC protocol suitable for a wider range of applications.

5. Efficiency

    Credentials are in the order of few hundred bytes in size and take few hundred milliseconds to generate or process. 
