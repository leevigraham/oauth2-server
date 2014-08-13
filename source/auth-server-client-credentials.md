---
layout: layout
title: Authorization server with client credentials grant
permalink: /authorization-server/client-credentials-grant/
---

# Authorization server with client credentials grant

## Setup

Wherever you intialise your objects, initialize a new instance of the authorization server and bind the storage interfaces and authorization code grant:

~~~.language-php
$server = new \League\OAuth2\Server\AuthorizationServer;

$server->setSessionStorage(new Storage\SessionStorage);
$server->setAccessTokenStorage(new Storage\AccessTokenStorage);
$server->setClientStorage(new Storage\ClientStorage);
$server->setScopeStorage(new Storage\ScopeStorage);

$clientCredentials = new \League\OAuth2\Server\Grant\ClientCredentialsGrant();
$server->addGrantType($clientCredentials);
~~~

## Implementation

The client will request an access token so create an `/access_token` endpoint.

~~~.language-php
$router->post('/access_token', function (Request $request) use ($server) {

    try {

        $response = $server->issueAccessToken();
        return new Response(
            json_encode($response),
            200
            [
                'Content-type'  =>  'application/json',
                'Cache-Control' =>  'no-store',
                'Pragma'        =>  'no-store'
            ]
        );

    } catch (\Exception $e) {

        return new Response(
            json_encode([
                'error'     =>  $e->errorType,
                'message'   =>  $e->getMessage()
            ]),
            $e->httpStatusCode,
            $e->getHttpHeaders()
        );

    }

});
~~~