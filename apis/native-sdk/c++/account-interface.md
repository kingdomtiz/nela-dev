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
INT32 TSK_SetHoldIdentity(UINT32 nListVerb,BYTE* nIdentityID, PermissionInfo nPermission, int nLastTime,BYTE * pKeyBuf,INT32 nKeyLen);
```

<details>

<summary>Parameters</summary>

* UINT32
  * nListVerb - a standard list manipulation verb, no LIST\_VERB\_MODIFY semantics \[IN]
* CHAR \*
  * nIdentityID - the ID of this identity \[IN]
* PermissionInfo
  * nPermission - the permission corresponding to the identity, the identity held by the user has all permissions by default, and needs to be set externally \[IN]
* INT
  * nLastTime - the expiration date of the file when the public key of the identity is used for default encryption, please be sure to pass in -1, the creator of the file has the permission to use it indefinitely \[IN]
* CHAR \*
  * pKeyBuf - the key binary format information of the identity, byte unit \[IN]
* INT32
  * nKeyLen - the length of the key binary format information of the identity, in bytes \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success;
  * KError\_NoUser, failed, there is no currently logged in user;
  * KError\_SyntaxError, failure, syntax error (such as unsupported verb, etc.);
  * KError\_MultiObject, failure, semantic error, the Key already exists (the identity ID already exists);
  * KError\_CantFindObject, failure, semantic error, delete the key that does not exist (the identity ID does not exist);
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers);

</details>

{% hint style="info" %}
Check the below for more information on nListVerb and the PermissionInfo structure.
{% endhint %}

{% tabs %}
{% tab title="nListVerb" %}
nListVerb is defined as the following

```
#define LIST_VERB_DEFAULT 1 // Set the default default behavior
#define LIST_VERB_CLEAR 2   // Clear the match list
#define LIST_VERB_ADD 3     // Add matching items
#define LIST_VERB_DELETE 4  // Delete matches
#define LIST_VERB_MODIFY 5  // Modify matches
```

The EDFS-SDK library uses a linked list structure for user identity management, and external user identity operations are essentially various operations on the management linked list.

The identity held by the former user is a list, which is usually managed by LIST\_VERB\_CLEAR/LIST\_VERB\_ADD/LIST\_VERB\_DELETE.

When LIST\_VERB\_ADD all parameters obviously need to be supplied.

When LIST\_VERB\_DELETE, the parameter obviously only needs to provide nIdentityID.

LIST\_VERB\_DEFAULT is to set user identity;

When wishing to set (or change) a default identity, the identity should already be in the list of held identities, so the parameter only needs to provide nIdentityID.

When you want to clear the default identity, you should specify nIdentityID as IDENTITY\_NULL, and other parameters do not need to be provided.
{% endtab %}

{% tab title="PermissionInfo" %}
The PermissionInfo structure is the definition of identity permissions by the EDFS-SDK library. In essence, a 2-byte integer is used to represent the permissions owned by an identity. Currently, the EDFS-SDK library defines the following two permissions:

```
typedef struct _PermissionInfo{
    // Whether it is the Owner: 0 means non-Owner, 1 means Owner
    UINT16 bOwner : 1;
    // Whether it has distribution permission: 0 means no, 1 means yes.    
    UINT16 bAdjust : 1;    
}PermissionInfo, *PPermissionInfo;
```

In the EDFS-SDK library, bOwner means that the user is the owner of the encrypted digital file, and has all permissions to the encrypted digital file, including file reading, writing, sharing, decryption, etc.

Other non-Owner identities have only read access to encrypted digital files.

bAdjust is a special permission that allows identities with this permission to share encrypted files. That is, it has the right to add another identity to the ESeal identity area of the encrypted digital file to construct a new ESeal structure, and also has the right to delete or modify the eseal structure in the existing ESeal identity area.

In language development that does not support structures, you can simply pass in a 2-byte integer as a permission. -1 means the identity has all permissions, 1 means Owner permissions, 2 means share permissions.
{% endtab %}
{% endtabs %}

### Set the Default Zone

Set default encrypted paragraphs (zone).

```
INT32 TSK_SetDefaultZone(INT64 nLen);
```

<details>

<summary>Parameters</summary>

* INT32
  * nLen - the size (byte count) of the default encrypted paragraph to set \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success, success;
  * KError\_Other, other errors, usually memory allocation errors (or insufficient buffers);

</details>

{% hint style="info" %}
The default encrypted paragraph is fixed from the file header to the specified size of the file paragraphs.

The paragraph size is 0, which is effectively equal to unencrypted; the paragraph size is -1, which is effectively equal to encrypting the entire file.

Specifying the default encryption section is not necessary in some scenarios , but it is necessary in scenarios that require the library to automatically perform certain encryption and decryption actions (such as automatic file system encryption and decryption).

Paragraph encryption is EDFS-SDK's special support for file encryption. By default, EDFS-SDK's encryption of digital files is full-text encryption, that is, re-encoding and encryption according to the specified algorithm is performed on all data of the file.

But the outside can set the paragraph encryption configuration according to the needs, so that the EDFS-SDK can be more efficient in the process of encrypting or decrypting the file data. The basis of paragraph encryption is that almost all formats of digital files currently have format requirements for file data. By encrypting the data with a specified length starting from the file header, it is possible to re-encode only a part of the data, which can destroy the application software's requirements for the data file format. It is recommended that if you use the paragraph encryption function provided by EDFS-SDK, the paragraph configuration should not be less than 1MB, or should be even higher to achieve more security.
{% endhint %}

