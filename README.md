## Relaying user info in the entry point request

The usual way of creating a new chat request is by opening the start page of an entry point in the format of:
`https://app.joinswytchboard.com/start/{entryPointId}`.

Opening this link gives to the user an opportunity to see if the entry point is active, choose a chat type and continue on the next page where they can provide the needed info for starting a new convo.

On the occassion when this info (like name, email, phone etc.) is already established and provided, the user request can land directly on the page where they can start a new call request without inputting that data once again.

This can be achieved by passing a special parameter in the request named `sbclid`. The content of the parameter must be a stringified JSON containing the info, which is then encoded in a Base64 url-compatible string.

To elaborate, we have a JSON containing user info:
```json
{
    "name": "(string) Full name of user (optional)",
    "email": "(string) Email address of user (optional)",
    "phone": "(string) Phone number of user (optional)",
    "request_type": "(string) One of the following values: 'video_text'|'text' (reqired)",
    "intro_text": "(string) short info on the request intent, usually filled by the user (optional)"
}
```

The only required option is the request_type that will decide whether the chat will be text or text and video based. Of course, the other fields could also be required, but that depends on the entry point settings.

The JSON created should be stringified, and then encoded into Base64 format (unicode is supported), and the request will have the following format:
`https://app.joinswytchboard.com/start/{entryPointId}?sbclid={encodedUserData}`.

A precaution should be taken care about the size of the sbclid param, which will depend on the size of original JSON request object. Most relevant is the intro_message parameter which if provided then should be limited in size, to prevent max URL limits in the browser.