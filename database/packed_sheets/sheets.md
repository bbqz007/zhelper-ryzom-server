* [game_share](#game_share)
* [server_share](#server_share)
* [ai_serivece](#ai_serivece)
* [entities_game_service](#entities_game_service)
* [gpm_service](#gpm_service)
* [input_output_service](#input_output_service)

### game_share

ryzom/common/src/game_share/time_weather_season
  - static_light_cycle.h
  - weather_function_params_sheet_base.h
  - weather_setup_sheet_base.h
```c++
/**
 * Class containing the data used to manage day cycles ( read from sheets )
 * \author Nicolas Brigand
 * \author Nevrax France
 * \date 2003
 */
class CStaticLightCycle
{
public:

	struct SLightCycle
	{
		float DayHour;
		float DayToDuskHour;
		float DuskToNightHour;
		float NightHour;
		float NightToDayHour;
```
```c++
class CWeatherFunctionParamsSheetBase
{
public:
	uint32  DayLength;   // length of day, in hours
	uint32  CycleLength; // length of a cycle, in hours
	//
	float   MinThunderPeriod; // Min thunder period, in s.
	float   ThunderLength; // Length of a thunder strike, in s.
	//
	float   CloudWindSpeedFactor;
	float   CloudMinSpeed;
```
```c++
/**
 * Class to manage weather setup sheets
 * \author Nicolas Vizerie
 * \author Nevrax France
 * \date 2003
 */
class CWeatherSetupSheetBase
{
public:
	CWeatherStateSheet WeatherState;
	CCloudStateSheet   CloudState;
	NLMISC::TStringId  SetupName;
  
// state of weather, including clouds
class CCloudStateSheet
{
public:
	NLMISC::CRGBA AmbientDay;
	NLMISC::CRGBA DiffuseDay;
	NLMISC::CRGBA AmbientNight;
	NLMISC::CRGBA DiffuseNight;
	NLMISC::CRGBA AmbientDusk;
	NLMISC::CRGBA DiffuseDusk;
	uint32		  NumClouds;
	float		  DiffusionSpeed;

// state of weather, not including clouds
class CWeatherStateSheet
{
public:
	// Best aprox setup name when blended
	std::string BestSetupName;
	struct CFXInfos
	{
		std::string Name;
		float		Ratio;

		CFXInfos() { Ratio = 0.0f; }

		void		serial(class NLMISC::IStream &f) throw(NLMISC::EStream)
		{
			f.serial(Name, Ratio);
		}
	};
	// Fog (main & canopy)
	float		  FogRatio;
	NLMISC::CRGBA FogColorDay;
	NLMISC::CRGBA FogColorDusk;
	NLMISC::CRGBA FogColorNight;
	float         FogNear[NumFogType];
	float		  FogFar[NumFogType];
	float		  FogGradientFactor; // factor for the fog gradient
	// Lighting
	float		  Lighting;
	/** Bg read from the sheet
 	  */
	std::string   DayBackground;
	std::string   DuskBackground;
	std::string   NightBackground;

	// Wind
	float		  WindIntensity;
	// FX
	std::vector<CFXInfos> FXInfos;
	// Thunder
	float		  ThunderIntensity;
	NLMISC::CRGBA ThunderColor;

	std::string   LocalizedName;

```
### server_share

ryzom/server/src/server_share/
  - continent_container.h
  
```c++
	class CContinentContainer::CSheet
	{
	public:
		CSheet() {}

		std::string					Name;
		std::string					PacsRBank;
		std::string					PacsGR;
		std::string					LandscapeIG;
		std::vector<std::string>	ListIG;
```

### ai_serivece

ryzom/server/src/ai_service/
  - sheets.h
  
```c++
class CAIAction
: public IAIAction
{
private:
	NLMISC::CSheetId _SheetId;
	bool _SelfAction;
};

```
```c++
class CActionList
: public NLMISC::CDbgRefCount<CActionList>
{
public:
	NLMISC::CSheetId			_SheetId;
	std::vector<IAIActionCPtr>	_Actions;

	bool _HasNormalAction;
	bool _HasSelfAction;
```
```c++
class CCreature
: public ICreature
{
public:
	CCreature();
	
private:
	NLMISC::CSheetId _SheetId;
	uint32 _Level;
	
	// colors from sheet
	uint8 _ColorHead;
	uint8 _ColorArms;
	uint8 _ColorHands;
	uint8 _ColorBody;
	uint8 _ColorLegs;
	uint8 _ColorFeets;
	
	float _Radius;
	float _Height;
	float _Width;
	float _Length;
	float _BoundingRadius;
	
	// the entity is a bot object and cannot be traversed.
	bool _NotTraversable;
	
	// the entity is a fauna, even is used as npc, it keep it's fauna name
	bool _ForceDisplayCreatureName;
	
	float _BonusAggroHungry;
	float _BonusAggroVeryHungry;
	
	float _AssistDist;
	
	float _AggroRadiusNotHungry;
	float _AggroRadiusHungry;
	float _AggroRadiusHunting;
	
	float _AggroReturnDistCheck;
	float _AggroRadiusD1;
	float _AggroRadiusD2;
	float _AggroPrimaryGroupDist;
	float _AggroPrimaryGroupCoef;
	float _AggroSecondaryGroupDist;
	float _AggroSecondaryGroupCoef;
	float _AggroPropagationRadius;
	
	AITYPES::TFaunaType _FaunaType;
	
	float _Scale;
	
	float _DistToFront;
	float _DistToBack;
	float _DistToSide;
	
	float _DistModulator;
	float _TargetModulator;
	float _ScoreModulator;
	float _FearModulator;
	float _LifeLevelModulator;
	float _CourageModulator;
	float _GroupCohesionModulator;
	
	float _GroupDispersion;
	
	uint32 _XPLevel;
	uint32 _NbPlayers;
	
	uint32 _EnergyValue;
	
	bool _CanTurn;
	
	NLMISC::CDbgPtr<CActionList> _FightConfig[FIGHTCFG_MAX];
	
	NLMISC::CSheetId _LeftItem;
	NLMISC::CSheetId _RightItem;
	
	uint32 _MinFightDist;
	
	uint32 _FactionIndex;
	sint32 _FameForGuardAttack;
	
	std::string _AssistGroupIndexStr;
	std::string _AttackGroupIndexStr;
	std::string _GroupIndexStr;
	
	uint32 _GroupPropertiesIndex;
	
	/// the creature sheet can specify a multiplier that modulate the dynmaic groupe size
	uint32 _DynamicGroupCountMultiplier;
	
	std::string _BotName;
	
	TScriptCompList _ScriptCompList;
	
	TScriptCompList _UpdateScriptList;
	TScriptCompList _DeathScriptList;
	TScriptCompList _BirthScriptList;
	
	// Character Race
	EGSPD::CPeople::TPeople _Race;
```
```c++
class CRaceStats
: public IRaceStats
{
private:
	NLMISC::CSheetId _SheetId;
	std::string _Race;
```

[top](#)
### entities_game_service

ryzom/server/src/entities_game_service/egs_sheets/
  - egs_static_world.h
  - egs_static_text_emotes.h
  - egs_static_success_table.h
  - egs_static_rolemaster_phrase.h
  - egs_static_outpost.h
  - egs_static_guild_option.h
  - egs_static_game_sheet.h
  - egs_static_game_item.h
  - egs_static_emot.h
  - egs_static_deposit.h
  - egs_static_brick.h
  - egs_static_ai_action.h
```
```

[top](#)
### gpm_service

ryzom/server/src/gpm_service/
  - sheets.h
```c++
/**
 * Singleton containing database on information for actors
 * \author Sadge
 * \author Nevrax France
 * \date 2002
 */
class CSheets
{
public:
	class CSheet
	{
	public:
		float	WalkSpeed;
		float	RunSpeed;
		float	Radius;				// pacs primitive's radius
		float	Height;				// pacs primitive's height
		float	BoundingRadius;		// fighting radius
		float	Scale;				// entity scale

```

[top](#)
### input_output_service

ryzom/server/src/input_output_service/
  - string_manager.h
```c++
	/** Container for data extracted from entity sheet.
	 *	This is used to store 'static' information like gender.
	 */
	struct CStringManager::TSheetInfo
	{
		/// The name of the entity model.
		std::string			SheetName;
		/// The race of the creature.
		std::string			Race;
		/// The gender of this entity model.
		GSGENDER::EGender	Gender;
		/// The display name
//		std::string			DisplayName;
		/// Creature profile (aka career)
		std::string			Profile;
		/// Creature chat profile
		std::string			ChatProfile;
```

[top](#)
