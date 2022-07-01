# Digest Interface

The EDFS-SDK library provides a variety of digest interfaces, such as fast CRC digest, secure MD5 and SHA256 digest algorithms. The EDFS-SDK library uses the MD5 digest internally to sign the required encrypted digital files. Data can be digested externally by using the digest interface provided by the EDFS-SDK library.

EDFS-SDK digests externally specified data through a unified data stream digest method, and supports multiple external calls to digest algorithms to jointly digest a piece of super-long data. The same digest process is identified by a specified cookie.&#x20;

A typical digest process is as follows:

* Joint digests of multiple streams are supported with digest cookie pointer not NULL.
* The cookie is obtained when the digest is started, and the value remains unchanged until the digest is ended.
*   In short, the judgment is based on: when the stream pointer is empty, it means the start of the digest or the end of the digest;

    * When the stream pointer is not null, the stream is digested.
    * When the stream pointer is empty, it is the start digest if the value in pCookie is also NULL, and the end digest if the value in pCookie is not NULL.



Taking the CRC digest algorithm as an example, the typical usage is as follows:

1. Call TSK\_DigestCRC32(0, NULL, pCookie, NULL) to start the digest: the streams (nFlowLen and pFlow) are specified as null (nFlowLen is 0, pFlow is NULL), the digest cookie pointer (pCookie) is not NULL, where the stored cookie is 0, the interface will return a cookie to this pointer after success, and the digest result pointer (pCRC32) is specified as NULL, which means that the digest algorithm is initialized and is ready to start a unified joint digest of multiple streams.
2. Call TSK\_DigestCRC32(nFlowLen, pFlow, pCookie, NULL) for each stream to indicate the digest: The stream (nFlowLen and pFlow) specifies a specific stream digest, the cookie pointer (pCookie) is not NULL, where the stored digest cookie is the digest obtained in step 1 , the cookie digest result pointer (pCRC32) is specified as NULL, which means that the digest algorithm will digest the specified stream.
3. Call TSK\_DigestCRC32(0, NULL, pDigestID, pCRC32) to end the digest and receive the result: the streams (nFlowLen and pFlow) are specified as empty (nFlowLen is 0, pFlow is NULL), and the digest cookie pointer (pCookie) is not NULL, where The stored digest cookie is the digest cookie obtained in step 1, and the digest result pointer (pCRC32) is not NULL, which means that the digest algorithm is terminated and the current (final) digest result is returned.

### Digest CRC32

Iterative support for CRC32 digests of multiple streams.

```c
INT32 TSK_DigestCRC32(UINT32 nFlowLen, BYTE * pFlow, PVOID * pCookie, UINT32 * pCRC32);
```

<details>

<summary>Parameters</summary>

* UINT32
  * nFlowLen - is to specify the size of the stream for which the CRC32 digest is to be calculated \[IN]
* CHAR \*
  * pFlow - is to specify the stream for which the CRC32 digest is to be calculated \[IN]
* VOID \*
  * pCookie - specify the cookie needed for iteration \[IN/OUT]
* UINT32 \*
  * pCRC32 - padded with the final result of the iterative digest, a 32-bit CRC32 value \[OUT]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_Other, failed, other errors;

</details>

{% hint style="info" %}
CRC32 digest is a high-speed but unreliable digest algorithm that can be used to verify data that is used frequently.
{% endhint %}

### Digest MD5

Iterative support for MD5 algorithm digests over multiple streams.

```c
INT32 TSK_DigestMD5(UINT32 nFlowLen, BYTE * pFlow, PVOID * pCookie, BYTE * pMD5);
```

<details>

<summary>Parameters</summary>

* UINT32
  * nFlowLen - is to specify the size of the stream for which the MD5 digest is to be calculated \[IN]
* CHAR \*
  * pFlow - is to specify the stream for which the MD5 digest is to be calculated \[IN]
* VOID \*
  * pCookie - specify the cookie needed for iteration \[IN/OUT]
* CHAR \*
  * pMD5 - filled with the final result of the iteration digest, as a 16-byte array \[OUT]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_Other, failed, other errors;

</details>

{% hint style="info" %}
MD5 digest is a slow but reliable digest algorithm.
{% endhint %}

### Digest SHA256

Iterative support for SHA256 algorithm digests over multiple streams.

```c
INT32 TSK_DigestSha256(UINT32 nFlowLen, BYTE * pFlow, PVOID * pCookie, BYTE * pSHA256);
```

<details>

<summary>Parameters</summary>

* UINT32
  * nFlowLen - is to specify the size of the stream for which the SHA256 digest is to be calculated \[IN]
* CHAR \*
  * pFlow - is to specify the stream for which the SHA256 digest is to be calculated \[IN]
* VOID \*
  * pCookie - specify the cookie needed for iteration \[IN/OUT]
* CHAR \*
  * pSHA256 - filled with the final result of the iteration digest, as a 16-byte array \[OUT]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_Other, failed, other errors;

</details>

{% hint style="info" %}
SHA256 digest is a reliable digest algorithm that can be used for signature verification of arbitrary data.
{% endhint %}
