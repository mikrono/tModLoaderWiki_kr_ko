Under Construction

Dust  
public static int NewDust(Vector2 Position, int Width, int Height, int Type, float SpeedX = 0f, float SpeedY = 0f, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)

Player  
public void QuickSpawnItem(int item, int stack = 1)

Item  
public static int NewItem(int X, int Y, int Width, int Height, int Type, int Stack = 1, bool noBroadcast = false, int pfix = 0, bool noGrabDelay = false, bool reverseLookup = false)
		
NPC   
public void AddBuff(int type, int time, bool quiet = false)
public int HasBuff(int type)

Main  
public static void PlaySound(int type, int x = -1, int y = -1, int Style = 1)
public static void NewText(string newText, byte R = 255, byte G = 255, byte B = 255, bool force = false)

Projectile  
public static int NewProjectile(float X, float Y, float SpeedX, float SpeedY, int Type, int Damage, float KnockBack, int Owner = 255, float ai0 = 0f, float ai1 = 0f)