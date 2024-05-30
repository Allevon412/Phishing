<MFA and Web Security>
If the adversary learns your password the adversary will not know how to obtain the MFA token. This is game over for except in the case of how web works.
The session token needs to be stored in your web browser and will be embedded in every request.
The adversary can steal your web token and provide it to the web server.
The adversary can steal your token using reverse proxy simulation software such as evilginx.

<Stealing the token>
Cookies & local storage are ways to store session tokens. Evilginx can handle both.
Reverse proxy phishing is a one-to-one copy of the real website, proxied dynamically in real time from the real website server.
Phishing server is the middle-man server and will proxy outbound and inbound requests from victim and intended website.
The encrypted connection is made from the user to the phishing server and the phishing server can subsequently decrypt and analyze all traffic from the user.
if the user has mfa enabled on the account, they will be presented with a legitimate MFA challenge & the user will be asked for an MFA token.
EvilGinx will capture the session token once the victim signs into the website & the adversary will have access to the hijacked account.

<Preparing the Environment>
1) Create a new profile user in chrome, comes in handy when we create our own phishlets and we want to delete our cookies for testing. Will be doing it often.
2) setup extensions, edit this cookie, localStorageManager. setting up settings -> privacy bookmark.

<Intro >
help info
help.evilginx.com

go to the webpage you're trying to create a phishlet for
open source inspector -> go to network tab -> click preserve log to ensure we can capture all of our requests.
login to the website.
create yaml phishlet using example as template in the evilginx folder visual studio code can be used or any ide.
Best practice to leave the TLD in the domain parameter, but use the subdomain in the phish_sub & orig_sub parameter.