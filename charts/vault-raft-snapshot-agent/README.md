# vault-raft-snapshot-agent

![Version: 0.5.1](https://img.shields.io/badge/Version-0.5.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.11.0](https://img.shields.io/badge/AppVersion-v0.11.0-informational?style=flat-square)

Vault Raft Snapshot Agent takes periodic snapshots of Vault's Raft database and stores them on a local volume or an remote S3 bucket

This Chart deploys an instance of [Vault Raft Snapshot Agent](https://github.com/Argelbargel/vault-raft-snapshot-agent) in your cluster
and optionally creates a PersistentVolume to store the generated snapshots of Vault's Raft database in.

Besides installing an configuring this chart you'll have to configure your Vault to grant the agent permission to take snapshots of the raft database.
See [vault-raft-snapshot-agent's documentation](https://github.com/Argelbargel/vault-raft-snapshot-agent#authentication) for instructions.

**Homepage:** <https://argelbargel.github.io/vault-raft-snapshot-agent-helm/>

## Source Code

* <https://github.com/Argelbargel/vault-raft-snapshot-agent-helm/>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| config | object | `{"snapshots":{"frequency":"1h","retain":72,"storages":{"local":{"enabled":true,"volume":{"emptyDir":{}}}}},"vault":{"auth":{"kubernetes":{"role":"vault-raft-snapshot-agent"}},"nodes":{"autoDetectLeader":false,"urls":["http://127.0.0.1:8200"]}}}` | Defines the contents of the configuration-file for vault-raft-snapshot-agent.    Except for `local_storage` the keys and values are the same as in the agent's    [configuration file](https://github.com/Argelbargel/vault-raft-snapshot-agent) |
| config.snapshots.storages.local.enabled | bool | `true` | Enables/disables the local storage of snaphots.    If disabled the corresponding volume and volume-mounts will not be created |
| config.snapshots.storages.local.volume | object | `{"emptyDir":{}}` | Defines the kind of volume used to store the snapshots locally.    If you specify `persistentVolumeClaim` the chart can generate the    PVC for you. Just specify the claim as you would [normally do](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)    and add the property `create: true` and the relevant properties of your [PersistentVolumeClaimSpec]()    as key of `persistentVolumeClaim`. |
| config.vault.nodes.urls | list | `["http://127.0.0.1:8200"]` | Urls to the vault-nodes. Recommended to use a single url always pointing to the *leader* of your vault-cluster, e.g. `https?://vault-active.<vault-namespace>.svc.cluster.local:<vault-server service-port>` |
| deployment.extraAnnotations | object | `{}` | additional annotation to add to the deployment's metadata |
| deployment.extraLabels | object | `{}` | additional labels to add to the deployment's metadata |
| deployment.spec | object | `{"extraAnnotations":{},"extraEnv":[],"extraEnvFrom":[],"extraLabels":{},"extraVolumes":[],"image":{"pullPolicy":"IfNotPresent","repo":"ghcr.io/argelbargel/vault-raft-snapshot-agent","tag":null},"initContainer":{},"resources":{"limits":{},"requests":{}},"revisionHistoryLimit":null,"strategy":{"type":"Recreate"}}` | additional environment-variables to add to the pod |
| deployment.spec.extraAnnotations | object | `{}` | additional annotation to add to the pods's metadata |
| deployment.spec.extraEnvFrom | list | `[]` | additional environment-refs to add to the pod |
| deployment.spec.extraLabels | object | `{}` | additional labels to add to the pods's metadata |
| deployment.spec.extraVolumes | list | `[]` | additional volumes for the container. configures the pods `volumeMounts` and `volumes`-sections: <pre>- name: my-volume<br>  mountPath: /my-path<br>  emptyDir: {}</pre> `name` and `mountPath` are used both in `volumeMounts` and `volumes`, `readOnly` only applies to `volumeMounts` and any other key is added to `volumes` only |
| deployment.spec.image.pullPolicy | string | `"IfNotPresent"` | New releases of vault-raft-snapshot-agent always change the    `.Chart.AppVersion` of this chart thus must only be changed    if you use another repository than the default |
| deployment.spec.image.repo | string | `"ghcr.io/argelbargel/vault-raft-snapshot-agent"` | Image that is deployed (change e.g. for private registry-proxy) |
| deployment.spec.image.tag | string | `.Chart.AppVersion` | the image's tag |
| deployment.spec.initContainer | object | `{}` | specify optional init-container. Only properties `name`, `image`, `command` and `env` are used.    `name` and `image` are optional, by default alpine:3.19.1, which is the agents-base-image, is used as image    The init-container has access to all extraVolumes. |
| deployment.spec.resources | object | `{"limits":{},"requests":{}}` | resource limits and requests for the deployment |
| deployment.spec.resources.limits | object | `{}` | resource limits of the deployment |
| deployment.spec.resources.requests | object | `{}` | resource requests by the deployment |
| deployment.spec.revisionHistoryLimit | int | `nil` | see [kubernetes docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#clean-up-policy)    You might want to change this to a small value to avoid cluttering up the    UI of a Continuous Delivery Tool like Argo-CD |
| deployment.spec.strategy.type | string | `"Recreate"` | Update-strategy for the agent's pods    `Recreate` guarantees that no two snapshots get taken at the same time    `RollingUpdate` ensures that there's always one instance of the agent running |
| fullnameOverride | string | release-name + chart-name truncated to 63 chars | overrides the generated full-name for generated resources |
| global.namespace | string | `.Release.Namespace` | allows to override the release's namespace |
| nameOverride | string | `.Chart.Name` | overrides the generated name for generated resources |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | chart name according to name settings. `.fullNameOverride` and `.nameOverride` are taken into account | name of the service account to use. |

## License
- Source code is licensed under MIT
