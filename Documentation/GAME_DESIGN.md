# Capital World — Game Design

First-person 3D business and life simulator set in the fictional West Coast city of **Havenport**.
The player arrives with little money, rents an apartment, opens a coffee shop, and grows a business
empire across a chapter-based story.

Status legend used in this file:
- **DECIDED**: the owner chose this. Do not change it without asking.
- **PROPOSED**: Claude suggested it. The owner has not approved it yet, so it may change.
- **OPEN**: not decided yet.

---

## 1. Identity

| Item | Value | Status |
|---|---|---|
| Game title | Capital World | DECIDED |
| City name | Havenport | DECIDED |
| Unreal project / C++ module name | `CapitalWorld` | DECIDED |
| Platform | Windows PC | DECIDED |
| Camera | First person, visible body (you see your own arms, legs and torso) | DECIDED |
| Players | Single-player, with an architecture ready for multiplayer later | DECIDED |
| Language | English only. All player-facing text uses `FText` so translation stays possible | DECIDED |
| Currency | US dollars | DECIDED |
| Release plan | Beta test first, public release later | DECIDED |

## 2. Art direction — DECIDED

- **Cartoony, low-poly, colorful.** This replaces the original spec's "moderately realistic" style.
- **West Coast, sunny.** Bright palette, palm trees, stucco and glass storefronts, wide sidewalks, a coastal feel.
- **Characters are low-poly**, matching the world. MetaHumans are **not** used: they clash with the style and are too heavy for the target PC.
- Flat or simple gradient materials, a few shared texture atlases, strong silhouettes.
- Assets come from free assets whose license allows commercial use, plus assets Claude makes with Blender scripts. See `DECISIONS.md` D-009.

## 3. Time — DECIDED

- **1 in-game day = 36 real minutes.**
  - 1 in-game hour = 90 real seconds.
  - 1 in-game minute = 1.5 real seconds.
  - 1 in-game week = 4 h 12 min real time.
- Day/night cycle follows the in-game clock.

## 4. Modes and difficulty

| Item | Value | Status |
|---|---|---|
| Story mode | Chapter-based, Easy or Hard | DECIDED |
| Easy vs Hard differences | **Prices and costs** (rent, supplier prices, wages) and **customer demand** (how many customers come and how picky they are) | DECIDED |
| Sandbox mode | Unlocks after finishing the final chapter | DECIDED |
| Hard multipliers | Costs ×1.25, customer demand ×0.8 | PROPOSED |

## 5. Story and chapters

| Item | Value | Status |
|---|---|---|
| Structure | Chapters. Each chapter unlocks more of the game | DECIDED |
| Number of chapters | 8 or more | DECIDED (exact count OPEN) |
| End of each chapter | A big event that combines a **story cutscene**, a **challenge**, a **new area opening** and **new business types unlocking** | DECIDED |
| Story and lore | The owner has ideas and will share them later. **Do not write the final story without the owner.** | OPEN |

### Chapter 1 — DECIDED outline
Arrive in Havenport → rent an apartment → buy a bed and a fridge → take a couple of taxi rides → **work a starter job at the supermarket** to save money → open the **coffee shop**. The chapter ends with its first big event (details OPEN).

### Debt limits

The player may go into debt, up to a limit that grows with each chapter.

| Chapter | Debt limit | Status |
|---|---|---|
| 1 | −$5,000 | DECIDED |
| 2 | −$10,000 | DECIDED |
| 3 | −$50,000 | DECIDED |
| 4 | −$100,000 | PROPOSED |
| 5 | −$250,000 | PROPOSED |
| 6 | −$500,000 | PROPOSED |
| 7 | −$1,000,000 | PROPOSED |
| 8 | −$2,500,000 | PROPOSED |

Rules:
- **What counts:** the negative bank balance **plus** outstanding loans. DECIDED
- **At $0:** the player gets a warning. DECIDED
- **Past the limit:** a grace period of a few in-game days, then property is taken. DECIDED
- **Grace period length:** 3 in-game days (1 h 48 min real time). PROPOSED
- **Order property is taken in:** the least valuable non-home property first, then the rest. The home apartment goes last. PROPOSED

## 6. Player needs — hunger and energy

DECIDED: the player has **hunger** and **energy**.

Rules (DECIDED, confirmed by the owner):
- **Hunger at 0** slowly drains energy.
- **Energy at 0 because of hunger:** the player passes out and wakes up at the **hospital**.
- **Energy at 0 from being tired (not hunger):** the player passes out but does **not** go to the hospital. They wake up more slowly and with less energy than after normal sleep.

**OPEN:** where a tired player wakes up (on the spot, or at home).

Food sources (DECIDED): the **home fridge**, **stores**, and **restaurants or cafes**, including the player's own coffee shop.

PROPOSED numbers:
- Hunger goes from 100 to 0 in about 16 in-game hours.
- Energy goes from 100 to 0 in about 20 in-game hours awake.
- Sleeping in a bed restores energy fully in 8 in-game hours.
- Hospital bill: $250.

## 7. Character creator

DECIDED for the first version: **name, gender, and a few presets** (skin tone, hair, outfit colors). More options come later.

## 8. Getting around

| Item | Value | Status |
|---|---|---|
| Walking | Walk, run, jump, crouch | DECIDED (from spec) |
| Taxis | You can **ride along** in the taxi, with an option to **skip** the ride (fade, time passes based on distance) | DECIDED |
| Driving | **Arcade** handling: easy and forgiving | DECIDED |
| Taxi fare | $5 base + $2 per in-game km | PROPOSED |

## 9. Interaction and building

- **Placement uses grid snapping.** DECIDED
- Context prompts appear when the player looks at an interactive object (from the spec).
- Doors, containers, machines, registers, furniture pickup and placement.

## 10. Phone and computers — DECIDED

- **The phone** is used to order things, check prices, and talk to people (vendors and other businesses).
- **Talking** is text-message style, with reply choices **plus haggling** (negotiating prices with vendors).
- **Computers** exist at home and in each business, for ordering, finances and management.

## 11. First business: the coffee shop — DECIDED

- The **coffee shop** is the first playable business. It replaces the spec's convenience store and becomes the template for the business framework.
- **Making drinks:** the player walks to the coffee machine and presses E to make an order, until they hire baristas.
- **Checkout:** the register **shows the order total** and the player **types that amount in** to confirm the sale. There is no item-by-item scanning.

### Starter job: the supermarket — DECIDED
Before owning a business, the player **works a starter job at a supermarket** (owned by an NPC) to save up for the coffee shop.

PROPOSED details:
- The player clocks in at the supermarket and works a shift of 4 or 8 in-game hours.
- Work tasks reuse the business systems that the coffee shop will use later: **running the register** (the same "type in the shown total" checkout) and **restocking shelves** from the stockroom. Building the job first tests these systems before the player's own shop exists.
- Pay is per hour worked, paid into the bank account at the end of each shift.
- The player can quit at any time. Once the coffee shop opens, the job is optional.

### PROPOSED starting economy (Easy mode, Hard = ×1.25 costs)

| Item | Price |
|---|---|
| Starting cash | **$2,000** |
| Supermarket job wage | $25 / in-game hour |
| Studio apartment rent | $350 / in-game week, plus a 1-week deposit |
| Bed | $300 |
| Fridge | $400 |
| Groceries (one meal) | $8 |
| Small retail unit rent (coffee shop) | $900 / in-game week, plus a 1-week deposit |
| Used espresso machine (starter) | $1,200 |
| Counter and register | $800 |
| 2 tables and 4 chairs | $600 |
| Opening supplies (beans, milk, cups, pastries) | $400 |
| Barista wage | $18 / in-game hour |

How the numbers work:
- **Starting cash** covers the apartment, bed, fridge, a few taxi rides and food (about $1,550), with about $450 left over.
- **The job:** an 8-hour shift pays $200. Daily living costs (rent share plus 3 meals) are about $74, so the player saves about $126 per in-game day.
- **The coffee shop** costs about $4,800 to open.
  - Saving the full amount takes about 38 in-game days (about 23 real hours).
  - Chapter 1's −$5,000 debt limit lets a bold player open much earlier, in debt. That makes saving versus borrowing a real choice.
- These numbers will be tuned in playtesting.

PROPOSED menu (sell price / ingredient cost):

| Item | Sell price | Ingredient cost |
|---|---|---|
| Drip coffee | $2.50 | $0.40 |
| Espresso | $3.00 | $0.55 |
| Latte | $4.50 | $0.90 |
| Muffin (bought pre-made from a supplier) | $3.00 | $1.20 |

## 12. Business categories (long term, from the spec)

- **Retail:** convenience, supermarket, clothing, electronics, furniture, specialty.
- **Food:** restaurant, cafe, fast food, bakery.
- **Automotive:** dealership, repair, rental.
- **Corporate:** marketing, web design, consulting, professional services.
- **Logistics:** warehouse, storage, distribution.
- **Services:** gym, salon, cleaning.

Chapters unlock categories gradually. Which chapter unlocks which category is OPEN.

## 13. World

- **Starting area:** one dense, detailed neighborhood with a main commercial street, small shops, restaurants, apartments, parking, roads, small offices, delivery access and a service area.
- **Later districts:** Downtown, Industrial, Shopping, Residential, Suburban. These open at chapter ends.
- **Three building tiers:** fully interactive; accessible but simplified; background only.

## 14. Originality

The game is in the same genre as other business-life sims, but the city, brands, businesses, products, characters, UI and story are all original. Nothing is copied from existing games.
