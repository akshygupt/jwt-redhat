### Requesting JWT auth for a new webapp

Create a new servicenow ticket and provide them the information from https://mojo.redhat.com/docs/DOC-1142936

* clientid (Ex. 'unifiedui')
* Flows used (Typically standard)
* Valid redirect URI's (Ex. https://unified.gsslab.rdu2.redhat.com/*)
* Web Origins (Ex. https://unified.gsslab.rdu2.redhat.com)
* User information required (With Unifiedui we needed the username populated which was the sso and UDS used that as part of the request)

After IAM has created the client profile in their systems, JWT is ready to use.

Here is a snippet of code to use Jwt:

// Typescript 
```
import Jwt                            from 'jwt'
import { IKeycloakOptions }           from 'jwt/src/models'

declare global {
    interface Window {
        Jwt: any;
    }
}
window.Jwt = Jwt; // this is optional if you want to invoke the same instance elsewhere
const JwtOptions: Partial<IKeycloakOptions> = {
    clientId: 'unifiedui'
}

if(window.Jwt) Jwt.init(JwtOptions); // validate session and start the refresh timer

// once the session has initialized, ask session.js some questions
Jwt.onInit(async function () {
    // print user's authentication status, internal status, and their user info
    if(!Jwt.isAuthenticated()) {
        Jwt.login();
    } else {
        const loggedInUser = Jwt.getUserInfo();
        // Any other application specific code can go here.
    }
});
```

// Javascript 
```
const Jwt = require('jwt');
window.Jwt = Jwt; // this is optional if you want to invoke the same instance elsewhere

const jwtOptions = {
    clientId: 'unifiedui'
}

Jwt.init(jwtOptions); // validate session and start the refresh timer
// once the session has initialized, ask session.js some questions
Jwt.onInit(() => {
    // print user's authentication status, internal status, and their user info
    // if the user isn't authenticated, log them in
    if(!Jwt.isAuthenticated()) {
        Jwt.login();
    } else {
        const loggedInUser = Jwt.getUserInfo();
        // Any other application specific code can go here.
    }
});
```

### Contributing

Please click [Contributing Guide](https://gitlab.cee.redhat.com/sshumake/ascension/blob/master/CONTRIBUTING.md)
