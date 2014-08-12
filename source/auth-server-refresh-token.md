---
layout: layout
title: Authorization server with refresh token grant
permalink: /authorization-server/refresh-token-grant/
---

# Authorization server with refresh token grant

## Setup

Wherever you intialise your objects, initialize a new instance of the authorization server and bind the storage interfaces and authorization code grant:

~~~.language-php
$server = new \League\OAuth2\Server\AuthorizationServer;

$server->setSessionStorage(new Storage\SessionStorage);
$server->setAccessTokenStorage(new Storage\AccessTokenStorage);
$server->setClientStorage(new Storage\ClientStorage);
$server->setScopeStorage(new Storage\ScopeStorage);
$server->setAccessTokenStorage(new Storage\RefreshTokenStorage);

$refrehTokenGrant = new \League\OAuth2\Server\Grant\RefreshTokenGrant();
$server->addGrantType($refrehTokenGrant);
~~~

When the refresh token grant is enabled, a refresh token will automatically be created with access tokens issued requested using the [authorization code](/authorization-server/auth-code-grant/) or [resource owner password credentials](/authorization-server/resource-owner-password-credentials-grant/) grants.