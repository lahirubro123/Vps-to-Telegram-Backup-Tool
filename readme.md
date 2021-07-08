# tgsend.sh

Send or update messages or send text files to your own Telegram Bot API created using BotFather:

* https://core.telegram.org/bots#3-how-do-i-create-a-bot
* https://core.telegram.org/bots/api#sendmessage

## Telegram BotFather

![Telegram BotFather Bot Creation](/images/telegram-botfather-01.png)

# Dependencies

Requires jq json tool install:

```
apt-get -y install jq
```
or 
```
yum -y install jq
```

# Usage

Create and populate `tgtoken.ini` file in same directory as tgsend.sh with the following:

```
tgtoken='YOUR_TELEGRAM_BOT_API_TOKEN'
tgchatid='YOUR_TELEGRAM_CHATID'
tgapi="https://api.telegram.org/bot$tgtoken"
```

You can find your Telegram chat id to populate variable `tgchatid` using the following command:

```
tgtoken='YOUR_TELEGRAM_BOT_API_TOKEN'
tgapi="https://api.telegram.org/bot$tgtoken

curl -4s "$tgapi/getUpdates" | jq -r '.result[].message.chat.id'
```

Available commands:

```
./tgsend.sh 

Usage:

./tgsend.sh send "your message in double quotes"
./tgsend.sh sendhtml "your message in double quotes"

disable notifications:

./tgsend.sh sendq "your message in double quotes"
./tgsend.sh sendhtmlq "your message in double quotes"

update existing message with message_id:

./tgsend.sh update "your message in double quotes" message_id
./tgsend.sh updatehtml "your message in double quotes" message_id

update existing message with message_id & disable notifications:

./tgsend.sh updateq "your message in double quotes" message_id
./tgsend.sh updatehtmlq "your message in double quotes" message_id

send file

./tgsend.sh sendf filename
```

# Examples

## Send messages in plain text or HTML format

```
./tgsend.sh send "message text"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-05T08:13:12Z",
  "message": "message text"
}
```
```
./tgsend.sh sendhtml "message <i>html</i>"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-05T08:13:20Z",
  "message": "message html"
}
```
```
./tgsend.sh send "$(free -mlt)"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-06T00:30:14Z",
  "message": "total        used        free      shared  buff/cache   available\nMem:          31973        7189        2183        2229       22600       22162\nLow:          31973       29789        2183\nHigh:             0           0           0\nSwap:          2045           3        2042\nTotal:        34019        7192        4226"
}
```

![Telegram Messages](/images/telegram-send-04.png)

## Update an existing message with message_id

```
./tgsend.sh updateq "updated message" 38
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-08T01:05:35Z",
  "message": "[msgid: 38 Updated: Thu 08-Jul-21 01:05:34 UTC]  updated message"
}
```

![Telegram Messages](/images/telegram-send-06b.png)

## Send text file

```
./tgsend.sh sendf test.txt
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-05T08:50:19Z",
  "document": "test.txt",
  "mime": "text/plain",
  "size": 2
}
```

![Telegram File Send](/images/telegram-send-03.png)

## Send text message with Telegram message_id added

New feature which by default is enabled is to add the message_id to the messages via option `tg_addmsgid='y'`

```
./tgsend.sh send "message v3"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-06T22:33:34Z",
  "message": "[msgid: 37] message v3"
}
```

![Telegram File Send](/images/telegram-send-05.png)

Sending Centmin Mod LEMP stack cminfo command for JSON formatted cpu and memory statistics

```
./tgsend.sh send "$(cminfo sar-json | jq -r '."cpu-load"[]')"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-08T01:16:15Z",
  "message": "[msgid: 40] {\n  \"cpu\": \"all\",\n  \"user\": 1.21,\n  \"nice\": 0,\n  \"system\": 0.49,\n  \"iowait\": 0.43,\n  \"steal\": 0,\n  \"idle\": 97.87\n}"
}
```
```
./tgsend.sh send "$(cminfo sar-json | jq -r '."memory"')"
{
  "from": "centmin",
  "to": "George",
  "date": "2021-07-08T01:17:09Z",
  "message": "[msgid: 41] {\n  \"memfree\": 3111888,\n  \"memused\": 29629116,\n  \"memused-percent\": 90.5,\n  \"buffers\": 1323936,\n  \"cached\": 17754908,\n  \"commit\": 25211564,\n  \"commit-percent\": 72.37,\n  \"active\": 13057488,\n  \"inactive\": 9479208,\n  \"dirty\": 84,\n  \"swpfree\": 2091768,\n  \"swpused\": 3328,\n  \"swpused-percent\": 0.16,\n  \"swpcad\": 204,\n  \"swpcad-percent\": 6.13\n}"
}
```

![Telegram File Send](/images/telegram-send-07.png)