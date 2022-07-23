# Basic Interface

### Initialize Toolkit

Initialize the EDFS-SDK Native library.

```c
TSK_Init() int
```

<details>

<summary>Parameters</summary>

none

</details>

<details>

<summary>Return Values</summary>

* INT32
  * KError\_Success success
  * KError\_CantConnectKernel, failed, unable to connect to the EDFS file system kernel (only valid for Native dapp development).
  * KError\_Other, failed, other errors;

</details>

{% hint style="info" %}
The TSK\_Init interface is the EDFS-SDK initialization interface. All NativeApps that load the EDFS-SDK library should call the TSK\_Init interface when the App is started or initialized . The other API calls of the EDFS-SDK library depend on the successful call of the TSK\_Init interface.
{% endhint %}

### Uninitialize Toolkit

Reverse the initialization of the EDFS-SDK Native library.

```c
TSK_UnInit()
```

<details>

<summary>Parameters</summary>

none

</details>

<details>

<summary>Return Values</summary>

none

</details>

{% hint style="info" %}
DApp needs to call the TSK\_UnInit interface to complete the reverse initialization of the EDFS Native SDK library when the program exits, so that the EDFS Native SDK library can clean up and release all occupied system resources.
{% endhint %}
