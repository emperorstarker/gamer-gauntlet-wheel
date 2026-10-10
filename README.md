DISCLAIMER: THIS TOOL WAS CREATED WITH ASSISTANCE FROM AI. IT ASSISTED IN DIALING IN THE ANIMATION AND STREAMER.BOT CONNECTION. IT ASSISTED IN STRUCTURING THE README AS WELL. IF YOU DO NOT WANT TO USE TOOLS CREATED WITH ASSISTANCE FROM AI, FEEL FREE TO TAKE THE CODE AND ADJUST IT AS YOU WISH.

# Gamer Gauntlet Wheel

A small stream overlay that shows a rotating list of games, with the current game highlighted. It's built for challenges where you play a set of games in a row, inspired by the Ludwig God Gamer Gauntlet. The wheel sits on the current game and only moves when you press a hotkey: one key moves forward, another moves back. It runs through [Streamer.bot](https://streamer.bot).

## What's in this repo

- `index.html`: the overlay.
- `images/`: placeholder icons named `1`, `2`, `3`, and so on.
- 'IMPORT.txt': a Streamer.bot import string containing the action that controls the overlay, including the optional Twitch category syncing.

## 1. Set up the overlay

1. Download this repository (Code → Download ZIP) and unzip it somewhere on your PC.
2. Open `index.html` in a text editor and edit the **EDIT THIS SECTION** block at the top of the script:
   - `GAMES`: your game list, in order.
   - `IMAGE_FOLDER` and `IMAGE_EXTS`: where your icons are and which file types to look for (`png`, `jpg`, `jpeg`, `webp`, `gif` by default).
3. Put your icons in the `images` folder, named `1`, `2`, `3`, and so on, matching the order of your list. PNG and JPG files can be mixed, for example `1.png` and `2.jpg`. Keep extensions lowercase. If an image is missing, the wheel shows a numbered circle instead.
4. To use a different image for a specific game, write that entry as `{ title: "Game Name", image: "images/mygame.jpg" }`.

## 2. Add it to OBS

1. Add a **Browser** source.
2. Tick **Local file** and choose your `index.html`.
3. Set the width to **500** and the height to **200**.
4. Untick **Shutdown source when not visible**, so the wheel doesn't reset when you switch scenes.

## 3. Set up Streamer.bot

1. Go to **Servers/Clients → WebSocket Server**, start it, and use address `127.0.0.1` and port `8080` (or change `STREAMERBOT_URL` in `index.html` to match). This is the default server and port for Streamer.bot.
2. Click the "Import" button on the toolbar and use the text string in the IMPORT.txt file to import the actions that control the overlay directly into Streamer.bot.
3. The action and sub-actions will be disabled after importing. Enable the "Gamer Gauntlet Wheel" action.
4. Enable the **Execute Code** sub-action under the "Rotate Wheel" group. Only enable the **Execute Code** under the "Twitch Channel Category Syncing" group if you want that function. It is detailed further on and requires extra customization.

How it behaves:
- Right moves the wheel forward and Left moves it back.
- By default it wraps around at the ends. Set `loopAround` to `false` if you'd rather it stop at the first and last game.
- A second press within a fraction of a second is ignored, so an accidental double tap doesn't skip a game.
- The current position is saved in a Streamer.bot global variable called `gauntletIndex`. It survives restarting Streamer.bot.
- When the overlay loads, it asks Streamer.bot for the saved position, so refreshing it or restarting OBS keeps it on the right game.
- To start a new run, set `gauntletIndex` back to `0` in Streamer.bot's global variables, then refresh the browser source.
- The overlay reconnects automatically if Streamer.bot is started after OBS.


## 4. Optional: update your Twitch category too

This makes your Twitch category follow the wheel. It waits until the wheel has stayed on a game for a few seconds, so you can move through several games and only the one you stop on is sent to Twitch.

1. Right click the **Execute Code** sub-action in the "Twitch Channel Category Syncing" sub-action folder and click Edit Sub-Action.
2. Edit its `categories` list. Use the same order as your `GAMES` list, and spell each Twitch category name exactly as Twitch does. Leave an entry as `""` to leave the category alone for that game.
3. Set `delaySeconds` to how long the wheel should sit on a game before the category changes (5 by default).
4. Save, compile, and make sure the sub-action is enabled.
5. Test every game once. If Twitch doesn't accept a name, a warning appears in the Streamer.bot log.

Keep the action on a queue that isn't **blocking** (the default queue is fine). A blocking queue would make each key press wait for the delay before the wheel moves.

## Using your own artwork

The placeholder images in this repo are just numbers. Replace them with your own icons to represent the games in your list.