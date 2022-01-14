# GitHub-AWS-OIDC

It's like [`aws-actions/configure-aws-credentials`](https://github.com/aws-actions/configure-aws-credentials) for [OIDC](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect), but it sets `AWS_WEB_IDENTITY_TOKEN_FILE` instead of exporting temporary credentials that cannot be refreshed.

```yml
on: push
permissions:
  id-token: write
  contents: read
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: christopher-dG/gh-aws-oidc@v1
        with:
          role-arn: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
      - name: Test credentials
        run: aws sts get-caller-identity
      - name: Let credentials expire
        run: sleep 61m
      - name: Ensure that credentials are refreshed
        run: aws sts get-caller-identity
```
