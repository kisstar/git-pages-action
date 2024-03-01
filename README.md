# Deploy Action

This action will handle the deployment process of your project.

## Configuration

| Key             | Value Information                                                                                                                                            | Type | Required |
| :-------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- | :------- |
| git-hosts       | This is the hosts you wish to deploy to, for example, github.com or gitee.com.                                                                               | with | Yes      |
| repository-name | Allows you to specify a different repository path so long as you have permissions to push to it. This should be formatted like so: kisstar/git-pages-action. | with | Yes      |
| branch          | This is the branch you wish to deploy to, for example, gh-pages or docs.                                                                                     | with | Yes      |
| folder          | The folder in your repository that you want to deploy.                                                                                                       | with | Yes      |
| ssh_private_key | A private SSH key stored as a secret.                                                                                                                        | with | Yes      |

## Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3
      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16.x
      - name: Install and Build # This example project is built using npm and outputs the result to the 'build' folder.
        run: |
          npm ci
          npm run build

      - name: Set git user information
        run: |
          git config user.name $USER_NAME
          git config user.email $USER_EMAIL
        env:
          USER_NAME: kisstar
          USER_EMAIL: dwh.chn@foxmail.com

      - name: Deploy with gh-pages for Gitee
        uses: kisstar/git-pages-action@main
        with:
          git-hosts: gitee.com
          repository-name: kisstar/notebook
          branch: gh-pages
          folder: build
          ssh_private_key: ${{ secrets.GITEE_SSH_PRIVATE_KEY }}
```

## License

[MIT](LICENSE) © kisstar
