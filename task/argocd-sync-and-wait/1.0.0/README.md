Improved version of tekton hub task that adds the following features:

- Validates the deployment has the expected updated image
- Different behavior depending on whether automated sync is enabled

Task requires a secret called `argocd-env-secret` that specifies the token and server location.