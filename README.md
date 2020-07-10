# aws switch role via MFA for Mac

## 動作環境

以下が利用できる環境で動きます

- Mac
- bash
- peco

## 事前準備

- peco install

```
$ brew install peco
```

- awsコマンド使用準備

```
$ my_profile_name=fixme
$ aws configure --profile ${my_profile_name}
```

## 初期設定

- initサブコマンドを利用して、以下を設定
    - MFAログインする際に使用するプロファイル名の設定
    - 認証情報を暗号化して一時保存するためのパスワードの設定
    - MFA Device ARNの設定

```
$ ./aws-switch-role-via-mfa init
```

- init時に最後にprintされるコマンドを打つ

```
Example
---
: In case of zsh
$ echo "source ${HOME}/.zshrc.aws-switch-role-via-mfa" >> ~/.zshrc
$ source ~/.zshrc
---
```

- switch role用のconfigを書く
    - 以下のChrome ExtentionのConfigurationと同様にTOMLで書く
    - https://chrome.google.com/webstore/detail/aws-extend-switch-roles/jpmkfafbacpgapdghgdpembnojdlgkdl?utm_source=github

```
$ vim ~/.aws-switch-role-via-mfa/switch-role-config

: config例
$ cat ~/.aws-switch-role-via-mfa/switch-role-config
[session-name1]
aws_account_id = 123456789010
role_name = your-role1

[session-name2]
aws_account_id = 123456789011
role_name = your-role2
```

## 普段の使い方

1. MFAを利用して一時認証情報を取得し、暗号化して一時保存

```
$ aws-switch-role-via-mfa mfa

または

$ aws-switch-role-via-mfa mfa $MFA_TOKEN
```

2. assume roleで一時認証情報を取得し、暗号化して一時保存

```
$ aws-switch-role-via-mfa assume-role

: または

$ aws-switch-role-via-mfa assume-role $SESSION_NAME

: または

$ aws-switch-role-via-mfa assume-role-all
```

3. 環境変数に使用したい認証情報をexport (スイッチロール)

```
$ switch-role

: または

$ switch-role $SESSION_NAME
```

4. current roleの確認

```
$ current-role
```

5. 通常通りawsコマンドを利用

```
$ aws s3 ls
$ aws ec2 describe-instances
...
```
