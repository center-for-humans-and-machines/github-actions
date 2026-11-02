# github-actions

Shared GitHub Actions for the Center for Humans and Machines repositories.
Fix a step once here instead of in every repository.

| Action                       | Purpose                           |
| ---------------------------- | --------------------------------- |
| [`deploy-helm`](deploy-helm) | Deploy a Helm chart to Kubernetes |

## Usage

Pin the commit of a release tag; Dependabot's `github-actions` updates in
each repository then open a pull request for every new release.

```yaml
- name: Deploy with Helm
  uses: center-for-humans-and-machines/github-actions/deploy-helm@<commit sha> # v1.0.0
  with:
    app-name: my-app
    image-name: registry.example.org/my-app:${{ github.sha }}
    main-url: my-app.example.org
    namespace: my-namespace
    kubeconfig: ${{ secrets.KUBECONFIG }}
    dockercfg: ${{ secrets.DOCKERCFG }}
```

The action runs in the calling repository's checkout, so paths such as
`chart-dir` and `values-file` refer to that repository. See
[`deploy-helm/action.yml`](deploy-helm/action.yml) for all inputs.

This repository is public because public repositories cannot use actions
from private ones. Never put secrets, hosts or other project values here;
callers pass them as inputs.

## Development

```sh
pre-commit install
pre-commit run --all-files
```

## Release

Follow [Semantic Versioning](https://semver.org): renaming or removing an
input, or changing a default, is a major release. Add the release to
[CHANGELOG.md](CHANGELOG.md), then tag the merged commit on `main`:

```sh
git tag --sign v1.1.0 --message v1.1.0
git push origin v1.1.0
```
