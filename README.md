# vcf9-vm-gitops-lab

GitOps for plain old VMs on VMware Cloud Foundation 9: VM Service
(`vmoperator.vmware.com/v1alpha5`) consumed through a VCF Automation
All Apps namespace, reconciled from this repo by Argo CD.

Companion repo for the blog post on [rutgerblom.com](https://rutgerblom.com)
(link added when the post is published).

## Layout

| Path | Purpose | Synced by Argo CD |
| --- | --- | --- |
| `vms/` | Cloud-init Secret and VirtualMachine manifests | Yes |
| `policies/` | Kyverno policy, checked with the Kyverno CLI before merge | No |
| `argocd/` | The Argo CD Application that points at `vms/` | No |

## Using it in your own lab

1. Replace `kubevm-lab-hth89` with your own namespace name. VCF Automation
   adds a random suffix when it creates the namespace.
2. Set `imageName` to an image from `kubectl get vmi` and `storageClass` to the
   storage class assigned in your namespace class.
3. Put your own SSH public key in `vms/web01-cloudinit.yaml`.
4. Validate: `kubectl apply --dry-run=server -f vms/`
5. Check policy: `kyverno apply policies/require-vm-expiry.yaml --resource vms/web01.yaml`

Never commit passwords, tokens or private keys to this repo.
