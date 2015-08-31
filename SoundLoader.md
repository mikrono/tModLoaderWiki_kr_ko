# SoundLoader

This class is used to keep track of and support the existence of custom sounds that have been added to the game.

## Fields

### public const int customSoundType

This value should be passed as the first parameter to Main.PlaySound whenever you want to play a custom sound that is not an item, npcHit, or npcKilled sound.

## Methods

### public static int GetSoundSlot(SoundType type, string sound)

Returns the style (last parameter passed to Main.PlaySound) of the sound corresponding to the given SoundType and the given sound file path. Returns 0 if there is no corresponding style.