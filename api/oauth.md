#OAuth

OAuth 2.0 is supported on Delicious now, and here are the steps you need to take if you decide to use it in your development. 

You can always create an application [here](https://delicious.com/settings/developer). (Log in to Delicious if it sends you to landing page)

* Send request to Delicious for users’ authorization.
* Delicious present an authorization page to users on which they decide to authorize or not.
* After users’ authorization, Delicious will redirect to your redirect url with request token.
* Exchange `access_token` with request token returned to you in last step.
* Call Delicious API with `access_token`.

---

## `/auth/authorize`

Request for users’ authorization. 

### Parameters

- `client_id={app_key}` (required).
- `redirect_uri={www.example.com}` (required).

### Examples

#### Example request

https://delicious.com/auth/authorize?client_id=f5dad5a834775d3811cdcfd6a37af312&redirect_uri=http://www.example.com/redirect

#### Example result

https://www.example.com/redirect?code=fa746b2eb266cab06f34fb7bc3d51160


## `/auth/token`

Get `access_token`.

### Parameters

- `client_id={app_key}` (required).
- `client_secret={app_secret}` (required).
- `grant_type={code|credentials}` (required).
- `redirect_uri={www.example.com}` (required).
- `code={request_token}` (optional) -- required when `grant_type=code`.
- `username` (optional) -- required when `grant_type=credentials`.
- `password` (opitional) -- required when `grant_type=credentials`.

### Examples

#### Example request

https://avosapi.delicious.com/api/v1/oauth/token?client_id=f5dad5a834775d3811cdcfd6a37af312&client_secret=7363879fee6c3ab0f93efbd24111ad34&grant_type=code&code=fa746b2eb266cab06f34fb7bc3d51160

#### Example result

```{
    "status": "success",
    "delta_ms": 74,
    "server": "del-api-test",
    "session": "iteqcp5llxi5ulw2hzqy3uo",
    "api_mgmt_ms": 0,
    "version": "v1",
    "access_token": "7421140-262ce921d8572ab75031bfb505e46a1c"
}```

## API call with access token

To access protected resource, simply include access token in HTTP header:

```
GET ... HTTP/1.1
...
Authorization: Bearer <ACCESS_TOKEN>
...
```

You can always test with curl:

```
curl "https://delicious.com/<API_URL>" -H "Authorization: Bearer <ACCESS_TOKEN>"
```



