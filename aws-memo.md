# AWS memo

## Local

### Change profile

```bash
export AWS_DEFAULT_PROFILE=hogepiyo
```

## CodeCommit

### Create CodeCommit

```bash
aws codecommit create-repository --repository-name {name} --repository-description {description}
```

### Make develop branch

Get commit-id first.

```bash
aws codecommit get-branch --repository-name {name} --branch-name master
```

```json
{
    "branch": {
        "branchName": "master",
        "commitId": "{use this value}"
    }
}
```

Create develop and set it as default branch.

```bash
aws codecommit create-branch --repository-name {name} --branch-name develop --commit-id {commitId}
aws codecommit update-default-branch --repository-name {name} --default-branch-name develop
```

## S3

### sync with content-type.

<script src="https://gist.github.com/yuchesc/82b1a866f4b96a79642b8c6073d32c4d.js"></script>
