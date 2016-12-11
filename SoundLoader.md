# SoundLoader

This class is used to keep track of and support the existence of custom sounds that have been added to the game.

## Fields

### public const int customSoundType

This value should be passed as the first parameter to Main.PlaySound whenever you want to play a custom sound that is not an item, npcHit, or npcKilled sound.

## Methods

### public static int GetSoundSlot(SoundType type, string sound)

Returns the style (last parameter passed to Main.PlaySound) of the sound corresponding to the given SoundType and the given sound file path. Returns 0 if there is no corresponding style.

### public static LegacySoundStyle GetLegacySoundSlot(SoundType type, string sound)

Returns a LegacySoundStyle object which encapsulates both a sound type and a sound style (This is the new way to do sounds in 1.3.4) Returns null if there is no corresponding style.