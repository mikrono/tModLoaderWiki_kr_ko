This wiki page serves to act as a reference for most of the technical aspects of the `Terraria.Player::Update(int)` method.  
If you want a full reference, decompile tModLoader via the `setup.bat` tool (see: [tModLoader guide for contributors](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-contributors)).

The update "tasks" are listed in logical order, using numbered lists to indicate relevance.

1. Ground/Air Speed Stat Resetting
    * `Player::maxFallSpeed` is set to `10`
    * `Player::gravity` is set to `Player::defaultGravity` (`0.4`)
    * `Player::jumpHeight` is set to `15`
    * `Player::jumpSpeed` is set to `5.01`
    * `Player::maxRunSpeed` and `Player::accRunSpeed` are set to `3`
    * `Player::runAcceleration` is set to `0.08`
    * `Player::runSlowdown` is set to `0.2`
1. `Player::onWrongGround` (the variable responsible for preventing minecart movement if not on a minecart track) is cleared if the player's mount is inactive or not a minecart
1. `Player::heldProj` is set to `-1`
1. `Player::instantMovementAccumulatedThisFrame` (a variable used for sideways pulley movement) is set to `Vector2.Zero`
1. If the player is holding the Portal Gun or has recently exited a Portal Gun portal (`Player::PortalPhysicsEnabled`), `Player::maxFallSpeed` is set to `35`
1. Wet Movement Checking
    1. If `Player::wet` is set...
        1. If `Player::honeyWet` is set:
            * `Player::gravity` is reduced to `0.1`
            * `Player::maxFallSpeed` is reduced to `3`
        1. Otherwise, if `Player::merman` is set:
            * `Player::gravity` is reduced to `0.3`
            * `Player::maxFallSpeed` is reduced to `7`
        1. Otherwise, if `Player::trident` (a variable set to `true` when the player is holding a Trident and not in a mount/minecart) is set:
            * `Player::gravity` is reduced to `0.25`
            * `Player::maxFallSpeed` is reduced to `6`
            * `Player::jumpHeight` is increased to `25`
            * `Player::jumpSpeed` is increased to `5.51`
            * If the player is holding the UP movement key (`Player::controlUp`):
                * `Player::gravity` is reduced to `0.1`
                * `Player::maFallSpeed` is reduced to `2`
        1. Otherwise:
            * `Player::gravity` is reduced to `0.2`
            * `Player::maxFallSpeed` is reduced to `5`
            * `Player::jumpHeight` is increased to `30`
            * `Player::jumpSpeed` is increased to `6.01`
1. If the player has the Distorted debuff:
    * `Player::gravity` is set to `0`
1. `Player::maxFallSpeed` is incremented by `0.01`

// TODO: finish docs