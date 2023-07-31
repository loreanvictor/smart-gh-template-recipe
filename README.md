```
┏┓          ┏┓• ┓┏  ┓   ┏┳┓       ┓      
┗┓┏┳┓┏┓┏┓╋  ┃┓┓╋┣┫┓┏┣┓   ┃ ┏┓┏┳┓┏┓┃┏┓╋┏┓┏
┗┛┛┗┗┗┻┛ ┗  ┗┛┗┗┛┗┗┻┗┛   ┻ ┗ ┛┗┗┣┛┗┗┻┗┗ ┛
                                ┛        
```
Make your GitHub template smarter. Adds a workflow to your repository that will automatically run
your [`tmplr`](https://github.com/loreanvictor/tmplr) recipe (`.tmplr.yml` file in the root of your project) whenver someone
uses your repository as a template, allowing you to fill it up using contextual values accessible in GitHub Actions.

## Usage

```bash
npx tmplr use trcps/smart-gh-template
```

Use contextual values in your recipe:

```yaml
# .tmplr.yml
steps:
  # ...

  - read: owner_name
    from: env.owner_name

  - read: owner_email
    from: env.owner_email

  - read: repo_name
    from: env.repo_name

  - read: repo_url
    from: env.repo_url

  # ...
```

If you need to access more info from [GitHub Actions context](https://docs.github.com/en/actions/learn-github-actions/contexts)
for your recipe, add them to `.github/workflows/init.yml`:

```yaml
# .github/workflows/init.yml
name: init
on:
  push:
    branches: main
jobs:
  build:
    # ...

      - name: Apply template
        run: npx tmplr && rm -fr .tmplr.yml && rm -fr .github/workflows/init.yml
        env:
          owner_name: ${{ github.event.repository.owner.name }}
          owner_email: ${{ github.event.repository.owner.email }}
          repo_name: ${{ github.event.repository.name }}
          repo_url: ${{ github.event.repository.ssh_url }}
          # 👇 Add your value like this:
          <your_variable>: ${{ <your_contextual_value> }}}

    # ...
```

<br>

Note that [tmplr contextual values](https://github.com/loreanvictor/tmplr#contextual-values) are still available for your recipe,
specifically [git context](https://github.com/loreanvictor/tmplr#git-context) is accessible, which might be easier to use, rather than
Github Actions context.

```yaml
# .tmplr.yml
steps:
  - read: owner_name
    from: git.repo_owner

  - read: owner_email
    from: git.author_email

  # ...
```

<br><br>