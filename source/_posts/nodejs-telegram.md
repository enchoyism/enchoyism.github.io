---
title: Node.js + Telegram 연동하기
date: 2018-01-11 23:10:50
tags: [Node.js Telegram]
---
Node.js와 [Telegram](https://core.telegram.org/) Bot API를 이용하여 메시지를 전달하는 간단한 방법을 소개한다. [Telegram Messenger](https://telegram.org/) 설치가 선행되어야 한다.

# Create Bot

![nodejs-telegram-1](/images/nodejs-telegram-1.png)

1.  @BotFather에게 /start 메시지를 보내어 수행할 수 있는 업무 리스트를 받는다.
2.  Bot을 생성하기 위해 /newbot 메시지를 보낸다.
3.  생성할 Bot이름과 Bot이 사용할 username(**bot**으로 끝나야함)을 전달한다.
4.  위의 과정을 마치면 생성된 Bot에 접근할 수 있는 주소와 **token** 정보를 받게된다. t.me/{username}으로 접근하여 봇과의 채팅을 시작한다.

``` plain
Done! Congratulations on your new bot.
You will find it at t.me/{username} You can now add a description,
about section and profile picture for your bot,
see /help for a list of commands.
By the way, when you've finished creating your cool bot,
ping our Bot Support if you want a better username for it.
Just make sure the bot is fully operational before you do this.

Use this token to access the HTTP API: {token}

For a description of the Bot API,
see this page: https://core.telegram.org/bots/api
```

# Message push

## Get chat_id

``` bash
$ curl -XPOST 'https://api.telegram.org/bot{token}/getUpdates'
{
	"ok": true,
	"result": {
		"message_id": 11,
		"from": {
			"id": {chat_id},
			"is_bot": true,
			"first_name": 	"{first_name}",
			"username": 	"{username}"
		},
	... 이하생략
	}
}
```

## Send message

``` bash
$ curl -XPOST 'https://api.telegram.org/bot{token}/sendmessage' -d '
	&chat_id={chat_id}&text=hello
'
{
	"ok": true,
	"result": [
		... 이하생략</span>
	]
}
```

# Node.js sample

``` javascript
'use strict';

const request = require('request');

const send = (text, callback) => {
    const token = 'YOUR_TOKEN';
    const chat_id = 'YOUR_CHAT_ID';
    const options = {
        url: `https://api.telegram.org/bot${token}/sendmessage`,
        form: { text: text, chat_id: chat_id }
    };

    request.post(options, (error) => {
        if (error) {
            return callback(error);
        }

        callback();
    });
};

send('hello telegram bot', (error) => {
    if (error) {
        console.error(`send error: ${error}`);
        process.exit(1);
    }

    console.log('send success');
    process.exit(0);
});
```
