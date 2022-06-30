# Account Interface

User related; the library only supports single one account to login.

Each identity has a unique 20-byte identity ID, which can be generated externally to meet EDFS-SDK requirements through a specific algorithm at the time of user generation.

Each account may have multiple identities, but only one can represent the user identity of the account (representing this person).

In the EDFS protocol, identity is a very special concept. Identity is separate from account, and account represents a user. In simple terms, identity is the authority that a user has. For example, a user joins group A, community B, and uses an application C. Then this user should be visible to all group A, community B, and application C's internal encrypted files, and the user's own private files. Therefore, this user has at least 4 identities. Correspondingly, it can be said that the user has at least the keys of the asymmetric encryption algorithms corresponding to these 4 identities. You can use the keys of these identities to try to decrypt the encrypted files belonging to these identities. The user can then use these encrypted files.

However, when the user logs in, the user's own identity is used to log in to the EDFS-SDK library, and other identities are configured to the user through the application's own management.

### Login

Login to an account.

```
INT32 TSK_LoginUser(BYTE* nIdentityID);
```

<details>

<summary>Parameters</summary>

* CHAR \*
  * nIdentityID - the corresponding internal ID representing the unique identity of the user

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success
  * KError\_MultiUser, failed, there are other logged in users
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers)

</details>

### Logout

Log out of the current account.

```
INT32 TSK_LogoutUser();
```

<details>

<summary>Parameters</summary>

none

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success

</details>

### Set Identity

Set the identity held by the current user of the EDFS-SDK library.

```
INT32 TSK_SetDefaultCipher(BYTE * tagCipher);
```

<details>

<summary>Parameters</summary>

* CHAR \*
  * tagCipher - the algorithm tag to be obtained (that is, the GUID label of the algorithm), which is fixed to 16 bytes (this parameter can be NULL, indicating that the default algorithm is specified) \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_CipherError, failed, the algorithm could not be found;
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers);

</details>

{% hint style="info" %}
The default algorithm is set through this interface on the application layer interface.

If the specified algorithm tag is NULL during encryption, the default algorithm will be used for encryption.

The default encrypt
{% endhint %}



### g the Default Algorithm

Set the current default algorithm.

```
INT32 TSK_SetDefaultCipher(BYTE * tagCipher);
```

<details>

<summary>Parameters</summary>

* CHAR \*
  * tagCipher - the algorithm tag to be obtained (that is, the GUID label of the algorithm), which is fixed to 16 bytes (this parameter can be NULL, indicating that the default algorithm is specified) \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_CipherError, failed, the algorithm could not be found;
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers);

</details>

{% hint style="info" %}
The default algorithm is set through this interface on the application layer interface.

If the specified algorithm tag is NULL during encryption, the default algorithm will be used for encryption.

The default encrypt
{% endhint %}

