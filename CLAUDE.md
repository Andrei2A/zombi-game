# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Roblox 2015-style 3D zombie survival browser game. Single HTML file using Three.js. Russian language UI.

## Running the Game

Open `index.html` in a web browser. No build process or server required.

## Architecture

**Single-file structure** (`index.html`):
- CSS styles (~lines 7-280) - UI for inventory, shop, HUD, infection timer, game over
- HTML (~lines 281-400) - UI elements, shop items, inventory slots
- JavaScript (~line 400+) - Three.js scene, all game logic

**Core Classes/Objects**:
- `RobloxCharacter` class (~line 435) - Player model with animated limbs, equipment groups (vestGroup, helmetGroup, hazmatGroup), weapon attachments
- `player` object - Instance with additional properties: skinMaterial, eyeMaterial, gun, ak47, shotgun, showGun()

**Zombie System**:
- `createZombie(x, z, type)` - Types: 'normal' (green), 'red' (fast, wave 2+), 'swat' (armored, wave 3+)
- `zombies[]` array - Active zombies
- `deadZombies[]` array - Corpses that remain on ground
- `spawnNewWave()` - Increasing difficulty, more zombies per wave

**Weapons** (inventory slots 1-3):
- Pistol (default) - 30 ammo, moderate fire rate
- AK-47 (shop 400 coins) - 30 ammo, fast fire rate, two-handed
- Shotgun (shop 400 coins) - 8 ammo, slow but kills instantly, explodes heads

**Combat System**:
- `checkBulletZombieCollision()` - Headshot detection, limb detection, damage calculation
- `killZombie()` - Death with gore effects (blood pool, organs, bones)
- `explodeHead()` - Shotgun headshot effect with chunks
- `createBulletWound()` - Body wounds attached to zombie mesh
- `createLimbBlood()` - Blood/flesh chunks when shotgun hits limbs

**Limb Damage System** (shotgun only):
- Leg shot: zombie crawls (isCrawling=true), 70% speed reduction, legs hidden
- Arm shot: 40% damage reduction per arm, arm hidden
- Both arms shot: 80% damage reduction total
- Zombie properties: leftArmShot, rightArmShot, legsShot, isCrawling, originalSpeed, originalDamage

**Infection System**:
- `startInfection()` - 2-minute timer, speeds up with each bite
- `playerTurnsZombie()` - Visual transformation (green skin, red eyes), game over
- `cureInfection()` / `buyAntidote()` - Cure via shop (100 coins)

**Shop System** (B key toggles):
- `buyArmor()` - +50 armor (50 coins)
- `buyAK47()` / `buyShotgun()` - Weapons
- `buyHazmatArmor()` - +600 armor with gas mask (600 coins)
- `updateShopUI()` - Shows/hides items based on purchase state

**Gore System**:
- `allGoreObjects[]` - Tracked for cleanup on restart
- Blood pools, organs, bones created in killZombie()
- Head chunks from explodeHead()

**Audio**: Web Audio API for procedural sounds (playShootSound, playAK47Sound, playShotgunSound, playReloadSound)

**Controls**: Arrow keys (move), Space (jump), Mouse (aim/shoot), R (reload), 1-5 (inventory), B (shop)

## Dependencies

Three.js r128 from CDN (cdnjs.cloudflare.com)
