## AWS CodeCommit 복수 계정 설정

### ~/.ssh/config 설정

```bash
# This file is:  ~/.ssh/config

# You may have other (non-CodeCommit) SSH credentials stored in this
# config file – in addition to the CodeCommit settings shown below.

# NOTE: Make sure to run [ chmod 600 ~/.ssh/config ] after creating this file!

# Credentials for Account1
Host awscc-account1                                 # 'awscc-account1' is a name you pick
  Hostname git-codecommit.us-east-1.amazonaws.com   # This points to CodeCommit in the 'US East' region
  User A1EXAMPLE01234567891                         # UserID as provided by IAM Security Credentials (SSH)
  IdentityFile ~/.ssh/account1-awsCC-rsa            # Path to corresponding key file

# Credentials for Account2
Host awscc-account2
  Hostname git-codecommit.us-east-1.amazonaws.com
  User A2EXAMPLE01234567892
  IdentityFile ~/.ssh/account2-awsCC-rsa

# Credentials for Account3
Host awscc-account3
  Hostname git-codecommit.us-east-1.amazonaws.com
  User A3EXAMPLE01234567893
  IdentityFile ~/.ssh/account3-awsCC-rsa
```

### 리포지토리와 소통방법

| Command |                                                                              |
| ------- | ---------------------------------------------------------------------------- |
| as-is   | git clone ssh://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/my-repo |
| to-be   | git clone ssh://awscc-account1/v1/repos/my-repo                              |

- `awscc-account1` 별칭은 `~/.ssh/config` 파일에 설정된 `Host` 명을 가져다가 사용함

### 기존 리포지토리 업데이트

- 로컬 머신에 이미 클론된 리포지토리가 있을 경우 `/path/to/my-repo/.git/config` 파일에 설정이 있음
- 설정 파일에서 `[remote “origin”]` 섹션을 찾아서 `url` 을 `to-be` 로 변경하면 됨
