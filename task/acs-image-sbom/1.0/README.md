This task generates an SBOM for a specified image using ACS using the roxctl CLI and then optionally
exports it to CycloneDX (aka RHTPA).

To use this task two secrets must be provided in workspaces `acs-central` and `upload-sbom`.

`acs-central` secret has the following fields (note change for your environment):

```
apiVersion: v1
kind: Secret
metadata:
  name: acs-central
type: Opaque
data:
  rox_api_token: xxxxxx
  rox_central_endpoint: central-stackrox.apps.hub.ocplab.com:443
```

`upload-sbom` has the following fields, note that you need to have an OIDC user which
will map the scope `create:document` in order to upload the SBOM successfully to RHTPA.

```
apiVersion: v1
kind: Secret
metadata:
  name: upload-sbom
type: Opaque
data:
  OIDC_CLIENT_ID: sbom
  OIDC_PASSWORD: xxxxx
  OIDC_TOKEN_ENDPOINT: https://sso.ocplab.com/realms/ocplab/protocol/openid-connect/token
  OIDC_USER: sbom
```

Here is a sample taskrun:

```
apiVersion: tekton.dev/v1
kind: TaskRun
metadata:
  name: test-sbom
  namespace: product-catalog-cicd
spec:
  params:
  - name: image
    value: quay.io/gnunn/server@sha256:f1362517d19f1f2052e483866fda6bd777843d9818ada6a70d7f1ce395a7f614
  - name: cyclonedxHostUrl
    value: https://server.apps.hub.ocplab.com
  serviceAccountName: pipeline
  taskRef:
    kind: Task
    params:
    - name: pathInRepo
      value: task/acs-image-sbom/1.0/acs-image-sbom.yaml
    resolver: git
  workspaces:
  - name: acs-central
    secret:
      secretName: roxsecrets
  - name: upload-sbom
    secret:
      secretName: upload-sbom
```
