#OAuth

OAuth 2.0 is supported on Delicious now, and here are the steps you need to take if you decide to use it in your development.

* Send request to Delicious for users' authorization
* Delicious present an authorization page to users on which they decide to authorize or not
* After users' authorization, Delicious will redirect to your redirect url with request token
* Exchange access_token with request token returned to you in last step
* Call Delicious API with access_token

---

## `/auth`
 Request for users' authorization

### Parameters

- `client_id={app_key}` (required)
- `redirect_uri={www.example.com}` (required)

### Examples

#### Example request

https://www.delicious.com/auth?client_id=f5dad5a834775d3811cdcfd6a37af312&redirect_uri=www.example.com/redirect

#### Example result

https://www.example.com/redirect?code=fa746b2eb266cab06f34fb7bc3d51160


## `/token`
  Get access_token

### Parameters

- `client_id={app_key}` (required)
- `client_secret={app_secret}` (required)
- `grant_type={code|credentials}` (required)
- `redirect_uri={www.example.com}` (required)
- `code={request_token}` (optional) -- required when ```grant_type=code```
- `username` (optional) -- required when ```grant_type=credentials```
- `password` (opitional) -- required when ```grant_type=credentials```

### Examples

#### Example request

https://delicious.com/token?client_id=f5dad5a834775d3811cdcfd6a37af312&client_secret=7363879fee6c3ab0f93efbd24111ad34&grant_type=code&redirect_uri=www.example.com/redirect&code=fa746b2eb266cab06f34fb7bc3d51160

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


