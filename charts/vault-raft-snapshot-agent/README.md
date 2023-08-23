# vault-raft-snapshot-agent

![Version: 0.1.1](https://img.shields.io/badge/Version-0.1.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.4.1](https://img.shields.io/badge/AppVersion-v0.4.1-informational?style=flat-square)

vault-raft-snapshot-agent takes periodic snapshots of vault's raft database and writes it to a local file or an S3 bucket

**Homepage:** <https://github.com/Argelbargel/vault-raft-snapshot-agent>

## Values

<table>
	<thead>
		<th>Key</th>
		<th>Type</th>
		<th>Default</th>
		<th>Description</th>
	</thead>
	<tbody>
		<tr>
			<td>config</td>
			<td>object</td>
			<td><pre lang="json">
{
  "addr": "http://127.0.0.1:8200",
  "frequency": "1h",
  "k8s_auth_path": "kubernetes",
  "k8s_auth_role": "vault-raft-snapshot-agent",
  "local_storage": {
    "enabled": true,
    "volume": {
      "emptyDir": {}
    }
  },
  "retain": 72,
  "vault_auth_method": "k8s"
}
</pre>
</td>
			<td>defines the contents of the configuration-file for vault-raft-snapshot-agent    except for `local_storage` the keys and values are the same as in the agents    [configuration file](https://github.com/Argelbargel/vault-raft-snapshot-agent)</td>
		</tr>
		<tr>
			<td>config.local_storage.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>enables/disables the local storage of snaphots    if disabled the corresponding volume and volume-mounts will not be created</td>
		</tr>
		<tr>
			<td>config.local_storage.volume</td>
			<td>object</td>
			<td><pre lang="json">
{
  "emptyDir": {}
}
</pre>
</td>
			<td>defines the kind of volume used to store the snapshots locally    If you specify `persistentVolumeClaim` the chart can generate the    PVC for you. Just specify the claim as you would [normally do](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)    and add the property `create: true` and the relevant properties of your [PersistentVolumeClaimSpec]()    as key of `persistentVolumeClaim`.</td>
		</tr>
		<tr>
			<td>deployment.image.pullPolicy</td>
			<td>string</td>
			<td><pre lang="json">
"IfNotPresent"
</pre>
</td>
			<td>new releases of vault-raft-snapshot-agent always change the    `.Chart.AppVersion` of this chart thus must only be changed    if you use another repository than above</td>
		</tr>
		<tr>
			<td>deployment.image.repo</td>
			<td>string</td>
			<td><pre lang="json">
"ghcr.io/argelbargel/vault-raft-snapshot-agent"
</pre>
</td>
			<td>image that is deployed (change e.q. for private registry-proxy)</td>
		</tr>
		<tr>
			<td>deployment.image.tag</td>
			<td>string</td>
			<td><pre lang="json">
null
</pre>
</td>
			<td>the image's tag</td>
		</tr>
		<tr>
			<td>deployment.revisionHistoryLimit</td>
			<td>int</td>
			<td><pre lang="json">
null
</pre>
</td>
			<td>see [kubernetes docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#clean-up-policy)    You might want to change this to a small value to avoid cluttering up the    UI of a Continuous Delivery Tool like Argo-CD</td>
		</tr>
		<tr>
			<td>deployment.strategy.type</td>
			<td>string</td>
			<td><pre lang="json">
"Recreate"
</pre>
</td>
			<td>Update-strategy for the agent's pods    `Recreate` guarantees that no two snapshots get taken at the same time    `RollingUpdate` ensures that there's always one instance of the agent running</td>
		</tr>
	</tbody>
</table>

