---
description: Identity Algorithm (Asymmetric Encryption Algorithm) Interface
---

# Identity Algorithm Interface

The EDFS-SDK library uses the ECC256 elliptic curve encryption algorithm by default as the asymmetric encryption algorithm in the electronic envelope mode. The selected curve is the standard SECP256K1 parameter. The detailed parameters are as follows:

_**prime**：FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEFFFFFC2F_

_**A**：0000000000000000000000000000000000000000000000000000000000000000_

_**B**：0000000000000000000000000000000000000000000000000000000000000007_

_**order**：FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141_

_**Gx**：79BE667EF9DCBBAC55A06295CE870B07029BFCDB2DCE28D959F2815B16F81798_

_**Gy**：483ADA7726A3C4655DA4FBFC0E1108A8FD17B448A68554199C47D08FFB10D4B8_

### Create an Identity Object

Generate (issue) an identity object.

```c
INT32 TSK_IdentityIssue(IdentityObject * pIdentity, BYTE * pSeed, UINT32 nSeedLen);
```

<details>

<summary>Parameters</summary>

* IdentityObject \*
  * pIdentity \[OUT] - The obtained identity object (opaque semantics).
* BYTE \*
  * pSeed \[IN] - The seed information of the specified identity key information.
* UINT32
  * nSeedLen \[IN] - The length of the seed information of the specified identity key information, in bytes.

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, failed, other errors

</details>

{% hint style="info" %}
The identity object is an opaque object entity, and the outside does not need to know its internal composition.

Only supports ECC 256K1 identity If the seed information is specified externally, the generated identity information will be consistent, and the seed information is mandatory to be 32 bytes in length. The outside can use a fixed seed information for the identity of the same user through a specific algorithm design, and the user identity generated by calling the method identity interface multiple times will be fixed to support the distributed program design and development process.

The identity key information of the user is always stored only in the device trusted by the user. Only the public key corresponding to the identity key is disclosed to the public to realize the secure encrypted digital file sharing function based on the EDFS protocol.
{% endhint %}

### Create an Identity Object (External)

Generate (issue) an identity object and directly return a keystream form of the results

```c
INT32 TSK_IdentityIssueEx(BYTE * pSeed, UINT32 nSeedLen, BYTE * pPublicKeyBuf, UINT32 & nPublicKeyLen, BYTE * pPrivateKenBuf, UINT32 & nPrivateKeyLen, BYTE * pKeyID);
```

<details>

<summary>Parameters</summary>

* BYTE \*
  * pSeed \[IN] - The seed information of the specified identity key information.
* UINT32
  * nSeedLen \[IN] - The length of the seed information of the specified identity key information, in bytes.
* BYTE \*
  * pPublicKeyBuf \[IN/OUT] - Identity public key information data stream.
* UINT32 &
  * nPublicKeyLen \[IN/OUT] - The length of the external public key buffer.
* BYTE \*
  * pPrivateKeyBuf \[IN/OUT] - Identity private key information data stream.
* UINT32 &
  * nPrivateKeyLen \[IN/OUT] - The length of the private key buffer passed in from the outside.
* CHAR \*
  * pKeyID \[IN/OUT] - Identity key ID, fixed 20 bytes.

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, failed, other errors

</details>

{% hint style="info" %}
The function of TSK\_IdentityIssueEx is the same as that of TSK\_IdentityIssue. The difference is that it returns the binary stream of public and private keys that can be directly parsed and saved externally. Externally, the TSK\_IdentityIssueEx interface can be used to obtain the public and private key data of the identity key and save it directly or do other processing. In the design of the EDFS-SDK library, the buffer memory is prepared in advance by the external program, and is passed to the EDFS-SDK library together with the buffer length information. The EDFS-SDK library operates directly on the externally prepared buffer. , the release of the buffer is also the responsibility of the external program itself.
{% endhint %}

### Release an Identity Object

Release an identity object.

```c
INT32 TSK_IdentityFree(IdentityObject identity);
```

<details>

<summary>Parameters</summary>

* IdentityObject
  * identity \[IN] - The identity object to release (opaque semantics).

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success

</details>

{% hint style="info" %}
Corresponding to the TSK\_IdentityIssue function, an opaque semantic user identity is generated externally through the TSK\_IdentityIssue function. After use, the Free interface needs to be called to release the memory space occupied by the opaque semantic user identity object, following the principle of whoever applies, whoever releases.
{% endhint %}

### Export an Identity Object

Export an identity object in binary form according to the specified nAction(public or private key).

```c
INT32 TSK_IdentityExport(IdentityObject identity, BYTE nAction, UINT32 BufLen, BYTE * Buf, UINT32 * pWrittenLen);
```

<details>

<summary>Parameters</summary>

* IdentityObject
  * identity \[IN] - The target identity object (opaque semantics).
* BYTE
  * nAction \[IN] - The action selection:
    * 1 means public key,
    * 2 means private key,
    * 3 means key identity ID
* UINT32
  * BufLen \[IN] - The receive buffer size of the data to output.
* BYTE \*
  * Buf \[OUT] - The receive buffer of the data to be output.
* UINT32 \*
  * pWrittenLen \[IN/OUT] - The size of the actual output data returned.

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

{% hint style="info" %}
Externally, the public key or private key corresponding to the identity can be exported from the specified user identity object with opaque semantics through the TSK\_IdentityExport interface. However, the prerequisite is that the opaque semantic object itself contains two kinds of information. According to the rules of the ECC encryption algorithm, if the identity object contains private key information, the binary stream data of the public key or private key can be arbitrarily derived. If the identity object only contains public key information, only the binary stream data of the public key can be exported.
{% endhint %}

### Import an Identity Object

Recover identity objects from reading information from data.

```c
INT32 TSK_IdentityImport(BYTE Action, UINT32 BufLen, BYTE * Buf, IdentityObject * pIdentity);
```

<details>

<summary>Parameters</summary>

* BYTE
  * Action \[IN] - The action selection:
    * 1 means public key
    * 2 means private key
* UINT32
  * BufLen \[IN] - The externally prepared buffer size.
* BYTE \*
  * Buf \[OUT] - An externally prepared identity key binary stream data.
* IdentityObject \*
  * pIdentity \[OUT] - The restored identity object (opaque semantics).

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

{% hint style="info" %}
Corresponding to the TSK\_IdentityExport interface, import the binary stream data of a key passed in from the outside and restore it to an identity object with opaque semantics.
{% endhint %}

### Encryption/Signing with Identity Object

Perform encryption and/or signing operations using identity objects.

```c
INT32 TSK_IdentityEncrypt(IdentityObject identity, BYTE cryptAction, UINT32 nSrcFlowLen, BYTE * pSrcFlow, UINT32 nTarFlowLen, BYTE * pTarFlow, UINT32 * pTarFlowReturnLen);
```

<details>

<summary>Parameters</summary>

* IdentityObject
  * identity \[IN] - The identity object (opaque) being used.
* BYTE
  * cryptAction \[IN] - The action selected:
    * 1 means public key
    * 2 means private key
* UINT32
  * nSrcFlowLen \[IN] - Specifies the buffer length of the original stream.
* BYTE \*
  * pSrcFlow - \[IN] Specifies the raw stream buffer.
* UINT32
  * nTarFlowLen \[IN/OUT] - Specifies the buffer length of the target stream after receiving processing.
* CHAR \*
  * pTarFlow \[IN/OUT] - Specifies the buffer area for receiving the processed target stream.
* UINT32 \*
  * pTarFlowReturnLen \[OUT] - The length of the returned target stream.

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

{% hint style="info" %}
Based on the characteristics of the ECC256 algorithm, each ECC256 encryption is in units of 64 bytes, which are transparent to the outside world.

​Data of any length can be passed in from the outside, and the interface will encrypt the data in 64-byte cycles and return the total encrypted data to the external, the buffer for receiving the encrypted result should be of sufficient length and prepared in the ratio of original data/encrypted data 64/154.

​The ECC encryption algorithm supports the use of public or private keys to perform encryption operations on data. Specifying the public key for encryption is to perform an encryption operation on the data to be encrypted, and specifying the private key for encryption is actually to perform a digital signature operation on the encrypted data.

​If the incoming identity object only contains public key information, the private key encryption operation cannot be supported. If the incoming identity object contains private key information, both public and private key encryption/signature operations are supported.
{% endhint %}

### Decryption/Verification with Identity Objects

Perform decrypt/verify operations using identity objects.

```c
INT32 TSK_IdentityDecrypt(IdentityObject identity, BYTE cryptAction, UINT32 nSrcFlowLen, BYTE * pSrcFlow, UINT32 nTarFlowLen, BYTE * pTarFlow, UINT32 * pTarFlowReturnLen);
```

<details>

<summary>Parameters</summary>

* IdentityObject
  * identity \[IN] - The identity object (opaque) to be used.
* BYTE
  * cryptAction \[IN] - The operation selection:
    * 1 means decryption with the public key (sign verification)
    * 2 means decryption with the private key
* UINT32
  * nSrcFlowLen \[IN] - Specifies the buffer length of the original stream.
* BYTE \*
  * pSrcFlow \[IN] - Specifies the raw stream buffer.
* UINT32
  * nTarFlowLen \[IN/OUT] - Specifies the buffer length of the target stream after receiving processing.
* BYTE \*
  * pTarFlow \[IN/OUT] - Specifies the buffer area for receiving the processed target stream.
* UINT32 \*
  * pTarFlowReturnLen \[OUT] - The length of the returned target stream.

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

{% hint style="info" %}
The buffer to receive the decrypted result should be of sufficient length, in the ratio of original data/decrypted data 154/64.
{% endhint %}
