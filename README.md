

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
## 配置文件设置

在根目录中创建一个名为 `.env` 的文件并将所有变量添加到其中。以下是 `.env` 文件的示例内容：

```sh
API_ID=452525
API_HASH=esx576f8738x883f3sfzx83
BOT_TOKEN=55838383:yourtbottokenhere
MULTI_TOKEN1=55838383:yourfirstmulticlientbottokenhere
MULTI_TOKEN2=55838383:yoursecondmulticlientbottokenhere
MULTI_TOKEN3=55838383:yourthirdmulticlientbottokenhere
BIN_CHANNEL=-100
PORT=8081
FQDN=你的ip/或者域名
HAS_SSL=False
```

### 必要变量

在运行机器人之前，您需要设置以下必需的变量：

- `API_ID`：这是您的 Telegram 帐户的 API ID，可以从 my.telegram.org 上获取。

- `API_HASH`：这是您的 Telegram 帐户的 API 散列值，也可以从 my.telegram.org 上获取。

- `BOT_TOKEN`：这是 Telegram 媒体流 Bot 的机器人令牌，可以从 [@BotFather](https://telegram.dog/BotFather) 获取。

- `BIN_CHANNEL`：这是日志频道的通道 ID，在该频道中，机器人将转发媒体消息并存储这些文件以使生成的直接链接正常工作。要获取频道 ID，请创建一个新的 Telegram 频道（公共或私人），在该频道中发布一些内容，将消息转发到[@missrose_bot](https://telegram.dog/MissRose_bot)，**回复所转发的信息**，然后使用 /id 命令。复制转发的频道 ID 并将其粘贴到此字段中。

### 可选变量

除了必填变量外，您还可以设置以下可选变量：

- `ALLOWED_USERS`：用户 Telegram ID 的列表，仅限这些用户使用机器人。
> **注意**
> 如果不填写此字段，则任何人都可以使用您的机器人实例。
> 您也可以通过添加逗号（,）分隔的 ID 来添加多个用户。

- `HASH_LENGTH`：生成链接 URL 时的自定义哈希长度。哈希长度必须大于5且小于64。

- `SLEEP_THRESHOLD`：设置全局 Bot 实例中发生洪水等待异常时的睡眠阈值。引发低于此阈值的异常请求将在休眠所需时间后自动重新调用。默认值为60秒。最好不要填写此字段。

- `WORKERS`：设置处理传入更新的并发工作器的最大数量。默认值为3。

- `PORT`：设置 Web 应用程序侦听的端口。默认值为8080。

- `WEB_SERVER_BIND_ADDRESS`：设置服务器绑定地址。默认值为0.0.0.0。

- `NO_PORT`：可以是 True 或 False。如果设置为 True，则不会显示端口。
> **注意**
> 要使用此设置，必须将您的`PORT`指向HTTP协议的80端口或HTTPS协议的443端口，以使生成的链接起作用

- `FQDN`：如果存在，则为完全限定域名。默认为`WEB_SERVER_BIND_ADDRESS`
- `HAS_SSL`：可以是True或False。如果设置为True，则生成的链接将采用HTTPS格式。
- `KEEP_ALIVE`：如果您希望服务器每隔`PING_INTERVAL`秒自行ping以避免睡眠，请将其设置为True。在PaaS免费层中非常有用。默认为`False`
- `PING_INTERVAL`：您希望每次ping服务器以避免睡眠的时间（如果您在某些PaaS上），以毫秒为单位。默认为`1200`或20分钟。
- `USE_SESSION_FILE`：为客户端使用会话文件而不是将pyrogram sqlite数据库存储在内存中。

### 如何使用多机器人支持

> **注意**
> 什么是多机器人功能，它是做什么用的？<br>
> 此功能将 Telegram API 请求共享给其他机器人，以避免触发 floodwaited(一种在后台进行速率限制的方式，以避免过量占用其服务器资源)，从而使服务器能够处理更多请求。<br>

要启用多机器人，请生成新的机器人令牌，并将其添加为环境变量，其中包含以下键名。

`MULTI_TOKEN1`：在此处添加第一个机器人令牌。

`MULTI_TOKEN2`：在此处添加第二个机器人令牌。

您还可以添加任意数量的机器人（最大限制尚未测试）`MULTI_TOKEN3`、`MULTI_TOKEN4`等。

> **警告**
> 在使用机器人之前，请不要忘记将所有机器人（包括多客户端机器人）作为管理员添加到 `BIN_CHANNEL` 中。

`/start`：用于检查机器人是否在线。

要获取即时的流链接，只需将任何媒体转发给机器人，然后机器人会立即回复该 Telegram 媒体消息的直接链接。
## 常问问题

- 链接会保持有效多长时间？链接是否有过期时间？

> 只要您的机器人存在并且您没有删除日志频道，链接就会保持有效。
