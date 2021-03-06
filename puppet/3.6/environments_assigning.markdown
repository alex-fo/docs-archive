---
layout: default
title: "Assigning Nodes to Environments"
---

[enc]: /guides/external_nodes.html
[node terminus]: ./subsystem_catalog_compilation.html#step-1-retrieve-the-node-object
[enc_environment]: /guides/external_nodes.html#environment
[puppet.conf]: ./config_file_main.html
[env_setting]: ./configuration.html#environment


By default, all nodes are assigned to a default environment named `production`.

There are two ways to assign nodes to a different environment:

* Via your [ENC][] or [node terminus][]
* Via each agent node's puppet.conf

The value from the ENC is authoritative, if it exists. If the ENC doesn't specify an environment, the node's config value is used.

## Assigning Environments Via an ENC

The interface to set the environment for a node will be different for each ENC. Some ENCs cannot manage environments.

When writing an ENC, simply ensure that the `environment:` key is set in the YAML output that the ENC returns. [See the documentation on writing ENCs for details.][enc_environment]

If the environment key isn't set in the ENC's YAML output, the puppet master will just use the environment requested by the agent.

## Assigning Environments Via the Agent's Config File

In [puppet.conf][] on each agent node, you can set [the `environment` setting][env_setting] in either the `agent` or `main` config section. When that node requests a catalog from the puppet master, it will request that environment.

If you are using an ENC and it specifies an environment for that node, it will override whatever is in the config file.

## Non-Existent Environments

**With directory environments,** nodes **can't** be assigned to unconfigured environments. If a node is assigned to an environment which doesn't exist --- that is, there is no directory of that name in any of the `environmentpath` directories --- the puppet master will fail compilation of its catalog.

**With config file environments,** nodes **can** be assigned to environments that are not configured. This will cause them to fall back to global values for `modulepath` and `manifest`.

