---
description: Sign up for CoreWeave Cloud and configure access
---

# Cloud Account and Access

## Get started

Welcome to CoreWeave Cloud!

### Create an account

The first step to getting access to CoreWeave Cloud is to [sign up for an account](https://cloud.coreweave.com/signup).

After providing payment information, you will receive a verification email to the account provided. After verifying your address, your information will be submitted to our team to verify your account request. You will also be redirected to a page that confirms the request was received.

{% hint style="success" %}
**Tip**

For a higher chance of admission, it is strongly recommended to fill out [the inbound sales form](https://coreweave.com/contact-us) after submitting your account request. **Please note, account approval may take up to three business days.**
{% endhint %}

### Obtain CoreWeave access credentials

{% hint style="danger" %}
**Important**

Only [**organization admins**](cloud-account-management/#organization) may generate, view, and delete [access tokens](cloud-account-management/#organization).

For each user who requires a separate Access Token, a new Token and corresponding Kubeconfig file must be generated by the organization admin. The admin must then share this with the user in a secure manner.
{% endhint %}

At CoreWeave, [Kubeconfig files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) are used to interact with our Kubernetes cluster via clients such as `kubectl`.

**API access tokens** are used for programmatic access to CoreWeave Cloud applications, such as [Prometheus](../../coreweave-kubernetes/prometheus/). Once the organization admin account has been approved and activated, the organization admin may [log in to the CoreWeave Cloud UI](https://cloud.coreweave.com), then navigate to the **API Access** page from the left-hand menu.

New Kubeconfig files and credentials as well as new API tokens are created and managed on this page.

<figure><img src="../.gitbook/assets/image (113).png" alt="Screenshot of the &#x22;API &#x26; Kubeconfig&#x22; page in the Cloud app"><figcaption></figcaption></figure>

### Generate a new Kubeconfig file

{% hint style="danger" %}
**Warning**

**The Kubeconfig and Access Token are shown** **only once!**\
Be sure to save this file and the token in a secure location. If you lose your Access Token, it can be found inside your [Kubeconfig file](getting-started.md#configure-kubernetes).
{% endhint %}

Organization admins may generate new Access Tokens by navigating to the [API Access page](https://cloud.coreweave.com/api-access) on the Cloud UI. From here, click the **API & Kubeconfig** tab at the top right of the page, then click the **Create a New Token** button to the right.

Once prompted, give the token a recognizable name, then click the **Generate** button. Generating the new token will also create a Kubeconfig file with the name `cw-kubeconfig`, which will embed your token. This file will download automatically.

{% hint style="info" %}
**Note**

If you would like to prevent the Kubeconfig file from downloading automatically, un-check the **Automatically download Kubeconfig** checkbox.
{% endhint %}

<figure><img src="../.gitbook/assets/image (4) (4).png" alt="The &#x22;New Access Token&#x22; modal"><figcaption><p>The "New Access Token" modal</p></figcaption></figure>

### Install Kubernetes tools

Once you have obtained your Access Credentials from your organization admin, download the Kubernetes command line tools to your local machine.

{% hint style="info" %}
**Additional Resources**

For more information on Kubernetes installation and configuration, please reference the [official Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
{% endhint %}

{% tabs %}
{% tab title="Linux" %}
## Installing `kubectl` on a Linux system

### **Downloading and installing the binary**

The following command is a simple way to install `kubectl` on your Linux system by downloading the binary.

{% code overflow="wrap" %}
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
{% endcode %}

Once you have the `kubectl` binary downloaded, install it by first making the binary executable:

```bash
$ chmod +x ./kubectl
```

Then, move the file into the system `bin` directory:

```bash
$ sudo mv ./kubectl /usr/local/bin/kubectl
```

{% hint style="info" %}
**Note**

If you would prefer to install Kubernetes using a Native Package Manager, please view [the Kubernetes guide on Installing using Native Package Manager](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management).
{% endhint %}

**Verifying the kubectl binary**

If you'd like to verify kubectl, you can verify it by running a checksum operation on the downloaded file prior to installing it, such as:

{% code overflow="wrap" %}
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(<kubectl.sha256) kubectl" | sha256sum --check
```
{% endcode %}

This should return `kubectl: OK` to confirm the file is indeed valid. If this returns an error, please review the [official `kubectl` installation guide](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/).
{% endtab %}

{% tab title="macOS" %}
## Installing `kubectl` on a macOS system

### **Installing using Homebrew**

Most Mac users use [Homebrew](https://brew.sh) to install packages.

If you do not already have Homebrew installed, follow the [Brew Installation](https://brew.sh) guide to do so, then install `kubectl` using Homebrew by running the following command:

```bash
$ brew install kubectl
```
{% endtab %}

{% tab title="Windows" %}
## Installing `kubectl` on a Windows system

### **Installing using PowerShell**

Using PowerShell, `kubectl` can be installed by using the following the command:

{% code overflow="wrap" %}
```powershell
& $([scriptblock]::Create((New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/coreweave/kubernetes-cloud/master/getting-started/k8ctl_setup.ps1')))
```
{% endcode %}

{% hint style="info" %}
**Note**

Add the `-Silent` flag to the end of this string for a non-interactive setup.
{% endhint %}

### **Installing using a Package Manager**

You can also install `kubectl` on Windows using a package manager such as [Chocolatey](https://chocolatey.org) or [Scoop](https://scoop.sh).

**Using** [**Chocolatey**](https://chocolatey.org)**:**

```powershell
$ choco install kubernetes-cli
```

**Using** [**Scoop**](https://scoop.sh)**:**

```powershell
$ scoop install kubectl
```

If you would prefer to download the `kubectl` executable directly, [follow the official Kubernetes instructions to download the latest version](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-kubectl-binary-with-curl-on-windows).
{% endtab %}
{% endtabs %}

### Configure Kubernetes

Finally, once you [have `kubectl` installed](getting-started.md#installing-the-kubernetes-command-line-tools), and the `cw-kubeconfig` file has been [generated and obtained from your organization admin](getting-started.md#generate-the-kubeconfig-file), the next step is to move the config file to the right location.

If this is your first time using Kubernetes, or you're using a system that has never had Kubernetes configured before, you probably don't have an existing kubeconfig.&#x20;

To verify, check the default path for your operating system:

<table><thead><tr><th width="253">Operating System</th><th>Default path</th></tr></thead><tbody><tr><td>Linux</td><td><code>~/.kube/config</code></td></tr><tr><td>macOS</td><td><code>/Users/&#x3C;username>/.kube/config</code></td></tr><tr><td>Windows</td><td><code>%USERPROFILE%\.kube\config</code></td></tr></tbody></table>

Also, check your environment and verify no value is set for `KUBECONFIG`:

{% tabs %}
{% tab title="Linux or macOS" %}
```bash
$ echo $KUBECONFIG
```
{% endtab %}

{% tab title="Windows" %}
```powershell
> echo %KUBECONFIG%
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Existing kubeconfig**

If a kubeconfig exists, [Advanced Kubeconfig Environments](../cloud-tools/advanced-kubeconfig-environments.md) explains how to merge them.
{% endhint %}

If you **do not** have an existing Kubeconfig file, follow the steps below for your operating system.&#x20;

{% tabs %}
{% tab title="Linux and macOS" %}
Assuming that `cw-kubeconfig` was downloaded to `~/Downloads`, create the directory and move the file to the correct location.

```
$ mkdir ~/.kube
$ mv ~/Downloads/cw-kubeconfig ~/.kube/config
```
{% endtab %}

{% tab title="Windows" %}
Assuming that `cw-kubeconfig` was downloaded to `%USERPROFILE%\Downloads`, create the directory and move the file to the correct location.

```
> mkdir %USERPROFILE%\.kube
> mv %USERPROFILE%\Downloads\cw-kubeconfig %USERPROFILE%\.kube\config
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Tip**

To use a different kubeconfig path, see [Advanced Kubeconfig Environments](../cloud-tools/advanced-kubeconfig-environments.md). &#x20;
{% endhint %}

### Verify Kubernetes credentials

Since your new account will not yet have any resources, listing your [cluster `secrets`](https://kubernetes.io/docs/concepts/configuration/secret/) is a good way to test that proper communication with the cluster is in place. To verify your CoreWeave Kubernetes configuration using `kubectl`, invoke the following commands.

First, set [the Kubernetes context](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/#context) to `coreweave`:

```bash
$ kubectl config set-context coreweave
```

Next, request [the `secret` objects](https://kubernetes.io/docs/concepts/configuration/secret/):

```bash
$ kubectl get secret
```

This should return your default `secrets`, such as:

```
NAME                           TYPE                                  DATA   AGE
default-token-frqgm            kubernetes.io/service-account-token   3      3h
```

## Congratulations! :tada:

You are now ready to use CoreWeave Cloud to deploy all types of services on CoreWeave's Kubernetes infrastructure!

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th data-hidden></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f4a5">💥</span> <strong>Check out some examples of what you can do on CoreWeave Cloud!</strong></td><td></td><td></td><td><a href="../../coreweave-kubernetes/examples/">examples</a></td></tr></tbody></table>