# Blades of Steel

A simple 16-bit style two-player platformer fencing game built with HTML, CSS, and JavaScript.

## Play

GitHub Pages link:

https://szhang2187.github.io/BladesOfSteel/

## Features

- Local two-player combat
- Online multiplayer lobbies with 4 digit join codes that work across devices and networks
- 16-bit pixel art: animated fencers, layered map scenery, hit sparks, dash trails, and screen shake
- HP bars and three-life match system
- Big arenas with 9 or 10 platforms each, every one reachable with a jump or double jump
- Fists to start, plus swords that appear on glowing weapon platforms at random spots
- Online sword throwing: a thrown sword arcs under gravity and hits much harder than a swing
- Dashing, jump, and double jump
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
- Attack or pick up a sword: `E`
- Dash: `R`

Player 2:

- Move: `Left Arrow` / `Right Arrow`
- Jump: `Up Arrow`
- Double jump: press `Up Arrow` again in the air
- Attack or pick up a sword: `Enter`
- Dash: `/`

Multiplayer:

Each player uses the same controls on their own computer:

- Move: `A` / `D`
- Jump: `Space`
- Double jump: press `Space` again in the air
- Attack: left click
- Throw your sword: right click
- Pick up a sword: `E`
- Dash: `Q` (`R` also works). You can't be hit during a dash, so it's how you slip through an attack, thrown swords included.

## How To Win

Each player starts with 100 HP and three lives. When a player loses all HP or falls into the void on maps that allow it, they lose a life. The first player to remove all three of the other player's lives wins.

Weapons:

- Everyone starts each round with fists: quick punches with short reach that deal 8 damage.
- A glowing weapon platform with a floating sword appears at a random reachable spot a few seconds into each round. Stand on it or next to it and press your attack key to grab the sword (`E` in multiplayer).
- A sword deals 20 damage and reaches more than twice as far. Its platform crumbles soon after the sword is taken, and another one appears a few seconds later while anyone is still unarmed.
- In multiplayer, right click throws your sword. It flies fast and falls under the same gravity as the fighters, so on flat ground it carries about half the arena, and much farther from a high platform. A hit deals 50 damage, half a health bar. You're back to fists once the sword leaves your hand.
- A sword that hits someone glances off and drops. One that misses sticks point first where it lands. Either way, either player can grab it with `E`. A sword that falls into the void is lost. No new weapon platform appears while a thrown sword is still in the arena.
- Everyone goes back to fists when a life is lost.

## Multiplayer Notes

Multiplayer works between any two devices with a browser and an internet connection, on different networks too. The site stays hosted on GitHub Pages with no game server. Each lobby uses two connection paths at once:

- **Relay:** public MQTT brokers (HiveMQ, shiftr.io, EMQX) over secure WebSockets. Works on almost any network, including mobile data and school or office Wi-Fi. Messages sent through the relay are not encrypted, so anyone listening on the broker could see player names and moves.
- **Direct:** a WebRTC peer-to-peer link set up with [Trystero](https://github.com/dmotz/trystero). It has lower latency, but some networks block it.

The match uses the direct link while it is working and switches to the relay automatically if the direct link fails or never connects.

How a match stays smooth:

- **Your own fighter moves instantly.** Each player simulates their own fighter's movement, jumps, and dashes on their own screen and sends its position 20 to 30 times a second. The host still decides everything that matters for the score: hits, damage, deaths, swords, and rounds.
- **Your opponent moves smoothly.** Their updates are buffered and drawn a little in the past (usually 60 to 220 ms, adapting to the connection), blending between updates so they move at full frame rate. If updates stall, the game pauses the opponent briefly, then replays the backlog and quietly catches up instead of jumping.
- **Hits are judged by what the guest saw.** When the guest swings, the host checks the swing against where its own fighter appeared on the guest's screen at that moment (lag compensation).
- **Thrown swords fly at once and stay in step.** Your own throw leaves your hand immediately. On the other screen, the sword leaves your fighter's hand when that screen shows you throwing, then catches up to the present within a third of a second, so a hit shows when it actually lands. The host flies its own copy of a guest's throw from the same spot and moment, and judges its hits against what the guest saw. Where a sword lands is decided by the host, so it's stuck in the same spot on both screens.
- **Fast lane for positions.** On the direct link, position updates use a separate data channel that never waits for lost packets, so one dropped packet doesn't hold up the rest. On the relay, each update goes to two brokers and the first copy to arrive wins.
- **Moving platforms line up.** Both screens run Neo York's platforms on the host's clock, so they're in the same place for both players.

To play multiplayer:

1. Both players choose Multiplayer Mode and enter a name.
2. One player chooses Create Game and shares the 4 digit join code.
3. The other player chooses Join Game and enters that code.
4. When both players are in the lobby, the match starts automatically.

Troubleshooting:

- Both players need the same version of the page. After an update, refresh (Ctrl+Shift+R) on every device; different versions can't see each other's lobbies.
- Open the browser console (F12). It shows `Multiplayer: using direct peer-to-peer link` or `Multiplayer: using relay servers`.
- Add `?net=relay` or `?net=direct` to the page URL to test one path at a time. Both players must use the same setting.
- To let more players use the direct link, add TURN servers to `MULTIPLAYER_TURN_SERVERS` in `index.html`, for example from a free Metered or ExpressTURN account.

## Graphics

Everything in a match is pixel art drawn in code on one small canvas (360 pixels tall), then scaled up with crisp pixels the way a 16-bit console would. There are no image files to edit:

- Fighters are built from pose skeletons in `FIGHTER_POSES`, with team colors in `FIGHTER_PALETTES`.
- Each map's scenery comes from `buildWeizenArt`, `buildAshigaruArt`, or `buildNeoArt`.
- Platform layouts are in `WEIZEN_LAYOUT`, `ASHIGARU_LAYOUT`, and `NEO_LAYOUT`, as fractions of the screen. Damage, reach, and timing for fists and swords are in `WEAPONS`. A thrown sword's damage, speed, and lift are in `THROWN_SWORD_DAMAGE`, `THROW_SPEED_PER_WIDTH`, and `THROW_LIFT_PER_HEIGHT`.
- The fighter elements in the page are invisible hitboxes, so changing the art never changes gameplay.

## Publishing

GitHub Pages serves the site from the `gh-pages` branch. The `Publish site` workflow in `.github/workflows/publish-gh-pages.yml` fast-forwards `gh-pages` to `main` on every push. Pushing to `main` puts the game live within a minute or two.

## Run Locally

Open `index.html` in a browser, or start a local server from the project folder:

```powershell
python -m http.server 8000
```

Then visit:

```text
http://localhost:8000
```
