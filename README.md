# github-release-bot
![](https://github.com/Ivan-Feofanov/github-release-bot/workflows/lint-and-test/badge.svg)
### Getting started
##### Create telegram bot:
Just follow [instruction](https://core.telegram.org/bots#3-how-do-i-create-a-bot)

##### Fill environment variables:
.env file example:
```shell script
ENVIRONMENT=production
TOKEN=your_bot_token
PROXY_URL=socks5://socks5_proxy_url:socks5_proxy_port
PROXY_USERNAME=socks5_proxy_username
PROXY_PASSWORD=socks5_proxy_url
SECRET_TOKEN=you_secret_token
```
If you don't need proxy just ignore proxy settings.  
If you set ENVIRONMENT value not eqal to "production" bot will ignore security settings.  

You can generate strong token with terminal command:
```shell script
openssl rand -hex 32
```
##### Start your bot:
```shell script
#docker run --env-file .env -p 80:80 --name release-bot feofanov/github-release-bot
docker run --env-file .env -p 80:80 --name release-bot easy-quest/gitbot
```
---
### Release webhook example:
Create your bot url:  
`bot_url = https://where_you_bot.live/release/ + ?chat_id=you_chat_id&release_only=true`   
Set query param **release_only** in true if you don't want getting 
unpublished, created, edited, deleted, or prereleased events.

##### Chat id
First you should invite your bot in chat and then you can find your
 chat id in your bot api:  
`https://api.telegram.org/bot5071233702:AAEju1zUpmvqHzA4zhsQ7XZMAk1R-BFC6zQ/getUpdates` 

##### Github settings
* Open you github project settings and find **Webhook** section
* Add new webhook with params:
  * Payload URL = your bot url
  * Content type = application/json
  * Secret = you_secret_token
* Choose *Let me select individual events*
* Unselect **Pushes** and select **Releases**
* Press **Add webhook**

Congratulations, you're perfect!

##### Release description example:
Since telegram can understand only 
[part of markdown tags](https://core.telegram.org/bots/api#markdown-style), 
bot tries to convert standard markdown into tg-markdowned message.  
Right now bot can correctly format next tags:
* header (`###`) (only three octothorps)
* list (`*`)
* link(`[link name](http://link.address)`)
Correct description example:
```markdown
### Release part subtitle
* [first point title](https://link-tod-escriot.ion/)
* [second point title](https://link-tod-escriot.ion/)
``` 

---
### Custom message
You also can send any custom messages via bot using POST-endpoint '/message/'
This endpoint accepts next params:
```json
{
  "text": "any string you wanna send to bot",
  "chat_id": "your chat id",
  "parse_mode": "markdown|html"
}
```
#### Custom message hook usage example:
```shell script
#!/bin/bash
PROJECT_NAME="my-project-name"
GIT_TAG=$(git describe --tags --abbrev=0)
HOST="http://example.com"
PORT="80"
CHAT_ID="my_chat_id"
KEY="my_secret_key"

PAYLOAD=\
"{
    \"text\":\"Project *$PROJECT_NAME* was succefully deployed with tag *$GIT_TAG*\",
    \"chat_id\": \"$CHAT_ID\",
    \"parse_mode\": \"markdown\"
}"

SIGN="sha1="$(echo -n "$PAYLOAD" | openssl sha1 -hmac $KEY | sed -e 's/^.* //')

ADDRESS="${HOST}:${PORT}/message/"
curl -X POST $ADDRESS -H "X-Hub-Signature: $SIGN" -d "$PAYLOAD"
```


# A Python Tor template on Gitpod

This is template configured for ephemeral development environments on [Gitpod](https://www.gitpod.io/).

## prebuild
[![prebuild](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#prebuild/https://github.com/easy-quest/gitbot)


## Get Started With Your Own Project
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/easy-quest/gitbot)



### A new project

Click the above "Open in Gitpod" button to start a new workspace. Once you're ready to push your first code changes, Gitpod will guide you to fork this project so you own it.

### An existing project

To get started with Python on Gitpod, add a [`.gitpod.yml`](./.gitpod.yml) file which contains the configuration to improve the developer experience on Gitpod. To learn more, please see the [Getting Started](https://www.gitpod.io/docs/getting-started) documentation.

