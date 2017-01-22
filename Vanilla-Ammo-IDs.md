The following is a list of constants found in the Terraria.ID.AmmoID class.

AmmoIDs are used to define ammo classes, groups of items that all work with the same weapons. AmmoIDs are based off of the ItemID of the first ammo item in that class. For example, the ItemID MusketBall has a value of 97 and the AmmoID for Bullet also has a value of 97. All other Bullets in the game set their item.ammo to AmmoID.Bullet, and all weapons that use Bullet ammo set their item.useAmmo to AmmoID.Bullet. For ModItems, please follow this numbering convention as well to avoid ID overlap.

public static int None = 0;    
public static int Gel = 23;    
public static int Arrow = 40;    
public static int Coin = 71;    
public static int FallenStar = 75;    
public static int Bullet = 97;    
public static int Sand = 169;    
public static int Dart = 283;    
public static int Rocket = 771;    
public static int Solution = 780;    
public static int Flare = 931;    
public static int Snowball = 949;    
public static int StyngerBolt = 1261;    
public static int CandyCorn = 1783;    
public static int JackOLantern = 1785;    
public static int Stake = 1836;    
public static int NailFriendly = 3108;    