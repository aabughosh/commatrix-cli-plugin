# commatrix-cli-plugin

This repository implements a single kubectl plugin for switching the namespace
that the current KUBECONFIG context points to. In order to remain as indestructive
as possible, no existing contexts are modified.

**Note:** go-get or vendor this package as `k8s.io/sample-cli-plugin`.

This particular example demonstrates how to perform basic operations such as:

* How to create a new custom command that follows kubectl patterns
* How to obtain a user's KUBECONFIG settings and modify them
* How to make general use of the provided "cli-runtime" set of helpers for kubectl and third-party plugins

It makes use of the genericclioptions in [k8s.io/cli-runtime](https://github.com/kubernetes/cli-runtime)
to generate a set of configuration flags which are in turn used to generate a raw representation of
the user's KUBECONFIG, as well as to obtain configuration which can be used with RESTClients when sending
requests to a kubernetes api server.

## Details

The sample cli plugin uses the [client-go library](https://github.com/kubernetes/client-go/tree/master/tools/clientcmd) to patch an existing KUBECONFIG file in a user's environment in order to update context information to point the client to a new or existing namespace.

In order to be as non-destructive as possible, no existing contexts are modified in any way. Rather, the current context is examined, and matched against existing contexts to find a context containing the same "AuthInfo" and "Cluster" information, but with the newly desired namespace requested by the user.

## Purpose

This is an example of how to build a kubectl plugin using the same set of tools and helpers available to kubectl.

## Running

```sh
# assumes you have a working KUBECONFIG
$ go build cmd/kubectl-commatrix.go
# place the built binary somewhere in your PATH
$ cp ./kubectl-commatrix /usr/local/bin

# you can now begin using this plugin as a regular kubectl command:
# update your configuration to point to "new-namespace"
$ kubectl commatrix generate

```
