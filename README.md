# Blades of Steel

A simple 16-bit style two-player platformer fencing game built with HTML, CSS, and JavaScript.

## Play

GitHub Pages link:

https://szhang2187.github.io/BladesOfSteel/

## Features

- Local two-player sword combat
- Online multiplayer lobbies with 4 digit join codes that work across devices and networks
- HP bars and three-life match system
- Sword attacks, dashing, jump, and double jump
- Dash invulnerability
- Map selection screen
- Settings screen with sound effect volume
- Help screen with controls
- Three maps:
  - Weizen Farm
  - Ashigaru Forest
  - Neo York

## Controls

Singleplayer:

Player 1:

- Move: `A` / `D`
- Jump: `W`
- Double jump: press `W` again in the air
- Attack: `E`
- Dash: `R`

Player 2:

- Move: `Left Arrow` / `Right Arrow`
- Jump: `Up Arrow`
- Double jump: press `Up Arrow` again in the air
- Attack: `Enter`
- Dash: `/`

Multiplayer:

Each player uses the same controls on their own computer:

- Move: `A` / `D`
- Jump: `Space`
- Double jump: press `Space` again in the air
- Attack: `E`
- Dash: `R`

## How To Win

Each player starts with 100 HP and three lives. Sword hits deal 20 damage. When a player loses all HP or falls into the void on maps that allow it, they lose a life. The first player to remove all three of the other player's lives wins.

## Multiplayer Notes

Multiplayer works between any two devices with a browser and an internet connection, on different networks too. The site stays hosted on GitHub Pages with no game server. Each lobby uses two connection paths at once:

- **Relay:** public MQTT brokers (HiveMQ, shiftr.io, EMQX) over secure WebSockets. Works on almost any network, including mobile data and school or office Wi-Fi. Messages sent through the relay are not encrypted, so anyone listening on the broker could see player names and moves.
- **Direct:** a WebRTC peer-to-peer link set up with [Trystero](https://github.com/dmotz/trystero). It has lower latency, but some networks block it.

The match uses the direct link while it is working and switches to the relay automatically if the direct link fails or never connects.

To play multiplayer:

1. Both players choose Multiplayer Mode and enter a name.
2. One player chooses Create Game and shares the 4 digit join code.
3. The other player chooses Join Game and enters that code.
4. When both players are in the lobby, the match starts automatically.

Troubleshooting:

- Open the browser console (F12). It shows `Multiplayer: using direct peer-to-peer link` or `Multiplayer: using relay servers`.
- Add `?net=relay` or `?net=direct` to the page URL to test one path at a time. Both players must use the same setting.
- To let more players use the direct link, add TURN servers to `MULTIPLAYER_TURN_SERVERS` in `index.html`, for example from a free Metered or ExpressTURN account.

## Run Locally

Open `index.html` in a browser, or start a local server from the project folder:

```powershell
python -m http.server 8000
```

Then visit:

```text
http://localhost:8000
```
