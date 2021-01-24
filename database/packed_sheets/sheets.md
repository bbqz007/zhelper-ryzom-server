game_share

ryzom/common/src/game_share/time_weather_season
  - static_light_cycle.h
  - weather_function_params_sheet_base.h
  - weather_setup_sheet_base.h
```
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
```
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
server_share

ryzom/server/src/server_share/
  - continent_container.h
```
```
ai_serivece

ryzom/server/src/ai_service/
  - sheets.h
  
```
```
entities_game_service

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
gpm_service

ryzom/server/src/gpm_service/
  - sheets.h
```
```
input_output_service

ryzom/server/src/input_output_service/
  - string_manager.h
```
```
