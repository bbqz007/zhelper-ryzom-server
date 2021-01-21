```sql
--
-- Table structure for table `domain`
--

CREATE TABLE `domain` (
  `domain_id` int(10) unsigned NOT NULL,
  `domain_name` varchar(32) NOT NULL DEFAULT '',
  `status` enum('ds_close','ds_dev','ds_restricted','ds_open') NOT NULL DEFAULT 'ds_dev',
  `patch_version` int(10) unsigned NOT NULL DEFAULT '0',
  `backup_patch_url` varchar(255) DEFAULT NULL,
  `patch_urls` text,
  `login_address` varchar(255) NOT NULL DEFAULT '',
  `session_manager_address` varchar(255) NOT NULL DEFAULT '',
  `ring_db_name` varchar(255) NOT NULL DEFAULT '',
  `web_host` varchar(255) NOT NULL DEFAULT '',
  `web_host_php` varchar(255) NOT NULL DEFAULT '',
  `description` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`domain_id`),
  UNIQUE KEY `name_idx` (`domain_name`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb4;
/*!40101 SET character_set_client = @saved_cs_client */;

```
```sql
--
-- Table structure for table `permission`
--

CREATE TABLE `permission` (
  `PermissionId` int(11) NOT NULL AUTO_INCREMENT,
  `UId` int(10) unsigned NOT NULL DEFAULT '0',
  `DomainId` int(11) NOT NULL DEFAULT '-1',
  `ShardId` int(10) NOT NULL DEFAULT '-1',
  `AccessPrivilege` set('OPEN','DEV','RESTRICTED') NOT NULL DEFAULT 'OPEN',
  PRIMARY KEY (`PermissionId`),
  KEY `UIDIndex` (`UId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
/*!40101 SET character_set_client = @saved_cs_client */;
```
```sql
--
-- Table structure for table `shard`
--
CREATE TABLE `shard` (
  `ShardId` int(10) NOT NULL DEFAULT '0',
  `domain_id` int(11) unsigned NOT NULL DEFAULT '0',
  `WsAddr` varchar(64) DEFAULT NULL,
  `NbPlayers` int(10) unsigned DEFAULT '0',
  `Name` varchar(255) DEFAULT 'unknown shard',
  `Online` tinyint(1) unsigned DEFAULT '0',
  `Version` varchar(64) NOT NULL DEFAULT '',
  `FixedSessionId` int(11) unsigned NOT NULL DEFAULT '0',
  `State` enum('ds_close','ds_dev','ds_restricted','ds_open') NOT NULL DEFAULT 'ds_dev',
  `MOTD` text NOT NULL,
  PRIMARY KEY (`ShardId`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb4 COMMENT='contains all shards information for login system';
```
```sql
--
-- Table structure for table `user`
--

CREATE TABLE `user` (
  `UId` int(10) NOT NULL,
  `Login` varchar(64) NOT NULL DEFAULT '',
  `Password` varchar(106) DEFAULT NULL,
  `ShardId` int(10) NOT NULL DEFAULT '-1',
  `State` enum('Offline','Online') NOT NULL DEFAULT 'Offline',
  `Privilege` varchar(255) NOT NULL DEFAULT '',
  `GroupName` varchar(255) NOT NULL DEFAULT '',
  `FirstName` varchar(255) NOT NULL DEFAULT '',
  `LastName` varchar(255) NOT NULL DEFAULT '',
  `Birthday` varchar(32) NOT NULL DEFAULT '',
  `Gender` tinyint(1) unsigned NOT NULL DEFAULT '0',
  `Country` char(2) NOT NULL DEFAULT '',
  `Email` varchar(255) NOT NULL DEFAULT '',
  `Address` varchar(255) NOT NULL DEFAULT '',
  `City` varchar(100) NOT NULL DEFAULT '',
  `PostalCode` varchar(10) NOT NULL DEFAULT '',
  `USState` char(2) NOT NULL DEFAULT '',
  `Chat` char(2) NOT NULL DEFAULT '0',
  `BetaKeyId` int(10) unsigned NOT NULL DEFAULT '0',
  `CachedCoupons` varchar(255) NOT NULL DEFAULT '',
  `ProfileAccess` varchar(45) DEFAULT NULL,
  `Level` int(2) NOT NULL DEFAULT '0',
  `CurrentFunds` int(4) NOT NULL DEFAULT '0',
  `IdBilling` varchar(255) NOT NULL DEFAULT '',
  `Community` char(2) NOT NULL DEFAULT '--',
  `Newsletter` tinyint(1) NOT NULL DEFAULT '1',
  `Account` varchar(64) NOT NULL DEFAULT '',
  `ChoiceSubLength` tinyint(2) NOT NULL DEFAULT '0',
  `CurrentSubLength` varchar(255) NOT NULL DEFAULT '0',
  `ValidIdBilling` int(4) NOT NULL DEFAULT '0',
  `GMId` int(4) NOT NULL DEFAULT '0',
  `ExtendedPrivilege` varchar(128) NOT NULL DEFAULT '',
  `ToolsGroup` varchar(20) NOT NULL DEFAULT '',
  `Unsubscribe` date NOT NULL DEFAULT '0000-00-00',
  `SubDate` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  `SubIp` varchar(20) NOT NULL DEFAULT '',
  `SecurePassword` varchar(32) NOT NULL DEFAULT '',
  `LastInvoiceEmailCheck` date NOT NULL DEFAULT '0000-00-00',
  `FromSource` varchar(8) NOT NULL DEFAULT '',
  `ValidMerchantCode` varchar(13) NOT NULL DEFAULT '',
  `PBC` tinyint(1) NOT NULL DEFAULT '0',
  `ApiKeySeed` varchar(8) DEFAULT NULL
) ENGINE=MyISAM AUTO_INCREMENT=13 DEFAULT CHARSET=utf8mb4 COMMENT='contains all users information for login system';

```
