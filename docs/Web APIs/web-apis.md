---
title: REST APIs
hide:
  - footer
---
 
# REST APIs

!!! warning "Prerequisites"
    Before reading this document, it is recommended that you read the documents:

      - The two technologies and the tools.
      - Fundamental definitions.

## Connection operations
The system offers JSON REST APIs, the authentication procedure is handled via the OAuth 2 protocol based on credentials. 
To make authentication possible, the user must have a Nextome account. The user must then provide Nextome with an email and follow the instructions to set the password and activate the account.
After creating the account, it will be possible to proceed with authentication.
First you need to generate the 'clientId' and the 'clientSecret' through the Nextome portal through the following operations:

- access the portal [Nextome Hub Web](https://hub.nextome.com/dashboard) by entering the credentials provided by Nextome
- click on the icon of the user at the top right (in the figure 'MR')
    <p style="text-align: center;"><img src="/assets/Web APIs/1.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 1: Dashboard.</font> </figcaption></p>

- after clicking on the name of the user, you can access the following screen dedicated to the user
        <p style="text-align: center;"><img src="/assets/Web APIs/2.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure : User's Section.</font> </figcaption></p>

- click on 'ADD API APP' to generate the 'clientId' and the 'clientSecret'

Then it is necessary to retrieve the token (which has a duration of one hour) via the POST request to the endpoint *https://api.nextome-ext.com/apps/auth* , which has formed a JSON body with two 'clientId' fields and 'clientSecret'. 

For example:
```
{
  "clientId": "97sv0tntua35arne6uo30614r9",
  "clientSecret": "10hj5i07htnu18jitial2dhg5tn4kd6fel6gg4clp3r6a8cj6o11"
}
```

The 'clientId' and the 'clientSecret' can be generated independently via the portal as mentioned above.
The token obtained in the accessToken field will be used to authenticate all other requests. To do this, you need to add the Authorization field formatted as follows in the header: Bearer `<here the token>`. 

For example:
```
“Bearer      eyJraWQiOiJIRTBQY2NqeE1ZcEt1cit1SExUa0dHdmdOMHJ6Z2hFTnhiUDZiTHArXC95dz0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJiMTU0ODOxYS1kMGY5LTRlYWQtODE4OS1kYjBjZDFjOTA5ZDUiLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuZXUtd2VzdC0xLmFtYXpvbmF3cy5jb21cL2V1LXdlc3QtMV9OM ......
```


The API call specifications are avaible at the [Swagger](https://api.nextome.dev/swagger-ui/index.html#/).

