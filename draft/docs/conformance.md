# CNCF conformance for qbo

This section contains instuctions on how to perform CNCF [conformance](https://www.cncf.io/certification/software-conformance/) tests for `qbo`. Test are performed using a diagnostic tool called [sonobuoy](https://github.com/vmware-tanzu/sonobuoy)

> From `qbo` web console
## Requirements
---
> * Chrome or Firefox browser

## Conformance Test
---
> Conformance tests are performed by running the `conformance` script. It uses a typing bot called `qbot` that will type the commands for you. Alternatively you can type the command yourself in the shell


`conformance` will perform the following tasks:
* Create a `qbo` cluster
* Configure `kubectl`
* Download and configure `sonobuoy` 
* Run confomance test on the version entered.

Usage:

```bash
./conformance help
>>> ./conformance help                 -- Show usage
>>> ./conformance list                 -- List available Kubernetes image tags
>>> ./conformance run {tag}            -- Run CNCF conformance results for qbo
```

List available Kubernetes tags:

```bash
./conformance list
```
Select version and run conformance test. Example: 
```bash
./conformance run v1.28.0
```
## Conformance Test Results
---
```bash
cat /tmp/qbo/sonobuoy/v1.28.0/qbo/e2e.log | grep Pass
```

## CNCF Conformance Website

> https://www.cncf.io/certification/software-conformance/
