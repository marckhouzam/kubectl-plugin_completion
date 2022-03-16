# kubectl plugin-completion

## TL;DR

**Requires [PR 105867](https://github.com/kubernetes/kubernetes/pull/105867)**

Plugin that generates shell completion scripts for over half the kubectl plugins.

```bash
$ curl -so kubectl-plugin_completion https://raw.githubusercontent.com/marckhouzam/kubectl-plugin_completion/main/kubectl-plugin_completion
$ chmod +x kubectl-plugin_completion
$ # mv kubectl-plugin_completion <some directory on $PATH>
$ kubectl plugin-completion generate
$ export PATH=$PATH:$HOME/.kubectl-plugin-completion
```
## What

There is an ongoing effort to add support for shell completion for kubectl plugins.
Please see https://github.com/kubernetes/kubernetes/pull/105867 for details.
If merged, it will be possible for any kubectl plugin to provide shell completion.
However, each plugin will need to provide the necessary script itself; this
will take time.

Until this happens, the `plugin-completion` plugin automatically generates the required
shell completion scripts when possible, for the currently installed plugins.

## Installation

1. Download the plugin file: `curl -so kubectl-plugin_completion https://raw.githubusercontent.com/marckhouzam/kubectl-plugin_completion/main/kubectl-plugin_completion`
1. Give the `kubectl-plugin_completion` file executable permissions (`chmod +x kubectl-plugin_completion`)
1. Move the `kubectl-plugin_completion` file to a directory that is on your `$PATH` variable
1. Add the `$HOME/.kubectl-plugin-completion` directory at the end of your `$PATH` variable

## Usage

### Generate the scripts
```
kubectl plugin-completion generate
```
will generate completion scripts for all installed plugins that are supported.
Once done, you will be able to do use shell completion with your plugins.  For example:
```
$ kubectl plugin-completion generate
Shell completion for plugin 'krew' has been generated.
Shell completion for plugin 'plugin_completion' has been generated.

$ kubectl plugin-completion [tab][tab]
clean      generate   list       supported

$ kubectl krew [tab][tab]
completion  help        index       info        install     list        search      uninstall   update      upgrade     version
```

After running this command, the `plugin-completion` plugin itself will have shell completion enabled.

**Note**: Whenever you install a new plugin you will need to run the `generate` command again. 

### Cleanup
```
kubectl plugin-completion clean
```
will remove all generated completion scripts.

### List
```
kubectl plugin-completion list
```
will list the currently installed plugins for which a completion script has been generated.

### Supported plugins
```
kubectl plugin-completion supported
```
will list the installed plugins for which a shell completion script can be generated.

## How

The plugin takes advantage of the fact that over 50% of plugins registered with krew use the
Cobra library, which automatically supports shell completion.  This plugin therefore automatically
generates a shell completion script that uses Cobra's functionality.

You can find the generated scripts under `$HOME/.kubectl-plugin-completion`.
