# GitHub Profile 3D Contrib.

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-gitblock.svg)

[日本語](./docs/README.ja-jp.md) | [Español](./docs/README.es-es.md)

## Overview

This GitHub Action creates a GitHub contribution calendar on a 3D profile image.

## How to use (GitHub Actions) - Basic

This action generate your github profile 3d contribute calendar and make a commit to your repo.
You can also trigger action by yourself after add this action.

### step 1. Create special repository.

Create a repository on GitHub with the same name as your user name.

* e.g. If the user name is `octocat`, create a repository named `octocat/octocat`.
* ref. [Managing your profile README](https://docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme)

In this repository, do the following.

### step 2. Create workflow file.

Create a workflow file like the one below.

* `.github/workflows/profile-3d.yml`

The schedule is set to start once a day.
Please correct the startup time to a convenient time.

```yaml:.github/workflows/profile-3d.yml
name: GitHub-Profile-3D-Contrib

on:
  schedule: # 03:00 JST == 18:00 UTC
    - cron: "0 18 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    name: generate-github-profile-3d-contrib
    steps:
      - uses: actions/checkout@v4
      - uses: yoshi389111/github-profile-3d-contrib@latest
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: ${{ github.repository_owner }}
      - name: Commit & Push
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add -A .
          if git commit -m "generated"; then
            git push
          fi
```

Note: If you also want to include the private repository, register the "personal access token" in the repository and set it to GITHUB_TOKEN specified in the workflow file.

This will add the action to the repository.

#### Environment variables

* `GITHUB_TOKEN` : (required) access token
* `USERNAME` : (required) target user name (or specify with an argument).
* `MAX_REPOS` : (optional) max repositories, default 100 - since ver. 0.2.0
* `SETTING_JSON` : (optional) settings json file path. See `sample-settings/*.json` and `src/type.ts` in `yoshi389111/github-profile-3d-contrib` repository for details. - since ver. 0.6.0
* `GITHUB_ENDPOINT` : (optional) Github GraphQL endpoint. e.g. `https://github.mycompany.com/api/graphql` - since ver. 0.8.0
* `YEAR` : (optional) For past calendars, specify the year. - since ver. 0.8.0

#### About `GITHUB_TOKEN`

The `secrets.GITHUB_TOKEN` set in the `GITHUB_TOKEN` environment variable in the sample is a special access token automatically created by GitHub.

- GitHub Docs: [Automatic token authentication](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication)

If you want to generate a contribution calendar for public repositories only, register this value.
There is no need to create a secret manually.

Also, if you want to include activity in your private repositories in your contribution calendar, check "Include private contributions on my profile" in the "Profile settings" section of "Public profile" in your user settings.

Furthermore, if you want to aggregate language information from private repositories, create an access token with the appropriate permissions.
Register that access token as a secret with any name you like (For example, `MY_PERSONAL_ACCESS_TOKEN`).
However, please note that user-created secrets cannot start with `GITHUB_`.

- GitHub Docs: [About secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/about-secrets)

Specify that secret in the `GITHUB_TOKEN` environment variable.

```diff
          env:
-           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
+           GITHUB_TOKEN: ${{ secrets.MY_PERSONAL_ACCESS_TOKEN }}
            USERNAME: ${{ github.repository_owner }}
```

#### About the time to schedule

In the sample, it is set to start at 18:00 UTC.
This is because it will run at midnight JST, which is the author's local time.

```yaml
on:
  schedule: # 03:00 JST == 18:00 UTC
    - cron: "0 18 * * *"
```

Feel free to change it to any time you like when actually using it.
However, please note that it must be specified in UTC.

### step 3. Manually launch the action

Launch the added action.

* `Actions` -> `GitHub-Profile-3D-Contrib` -> `Run workflow`

The profile image is generated with the following paths.

* `profile-3d-contrib/profile-green-animate.svg`
* `profile-3d-contrib/profile-green.svg`
* `profile-3d-contrib/profile-season-animate.svg`
* `profile-3d-contrib/profile-season.svg`
* `profile-3d-contrib/profile-south-season-animate.svg`
* `profile-3d-contrib/profile-south-season.svg`
* `profile-3d-contrib/profile-night-view.svg`
* `profile-3d-contrib/profile-night-green.svg`
* `profile-3d-contrib/profile-night-rainbow.svg`
* `profile-3d-contrib/profile-gitblock.svg`

Alternatively, if `SETTING_JSON` is specified, the following image will be generated.

* `profile-3d-contrib/profile-customize.svg`

example: green version

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-green-animate.svg)

example: season version (Northern Hemisphere.)

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-season-animate.svg)

example: season version (Southern Hemisphere.)

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-south-season-animate.svg)

example: night view version

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-night-view.svg)

example: night green version

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-night-green.svg)

example: night rainbow version

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-night-rainbow.svg)

example: git block version

![svg](https://raw.githubusercontent.com/yoshi389111/github-profile-3d-contrib/main/docs/demo/profile-gitblock.svg)

### step 4. Add image to README.md

Add the path of the generated image to the readme file.

e.g.

```md
![](./profile-3d-contrib/profile-green-animate.svg)
```

## How to use (GitHub Actions) - Advanced examples

#### [More info in EXAMPLES.md](./EXAMPLES.md)

## How to use (local)

Set the `GITHUB_TOKEN` environment variable to the value of "personal access token".

```shell-session
export GITHUB_TOKEN=XXXXXXXXXXXXXXXXXXXXX
```

Run it with your GitHub user specified.

```shell-session
node_modules/.bin/ts-node src/index.ts USER_NAME
```

or

```shell-session
npm run build
node . USER_NAME
```


## Licence

MIT License

(C) 2021 SATO, Yoshiyuki
