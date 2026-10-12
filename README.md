# Swarm Arena

A top-down arena survival shooter that runs in a single HTML file. No build step, no dependencies: just a canvas and some JavaScript.

**Play it:** https://victorrong66.github.io/2d-games-base/

## The game

You pilot a small ship in a large arena while swarms of monsters close in. Your ship **fires automatically** at the closest enemy, so all you have to do is move: dodge, kite, and stay alive.

- **Stages.** Each stage has a kill goal. Clear it and the next, tougher stage begins.
- **Upgrades.** Between stages you pick one of three upgrades (rapid fire, heavy rounds, split shot, piercing, orbit blades, shockwave, regen and more) and recover 25% health.
- **Enemies.** Grunts, fast runners, tanky brutes, and spitters that shoot back.
- **Drops.** Defeated enemies sometimes drop **health packs** (+20 HP) or a **shield**, which blocks damage for a few seconds, deflects enemy shots back as your own bullets, and shoves monsters away.
- **Bosses.** Every 5th stage is a boss stage with four boss types, each with its own attack:
  - **Hive Queen** fires rings of bullets
  - **Gloop Splitter** splits into smaller blobs when killed
  - **Rage Blob** winds up and dashes at you
  - **Void Swirl** sprays spiral bullet streams

  Later boss stages send several bosses at once.
- **Save file.** Your run saves itself in your browser at the start of every stage and when the upgrade screen opens. Close the tab or pick **Quit to menu** from the pause screen, and the start screen offers **Continue** from that point. Dying ends the run and clears the save.
- **Best score** is saved in your browser.

## Controls

| Platform | Move | Pause |
| --- | --- | --- |
| Phone / tablet | Touch and drag anywhere to steer (virtual joystick) | Tap the **II** button |
| Desktop | **WASD** or **arrow keys** | **P** (or Esc) |

Shooting is automatic. The game also pauses itself when you switch tabs.

## Running it

Download or clone the repo and open `index.html` in any modern browser. That's it.

```sh
git clone https://github.com/Victorrong66/2d-games-base.git
cd 2d-games-base
open index.html        # macOS; on Windows use `start index.html`, on Linux `xdg-open index.html`
```

## License

[MIT](LICENSE)
