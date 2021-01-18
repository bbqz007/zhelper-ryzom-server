server/data_shard/datasets.packed_sheets

```loadForm("datasets.packed_sheets")``` to map<CSheetId, TDataSetSheet>
```
00000000  48 53 4b 50 05 00 00 00  00 dd 00 00 00 07 00 00  |HSKP............|
00000010  00 0e 00 00 00 5f 64 61  74 61 5f 65 6c 65 6d 2e  |....._data_elem.|
00000020  64 66 6e 0e 00 00 00 5f  64 61 74 61 5f 65 6c 65  |dfn...._data_ele|
00000030  6d 2e 74 79 70 0b 00 00  00 62 6f 6f 6c 65 61 6e  |m.typ....boolean|
00000040  2e 74 79 70 0b 00 00 00  64 61 74 61 73 65 74 2e  |.typ....dataset.|
00000050  64 66 6e 07 00 00 00 69  6e 74 2e 74 79 70 0a 00  |dfn....int.typ..|
00000060  00 00 73 69 6e 74 33 32  2e 74 79 70 0a 00 00 00  |..sint32.typ....|
00000070  73 74 72 69 6e 67 2e 74  79 70 03 00 00 00 0f 00  |string.typ......|
00000080  00 00 07 00 00 00 00 00  00 00 01 00 00 00 02 00  |................|
00000090  00 00 03 00 00 00 04 00  00 00 05 00 00 00 06 00  |................|
000000a0  00 00 0f 01 00 00 07 00  00 00 00 00 00 00 01 00  |................|
000000b0  00 00 02 00 00 00 03 00  00 00 04 00 00 00 05 00  |................|
000000c0  00 00 06 00 00 00 0f 02  00 00 07 00 00 00 00 00  |................|
000000d0  00 00 01 00 00 00 02 00  00 00 03 00 00 00 04 00  |................|
000000e0  00 00 05 00 00 00 06 00  00 00 05 00 00 00 03 00  |................|
000000f0  00 00 03 00 00 00 0f 00  00 00 07 00 00 00 66 65  |..............fe|
00000100  5f 74 65 6d 70 20 a1 07  00 31 00 05 00 00 00 53  |_temp ...1.....S|
00000110  68 65 65 74 04 00 00 00  00 0b 00 00 00 53 68 65  |heet.........She|
00000120  65 74 53 65 72 76 65 72  04 00 00 00 00 01 00 00  |etServer........|
00000130  00 58 05 00 00 00 00 01  00 00 00 59 05 00 00 00  |.X.........Y....|
00000140  00 01 00 00 00 5a 05 00  00 00 00 05 00 00 00 54  |.....Z.........T|
00000150  68 65 74 61 08 00 00 00  00 0a 00 00 00 41 49 49  |heta.........AII|
00000160  6e 73 74 61 6e 63 65 04  00 00 00 00 04 00 00 00  |nstance.........|
00000170  4d 6f 64 65 06 00 00 00  00 09 00 00 00 42 65 68  |Mode.........Beh|
00000180  61 76 69 6f 75 72 06 00  00 00 00 09 00 00 00 4e  |aviour.........N|

```
```
(gdb) p container.begin()->second
$16 = {DataSetName = "fe_temp", NbProperties = 49, MaxNbRows = 500000, 
  PropertyNames = std::vector of length 49, capacity 49 = {"Sheet", 
    "SheetServer", "X", "Y", "Z", "Theta", "AIInstance", "Mode", "Behaviour", 
    "NameIndex", "Target", "VisualPropertyA", "VisualPropertyB", 
    "VisualPropertyC", "EntityMounted", "RiderEntity", "TickPos", "LocalX", 
    "LocalY", "LocalZ", "ContextualProperty", "AvailableImpulseBitSize", 
    "Cell", "VisionCounter", "CombatState", "CurrentHitPoints", 
    "MaxHitPoints", "BestRole", "BestRoleLevel", "CurrentRunSpeed", 
    "CurrentWalkSpeed", "Stunned", "WhoSeesMe", "Bars", "TeamId", 
    "ActionFlags", "TargetList", "VisualFX", "GuildSymbol", "GuildNameId", 
    "EventFactionId", "PvpMode", "PvpClan", "Fuel", "InOutpostZoneAlias", 
    "InOutpostZoneSide", "OwnerPeople", "OutpostInfos", "NPCAlias"}, 
  PropIndexToType = std::vector of length 49, capacity 49 = {TypeUint32, 
    TypeUint32, TypeSint32, TypeSint32, TypeSint32, TypeFloat, TypeUint32, 
    TypeUint64, TypeUint64, TypeUint32, TypeSint32, TypeUint64, TypeUint64, 
    TypeUint64, TypeSint32, TypeSint32, TypeUint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeUint16, TypeUint16, TypeSint32, TypeUint8, TypeUint8, 
    TypeSint32, TypeSint32, TypeUint16, TypeUint16, TypeFloat, TypeFloat, 
    TypeBool, TypeUint64, TypeUint32, TypeUint16, TypeUint16, TypeSint32, 
    TypeSint16, TypeUint64, TypeUint32, TypeUint32, TypeUint8, TypeUint32, 
    TypeBool, TypeUint32, TypeUint8, TypeUint8, TypeUint16, TypeUint32}, 
  PropIsList = std::vector<bool> of length 49, capacity 64 = {0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}, 
  EntityTypesFilter = std::vector of length 6, capacity 6 = {0 '\000', 
    6 '\006', 1 '\001', 2 '\002', 16 '\020', 17 '\021'}}

```
```
(gdb) p (++container.begin())->second
$17 = {DataSetName = "pet", NbProperties = 5, MaxNbRows = 1, 
  PropertyNames = std::vector of length 5, capacity 5 = {"ItemId", 
    "CreatureSheet", "OwnerIndex", "CreatureIndex", "Order"}, 
  PropIndexToType = std::vector of length 5, capacity 5 = {TypeUint64, 
    TypeUint32, TypeUint32, TypeUint32, TypeUint64}, 
  PropIsList = std::vector<bool> of length 5, capacity 64 = {0, 0, 0, 0, 0}, 
  EntityTypesFilter = std::vector of length 1, capacity 1 = {255 '\377'}}

```
```
(gdb) p (++(++container.begin()))->second
$18 = {DataSetName = "fame", NbProperties = 103, MaxNbRows = 16000, 
  PropertyNames = std::vector of length 103, capacity 103 = {"Civilisation", 
    "Guild", "FameMemory", "Fame_0", "Fame_1", "Fame_2", "Fame_3", "Fame_4", 
    "Fame_5", "Fame_6", "Fame_7", "Fame_8", "Fame_9", "Fame_10", "Fame_11", 
    "Fame_12", "Fame_13", "Fame_14", "Fame_15", "Fame_16", "Fame_17", 
    "Fame_18", "Fame_19", "Fame_20", "Fame_21", "Fame_22", "Fame_23", 
    "Fame_24", "Fame_25", "Fame_26", "Fame_27", "Fame_28", "Fame_29", 
    "Fame_30", "Fame_31", "Fame_32", "Fame_33", "Fame_34", "Fame_35", 
    "Fame_36", "Fame_37", "Fame_38", "Fame_39", "Fame_40", "Fame_41", 
    "Fame_42", "Fame_43", "Fame_44", "Fame_45", "Fame_46", "Fame_47", 
    "Fame_48", "Fame_49", "Fame_50", "Fame_51", "Fame_52", "Fame_53", 
    "Fame_54", "Fame_55", "Fame_56", "Fame_57", "Fame_58", "Fame_59", 
    "Fame_60", "Fame_61", "Fame_62", "Fame_63", "Fame_64", "Fame_65", 
    "Fame_66", "Fame_67", "Fame_68", "Fame_69", "Fame_70", "Fame_71", 
    "Fame_72", "Fame_73", "Fame_74", "Fame_75", "Fame_76", "Fame_77", 
    "Fame_78", "Fame_79", "Fame_80", "Fame_81", "Fame_82", "Fame_83", 
    "Fame_84", "Fame_85", "Fame_86", "Fame_87", "Fame_88", "Fame_89", 
    "Fame_90", "Fame_91", "Fame_92", "Fame_93", "Fame_94", "Fame_95", 
    "Fame_96", "Fame_97", "Fame_98", "Fame_99"}, 
  PropIndexToType = std::vector of length 103, capacity 103 = {TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, 
    TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32, TypeSint32}, 
  PropIsList = std::vector<bool> of length 103, capacity 128 = {0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}, 
  EntityTypesFilter = std::vector of length 4, capacity 4 = {0 '\000', 
    12 '\f', 13 '\r', 14 '\016'}}

```
