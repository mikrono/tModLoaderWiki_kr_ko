## The Basis ##
When making a banner, different values are used for enemies. Say for example you wanted to create a banner that gave you buffs for blue and green slimes. If you used their actual IDs, you would instead be buffed for completely different enemies. That's because the ID value is passed through to this function, which returns a different value based on which ID you use.

## The List ##  
This is the list of conversions made found in Terraria.Item.BannerToNPC. Below this is a second list, containing the code from Terraria.Item.NPCToBanner. It may prove useful.

    // Terraria.Item
    public static int BannerToNPC(int i)
    {
    switch (i)
    {
    case 1:
            return 102;
    case 2:
            return 250;
    case 3:
            return 257;
    case 4:
            return 69;
    case 5:
            return 157;
    case 6:
            return 77;
    case 7:
            return 49;
    case 8:
            return 74;
    case 9:
            return 163;
    case 10:
            return 241;
    case 11:
            return 242;
    case 12:
            return 239;
    case 13:
            return 39;
    case 14:
            return 46;
    case 15:
            return 120;
    case 16:
            return 85;
    case 17:
            return 109;
    case 18:
            return 47;
    case 19:
            return 57;
    case 20:
            return 67;
    case 21:
            return 173;
    case 22:
            return 179;
    case 23:
            return 83;
    case 24:
            return 62;
    case 25:
            return 2;
    case 26:
            return 177;
    case 27:
            return 6;
    case 28:
            return 84;
    case 29:
            return 161;
    case 30:
            return 181;
    case 31:
            return 182;
    case 32:
            return 224;
    case 33:
            return 226;
    case 34:
            return 162;
    case 35:
            return 259;
    case 36:
            return 256;
    case 37:
            return 122;
    case 38:
            return 27;
    case 39:
            return 29;
    case 40:
            return 26;
    case 41:
            return 73;
    case 42:
            return 28;
    case 43:
            return 55;
    case 44:
            return 48;
    case 45:
            return 60;
    case 46:
            return 174;
    case 47:
            return 42;
    case 48:
            return 169;
    case 49:
            return 206;
    case 50:
            return 24;
    case 51:
            return 63;
    case 52:
            return 236;
    case 53:
            return 199;
    case 54:
            return 43;
    case 55:
            return 23;
    case 56:
            return 205;
    case 57:
            return 78;
    case 58:
            return 258;
    case 59:
            return 252;
    case 60:
            return 170;
    case 61:
            return 58;
    case 62:
            return 212;
    case 63:
            return 75;
    case 64:
            return 223;
    case 65:
            return 253;
    case 66:
            return 65;
    case 67:
            return 21;
    case 68:
            return 32;
    case 69:
            return 1;
    case 70:
            return 185;
    case 71:
            return 164;
    case 72:
            return 254;
    case 73:
            return 166;
    case 74:
            return 153;
    case 75:
            return 141;
    case 76:
            return 225;
    case 77:
            return 86;
    case 78:
            return 158;
    case 79:
            return 61;
    case 80:
            return 196;
    case 81:
            return 104;
    case 82:
            return 155;
    case 83:
            return 98;
    case 84:
            return 10;
    case 85:
            return 82;
    case 86:
            return 87;
    case 87:
            return 3;
    case 88:
            return 175;
    case 89:
            return 197;
    case 90:
            return -6;
    case 91:
            return 273;
    case 92:
            return 379;
    case 95:
            return 287;
    case 96:
            return 101;
    case 97:
            return 217;
    case 98:
            return 168;
    case 99:
            return 81;
    case 100:
            return 94;
    case 101:
            return 183;
    case 102:
            return 34;
    case 103:
            return 218;
    case 104:
            return 7;
    case 105:
            return 285;
    case 106:
            return 52;
    case 107:
            return 71;
    case 108:
            return 288;
    case 109:
            return 350;
    case 110:
            return 347;
    case 111:
            return 251;
    case 112:
            return 352;
    case 113:
            return 316;
    case 114:
            return 93;
    case 115:
            return 289;
    case 116:
            return 152;
    case 117:
            return 342;
    case 118:
            return 111;
    case 119:
            return -3;
    case 120:
            return 315;
    case 121:
            return 277;
    case 122:
            return 329;
    case 123:
            return 304;
    case 124:
            return 150;
    case 125:
            return 243;
    case 126:
            return 147;
    case 127:
            return 268;
    case 128:
            return 137;
    case 129:
            return 138;
    case 130:
            return 51;
    case 131:
            return -10;
    case 132:
            return 351;
    case 133:
            return 219;
    case 134:
            return 151;
    case 135:
            return 59;
    case 136:
            return 381;
    case 137:
            return 388;
    case 138:
            return 386;
    case 139:
            return 389;
    case 140:
            return 385;
    case 141:
            return 383;
    case 142:
            return 382;
    case 143:
            return 390;
    case 144:
            return 387;
    case 145:
            return 144;
    case 146:
            return 16;
    case 147:
            return 283;
    case 148:
            return 348;
    case 149:
            return 290;
    case 150:
            return 148;
    case 151:
            return -4;
    case 152:
            return 330;
    case 153:
            return 140;
    case 154:
            return 341;
    case 155:
            return -7;
    case 156:
            return 281;
    case 157:
            return 244;
    case 158:
            return 301;
    case 159:
            return -8;
    case 160:
            return 172;
    case 161:
            return 269;
    case 162:
            return 305;
    case 163:
            return 391;
    case 164:
            return 110;
    case 165:
            return 293;
    case 166:
            return 291;
    case 167:
            return 121;
    case 168:
            return 56;
    case 169:
            return 145;
    case 170:
            return 143;
    case 171:
            return 184;
    case 172:
            return 204;
    case 173:
            return 326;
    case 174:
            return 221;
    case 175:
            return 292;
    case 176:
            return 53;
    case 177:
            return 45;
    case 178:
            return 44;
    case 179:
            return 167;
    case 180:
            return 380;
    case 183:
            return -9;
    case 184:
            return 343;
    case 185:
            return 338;
    case 186:
            return 471;
    case 187:
            return 498;
    case 188:
            return 496;
    case 189:
            return 494;
    case 190:
            return 462;
    case 191:
            return 461;
    case 192:
            return 468;
    case 193:
            return 477;
    case 195:
            return 469;
    case 196:
            return 460;
    case 197:
            return 466;
    case 198:
            return 467;
    case 199:
            return 463;
    case 201:
            return 480;
    case 202:
            return 481;
    case 203:
            return 483;
    case 204:
            return 482;
    case 205:
            return 489;
    case 206:
            return 490;
    case 207:
            return 513;
    case 208:
            return 510;
    case 209:
            return 509;
    case 210:
            return 508;
    case 211:
            return 524;
    case 212:
            return 529;
    case 213:
            return 533;
    case 214:
            return 532;
    case 215:
            return 530;
    case 216:
            return 411;
    case 217:
            return 402;
    case 218:
            return 407;
    case 219:
            return 409;
    case 221:
            return 405;
    case 222:
            return 418;
    case 223:
            return 417;
    case 224:
            return 412;
    case 225:
            return 416;
    case 226:
            return 415;
    case 227:
            return 419;
    case 228:
            return 424;
    case 229:
            return 421;
    case 230:
            return 420;
    case 231:
            return 423;
    case 232:
            return 428;
    case 233:
            return 426;
    case 234:
            return 427;
    case 235:
            return 429;
    case 236:
            return 425;
    case 237:
            return 216;
    case 238:
            return 214;
    case 239:
            return 213;
    case 240:
            return 215;
    case 241:
            return 520;
    case 242:
            return 156;
    case 243:
            return 64;
    case 244:
            return 103;
    case 245:
            return 79;
    case 246:
            return 80;
    case 247:
            return 31;
    case 248:
            return 154;
    case 249:
            return 537;
    case 250:
            return 220;
    case 251:
            return 541;
    case 252:
            return 542;
    case 253:
            return 543;
    case 254:
            return 544;
    case 255:
            return 545;
    case 256:
            return 546;
    case 257:
            return 555;
    case 258:
            return 552;
    case 259:
            return 566;
    case 260:
            return 570;
    case 261:
            return 574;
    case 262:
            return 572;
    case 263:
            return 568;
    case 264:
            return 558;
    case 265:
            return 561;
    case 266:
            return 578;
    }
    if (i >= 580)
    {
            return i;
    }
    return 0;
    }

### The Second Coming: ###

    // Terraria.Item
    public static int NPCtoBanner(int i)
    {
    switch (i)
    {
    case -10:
            return 131;
    case -9:
            return 183;
    case -8:
            return 159;
    case -7:
            return 155;
    case -6:
            return 90;
    case -4:
            return 151;
    case -3:
            return 119;
    case -2:
    case 121:
            return 167;
    case 1:
    case 302:
    case 333:
    case 334:
    case 335:
    case 336:
            return 69;
    case 2:
    case 133:
    case 190:
    case 191:
    case 192:
    case 193:
    case 194:
    case 317:
    case 318:
            return 25;
    case 3:
    case 132:
    case 186:
    case 187:
    case 188:
    case 189:
    case 200:
    case 319:
    case 320:
    case 321:
    case 331:
    case 332:
    case 430:
    case 432:
    case 433:
    case 434:
    case 435:
    case 436:
            return 87;
    case 6:
            return 27;
    case 7:
            return 104;
    case 10:
    case 11:
    case 12:
    case 95:
    case 96:
    case 97:
            return 84;
    case 16:
            return 146;
    case 21:
    case 201:
    case 202:
    case 203:
    case 449:
    case 450:
    case 451:
    case 452:
            return 67;
    case 23:
            return 55;
    case 24:
            return 50;
    case 26:
            return 40;
    case 27:
            return 38;
    case 28:
            return 42;
    case 29:
            return 39;
    case 31:
    case 294:
    case 295:
    case 296:
            return 247;
    case 32:
            return 68;
    case 34:
            return 102;
    case 39:
    case 40:
    case 41:
            return 13;
    case 42:
    case 176:
    case 231:
    case 232:
    case 233:
    case 234:
    case 235:
            return 47;
    case 43:
            return 54;
    case 44:
            return 178;
    case 45:
            return 177;
    case 46:
    case 303:
    case 337:
    case 540:
            return 14;
    case 47:
            return 18;
    case 48:
            return 44;
    case 49:
            return 7;
    case 51:
            return 130;
    case 52:
            return 106;
    case 53:
            return 176;
    case 55:
    case 230:
            return 43;
    case 56:
            return 168;
    case 57:
            return 19;
    case 58:
            return 61;
    case 59:
            return 135;
    case 60:
            return 45;
    case 61:
            return 79;
    case 62:
    case 66:
            return 24;
    case 63:
            return 51;
    case 64:
            return 243;
    case 65:
            return 66;
    case 67:
            return 20;
    case 69:
            return 4;
    case 71:
            return 107;
    case 73:
            return 41;
    case 74:
            return 8;
    case 75:
            return 63;
    case 77:
            return 6;
    case 78:
            return 57;
    case 79:
            return 245;
    case 80:
            return 246;
    case 81:
            return 99;
    case 82:
            return 85;
    case 83:
            return 23;
    case 84:
            return 28;
    case 85:
            return 16;
    case 86:
            return 77;
    case 87:
    case 88:
    case 89:
    case 90:
    case 91:
    case 92:
            return 86;
    case 93:
            return 114;
    case 94:
            return 100;
    case 98:
    case 99:
    case 100:
            return 83;
    case 101:
            return 96;
    case 102:
            return 1;
    case 103:
            return 244;
    case 104:
            return 81;
    case 109:
            return 17;
    case 110:
            return 164;
    case 111:
            return 118;
    case 120:
            return 15;
    case 122:
            return 37;
    case 137:
            return 128;
    case 138:
            return 129;
    case 140:
            return 153;
    case 141:
            return 75;
    case 143:
            return 170;
    case 144:
            return 145;
    case 145:
            return 169;
    case 147:
            return 126;
    case 148:
    case 149:
            return 150;
    case 150:
            return 124;
    case 151:
            return 134;
    case 152:
            return 116;
    case 153:
            return 74;
    case 154:
            return 248;
    case 155:
            return 82;
    case 156:
            return 242;
    case 157:
            return 5;
    case 158:
    case 159:
            return 78;
    case 161:
    case 431:
            return 29;
    case 162:
            return 34;
    case 163:
    case 238:
            return 9;
    case 164:
    case 165:
            return 71;
    case 166:
            return 73;
    case 167:
            return 179;
    case 168:
            return 98;
    case 169:
            return 48;
    case 170:
    case 171:
    case 180:
            return 60;
    case 172:
            return 160;
    case 173:
            return 21;
    case 174:
            return 46;
    case 175:
            return 88;
    case 177:
            return 26;
    case 179:
            return 22;
    case 181:
            return 30;
    case 182:
            return 31;
    case 183:
            return 101;
    case 184:
            return 171;
    case 185:
            return 70;
    case 195:
    case 196:
            return 80;
    case 197:
            return 89;
    case 198:
    case 199:
            return 53;
    case 204:
            return 172;
    case 205:
            return 56;
    case 206:
            return 49;
    case 212:
            return 62;
    case 213:
            return 239;
    case 214:
            return 238;
    case 215:
            return 240;
    case 216:
            return 237;
    case 217:
            return 97;
    case 218:
            return 103;
    case 219:
            return 133;
    case 220:
            return 250;
    case 221:
            return 174;
    case 223:
            return 64;
    case 224:
            return 32;
    case 225:
            return 76;
    case 226:
            return 33;
    case 236:
    case 237:
            return 52;
    case 239:
    case 240:
            return 12;
    case 241:
            return 10;
    case 242:
            return 11;
    case 243:
            return 125;
    case 244:
            return 157;
    case 250:
            return 2;
    case 251:
            return 111;
    case 252:
            return 59;
    case 253:
            return 65;
    case 254:
    case 255:
            return 72;
    case 256:
            return 36;
    case 257:
            return 3;
    case 258:
            return 58;
    case 259:
    case 260:
            return 35;
    case 268:
            return 127;
    case 269:
    case 270:
    case 271:
    case 272:
            return 161;
    case 273:
    case 274:
    case 275:
    case 276:
            return 91;
    case 277:
    case 278:
    case 279:
    case 280:
            return 121;
    case 281:
    case 282:
            return 156;
    case 283:
    case 284:
            return 147;
    case 285:
    case 286:
            return 105;
    case 287:
            return 95;
    case 288:
            return 108;
    case 289:
            return 115;
    case 290:
            return 149;
    case 291:
            return 166;
    case 292:
            return 175;
    case 293:
            return 165;
    case 301:
            return 158;
    case 304:
            return 123;
    case 305:
    case 306:
    case 307:
    case 308:
    case 309:
    case 310:
    case 311:
    case 312:
    case 313:
    case 314:
            return 162;
    case 315:
            return 120;
    case 316:
            return 113;
    case 326:
            return 173;
    case 329:
            return 122;
    case 330:
            return 152;
    case 338:
    case 339:
    case 340:
            return 185;
    case 341:
            return 154;
    case 342:
            return 117;
    case 343:
            return 184;
    case 347:
            return 110;
    case 348:
    case 349:
            return 148;
    case 350:
            return 109;
    case 351:
            return 132;
    case 352:
            return 112;
    case 379:
            return 92;
    case 380:
            return 180;
    case 381:
            return 136;
    case 382:
            return 142;
    case 383:
    case 384:
            return 141;
    case 385:
            return 140;
    case 386:
            return 138;
    case 387:
            return 144;
    case 388:
            return 137;
    case 389:
            return 139;
    case 390:
            return 143;
    case 391:
            return 163;
    case 402:
    case 403:
    case 404:
            return 217;
    case 405:
    case 406:
            return 221;
    case 407:
    case 408:
            return 218;
    case 409:
            return 219;
    case 411:
            return 216;
    case 412:
    case 413:
    case 414:
            return 224;
    case 415:
            return 226;
    case 416:
            return 225;
    case 417:
            return 223;
    case 418:
            return 222;
    case 419:
            return 227;
    case 420:
            return 230;
    case 421:
            return 229;
    case 423:
            return 231;
    case 424:
            return 228;
    case 425:
            return 236;
    case 426:
            return 233;
    case 427:
            return 234;
    case 428:
            return 232;
    case 429:
            return 235;
    case 460:
            return 196;
    case 461:
            return 191;
    case 462:
            return 190;
    case 463:
            return 199;
    case 466:
            return 197;
    case 467:
            return 198;
    case 468:
            return 192;
    case 469:
            return 195;
    case 471:
            return 186;
    case 477:
            return 193;
    case 480:
            return 201;
    case 481:
            return 202;
    case 482:
            return 204;
    case 483:
            return 203;
    case 489:
            return 205;
    case 490:
            return 206;
    case 494:
    case 495:
            return 189;
    case 496:
    case 497:
            return 188;
    case 498:
    case 499:
    case 500:
    case 501:
    case 502:
    case 503:
    case 504:
    case 505:
    case 506:
            return 187;
    case 508:
            return 210;
    case 509:
            return 209;
    case 510:
    case 511:
    case 512:
            return 208;
    case 513:
    case 514:
    case 515:
            return 207;
    case 520:
            return 241;
    case 524:
    case 525:
    case 526:
    case 527:
            return 211;
    case 528:
    case 529:
            return 212;
    case 530:
    case 531:
            return 215;
    case 532:
            return 214;
    case 533:
            return 213;
    case 537:
            return 249;
    case 541:
            return 251;
    case 542:
            return 252;
    case 543:
            return 253;
    case 544:
            return 254;
    case 545:
            return 255;
    case 546:
            return 256;
    case 552:
    case 553:
    case 554:
            return 258;
    case 555:
    case 556:
    case 557:
            return 257;
    case 558:
    case 559:
    case 560:
            return 264;
    case 561:
    case 562:
    case 563:
            return 265;
    case 566:
    case 567:
            return 259;
    case 568:
    case 569:
            return 263;
    case 570:
    case 571:
            return 260;
    case 572:
    case 573:
            return 262;
    case 574:
    case 575:
            return 261;
    case 578:
            return 266;
    }
    if (i >= 580)
    {
            return NPCLoader.GetNPC(i).banner;
    }
    return 0;
    }