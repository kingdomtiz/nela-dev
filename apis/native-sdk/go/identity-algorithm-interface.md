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

### Create an Identity Object (External)

Generate (issue) an identity object and directly return a keystream form of the results

```c
TSK_IdentityIssueEx(pSeed []byte) ([]byte, []byte, []byte, int) 
```

<details>

<summary>Parameters</summary>

* \[ ] BYTE
  * pSeed - The seed information of the specified identity key information \[IN]

</details>

<details>

<summary>Return Values</summary>

* \[ ] BYTE
  * pPublicKeyBuf - identity public key information data stream \[IN/OUT]
* \[ ] BYTE
  * pPrivateKeyBuf - identity private key information data stream \[IN/OUT]
* \[ ] BYTE
  * pKeyID - identity key ID, fixed 20 bytes \[IN/OUT]
* INT
  * KError\_Success success
  * KError\_Other, failed, other errors;

</details>

{% hint style="info" %}
The function of TSK\_IdentityIssueEx is the same as that of TSK\_IdentityIssue. The difference is that it returns the binary stream of public and private keys that can be directly parsed and saved externally. Externally, the TSK\_IdentityIssueEx interface can be used to obtain the public and private key data of the identity key and save it directly or do other processing. In the design of the EDFS-SDK library, the buffer memory is prepared in advance by the external program, and is passed to the EDFS-SDK library together with the buffer length information. The EDFS-SDK library operates directly on the externally prepared buffer. , the release of the buffer is also the responsibility of the external program itself.
{% endhint %}
