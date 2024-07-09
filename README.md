# sleep
A simple tutorial to set up remote-lock for windows.

## Step 1. Set up environment

Y'know the usual, venv, that kind of thing.
For those of you who don't know how to do this, simply run these commands or whatever

in a projects folder or something

```
py -m venv venv
```
```
venv\Scripts\activate
```

next, install the requirement (lol)
```
pip install -q discord 
```

## Step 1.5. Run this silly little command so your computer doesnt explode when you run sleep

```
powercfg /hibernate off
```

## Step 2. Make the app

Super basic discord.py script

```python
import discord
from discord.ext import commands
import ctypes
import os

TOKEN = "hahanotokenforyouparttwo" # replace with discord token

intents = discord.Intents.default()
intents.message_content = True # make sure this is also enabled in your bot config thingy

bot = commands.Bot(command_prefix='?', intents=intents) # you could use commands if you wanted I suppose

TARGET_USER_ID = {your uid as int here}

@bot.event
async def on_ready():
    print(f'{bot.user} has connected to Discord!')

@bot.event
async def on_message(message):
    if message.author.id == TARGET_USER_ID:
        content = message.content.lower()
        if content == 'lock':
            await message.delete()
            ctypes.windll.user32.LockWorkStation()
        elif content == 'sleep':
            await message.delete()
            os.system("rundll32.exe powrprof.dll,SetSuspendState 0,1,0")
    else:
        message.channel.send("Insufficient permissions.")

    await bot.process_commands(message)

bot.run(TOKEN)
```

## Step 3. Make the batch file in shell:startup

```sh
REM #!/bin/bash SHABANG!! scared? sorry, force of habbit
call path\to\your\venv\Scripts\activate.bat
pyw path\to\your\app.py
exit REM this doesnt work lol you'll have to close the window manually
```

cool beans
