---
title: Transifex Deployment
layout: en
deploy: v1

---

Travis CI supports uploading to [Transifex](https://www.transifex.com/).

A minimal configuration is:

```yaml
deploy:
  provider: transifex
  controller: transifex.transifexapps.com
  username: "Transifex User Name"
  password: "Transifex Password"
  app: App_name
  cli_version: vX.Y.Z  # e.g. v2.7.0 being the latest at this time
```
{: data-file=".travis.yml"}

It is recommended that you encrypt your password.
Assuming you have the Travis CI command line client installed, you can do it like this:

```bash
$ travis encrypt "YOUR TRANSIFEX PASSWORD" --add deploy.password
```

You will be prompted to enter your api key on the command line.

You can also have the `travis` tool set up everything for you:

```bash
$ travis setup transifex
```

Keep in mind that the above command has to run in your project directory, so it can modify the `.travis.yml` for you.

## Conditional Releases

You can deploy only when certain conditions are met.
See [Conditional Releases with `on:`](/user/deployment/#conditional-releases-with-on).

## The .gitignore method

As this deployment strategy relies on `git`, be mindful that the deployment will
honor `.gitignore`.

If your `.gitignore` file matches something that your build creates, use
[`before_deploy`](#running-commands-before-and-after-deploy) to change
its content.

## Run Commands Before or After Deploy

Sometimes you want to run commands before or after triggering a deployment. You can use the `before_deploy` and `after_deploy` stages for this. These will only be triggered if Travis CI is actually pushing a release.

```yaml
    before_deploy: "echo 'ready?'"
    deploy:
      ..
    after_deploy:
      - ./after_deploy_1.sh
      - ./after_deploy_2.sh
```
{: data-file=".travis.yml"}
