# GitLab CE Docker Image with OpenID Connect for Dex

This repo updates the [computersciencehouse docker](https://hub.docker.com/r/computersciencehouse/gitlab-ce-oidc/)  image leveraging the patch from https://michael.stapelberg.de/Artikel/gitlab-with-dex that updates the omniauth OIDC gem to not request UserInfo since Dex [currently](https://github.com/coreos/dex/issues/376) does not support one. 

This repository serves to add OpenID Connect support to the official GitLab Docker image. The resulting image, which is automatically built when the upstream `gitlab/gitlab-ce` image is updated, can be found on the [Docker Hub](https://hub.docker.com/r/computersciencehouse/gitlab-ce-oidc/).

## Configuration
To use this image, add the following options to your `GITLAB_OMNIBUS_CONFIG` environment variable, replacing the noted variables with the appropriate values from your IdP:

```
gitlab_rails['omniauth_enabled'] = true;
gitlab_rails['omniauth_allow_single_sign_on'] = true;
gitlab_rails['omniauth_block_auto_created_users'] = false;
gitlab_rails['omniauth_auto_sign_in_with_provider'] = '<PROVIDER_NAME>';
gitlab_rails['omniauth_providers'] = [{'name'=>'openid_connect', 'args'=>{'name'=>'<PROVIDER_NAME>', 'scope'=>['openid', 'profile'], 'response_type'=>'code', 'discovery'=>true, 'issuer'=>'<ISSUER_URL>', 'client_options'=>{'port'=>'443', 'scheme'=>'https', 'host'=>'<IDP_HOSTNAME>', 'identifier'=>'<CLIENT_ID>', 'secret'=>'<CLIENT_SECRET>', 'redirect_uri'=>'https://<GITLAB_URL>/users/auth/<PROVIDER_NAME>/callback'}}}];
```
