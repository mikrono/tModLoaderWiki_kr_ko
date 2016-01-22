Under Construction

Dust  
public static int NewDust(Vector2 Position, int Width, int Height, int Type, float SpeedX = 0f, float SpeedY = 0f, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)

Player  
public void QuickSpawnItem(int item, int stack = 1)

NPC   
public void AddBuff(int type, int time, bool quiet = false)
public int HasBuff(int type)

Main  
public static void PlaySound(int type, int x = -1, int y = -1, int Style = 1)

Projectile  
public static int NewProjectile(float X, float Y, float SpeedX, float SpeedY, int Type, int Damage, float KnockBack, int Owner = 255, float ai0 = 0f, float ai1 = 0f)