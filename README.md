

## 关于机器人

<p align="center">
    <a herf="https://github.com/EverythingSuckz/TG-FileStreamBot">
        <img src="https://telegra.ph/file/a8bb3f6b334ad1200ddb4.png" height="100" width="100" alt="Telegram Logo">
    </a>
</p>
<p align='center'>
    这个机器人可以为您获取 Telegram Web文件的直链。
</p>

### 原始版本库

代码来自自 [Megatron](https://github.com/eyaadh/megadlbot_oss)，感谢 [eyaadh](https://github.com/eyaadh) 的项目。

## 如何运行TG-FilelinkBot

你可以在 VPS 或者本地运行，也可以使用 Docker 部署。运行前需要设置一些必需变量，如 API ID、API HASH 和 BOT_TOKEN 等。

### 在 VPS 或本地运行

```sh
git clone https://github.com/lzw981731/TG-FilelinkBot
cd TG-FilelinkBot
python3 -m venv ./venv
. ./venv/bin/activate
pip3 install -r requirements.txt
python3 -m WebStreamer
```

终止机器人运行，按下组合键 <kbd>CTRL</kbd>+<kbd>C</kbd>

> **如果您想在 后台运行此机器人，请按照以下步骤操作。**
> ```sh
> sudo apt install tmux -y
> tmux
> python3 -m WebStreamer
> ```
> 现在您的机器人已经在后台运行。

### 使用 Docker 部署
首先克隆仓库
```sh
git clone https://lzw981731/TG-FilelinkBot
cd TG-FilelinkBot
```
然后构建 Docker 镜像

使用以下命令构建 Docker 镜像并标记名称为 stream-bot：
```sh
docker build . -t stream-bot
```
现在创建 `.env` 文件并设置变量，然后启动容器：
```sh
docker run -d --restart unless-stopped --name TG-FilelinkBot \
-v /PATH/TO/.env:/app/.env \
-p 8081:8081 \
stream-bot
```
由于 `PORT` 变量用于 URL 生成且必须与容器公开端口一致，因此请确保这两个端口号一致。如果您更改了 `PORT` 变量，请相应更改 docker run 命令（例如：`PORT=9000` -> `-p 9000:9000`）。

如果您已经启动了机器人并需要更改 `.env` 文件中的变量，则只需重新启动容器即可更新机器人设置：
```sh
docker restart TG-FilelinkBot
```


### 使用 docker-compose 部署

首先安装 docker-compose。如果是基于 Debian 的系统，请运行以下命令：
```sh
sudo apt install docker-compose -y
```

然后，克隆存储库：
```sh
git clone https://github.com/lzw981731/TG-FilelinkBot
cd TG-FilelinkBot
```

无需创建 `.env` 文件，只需编辑 `docker-compose.yml` 中的变量。

现在运行 compose 文件：
```sh
sudo docker compose up -d
```
## Setting up things

If you're locally hosting, create a file named `.env` in the root directory and add all the variables there.
An example of `.env` file:

```sh
API_ID=452525
API_HASH=esx576f8738x883f3sfzx83
BOT_TOKEN=55838383:yourtbottokenhere
MULTI_TOKEN1=55838383:yourfirstmulticlientbottokenhere
MULTI_TOKEN2=55838383:yoursecondmulticlientbottokenhere
MULTI_TOKEN3=55838383:yourthirdmulticlientbottokenhere
BIN_CHANNEL=-100
PORT=8080
FQDN=yourserverip
HAS_SSL=False
```

### Mandatory Vars
Before running the bot, you will need to set up the following mandatory variables:

- `API_ID` : This is the API ID for your Telegram account, which can be obtained from my.telegram.org.

- `API_HASH` : This is the API hash for your Telegram account, which can also be obtained from my.telegram.org.

- `BOT_TOKEN` : This is the bot token for the Telegram Media Streamer Bot, which can be obtained from [@BotFather](https://telegram.dog/BotFather).

- `BIN_CHANNEL` :  This is the channel ID for the log channel where the bot will forward media messages and store these files to make the generated direct links work. To obtain a channel ID, create a new telegram channel (public or private), post something in the channel, forward the message to [@missrose_bot](https://telegram.dog/MissRose_bot) and **reply the forwarded message** with the /id command. Copy the forwarded channel ID and paste it into the this field.

### Optional Vars
In addition to the mandatory variables, you can also set the following optional variables:

- `ALLOWED_USERS`: The user Telegram IDs of users to which the bot only reply to.
> **Note**
> Leave this field empty and anyone will be able to use your bot instance.
> You may also add multiple users by adding the IDs separated by comma (,)

- `HASH_LENGTH` : This is the custom hash length for generated URLs. The hash length must be greater than 5 and less than 64.


- `SLEEP_THRESHOLD` : This sets the sleep threshold for flood wait exceptions that occur globally in the bot instance. Requests that raise flood wait exceptions below this threshold will be automatically invoked again after sleeping for the required amount of time. Flood wait exceptions requiring longer waiting times will be raised. The default value is 60 seconds. Better leave this field empty.


- `WORKERS` : This sets the maximum number of concurrent workers for handling incoming updates. The default value is 3.


- `PORT` : This sets the port that your webapp will listen to. The default value is 8080.


- `WEB_SERVER_BIND_ADDRESS` : This sets your server bind address. The default value is 0.0.0.0.

- `NO_PORT` : This can be either True or False. If set to True, the port will not be displayed.
> **Note**
> To use this setting, you must point your `PORT` to 80 for HTTP protocol or to 443 for HTTPS protocol to make the generated links work.

- `FQDN` :  A Fully Qualified Domain Name if present. Defaults to `WEB_SERVER_BIND_ADDRESS`

- `HAS_SSL` : This can be either True or False. If set to True, the generated links will be in HTTPS format.

- `KEEP_ALIVE`: If you want to make the server ping itself every `PING_INTERVAL` seconds to avoid sleeping. Helpful in PaaS Free tiers. Defaults to `False`

- `PING_INTERVAL` : The time in ms you want the servers to be pinged each time to avoid sleeping (If you're on some PaaS). Defaults to `1200` or 20 minutes.

- `USE_SESSION_FILE` : Use session files for client(s) rather than storing the pyrogram sqlite database in the memory

### For making use of Multi-Client support

> **Note**
> What it multi-client feature and what it does? <br>
> This feature shares the Telegram API requests between other bots to avoid getting floodwaited (A kind of rate limiting that Telegram does in the backend to avoid flooding their servers) and to make the server handle more requests. <br>

To enable multi-client, generate new bot tokens and add it as your environmental variables with the following key names. 

`MULTI_TOKEN1`: Add your first bot token here.

`MULTI_TOKEN2`: Add your second bot token here.

you may also add as many as bots you want. (max limit is not tested yet)
`MULTI_TOKEN3`, `MULTI_TOKEN4`, etc.

> **Warning**
> Don't forget to add all these bots to the `BIN_CHANNEL` for the proper functioning

## How to use the bot

> **Warning**
> Before using the  bot, don't forget to add all the bots (multi-client ones too) to the `BIN_CHANNEL` as an admin
 
`/start` : To check if the bot is alive or not.

To get an instant stream link, just forward any media to the bot and boom, the bot instantly replies a direct link to that Telegram media message.

## FAQ

- How long the links will remain valid or is there any expiration time for the links generated by the bot?
> The links will will be valid as longs as your bot is alive and you haven't deleted the log channel.

## Contributing

Feel free to contribute to this project if you have any further ideas

## Contact me

[![Telegram Channel](https://img.shields.io/static/v1?label=Join&message=Telegram%20Channel&color=blueviolet&style=for-the-badge&logo=telegram&logoColor=violet)](https://xn--r1a.click/WhySooSerious)
[![Telegram Group](https://img.shields.io/static/v1?label=Join&message=Telegram%20Group&color=blueviolet&style=for-the-badge&logo=telegram&logoColor=violet)](https://xn--r1a.click/WhyThisUsername)

You can contact either via my [Telegram Group](https://xn--r1a.click/WhyThisUsername) ~~or you can PM me on [@EverythingSuckz](https://xn--r1a.click/EverythingSuckz)~~


## Credits

- Me
- [eyaadh](https://github.com/eyaadh) for his awesome [Megatron Bot](https://github.com/eyaadh/megadlbot_oss).
- [BlackStone](https://github.com/eyMarv) for adding multi-client support.
- [Dan Tès](https://telegram.dog/haskell) for his [Pyrogram Library](https://github.com/pyrogram/pyrogram)
- [TheHamkerCat](https://github.com/TheHamkerCat)

## Copyright

Copyright (C) 2022 [EverythingSuckz](https://github.com/EverythingSuckz) under [GNU Affero General Public License](https://www.gnu.org/licenses/agpl-3.0.en.html).

TG-FileStreamBot is Free Software: You can use, study share and improve it at your
will. Specifically you can redistribute and/or modify it under the terms of the
[GNU Affero General Public License](https://www.gnu.org/licenses/agpl-3.0.en.html) as
published by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version. 
