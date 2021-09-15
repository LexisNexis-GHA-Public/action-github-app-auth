# action-github-app-jwt-and-installation-token

This uses Octokit to fetch a GitHub App JWT as well as the installation access token if installationId is given.

## Development

Install the dependencies
```bash
$ yarn
```

Build the typescript and package it for distribution
```bash
$ yarn dist
```

## Inputs
| Key | Desc | Required |
| ------ | ------ | ------ |
| app_id | Github App ID | yes | <tr></tr>
| base64_pem_key | Base64 encoded Github App PEM Key | yes | 
| installation_id | Github App Installation ID | no |

## Outputs
| Key | Desc |
| ------ | ------ |
| jwt_token | Github App JWT |
| installation_access_token | Github App Installation access-token |

## Encoding a PEM Key file
```sh
$ cat pem_file_path.pem | base64
```

## Usage
Github App ID and encoded PEM File are always required.
Provide an installation ID if seeking an installation access token.<br>
The action will validate given installation id within the available installations for its Gihub App ID.

## Sample1 - Checkout a different repo
```
  - name: my-app-install tokens
    id: my-app
    uses: LexisNexis-GHA-Public/action-github-app-jwt-and-installation-token@v1
    with:
      app_id: ${{ secrets.APP_ID }}
      base64_pem_key: ${{ secrets.BASE64_PEM_KEY }}

  - name: Checkout private repo
    uses: actions/checkout@v2
    with:
      repository: febaisi/my-private-repo
      token: ${{ steps.my-app.outputs.jwt_token }}
```

## Sample2 - Clone another repo
```
  - name: my-app-install tokens
    id: my-app
    uses: LexisNexis-GHA-Public/action-github-app-jwt-and-installation-token@v1
    with:
      app_id: ${{ secrets.APP_ID }}
      base64_pem_key: ${{ secrets.BASE64_PEM_KEY }}
      installation_id: ${{ secrets.INSTALLATION_ID }}

  - uses: actions/checkout@v2
  - name: Clone an extra repo
    run: git clone https://x-access-token:${{ steps.my-app.outputs.installation_access_token }}@github.com/owner/repo.git

```

