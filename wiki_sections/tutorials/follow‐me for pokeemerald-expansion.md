# follow‐me for pokeemerald-expansion

> [!WARNING]
> **There is currently a game breaking bug associated with trying to bike or surf with a follower present.**  
> **Biking and surfing have been temporarily disabled while a follow-me follower is active.**  
> **All other aspects of this branch are working properly.**
>
> **Update:** expansion version 1.10 appears to have fixed the main issue with this bug. I still need to do some tweaking to make biking/surfing/diving work properly, but it shouldn't take me too long. Until then, those functionalities will remain isolated from regular use.

I have updated [ghoulslash’s follow-me feature branch](https://github.com/ghoulslash/pokeemerald/tree/follow_me) to be compatible with versions 1.9 and 1.10 of pokeemerald-expansion. Most of ghoulslash’s original code remains intact, so most of the credit still goes to them. I have, however, included some of my own quality of life features with this updated version.

To merge the branch into your project, enter these two commands into the console:  
`git remote add Bivurnum https://github.com/Bivurnum/pokeemerald`  
`git pull Bivurnum follow-me-expansion`

Or if you want to apply the changes manually, you can find the comparison here:  
[https://github.com/rh-hideout/pokeemerald-expansion/compare/master...Bivurnum:pokeemerald:follow-me-expansion](https://github.com/rh-hideout/pokeemerald-expansion/compare/master...Bivurnum:pokeemerald:follow-me-expansion)

<details>
    <summary><i>Note for merging into expansion 1.10:</i></summary>

>   Any merge conflicts should be able to be resolved by accepting both changes (current first).  
>   Any files that have been deleted in 1.10 can remain deleted.
</details>

## How to Use
### Set the Follower
The `setfollower` macro will turn the specified object into a follower. It requires the object id, the [follower flags](#follower-flags), and optionally a custom script and a [battle partner](#battle-partner). If you do not include a custom script name (or you set it to `0`), the follower will default to their normal interaction script. If there is a follower Pokémon present, it will be returned to its Pokeball until the follow-me follower is destroyed.

Here's an example:  
`setfollower 3, FOLLOWER_ALL, MyScript_Eventscript_CustomFollowerScript, PARTNER_STEVEN`  
This would turn object number 3 on the current map into a follower, give them access to all following behaviors, run that custom script when the player interacts with them, and adds the Steven battle partner to the follower ([more on this later](#battle-partner)).

### Follower Flags
These are required to tell the game what behavior you want the follower to have. They are defined in [include/constants/follow_me.h](https://github.com/Bivurnum/pokeemerald/blob/follow-me-expansion/include/constants/follow_me.h). The second list of flags is the same as the first, but with shortened names to make them easier to type when scripting. The first 7 flags in the list are individual behaviors, whereas the remaining three are bundles of flags. For example, if you use `CAN_SURF` in `setfollower`, the follower will be able to Surf behind the player. If you use `ALL_WATER` instead, the follower will be able to Dive and go up Waterfalls in addition to being able to Surf. Feel free to add your own custom bundles of flags to the file to meet your needs.

Keep in mind that certain flags should only be used if the object has the unique animation frames associated with that flag. For example, if you use `RUNNING_FRAMES`, but the object does not have frames specifically for a running animation, their movements will be very buggy whenever the player runs. However, if you do not use the `RUNNING_FRAMES` flag, that same object will simply use their regular walking animation frames, just sped up.

To make sure the follower uses the correct animation frames, you should add an entry to `gFollowerAlternateSprites` in [src/follow_me.c](https://github.com/Bivurnum/pokeemerald/blob/follow-me-expansion/src/follow_me.c#L72). Only do this if your object has distinct animation frames for different behaviors (running, biking, surfing, etc). Follow the template for May or Brendan that already exists there.

### Follower Movements
You can use the `applymovement` macro on a follower by using `OBJ_EVENT_ID_FOLLOW_ME` for the object id. This is convenient for making the follower walk off-screen before destroying them. If a follower is not immediately next to the player while the controls aren't locked, the follower will try to take steps to get back into position as the player moves, sometimes walking through impassable areas (like through the middle of buildings).

You can also use `facefollower` to make both the player and the follower face each other.

### Check for Follower
You can use the `checkfollower` macro to check whether or not a follow-me follower currently exists. It will set `VAR_RESULT` to `TRUE` if a follower exists, otherwise it will be set to `FALSE`.

### Destroy the Follower
The `destroyfollower` macro acts similarly to `removeobject`. It removes the follower object instantly and sets their flag (if they have one). If you have Pokémon followers enabled, the Pokémon will reappear the next time the player takes a step.

### Doorway Facing
By default, the player will turn to face the follower whenever they exit a doorway. In [include/constants/follow_me.h](https://github.com/Bivurnum/pokeemerald/blob/follow-me-expansion/include/constants/follow_me.h), you can toggle `FACE_FOLLOWER_ON_DOOR_EXIT` to FALSE if you want to disable that behavior.

## Battle Partner
If you assign a battle partner to the follower, that partner will fight alongside the player in all trainer battles while the follower is present. This turns all battles into multi battles, whether against one opponent or two.

You can use any battle partner that has been defined in [include/constants/battle_partner.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/constants/battle_partner.h). The partners' information and Pokémon teams can be set in [src/data/battle_partners.party](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/src/data/battle_partners.party).

Keep in mind that only the first 3 Pokémon in the player's party will participate in multi battles. The other three party members are temporarily replaced with the partner's Pokémon. This means the last three Pokémon in the player's party will not receive any experience points (even from the EXP Share).

There are a few battle partner configs that you can toggle in [include/constants/follow_me.h](https://github.com/Bivurnum/pokeemerald/blob/follow-me-expansion/include/constants/follow_me.h):
* `F_FLAG_HEAL_AFTER_FOLLOWER_BATTLE`: The player's party can be automatically healed after every partner battle. Set it to a flag to toggle it on/off with scripts, or set it to `ALWAYS` to have it happen every time.
* `F_FLAG_PARTNER_WILD_BATTLES`: The battle partner can join the player for wild battles. Set it to a flag to toggle it on/off with scripts, or set it to `ALWAYS` to have it happen every time.
* `FOLLOWER_WILD_BATTLE_VS_2`: Wild battles with a battle partner default to two wild Pokémon appearing. You can set this to `FALSE` to make only one wild Pokémon appear.
* `FOLLOWER_PARTY_PREVIEW`: By default, a preview of the player's and partner's teams will be shown at the start of every trainer battle. Set this to `FALSE` to disable this feature. 
