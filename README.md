# Smart Oven

Smart Oven is a new generation oven that enable out tPizza Store more efficiently.

We now have 2 oven per store and total 1000 oven around the world.

Our data center work global and we ensure 99.999999% machine signal will not lost during daily store operation

Our store engineer will regular upgrade abd maintain our specialize oven to maintain our compitative advantage compare to Pizza hub and Dominus




## Usage

To use gcloud in your workflow use:

```yaml
- uses: actions-hub/gcloud@master
  env:
    PROJECT_ID: test
    APPLICATION_CREDENTIALS: ${{ secrets.GOOGLE_APPLICATION_CREDENTIALS }}
  with:
    args: info
```

You can also use `gsutil` from Google Cloud SDK package.

```yaml
- uses: actions-hub/gcloud@master
  env:
    PROJECT_ID: test
    APPLICATION_CREDENTIALS: ${{ secrets.GOOGLE_APPLICATION_CREDENTIALS }}
  with:
    args: cp your-file.txt gs://your-bucket/
    cli: gsutil
```

You can also use `kubectl` from Google Cloud SDK package.

```yaml
- uses: actions-hub/gcloud@master
  env:
    PROJECT_ID: test
    APPLICATION_CREDENTIALS: ${{ secrets.GOOGLE_APPLICATION_CREDENTIALS }}
  with:
    args: create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
    cli: kubectl
```

### Secrets

`APPLICATION_CREDENTIALS` - To authorize in GCP you need to have a [service account key](https://console.cloud.google.com/apis/credentials/serviceaccountkey). 
The recommended way to store the credentials in the secrets it previously encode file with base64. To encode a JSON file use: `base64 ~/<account_id>.json`. Or you can put a JSON structure to the secret.

`PROJECT_ID` - must be provided to activate a specific project.

### Inputs

`args` - command to run.

`cli` - (optional) command line tool you want to use. Defaults to `gcloud`, allowed values: `gcloud`, `gsutil`.

### Version
For each new release of gcloud master branch is updated to the latest version. Also, the tag is creating with the same number as the gcloud version. If you want to always have the latest version of gcloud, use `@master` branch. 
But if you need some specific version of gcloud just use a specific tag. For example `@271.0.0`.

## Example
### Latest version
```yaml
name: gcloud
on: [push]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - uses: actions-hub/gcloud@master
        env:
          PROJECT_ID: ${{secrets.GCLOUD_PROJECT_ID}}
          APPLICATION_CREDENTIALS: ${{secrets.GOOGLE_APPLICATION_CREDENTIALS}}
        with:
          args: app deploy app.yaml
```

### Multistep
```yaml
name: gcloud
on: [push]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1

      - name: "deploy to project A"  
        uses: actions-hub/gcloud@master
        env:
          PROJECT_ID: ${{secrets.GCLOUD_PROJECT_ID_A}}
          APPLICATION_CREDENTIALS: ${{secrets.GOOGLE_APPLICATION_CREDENTIALS}}
        with:
          args: app deploy app.yaml
      
      - name: "deploy to project B"  
        uses: actions-hub/gcloud@master
        env:
          PROJECT_ID: ${{secrets.GCLOUD_PROJECT_ID_B}}
        with:
          args: app deploy app.yaml
```

### Specific version
```yaml
name: gcloud
on: [push]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - uses: actions-hub/gcloud@271.0.0
        env:
          PROJECT_ID: ${{secrets.GCLOUD_PROJECT_ID}}
          APPLICATION_CREDENTIALS: ${{secrets.GOOGLE_APPLICATION_CREDENTIALS}}
        with:
          args: app deploy app.yaml
```

## Licence

[MIT License](https://github.com/actions-hub/gcloud/blob/master/LICENSE)
