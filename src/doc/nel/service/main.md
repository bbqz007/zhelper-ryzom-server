[\[doc lisence\]](../../../../LISENCE)
[src](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/nel/src/net/service.cpp)
* preinit
    - init **IModuleManager** ```IModuleManager::getInstance();```
    - resolve work path
    - load **configure** files ```ConfigFile.load (cfn);```
    - setup **variable** with config file variable ```IVariable::init (ConfigFile);```
    - execute "Commands" script in configure files.
    - _hook_,Command line start,由继承类扩展实现 **```commandStart()```**
    - init **signal handlers** ```initSignal()```
    - Startup **Layer5 UnifiedNetwork**
        1. load the **sid** from "SId" in cfg.
        2. Register all network associations from "Networks" in cfg.
        3. specifict address:port to **NS**
        4. init **UnifiedNetwork** ```CUnifiedNetwork::getInstance()->init (&loc, _RecordingState, _ShortName, ListeningPort, _SId)```
    - Connect to the **local AES** and send identification
    - ```initAdmin()```
    - Add **callback** array
    - ```CTransportClass::init();```
        1. add callback array to UnifiedNetwork
        2. create an instance of all d'ifferent prop types
        3. we have to know when a service comes, so add callback (put the callback before all other one because we have to send this message first)
* init
    - _hook_,由继承类扩展实现 **```init();```**
    - Connects to the present services ```CUnifiedNetwork::getInstance()->connect();```
    - Say to the AES that the service is ready ```CMessage msgout ("SR"); CUnifiedNetwork::getInstance()->send("AES", msgout);```
    - ```_Initialized = true;```
* postinit    
    - Call the user command from the config file if any **"StartCommands"**
* main loop
    - count the amount of time to manage internal system
    - every second, check for CPU usage
    - _hook_,call the user update and exit if the user **update** asks it,由继承类扩展实现 **```IService::update ()```**
    - deal with any input waiting from stdin
    - count loop times ```NbUserUpdate++;```
    - count the amount of time to manage internal system
    - stop the loop if the exit signal asked
    - ```CConfigFile::checkConfigFiles ();```
    - ```updateAdmin ();```
    - ```CFile::checkFileChange();```
    - update updatable interface
    - get and manage **layer 5 messages** ```CUnifiedNetwork::getInstance()->update (_UpdateTimeout);```
    - **update modules** ```IModuleManager::getInstance().updateModules();```
    - Allow direct closure if the naming service was lost
    - do some statistics
* cleanup
    - _hook_,由继承类扩展实现 **```release ();```**
    - ```CTransportClass::release();```
    - Delete all network connection (naming client also)
    - warn the module layer that the application is about to close
    - remove the stdin monitor thread
    - release the module manager
    - release the network
    - stop the timeout thread
