## versite publish

Publish a version of your docs to a GitHub Pages branch using `versite`.

### Inputs

| Name            | Required | Default      | Description                                                                                                              |
| --------------- | -------- | ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `path`          | yes      |              | Path to built docs to deploy                                                                                             |
| `version`       | yes      |              | Version label to deploy                                                                                                  |
| `aliases`       | no       | `""`         | Aliases for the deployed version. Accepts space/newline-separated strings or YAML/JSON arrays (e.g. `[latest, stable]`). |
| `title`         | no       | `""`         | Title used for the deployed version                                                                                      |
| `remote`        | no       | `"origin"`   | Git remote to push to                                                                                                    |
| `branch`        | no       | `"gh-pages"` | Branch to deploy to                                                                                                      |
| `message`       | no       |              | Commit message for the deployment                                                                                        |
| `push`          | no       | `false`      | Whether to push the deployment commit                                                                                    |
| `deploy-prefix` | no       |              | Prefix to add to deployed paths                                                                                          |
| `versite-path`  | no       |              | Path to a prebuilt `versite` binary on the runner                                                                        |
| `versite-tag`   | no       |              | Release tag to install `versite` from `ConnerPetzold/versite` when `versite-path` is not provided                        |

### Usage

Minimal usage (installs latest release of `versite` by default if available):

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build docs
        run: |
          npm ci
          npm run build:docs
      - name: Deploy docs
        uses: ConnerPetzold/versite-publish@main
        with:
          path: dist/docs
          version: v1.2.3
```

Provide `aliases` as a YAML array:

```yaml
- name: Deploy docs with aliases
  uses: ConnerPetzold/versite-publish@main
  with:
    path: dist/docs
    version: v1.2.3
    aliases: [latest, stable]
    title: "Docs v1.2.3"
    update-aliases: true
    push: true
```

Multiline aliases are also supported:

```yaml
aliases:
  - latest
  - stable
```

Use a provided `versite` binary already on the runner:

```yaml
- name: Deploy using local versite
  uses: ConnerPetzold/versite-publish@main
  with:
    versite-path: ./bin/versite
    path: dist/docs
    version: v1.2.3
    push: true
```

Install a specific `versite` release tag before deploy:

```yaml
- name: Deploy using a specific versite tag
  uses: ConnerPetzold/versite-publish@main
  with:
    versite-tag: v0.3.0
    path: dist/docs
    version: v1.2.3
    remote: origin
    branch: gh-pages
    message: "Deploy docs v1.2.3"
    push: true
```

All booleans are strings in GitHub Action inputs; use `'true'`/`'false'` as shown.
