```shell
Docker image for shell-bot (Telegram bot execute host commands thru pipes). 

PREREQUISITES
 - Docker installed

INSTALLATION
 - Docker compose example: 

# Shell-bot -> Add script commandpipe.sh to system boot
  shell-bot:
    container_name: shell-bot
    image: baroka/shell-bot:latest
    restart: unless-stopped
    networks:
      - t2_proxy
    security_opt:
      - no-new-privileges:true
    volumes:
      - $DOCKERDIR/shell-bot/config:/hostpipe
    environment:
      - TZ=$TZ
      - PGID=$PGID
      - PUID=$PUID
      - BOT_TOKEN=$TELEGRAM_NOTIFIER_BOT_TOKEN
      - CHAT_ID=$TELEGRAM_NOTIFIER_CHAT_ID

 - $DOCKERDIR points to your local path with subdirectories with Docker repos
 - $BOT_TOKEN, $CHAT_ID. Telegram channel parameters

<<commandpipe.sh>>
#!/bin/sh

# Files
DOCKERDIR="xxx"
PIPEDIR="$DOCKERDIR/shell-bot/config"
PIPEFILE="$PIPEDIR/commandpipe"
PIPEOUT="$PIPEDIR/commandpipeout"
WHITELISTFILE="$DOCKERDIR/shell-bot/config/whitelist.csv"

# Listen for new commands in pipe
while true; do eval "$(cat $PIPEFILE | grep -f $WHITELISTFILE)" &> $PIPEOUT; done


<<whitelist.csv>> -> whitelisted commands
^ls
^cat


More info: 

  Execute host command from container
  https://stackoverflow.com/questions/32163955/how-to-run-shell-script-on-host-from-docker-container

  Docker sh bot
  https://github.com/yozel/shell-bot
  https://github.com/botgram/shell-bot
```