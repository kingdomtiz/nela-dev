# LocalService Interface

The EDFS-SDK library is embedded the local HTTPLocalServer module, which supports external use of standard HTTP requests to obtain the real data of an encrypted file, and supports insensitive decryption of an encrypted file through the browser without the EDFS-SDK real-time file system kernel driver environment.

### Start LocalService

Start the HttpLocalServer module inside EDFS-SDK.

```c
TSK_LocalServer_Start(port int) int 
```

<details>

<summary>Parameters</summary>

* INT
  * nPort - the port specified by LocalServer \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT
  * Return the result of the operation, refer to the definition of the return result of the TSK library

</details>

{% hint style="info" %}
In the development of NativeAPP without integrated EDFS kernel driver module, the local LocalServer is the only way to provide transparent and insensitive use of encrypted data files.
{% endhint %}

### Stop LocalService

Stop HttpLocalServer module inside EDFS-SDK.

```c
  TSK_LocalServer_Stop() 
```

<details>

<summary>Parameters</summary>

none

</details>

<details>

<summary>Return Values</summary>

none

</details>

### Set the System Time

```c
TSK_LocalServer_SetSystemTime(nTime int) 
```

<details>

<summary>Parameters</summary>

* INT
  * nTime - the system time set externally. Set the time as the number of seconds since midnight on January 1, 1970 (4-byte integer).

</details>

<details>

<summary>Return Values</summary>

none

</details>

### Get the Session Key

Get the current session's random key

```c
TSK_LocalServer_GetSessionKey(pSessionKey []byte) bool
```

<details>

<summary>Parameters</summary>

* \[ ] BYTE
  * pSessionKey, externally prepared buffer area for receiving SessionKey \[IN/OUT]

</details>

<details>

<summary>Return Values</summary>

* BOOL

</details>

{% hint style="info" %}
The SessionKey is a fixed 20-byte length information, which is used for LocalSever verification. After the library is started, the external must obtain the current SessionKey. The request to the LocalServer must bring the obtained SessionKey for the LocalServer to determine whether the request is valid.

For the convenience of external use, pSessionKey has been fixed as a visible string, and a trailing 0 has been added, so the actual session key is only 19 bytes.
{% endhint %}
