# make S3
aws s3 mb --region ap-northeast-1 s3://yuchesc
 
 
## CodeCommit

### delete repository
aws codecommit delete-repository --repository-name yuchesc

### create repository
aws codecommit create-repository --repository-name yuchesc

{
    "repositoryMetadata": {
        "repositoryName": "www",
        "cloneUrlSsh": "ssh://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/www",
        "lastModifiedDate": 1529135645.368,
        "repositoryId": "b8b7a1f5-ebec-4ff0-84c1-281d6d6b7ccb",
        "cloneUrlHttp": "https://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/www",
        "creationDate": 1529135645.368,
        "Arn": "arn:aws:codecommit:ap-northeast-1:658519869763:www",
        "accountId": "658519869763"
    }
}

### git clone

git config --global credential.helper "!aws codecommit credential-helper $@"
git config --global credential.UseHttpPath true

.gitconfig

[credential]
        helper = "!aws codecommit credential-helper $@"
        UseHttpPath = true
