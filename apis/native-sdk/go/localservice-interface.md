# LocalService Interface

The EDFS-SDK library is embedded the local HTTPLocalServer module, which supports external use of standard HTTP requests to obtain the real data of an encrypted file, and supports insensitive decryption of an encrypted file through the browser without the EDFS-SDK real-time file system kernel driver environment.

### Start LocalService

Start the HttpLocalServer module inside EDFS-SDK.

```c
INT32 TSK_LocalServer_Start(INT32 nPort);
```

<details>

<summary>Parameters</summary>

* INT32
  * nPort, the port specified by LocalServer \[IN]

</details>

<details>

<summary>Return Values</summary>

* INT32
  * Return the result of the operation, refer to the definition of the return result of the TSK library

</details>

{% hint style="info" %}
In the development of NativeAPP without integrated EDFS kernel driver module, the local LocalServer is the only way to provide transparent and insensitive use of encrypted data files.
{% endhint %}

### Stop LocalService

Stop HttpLocalServer module inside EDFS-SDK.

```c
 void TSK_LocalServer_Stop();
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
void TSK_LocalServer_SetSystemTime(INT32 nTime);
```

<details>

<summary>Parameters</summary>

* INT32
  * nTime - the system time set externally. Set the time as the number of seconds since midnight on January 1, 1970 (4-byte integer).

</details>

<details>

<summary>Return Values</summary>

none

</details>

### Get the Session Key

Get the current session's random key

```c
bool TSK_LocalServer_GetSessionKey(char pSessionKey[20]);
```

<details>

<summary>Parameters</summary>

* CHAR
  * pSessionKey, externally prepared buffer area for receiving SessionKey \[IN/OUT]

</details>

<details>

<summary>Return Values</summary>

none

</details>

{% hint style="info" %}
The SessionKey is a fixed 20-byte length information, which is used for LocalSever verification. After the library is started, the external must obtain the current SessionKey. The request to the LocalServer must bring the obtained SessionKey for the LocalServer to determine whether the request is valid.

For the convenience of external use, pSessionKey has been fixed as a visible string, and a trailing 0 has been added, so the actual session key is only 19 bytes.
{% endhint %}

### Set Website Parameters

Set the basic parameters of the current service dapp website.

```c
void TSK_LocalServer_SetCurrentWebSiteParam(WCHAR* strRootDir,WCHAR*
strDynamicFlag,WCHAR* strRemoteServer,WCHAR* strIndexFileName,WCHAR*
strPemFilePath);
```

<details>

<summary>Parameters</summary>

* WCHAR \*
  * strRootDir - the full path information of the root directory of the current dapp website \[IN]
* WCHAR \*
  * strDynamicFlag - dynamic request flag, LocalServer will use this flag to determine whether to forward the request to the corresponding server. If you need to support local LocalServer dynamic request forwarding, the dapp development process should strictly abide by the request specification defined by itself. The dynamic request flag can have Multiple, separated by |, for example /d|/api|/opt, localserver requires that the dynamic request ID must be at the beginning of the requested Uri
* WCHAR \*
  * strRemoteServer - dynamic request forwarding remote server
* WCHAR \*
  * strIndexFileName - the home page of the local dapp, NULL can be passed, and index.html is used as the home page by default
* WCHAR \*
  * strPemFilePath - if the remote server address is configured as https, the SSL public key of the server needs to be provided to LocalServer in the form of a standard pem certificate file

</details>

<details>

<summary>Return Values</summary>

none

</details>

{% hint style="info" %}
The built-in LocalServer in the EDFS Native SDK library can be used as a local HttpServer server. It supports downloading and opening the website data of DApps locally, providing DApps with a faster startup and use experience. DApps can store all static Resources are uploaded to distributed storage or other types of network storage in the form of compressed packages. When NativeApp opens the dapp, it can download the compressed package to the local and decompress it. Access to all static resources of the dapp can be performed locally through the LocalServer. Dynamic The request can be forwarded to the real server of the dapp for processing, relying on distributed storage or other third-party storage to provide a better solution for the operation of the dapp. At present, the built-in LocalServer in the EDFS Native SDK library only supports services for one dapp website at the same time. The open window of the dapp should be managed externally, and the local parameters of the website should be updated in time every time the dapp is opened.&#x20;

LocalServer currently only supports the acquisition of dapp static data and the forwarding of dynamic requests. To support dynamic request forwarding, the external configuration should be done. External can pass NULL through all three parameters, which means to clear the previous configuration data
{% endhint %}
