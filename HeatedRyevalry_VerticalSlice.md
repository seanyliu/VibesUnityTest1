# Heated Rye-valry - Vertical Slice Specification

## Overview
A cozy 3D dating sim meets bakery service game for mobile. Players work at a bakery with one beefcake coworker, juggling customer service with romance. Balance serving customers and flirting to keep both your cash register and your coworker's affinity high.

---

## Core Gameplay Loop

### Day Structure
- **Duration**: 3 minutes real-time per day
- **Vertical Slice Scope**: ONE complete day
- **Structure**:
  - **0:00-0:30** - Morning rush begins (slower pace)
    - 1-2 customers arrive
    - Coworker introduces themselves, first dialog choice
    - Tutorial prompts if needed
  - **0:30-2:00** - Peak rush (medium intensity)
    - Customers arrive every 15-20 seconds (6-7 total)
    - Coworker chats 2-3 times during this period
    - Juggling serving + baking + dialog choices
    - Display case starts running low
  - **2:00-3:00** - Final push (high intensity)
    - Last 3-4 customers
    - If affinity high enough, back room option appears
    - Decision: Serve customers OR kiss coworker
  - **3:00** - Day ends with scoring and results

### Win Condition
- Make $50+ to unlock "back room" option at end of day
- Successfully complete first kiss cutscene

---

## Player Actions

### Core Mechanics
1. **Serve Customers**
   - Click on display case item
   - Click on customer to deliver
   - Receive payment

2. **Bake Items**
   - Click baking station
   - Wait for timer (simple, no minigame in V-slice)
   - Click to retrieve baked good
   - Place in display case

3. **Respond to Dialog**
   - Coworker initiates conversation every ~45 seconds
   - 2-3 dialog choices appear with 2D portrait
   - Choice affects affinity

4. **Back Room Romance** (unlocks if affinity ≥70)
   - Player-initiated button appears
   - 30-second kiss cutscene
   - Customers queue up and get annoyed (money loss, affinity gain)

---

## Game Systems

### Customer System
- **Queue Size**: 3-5 customers at once
- **Total per Day**: 10-12 customer interactions
- **Requests**: Simple and specific ("I want a croissant")
- **Patience**: 45 seconds before visibly annoyed
- **If Ignored**: Customer stays but pays $0, leaves bad review
- **If Item Unavailable**: Must bake while they wait (pressure mechanic)

### Baked Goods (4 Types for V-Slice)
1. **Croissant** - $8
2. **Cookie** - $5
3. **Baguette** - $10
4. **Cupcake** - $7

- **Display Case Capacity**: 6 items total
- **Baking Time**: ~20-30 seconds per item
- **Starting Stock**: Case pre-filled with 1-2 of each item

### Affinity System
- **Starting Affinity**: 50/100
- **Visible UI**: Heart meter displayed on HUD
- **Dialog Choice Effects**:
  - Good choice: +5
  - Neutral choice: +1
  - Bad choice: -3
- **Back Room Unlock**: Affinity ≥70
- **Affinity Bonus from Money**:
  - $50+: +5 affinity
  - $30-49: +2 affinity
  - <$30: +0 affinity
- **Consequences**:
  - High affinity (70+): Unlock kiss scene
  - Low affinity (<30): Coworker quits next day (not shown in V-slice, but mentioned)

### Money System
- **Goal**: $50+ for "good day"
- **Prices**: See baked goods above
- **No Negative Consequences**: Low money just means less affinity bonus
- **End-of-Day Display**: Total earnings + affinity change

---

## Characters

### Player Character
- Hot guy (player avatar)
- Minimal characterization (player projection)
- Silent protagonist during gameplay

### Coworker - "Rye" (or Derek/Marcus/Jason - TBD)
- **Appearance**: Beefcake with big biceps, handsome face
- **Personality**: Flirty and confident (can adjust: shy/sweet or sarcastic/teasing)
- **Role**: Stands at register, looks pretty, initiates conversations
- **Does NOT serve customers** (just player does)
- **2D Portrait**: Anime-style bust shot with multiple expressions based on affinity

---

## Visual Design

### 3D Art Style
- **Style**: Low-poly cozy (Animal Crossing vibes)
- **Color Palette**: Pastel colors (warm yellows, soft pinks, cream whites)
- **Lighting**: Warm, inviting bakery atmosphere
- **Polygon Count**: Keep low for mobile optimization

### 2D Portrait Style
- **Style**: Anime-style
- **Framing**: Bust shot (head and shoulders, showing biceps)
- **Expressions**: 3-4 variants (happy, neutral, flirty, annoyed)
- **Background**: Simple gradient or bakery blur

### UI Style
- Clean, modern, friendly
- Rounded corners
- Pastel accents matching 3D environment
- Clear readable fonts for mobile

---

## Bakery Layout (Unity Scene Design)

**Single Room Layout:**
```
        [Back Room Door]
              |
    [Coworker Station] ← Rye stands here
              |
    [Display Case] ← Player serves here
         /    \
        /      \
[Oven/Baking]  [Customer Queue]
   Station        (outside)
```

- **Front**: Display case (player faces customers through window/counter)
- **Left**: Oven/baking station
- **Right**: Coworker's register/workstation
- **Back**: Door to back room (for kiss scenes)
- **Scale**: Small, everything reachable in 2-3 seconds of movement

---

## Controls & Camera

### Camera
- **Type**: Third-person, slightly elevated angle
- **Movement**: Fixed or gentle rotation around bakery
- **Style**: Isometric-ish for clarity

### Player Controls (Mobile-Optimized)
- **Movement**: Tap to move / Virtual joystick (WASD on PC for testing)
- **Interaction**: Tap on objects (customers, display case, oven, coworker)
- **Dialog**: Tap dialog choice buttons
- **UI**: Large touch targets (minimum 44x44 points)

### PC Controls (for Development/Testing)
- **Movement**: WASD
- **Interaction**: Click / E key
- **Camera**: Mouse to look (optional) or fixed

---

## Audio Design (Future Consideration)

- Cozy background music (lo-fi, warm instrumentation)
- Sound effects: Cash register ding, oven timer, footsteps, door chime
- Dialog text "blip" sounds (no voice acting in V-slice)
- Romantic music swell for kiss scenes

---

## Scope Boundaries (What's NOT in V-Slice)

❌ **NOT Included:**
- Multiple coworkers (just one: Rye)
- Coworker replacement mechanic (mentioned but not shown)
- Multiple days (just one 3-minute day)
- Baking minigames (simple timer only)
- Complex customer types (all have simple requests)
- Burning/failing baked goods
- Customer variety (generic sprites okay)
- Persistent save system
- Unlockables/progression
- Multiple kiss scenes (just one at end of day)
- Story/narrative beyond vertical slice demo

✅ **Must Have for V-Slice:**
- One complete 3-minute day
- 4 baked goods with simple baking
- 10-12 customer interactions
- 4 coworker conversations with dialog choices
- Visible affinity system
- Money/scoring system
- One kiss scene (if affinity threshold met)
- 3D bakery environment
- 2D portrait dialog system
- Basic tutorial/onboarding
- End-of-day results screen

---

## Success Metrics (for Playtesting)

- **Clarity**: Can players understand what to do within 30 seconds?
- **Engagement**: Do players want to replay to try different dialog choices?
- **Balance**: Can average players hit $50 and unlock kiss scene?
- **Feel**: Does it feel cozy and romantic, not stressful?
- **Interest**: Do playtesters want to see more coworkers/days?

---

## Development Priorities

1. **Phase 1**: Bakery environment + player movement
2. **Phase 2**: Customer system + serving mechanics
3. **Phase 3**: Baking station + item management
4. **Phase 4**: Dialog system + 2D portraits
5. **Phase 5**: Affinity system + back room scene
6. **Phase 6**: Scoring + end-of-day results
7. **Phase 7**: Polish + tutorial + juice

---

**Target Platform**: Mobile (iOS/Android)
**Engine**: Unity 6 with URP
**Estimated V-Slice Development Time**: [TBD based on team size]

*Last Updated: 2026-01-22*
