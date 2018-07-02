SonarQube
=========
Manual setup after install:


## 1. Install plugins
1. Install plugins from Marketplace:  
  Goto `Administration > Marketplace`
  * `GitLab`
  * `Google Authentication for SonarQube`
  * Update all existed plugins, and install additional languages support
2. Manual install plugins:  
  Drop plugin `.jar` file in SonarQube `/extensions/plugins` directory and restart the SonarQube.
  * [`Branch Plugin for SonarQube Community Edition`](https://github.com/s-pw/sonar-branch-community)


## 2. Config Branches
In `Administration > Configuration > General > Branches`, set:
```properties
# Detection of long living branches
sonar.branch.longLivedBranches.regex="master|develop"
```


## 3. Config GitLab
In `Administration > Configuration > GitLab > Reporting`, set:
```properties
# GitLab url
sonar.gitlab.url="https://git.teko.vn/"

# GitLab User Token
sonar.gitlab.user_token="9a618248b64db62d15b3"

# Ping the user
sonar.gitlab.ping_user=true
```


## 4. Config Google OAuth
In `Administration > Configuration > General`, set:
```properties
General
-------------------------------------------------
# Server base URL
sonar.core.serverBaseURL="http://sonarqube.teko.vn"
```

In `Administration > Configuration > Security`, set:
```properties
Security
-------------------------------------------------
# Force user authentication
sonar.forceAuthentication=true
```

```properties
Google
-------------------------------------------------
# Enabled
sonar.auth.googleoauth.enabled="true"

# OAuth client ID
sonar.auth.googleoauth.clientId.secured="2842b98236dc-128d014c84cd98ee14a1bef6cb1f5b32.apps.googleusercontent.com"

# OAuth Client Secret
sonar.auth.googleoauth.clientSecret.secured=7682d133f92de5-31de_41c9

# Allow users to sign-up
sonar.auth.googleoauth.allowUsersToSignUp=true (default)

# Login generation strategy
sonar.auth.googleoauth.loginStrategy="Same as Google login"

# Allowed Domain
sonar.auth.googleoauth.limitOauthDomain="teko.vn"
```


## 5. Users/Groups & Permissions
### 5.1. `Analyser` user
* Create user:
```yaml
Login:    analyser
Name:     Analyser
Password: whatever
```

* Edit `Global Permissions`:

|                      | Administer System | Administer Quality Profiles | Administer Quality Gates | Execute Analysis | Create Projects |
|----------------------|:-----------------:|:---------------------------:|:------------------------:|:----------------:|:---------------:|
| analyser             |                   |                             |                          |        [x]       |                 |
| sonar-administrators |        [x]        |             [x]             |            [x]           |        [x]       |       [x]       |


### 5.2. `sonar-superusers` group
* Create group:
```yaml
Name:         sonar-superusers
Description:  Project managers
```

* Edit `Permission Templates` / `Default template`:

|                      | Browse | See Source Code | Administer Issues | Administer | Execute Analysis |
|----------------------|:------:|:---------------:|:-----------------:|:----------:|:----------------:|
| sonar-administrators |        |                 |        [x]        |     [x]    |                  |
| sonar-superusers     |   [x]  |       [x]       |        [x]        |     [x]    |        [x]       |
| sonar-users          |   [x]  |       [x]       |                   |            |                  |


## 6. Hacking
Connect to SonarQube's **database**, and set:

* Issue [SONAR-10569](https://jira.sonarsource.com/browse/SONAR-10569)  
```
Table: organizations
|          kee         |         name         | new_project_private |
|:--------------------:|:--------------------:|:-------------------:|
| default-organization | Default Organization |         true        |
```

* Issue [#142](https://github.com/gabrie-allaigre/sonar-gitlab-plugin/issues/142)  
```
Table: properties
|                prop_key               | resource_id | text_value |
|:-------------------------------------:|:-----------:|-----------:|
| sonar.gitlab.max_critical_issues_gate |     null    |         -1 |
| sonar.gitlab.max_blocker_issues_gate  |     null    |         -1 |
```

## 7. Gitlab-CI jobs

```yaml
quality:sonarqube:
  stage: test
  image: dungdm93/sonar-scanner:3.2-alpine
  cache:
    key: sonar-cache
    paths: [ .sonar ]
  variables:
    SONAR_USER_HOME: ${CI_PROJECT_DIR}/.sonar
  script:
    - |
      case $CI_COMMIT_REF_NAME in
        "master" | "develop" | release/*) TARGET_BRANCH="master";;
        *) TARGET_BRANCH="develop";;
      esac
    - sonar-scanner
        -Dsonar.host.url=http://sonarqube.teko.vn
        -Dsonar.login=$SONAR_TOKEN
        -Dsonar.branch.name=$CI_COMMIT_REF_NAME
        -Dsonar.branch.target=$TARGET_BRANCH
        -Dsonar.gitlab.project_id=$CI_PROJECT_ID
        -Dsonar.gitlab.commit_sha=$CI_COMMIT_SHA
        -Dsonar.gitlab.ref_name=$CI_COMMIT_REF_NAME
```

## 8. TODO
1. [ ] HTTPS using [certbot](https://certbot.eff.org/)
