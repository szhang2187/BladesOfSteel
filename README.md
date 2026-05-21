# Blades of Steel

A simple 16-bit style two-player platformer fencing game built with HTML, CSS, and JavaScript.

## Play

GitHub Pages link:

https://szhang2187.github.io/BladesOfSteel/

## Features

- Local two-player sword combat
- Peer-to-peer multiplayer lobbies with 4 digit join codes
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

Multiplayer uses browser peer-to-peer networking through Trystero. The site can stay hosted on GitHub Pages, while the active match data is sent directly between players' browsers.

To play multiplayer:

1. Both players choose Multiplayer Mode and enter a name.
2. One player chooses Create Game and shares the 4 digit join code.
3. The other player chooses Join Game and enters that code.
4. When both players are in the lobby, the match starts automatically.

## Run Locally

Open `index.html` in a browser, or start a local server from the project folder:

```powershell
python -m http.server 8000
```

Then visit:

```text
http://localhost:8000
```
