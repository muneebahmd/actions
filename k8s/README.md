# Chkk Kubernetes Action

Integrating with Github Actions is as simple as downloading Chkk and running a scan on your Kubernetes manifests.

Here's an example of using Chkk K8s Action, in this case to run checks on a sample `k8s/k8s-manifest.yaml`:

```yaml
name: Example workflow for Chkk Kubernetes Action
on: push
jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    steps:
      - name: Run Chkk to catch reliability risks in your Kubernetes manifests
        uses: actions/checkout@v2
        uses: chkk-io/actions/k8s@main
        env:
          CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
        with:
          kubernetes-manifest: k8s/k8s-manifest.yaml
```

The Chkk GitHub Action has properties which are passed to the underlying CLI. These are passed to the action using `with`.

| Parameter           | Default | Description                                                  |
| :------------------ | :------ | ------------------------------------------------------------ |
| args                |         | Override the default arguments to the Chkk CLI               |
| kubernetes-manifest |         | The path to the Kubernetes manifest file you want to check.  |
| skip-checks         |         | A comma-separated list of checks you want to skip.           |
| run-checks       |         | A comma-separated list of specific checks you want to run instead of running all checks. |
| check-type          | reliability | Run specific type of checks. Available options: [all/reliability/security] |




List of supported `args`


| Argument            | Default | Description                              |
| :------------------ | :------ | :--------------------------------------- |
| --show-diff         | false   | Show line diff in result output               |
| --continue-on-error | false   | Do not raise error in case a check fails |


## Setup Access Token in GitHub repository secrets

1. Go to the `Settings` tab in your repository menu.

![settings.png](tutorial_images/settings.png)

2. From the side menu, select `Secrets`.

![secrets_menu.png](tutorial_images/secrets_menu.png)

3. Add your Chkk access token as a secret, as shown below.

![secret_setup.png](tutorial_images/secret_setup.png)

## Examples

### Run All Available Checks
To run all available checks, configure the Action with default parameters.

```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
```

### Show Line Diff for Failed Checks
To show the line diff for failed checks in the result output, configure the Action with following argument: `--show-diff`

```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
        args: '--show-diff'
```

### Run Specific Checks

To run one or more specific checks on the sample K8s manifest, configure the Action with following parameter:

`run-checks` : <list of comma separated checks. e.g. CHKK-K8S-27,CHKK-K8S-28>

```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
        run-checks: 'CHKK-K8S-27,CHKK-K8S-28'
```

### Skip Checks

To skip one or more specific checks, configure the Action with following parameter:

`skip-checks` : <list of comma separated checks. e.g. CHKK-K8S-27,CHKK-K8S-28>

```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
        skip-checks: 'CHKK-K8S-27,CHKK-K8S-28'
```




### Suppress Failing Checks

To prevent the overall Action from failing in case one or more checks fail, configure the Action with following argument: `--continue-on-error`

```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
        args: '--continue-on-error'
```

### Run Specific Type of Checks (Security/Reliability/All)

To run specific types of checks, configure the Action with the following argument: `--check-type` 

Available options are:

* security
* reliability (selected as default)
* all


```yaml
on: [push]

jobs:
  kubernetes-manifests:
    runs-on: ubuntu-latest
    name: Run Chkk to catch reliability risks in your Kubernetes manifests
    steps:
    - uses: actions/checkout@v2
    - uses: chkk-io/actions/k8s@main
      env:
        CHKK_ACCESS_TOKEN: ${{ secrets.CHKK_ACCESS_TOKEN }}
      with:
        kubernetes-manifest: 'k8s/k8s-manifest.yaml'
        args: '--check-type=all'
```


Made with 🧡 by Chkk