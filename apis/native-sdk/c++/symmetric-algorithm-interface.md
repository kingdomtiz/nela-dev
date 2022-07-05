# Symmetric Algorithm Interface

EDFS-SDK adopts the standard electronic envelope mode to protect digital files in the whole life cycle, uses symmetric encryption algorithm for file data, and uses a random key in the form of one-text-one-cipher encryption. For the random key of the symmetric encryption algorithm, use the public key of the asymmetric algorithm that represents the user's identity to encrypt it, and the encrypted data is called an eseal structure, which is stored in the eseal identity area of the file encryption header. When users use encrypted files, they must first try to decrypt each eseal structure in the eseal identity area with the private key of their own identity. After the encryption is successful, the random key of the symmetric algorithm used to encrypt the real file can be obtained. Finally get the real data of the file.

​The electronic envelope mechanism of EDFS-SDK organically combines the high efficiency of symmetric encryption algorithm and the high security of asymmetric encryption algorithm to provide users with a high-efficiency and high-security data life-cycle protection scheme.

​The symmetric encryption algorithm management module of the EDFS-SDK library is designed with scalable components. The library integrates AES256, DES3, GOST, MARS, SEAL and other common symmetric encryption algorithms by default to encrypt file data. APP developers or users can choose the symmetric encryption algorithm according to their needs and preferences. In addition, the EDFS-SDK library also supports adding more symmetric encryption algorithms to the library to provide users with more choices.

### Get Loaded Algorithms

Get all currently loaded algorithm descriptions.

```c
INT32 TSK_GetCipherDesc(BYTE * pDescBuf, INT32 nDescBufLen);
```

<details>

<summary>Parameters</summary>

* BYTE \*
  * pDescBuf \[IN/OUT] - An externally prepared buffer for receiving reports (it is recommended to apply for an 8K (8192) byte length buffer the first time).
* INT32
  * nDescBufLen \[IN] - The buffer length prepared externally to receive the report (the recommended first length is 8192 bytes).

</details>

<details>

<summary>Return Values</summary>

* INT32
  * Returns the actual length of the report. If the return value is greater than the buffer length passed in from the outside, no operation will be performed on the buffer, and the outside needs to re-apply for a buffer large enough according to the actual length, and obtain it again.

</details>

{% hint style="info" %}
On the application layer interface, the loaded algorithm is obtained and displayed through this interface.

The returned data is a collection of description information of all symmetric encryption algorithms supported by the TSK platform. The main format is as follows:

​(Total number of symmetric encryption algorithms supported: UINT32 data of 4 bytes length)

* The first encryption algorithm description&#x20;
  * The 16-byte encryption algorithm tag identifier is used to uniquely identify an algorithm, and subsequent interfaces use this tag to represent the algorithm Algorithm name string, unlimited length, with the final trailing 0 of the string as the end identifier The algorithm describes the information string in one sentence, with unlimited length, with the final trailing 0 of the string as the end identifier
* Description of the second encryption algorithm&#x20;
  * The 16-byte encryption algorithm tag identifier is used to uniquely identify an algorithm, and subsequent interfaces use this tag to represent the algorithm Algorithm name string, unlimited length, with the final trailing 0 of the string as the end identifier The algorithm describes the information string in one sentence, with unlimited length, with the final trailing 0 of the string as the end identifier
* More encryption algorithm descriptions

​Through the TSK\_GetCipherDesc interface, the list of all symmetric encryption algorithms supported by the current EDFS-SDK library can be obtained externally. It is more often used for interface display, and all currently supported algorithms are listed and displayed, so that users can choose the encryption algorithm to be used.
{% endhint %}

### Get Algorithm Details

Get a specified algorithm's details.

```c
INT32 TSK_GetCipherInfo(BYTE * tagCipher, UINT16 * pKeyLength, UINT16 * pBlockSize);
```

<details>

<summary>Parameters</summary>

* BYTE \*
  * tagCipher \[IN] - The algorithm tag to be obtained (that is, the GUID label of the algorithm), which is fixed to 16 bytes (this parameter can be NULL, indicating that the default algorithm is specified).
* UINT16 \*
  * pKeyLength \[IN/OUT] - Returns the key length (in bytes) required by the algorithm.
* UINT16 \*
  * pBlockSize \[IN/OUT] - The block size required by the algorithm (calculated in bytes, so the sequence algorithm returns 1).

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_CipherError, failed, the algorithm could not be found
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

{% hint style="info" %}
The application layer can use this interface to determine the key size and packet size of the algorithm when constructing the stream processing object. It can also be used for interface display to display the detailed information of an encryption algorithm.
{% endhint %}

### Setting the Default Algorithm

Set the current default algorithm.

```c
INT32 TSK_SetDefaultCipher(BYTE * tagCipher);
```

<details>

<summary>Parameters</summary>

* BYTE \*
  * tagCipher \[IN] - The algorithm tag to be obtained (that is, the GUID label of the algorithm), which is fixed to 16 bytes (this parameter can be NULL, indicating that the default algorithm is specified).

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_CipherError, failed, the algorithm could not be found;
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers);

</details>

{% hint style="info" %}
The default algorithm is set through this interface on the application layer interface.

If the specified algorithm tag is NULL during encryption, the default algorithm will be used for encryption.

The default encryption algorithm set by the upper layer is valid for a single startup. After exiting the EDFS-SDK library, the user's settings will be restored after restarting. Therefore, if the external APP needs to use this function, it can record the user's settings, active call to set the default algorithm when started again.
{% endhint %}
