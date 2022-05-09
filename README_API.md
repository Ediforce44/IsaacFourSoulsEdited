# Modding API 
## (For everybody who want to add Boosterpacks or Scripts)
In this chapter are the most important variables and functions for modders listed. Those attributes are grouped by the script in which they are implemented. \
The keys of Tables are normally written in caps. If a Table refers to something player specific the keys of the Table are the player colors `Red`, `Blue`, `Green` and `Yellow`. \
All functions with parameters and a return value will return `nil` if the parameters are missing. \
We will talk about INFO-MSGs. This are Messages which will be printed for one or more player. The message will tell them what happened and what happens.

## <b>Global</b>
> ### <u><b>Tables</b></u>
> - `CLICK_DELAY` containes the Value, which is used to determine a Double Click on something.\
> (Current value: 0.3 sec)
> 
> Colors (often used for prints and broadcasts):
> - `REAL_PLAYER_COLOR` containes HTML color codes with all four player colors. The keys are the ingame player colors **Red**, **Blue**, **Green** and **Yellow**.
> - `REAL_PLAYER_COLOR_RGB` containes RGB percentage values for the **REAL_PLAYER_COLOR** entries. It has the same keys as **REAL_PLAYER_COLOR**.
> - `PRINT_COLOR_PLAYER` simular to **REAL_PLAYER_COLOR**, but the keys are in caps.
> - `PRINT_COLOR_SPECIAL` containes many types of HTML color codes for all types of game components. The keys are also written in caps.
>
> GUIDs of Zones:
> - `ZONE_GUID_DECK` containes the GUIDs of all deck zones in the game.
> - `ZONE_GUID_SHOP` containes the GUIDs of the six Shop Zones. The keys are **ONE** to **SIX**.
> - `ZONE_GUID_MONSTER` containes the GUIDs of the six Monster Zones. The keys are **ONE** to **SIX**.
> - `ZONE_GUID_PLAYER` containes the GUIDs of the Player Zones in front of each player. The keys are the ingame player colors **Red**, **Blue**, **Green** and **Yellow**
> - `ZONE_GUID_SOUL` containes the GUIDs of the Soul Zones in front of each player next to their Player Zone. The keys are the ingame player colors.
>
> GUIDs of game components:
> - `HEART_TOKENS_GUID` containes Tables of GUIDs corresponding to the Heart Tokens in front of each player. The GUIDs of the Heart Tokens are in the correct order. The keys are the player colors.
> - `FIRST_HEARTS_GUID`. The keys are the player colors.
> - `COIN_COUNTER_GUID`. The keys are the player colors.
> - `MONSTER_HP_COUNTER_GUID` containes the GUIDs of the HP Counters for the six Monster Zones. The keys are **ONE** to **SIX**.
> - `COUNTER_BAGS_GUID` containes the GUIDs of the ingame bags of counters. The keys are in caps.
>
> Positions:
> - `DISCARD_PILE_POSITION` containes the positions of the discard piles for each deck. The keys are in caps.
>
> Other Tables:
> - `automaticRewarding` containes the *true* or *false* if the automatic rewarding is activated for a player. The keys are the player colors.

> ### <u><b>Variables</b></u>
> - `activePlayerColor` containes the player color *string* of the player who is the active player.

> ### <u><b>Getter Functions</b></u>
> - `getDeckFromZone(zoneGUID) : object` returns the first deck or card in the zone with the GUID **zoneGUID**. If there is no deck or card, it will return *nil*.
> > - `getMonsterDeck() : object` returns the first deck or card in the Monster Zone. If there is no deck or card it will return *nil*.
> - `getHappenDeck() : object` ...
> - `getTreasureDeck() : object` ...
> - `getLootDeck() : object` ...
> - `getBonusSoulDeck() : object` ...
> - `getFloorDeck() : object` ...
> - `getSoulTokenDeck() : object` ...
> - `getActivePlayerString() : string` returns the color of the active player as a *string* tinted in the same color.
> - `getActivePlayerZone() : zone` returns the Player Zone of the active player.
 
> ### <u><b>Other Functions</b></u>
> - `getCardFromDeck({deck}) : object` takes a card from the **deck** and return a reference on this card. If **deck** is a card it will return **deck**.
> - `getPlayerString({playerColor}) : string` returns a the **playerColor** as a *string* tinted in the same color.
> - `getCounterInZone({zone}) : object` returns the first object in **zone** with the name "Counter".
> - `isPlayerAuthorized({playerColor or player, ownerPlayer}) : bool` returns *true* if the the player **player** or player with the **playerColor** equals **ownerPlayer**. It will return also *true* if **player** or the player with the **playerColor** is an admin.
> - `findIntInScript({scriptString, varName}) : int` searches in the **scriptString** for a variable with the name **varName** and returns the value of it.
> - `switchRewardingMode({playerColor or player})` switches the Rewarding Mode for **player** or the player with the **playerColor**.
> - `colorPicker_attach({afterPickFunction, functionOwner, picker, reason, functionParams}) : bool` attaches **afterPickFunction** to the **Color-Picker System**. The **Color-Picker** is explained in chapter [Color-Picker](#color-picker). \
> Only **afterPickFunction** and **functionOwner** have to be set. If picker is unset, the active player will be the picker. \
> Returns *true* if **afterPickFunction** could be attached successfully.
> - `placeSoulToken({playerColor})` takes a Soul Token from the soul token deck and places it in the Soul Zone of the player with the color **playerColor**. Prints a INFO-MSG. If the soul zone is full the Soul Token will be dealed to the Hand Zone of the player.
> - `placeObjectInSoulZone({playerColor, object})` places **object** in the Soul Zone of the player with color **playerColor**. If the Soul Zone is full the **object** will be dealed to the Hand Zone of the player.
> - `placeObjectInPillZone({playerColor, object})` places **object** in the Pill Zone of the player with color **playerColor**. If the Pill Zone is full the **object** will be dealed to the Hand Zone of the player.
> - `placeObjectsInPlayerZone({playerColor, object})` places **objects** in the Player Zone of the player with color **playerColor**. If the Player Zone is full the **objects** will be dealed to the Hand Zone of the player.

## <b>Coins</b>
> ### <u><b>Variables</b></u>
> - `value` is the current amount of coins. \
> (Minimum: 0, Maximum: 999)

> ### <u><b>Functions</b></u>
> - `modifyCoins({modifier})` adds or subtracts the **modifier** based on its sign to the current amount of coins.

## <b>HP (Monster)</b>
> ### <u><b>Variables</b></u>
> - `value` is the current HP of the active monster in the corresponding zone. \
> (Minimum: 0, Maximum: 9)
 
> ### <u><b>Functions</b></u>
> - `updateHP({HP})` sets the *value* of this HP-Counter to **HP**. **HP** has to be positiv and lower than 10.
> - `reset()` sets the *value* of this HP-Counter to the HP-Value of the active monster in the corresponding zone, if it isn't 0. \
> (This function uses the *active_monster_attrs* from the Monster Zone)

## <b>Monster Zone</b>
> ### <u><b>Tables</b></u>
> - `active_monster_attrs` containes the attributes of the active monster in this zone. The keys are **GUID**, **NAME**, **HP**, **ATK** and **DMG**.
> - `active_monster_reward` containes the amount of rewards of the active monster in this zone. The keys are **CENTS**, **LOOT**, **TREASURES** and **SOULS**.
 
> ### <u><b>Variables</b></u>
> - `active` is *true*, if this zone is an active monster zone.
 
> ### <u><b>Getter Functions</b></u>
> - `getAttackButton() : button` returns a reference of the monster button of this zone. If the monster button is deactivated if will return *nil*.
> - `getState() : string` returns the state of this zone. The state of a zone is the same as the state of its monster button. And the state of a monster button is its *label*. \
> If the monster button of this zone is deactivated it will return *nil*. \
> All states of a monster zone and its monster button are listet in the *Table* **ATTACK_BUTTON_STATES** in the Monster-Deck Zone script.
 
> ### <u><b>Technical Functions</b></u>
> - `containsDeckOrCard() : bool` returns *true* if this zone containes a *deck* or *card*.
> - `resetMonsterZone()` sets the state/*label* of the monster button of this zone to **ATTACK** if this zone is *active*, otherwise to **INACTIVE**. If the monster button of this zone is deactivated the button will be activated. \
> The HP-Counter of this zone will also be reseted.
> - `deactivateAttackButton()` let the monster button of this zone disappear. (This function removes the button)
> - `activateAttackButton()` let the monster button reappear in the state **ATTACK** if the zone is active and **INACTIVE** otherwise. (This function creates the button)
> - `deactivateZone()` discards all *decks* or *cards* in this zone and sets the monster attributes of this zone to zero. It activates the monster button of this zone and sets it to the state **INACTIVE**.
> - `activateZone()` places a new monster card in this zone if there is no *deck* or *card*. It activates the monster button of this zone and sets it to the state **ATTACK**.
> - `changeButtonState({newState}) : bool` changes the state of the monster button of this zone to **newState** and return *true*. If the monster button of this zone is deactivated or the **newState** is no valid state, it will return *false*. 
 
> ### <u><b>Monster Functions</b></u>
> - `monsterDied()` sets the HP-Counter of this zone to 0 and the state of this zone to **DIED** and prints an *INFO-MSG*. Only works if the monster button of this zone is activated.
> - `monsterReanimated()` prints an *INFO-MSG* and sets the state of this zone to the last state of this zone before **monsterDied()** was called. (So you have to use it in combination with **monsterDied()**)
> - `finishMonster()` pays out the reward of the active monster to the active player if Auto-Rewarding for the active player is activated. /
>   - It takes the first *card* in this zone or the topmost *card* of the first *deck* in this zone and discard the card if the reward **SOULS** is 0. Otherwise the card will be placed in the soul zone of the active player.
>   - The `onDie()` function of the card will be executed and if it returns *true* or *nothing*, the card will be taged as **DEAD** and a new monster card will be placed in this zone it is empty.
> - `updateAttributes({HP, GUID, NAME, ATK, DMG})` updates the Table **active_monster_attrs** to the given values. All parameters are optional except **HP**. **HP** has to be set otherwise this function does nothing. \
> The HP-Counter for this zone will also be updated. \
> If this function is called with some unset parameters, the attributes will be set their standard value. \
> (Strandard: GUID = last GUID, NAME = "", ATK = -1, DMG = -1)
> - `updateRewards()` updates the Table **active_monster_reward** to the given values. \
> If this function is called with some unset parameters, the attributes will be set to 0.

## <b>Monster-Deck Zone</b>
> ### <u><b>Tables</b></u>
> - `CHOOSE_BUTTON_STATES` contains all valid states for the monster button of this Monster-Deck Zone. The keys are **ATTACK** and **CHOOSE**.
> - `ATTACK_BUTTON_STATES` contains all valid states for the monster buttons of the Monster Zones and the Monster Zone itself. The keys are **INACTIVE**, **ATTACK**, **ATTACKING**, **CHOOSE**, **DIED** and **EVENT**.
> - `MONSTER_TAGS` contains all tags which are used to tag a monster card. The keys are **NEW** and **DEAD**. \
> The tag **NEW** will be removed from a object if it enters a monster zone. The tag **DEAD** will be removed from a object if it enters the discard pile for monster cards.

> ### <u><b>Techincal Functions</b></u>
> - `containsDeckOrCard({zone}) : bool` returns *true* if the monster zone **zone** contains a *deck* or a *card*.
> - `deactivateChooseButton()` let the monster button of this zone disappear. (This function removes the button)
> - `activateChooseButton()` let the monster button reappear in the state **ATTACK**. (This function creates the button)
> - `resetMonsterZone()` sets the state/*label* of the monster button of this zone to **ATTACK**. If the monster button of this zone is deactivated the button will be activated.
> - `resetAllMonsterZones()` resets the Monster-Deck Zone and all other Monster-Zones. (Have a look at the **resetMonsterZone()** functions)
> - `changeZoneState({zone, newState})` changes the state of the Monster Zone **zone** to **newState**. The **newState** has to a state from **ATTACK_BUTTON_STATES**. What will happen on a state change depends on the **newState**. In some cases, all other monster buttons will be activated etc.

> ### <u><b>Monster Functions</b></u>
> - `discardActiveMonsterCard({zone})` is simular to **finishMonster()** from Monster Zone. 
>   - It takes the topmost *card* of the first *deck* in the **zone** or the first *card* in the **zone** and discards it (no matter if it has a **SOULS** attribute).
>   - The **onDie()** functions of the taken monster card will be called and if it returns *true* a new monster card will be placed in the **zone** if the **zone** is now empty. \
> If **onDie()** returns *false* the tag **DEAD** is removed from the taken monster card.
> - `discardMonsterObject({object})` places the **object** on to the discard pile for monster cards and adds the tag **DEAD** to the object.
> - `placeNewMonsterCard({zone, isTargetOfAttack}) : bool` takes the topmost *card* from the monster deck, flips it and places it in the Monster Zone **zone**.
>   - If the new card has a variable `isEvent` and if it is *true*, an INFO-MSG will be printed. \
>  If the new card also has an variable called `type` and if it equals the *EVENT_TYPE* **CURSE**, **dealEventToPlayer()** will be attached to the Color-Picker.
>   - If the new card is not a event and **isTargetOfAttack** is *true*, another INFO-MSG will be printed.
>   - This function will only return *false* if it couldn't find a new monster card to place it in the **zone**.

## <b>Shop Zone</b>
> ### <u><b>Variables</b></u>
> - `active` is *true*, if this zone is an active shop zone

> ### <u><b>Functions</b></u>
> - `containsDeckOrCard() : bool` returns *true*, if this zone containes a *deck* or *card*.
> - `deactivateZone()` discards all *decks* or *cards* in this zone. It activates the purchase button of this zone and sets it to the state **INACTIVE**.
> - `activateZone()` places a new treasure card in this zone if there is no *deck* or *card*. It activates the purchase button of this zone and sets it to the state **PURCHASE**.
> - `deactivatePurchaseButton()` let the purchase button of this zone disappear. (This function removes the button)
> - `activatePurchaseButton()` let the purchase button reappear in the state **PURCHASE** if the zone is active and **INACTIVE** otherwise. (This function creates the button)

## <b>Shop-Deck Zone</b>
> ### <u><b>Tables</b></u>
> - `PURCHASE_BUTTON_STATES` contains all valid states for the purchase button of the shop zones. The keys are **PURCHASE** and **INACTIVE**.
> - `SHOP_BUTTON_STATES` contains all valid states for the shop button of this Shop-Deck Zone. The keys are **PURCHASE** and nothing else.

> ### <u><b>Functions</b></u>
> - `deactivateShopButton()` let the shop button of this zone disappear. (This function removes the button)
> - `activateShopButton()` let the shop button reappear in the state **PURCHASE**.
> - `changeZoneState({zone, newState})` changes the state of the Shop Zone **zone** to **newState**. The **newState** has to be a state from **ATTACK_BUTTON_STATES**. What will happen on a state change depends on the **newState**.
> - `placeNewTreasureCard({zone}) : bool` takes the first *card* or the topmost *card* of the first *deck* in the treasure zone. It filps this card and put it into the shop zone **zone**. It returs *false* if no card was found in the treasure zone.

## <b>Player Zone</b>
> ### <u><b>Tables</b></u>
> - `attachedObjects` contains all objects that are currently in this Player Zone. The keys are the *GUIDs* of the objects. The value for a *GUID* is 1 if the corresponding object is in this Player Zone and *nil* otherwise.
 
> ### <u><b>Variables</b></u>
> - `INDEX_MAX` is the maximum index in this Player Zone. For every Player Zone it is 14. Every index belongs to a fixed position in this zone. Index 1 to 7 belongs to the lower row and 8 to 14 belongs to the upper row.

> ### <u><b>Functions</b></u>
> - `placeObjectInZone({object})` places **object** in this Player Zone. If this Player Zone is full the **object** will be dealed to the Hand Zone of the player.
> - `placeMultipleObjectsInZone({objects})` places the **objects** in this Player Zone. If this Player Zone is full the **objects** will be dealed to the Hand Zone of the player. It is more efficient than calling **placeObjectInZone()** multiple times.

## <b>Pill Zone</b>
> ### <u><b>Tables</b></u>
> - `attachedObjects` contains all objects that are currently in this Pill Zone. The keys are the *GUIDs* of the objects. The value for a *GUID* is 1 if the corresponding object is in this Pill Zone and *nil* otherwise.

> ### <u><b>Variables</b></u>
> - `INDEX_MAX` is the maximum index in this Pill Zone. For every Pill Zone it is 2. Every index belongs to a fixed position in this zone.

> ### <u><b>Functions</b></u>
> - `placeObjectInZone({object})` places **object** in this Pill Zone. If this Pill Zone is full the **object** will be dealed to the Hand Zone of the player.

## <b>Soul Zone</b>
> ### <u><b>Tables</b></u>
> - `attachedObjects` contains all objects that are currently in this Soul Zone. The keys are the *GUIDs* of the objects. The value for a *GUID* is 1 if the corresponding object is in this Soul Zone and *nil* otherwise.

> ### <u><b>Variables</b></u>
> - `INDEX_MAX` is the maximum index in this Soul Zone. For every Soul Zone it is 4. Every index belongs to a fixed position in this zone.

> ### <u><b>Functions</b></u>
> - `placeObjectInZone({object})` places **object** in this Soul Zone. If this Soul Zone is full the **object** will be dealed to the Hand Zone of the player.

## <b>Souls</b>
> ### <u><b>Variables</b></u>
> - `val` is the current amount of souls in this Soul Zone. \
> (Minimum: 0, Maximum: 4)