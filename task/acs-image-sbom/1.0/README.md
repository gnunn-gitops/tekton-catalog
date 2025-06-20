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
