# Call/Trigger GCP Firebase CF gen2 'onCall' function

If you would like to call your deployed `onCall` CF function from local terminal or GCP console you will need to

## Prerequisites

- Get URL of your `onCall` function e.g. `https://<region>-<project>>.cloudfunctions.net/<function_name>`

- You need to have acces to any user from Firebase Authentication (email+password) `https://console.firebase.google.com/project/<project>/authentication/users`

- You need to have `WEB API KEY` from `https://console.firebase.google.com/project/<project>/settings/general/`

## Make Auth call to get Firebase Auth ID (`idToken`)

Make auth request - simplest way is to do that via `signInWithPassword` but you can also use `signInWithCustomToken` using SDK.

Request:
```bash
curl --location --request POST 'https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=<WEB_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "<user_email>",
    "password": "<user_password>",
    "returnSecureToken":true}'
```

Response:
```json
{
    "kind": "identitytoolkit#VerifyPasswordResponse",
    "localId": "123",
    "email": "<user_email>",
    "displayName": "",
    "idToken": "id-token-to-use",
    "registered": true,
    "refreshToken": "example-refresh-token",
    "expiresIn": "3600"
}
```

Take `idToken` from response and use in next point.

## Send request/call your `onCall` CF from GCP Console or local 

Request:
```bash
curl --location --request POST 'https://<region>-<project>.cloudfunctions.net/<function_name>' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <idToken>' \
--data-raw '{
  "data": {
    "name": "Hello World"
  }
}'
```

# Links
- [Firebase https.onCall](https://firebase.google.com/docs/functions/callable-reference)
- [Firebase Auth REST API](https://firebase.google.com/docs/reference/rest/auth)