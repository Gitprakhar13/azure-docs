---
title: 'CLI (v2) attached Virtual Machine YAML schema'
titleSuffix: Azure Machine Learning
description: Reference documentation for the CLI (v2) attached Virtual Machine schema.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference

author: lostmygithubaccount
ms.author: copeters
ms.date: 10/21/2021
ms.reviewer: laobri
---

# CLI (v2) attached Virtual Machine YAML schema

The source JSON schema can be found at https://azuremlschemas.azureedge.net/latest/vmCompute.schema.json.

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

## YAML syntax

| Key | Type | Description | Allowed values | Default value |
| --- | ---- | ----------- | -------------- | ------- |
| `$schema` | string | The YAML schema. If you use the Azure Machine Learning VS Code extension to author the YAML file, including `$schema` at the top of your file enables you to invoke schema and resource completions. | | |
| `type` | string | **Required.** The type of compute. | `virtualmachine` | |
| `name` | string | **Required.** Name of the compute. | | |
| `description` | string | Description of the compute. | | |
| `resource_id` | string | **Required.** Fully qualified resource ID of the Azure Virtual Machine to attach to the workspace as a compute target. | | |
| `ssh_settings` | object | SSH settings for connecting to the virtual machine. | | |
| `ssh_settings.admin_username` | string | The name of the administrator user account that can be used to SSH into the virtual machine. | | |
| `ssh_settings.admin_password` | string | The password of the administrator user account. **One of `admin_password` or `ssh_private_key_file` is required.** | | |
| `ssh_settings.ssh_private_key_file` | string | The local path to the SSH private key file of the administrator user account. **One of `admin_password` or `ssh_private_key_file` is required.** | | |
| `ssh_settings.ssh_port` | integer | The SSH port on the virtual machine. | | `22` |

## Remarks

The `az ml compute` command can be used for managing Virtual Machines (VM) attached to an Azure Machine Learning workspace.

## Examples

Examples are available in the [examples GitHub repository](https://github.com/Azure/azureml-examples/tree/main/cli/resources/compute). Several are shown below.

## YAML: basic

:::code language="yaml" source="~/azureml-examples-cli-preview/cli/resources/compute/vm-attach.yml":::

## Next steps

- [Install and use the CLI (v2)](how-to-configure-cli.md)
