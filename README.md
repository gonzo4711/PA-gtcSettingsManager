PA-gtcSettingsManager
=====================

Planetary Annihilation Mod - Manage Settings, Tabs and SubGroups

### Why creating this mod?
After PAMM installation and activating a handfull mods, the Settings UI is crowded with settings of each mod.
Sometimes you are searching for the correct setting a while until you'll see it.
This is always annoying, so there should be a better solution for this.

### Alternatives?
We know, it's getting easier to setup a custom settings-key, since the 'rSettingsManager' was created and updated to the new settings-API.
BUT: Overview of the themed settings is still difficult - only modding development gets easier.

### What are the features?
You can actually create two kinds of settings (DropDown and Slider) - yes, this is the same like the 'rSettingsManager' does, but now
you can create TABS and SUBGROUPS for this settings as well!
It is also compatible to the 'rSettingsManager', so you can use both at the same time...
This mod will also recognize a change of settings after displaying the page - so you can append a setting while in runtime.

***************************************************************************************************************************************************

### Creating a custom setting

Creating a new setting with tab and groups is very easy - you can do this in one single function call:

####Function list:
```
model.addSetting.dropDown(id,title,options,defaultIndex,tabName[,groupName]);
model.addSetting.slider(id,title,minValue,maxValue,defaultValue,tabName[,groupName]);
model.addSetting.item(id,sData,tab);
model.extendSettings(tabName,groupName,extendData) OR model.extendSettings(tabName,extendData);
```

**Add a DropDown field**
```
model.addSetting.dropDown(id,title,options,defaultIndex,tabName[,groupName]);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of this field 					|
| title        | string | The displayed text in the settings VIEW 	|
| options      | object | An options object like: {0:'OFF',1:'ON'} 	|
| defaultIndex | string | The default key (must exist in options)	|
| tabName      | string | A current or new Tab name (i.e. "My Tab")	|
| groupName    | string | optional: A group name (i.e. "My Group")	|

**Add a Slider field**
```
model.addSetting.slider(id,title,minValue,maxValue,defaultValue,tabName[,groupName]);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of this field 					|
| title        | string | The displayed text in the settings VIEW 	|
| minValue     | int    | The slider min value 						|
| maxValue     | int    | The slider max value						|
| defaultValue | int    | The default value							|
| tabName      | string | A current or new Tab name (i.e. "My Tab")	|
| groupName    | string | optional: A group name (i.e. "My Group")	|

**Add a custom field (used by dropDown & slider)**
```
model.addSetting.item(id,sData,tab);
```
|         KEY      |  TYPE  | 				DESCRIPTION 				|
| ---------------- | ------ | ----------------------------------------- |
| id               | string | Identifier of this field 					|
| sData            | object | A data object with api-conformable-data	|
| sData._groupName | string | optional: A group name (i.e. 'My Group')	|
| tab              | string | A current or new Tab name (i.e. "My Tab")	|

**Extend settings to tab (& group)**
```
model.extendSettings(tabName,groupName,extendData) OR model.extendSettings(tabName,extendData);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| tabName      | string | A current or new Tab name (i.e. "My Tab")	|
| groupName    | string | optional: A group name (i.e. "My Group")	|
| extendData   | object | A settings object (see examples) 		    |

***************************************************************************************************************************************************

####Examples

This will create a new setting **my_setting_key**, called 'My Setting Title' to the **UI**-Tab with two settings: **OFF,ON**
```javascript
model.addSetting.dropDown('my_setting_key',"My Setting Title",{'0':'OFF','1':'ON'},0,"ui");
```

This will create a new setting **my_special_setting_key**, called 'My Setting Title 2' to the **UI**-Tab in a new group **My Group** with two settings: **OFF,ON**
*Hint:* If there exists only 'one' subGroup (such like 'default') it wouldn't be displayed - in this case there are two groups: "default and my_group", so both will be printed
```javascript
model.addSetting.dropDown('my_special_setting_key',"My Setting Title 2",{'0':'OFF','1':'ON'},0,"ui","My Group");
```

This will create a new setting **my_new_setting_key**, called **My New Setting Title** to a new created tab **PERFORMANCE** in a new group **My Group** with two settings: **OFF,ON**
```javascript
model.addSetting.dropDown('my_new_setting_key',"My New Setting Title",{'0':'OFF','1':'ON'},0,"performance","My Group");
```

This will extend the **UI** Tab and the **default** group with new settings
```javascript
model.extendSettings('ui',{
	'my_special_setting_key': {
		title: 'My Setting Title 2',
		options: [0,1],
		optionsText: ['OFF','ON'],
		default: 0
	}
});
```

This will extend the **PERFORMANCE** Tab and the **My Group** group with new settings
```javascript
model.extendSettings('performance','My Group',{
	'my_new_setting_key': {
		title: 'My New Setting Title',
		options: [0,1],
		optionsText: ['OFF','ON'],
		default: 0
	}
});
```

***************************************************************************************************************************************************

### Getting a setting value

Get a setting value is easily like creating a new one:

**Function list**
```javascript
settingHelper.getSetting(id[,tabName]);
settingHelper.getInitialSetting(id,defaultValue[,tabName]);
settingHelper.setSetting(id,tabName,newValue) || settingHelper.setSetting(id,newValue);
settingHelper.isSetting(id[,tabName]);
settingHelper.transferSetting(id,fromTab,toTab);
settingHelper.removeSetting(id[,tabName]);
```


**Get a setting**
```
settingHelper.getSetting(id[,tabName])
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| tabName      | string | optional: A tab name (i.e. "ui")			|
If you don't know the tab name, the settingHelper will search for it.
**HINT:** You never should use two settings with the same **id** in different tabs!


**Get a setting with initial value**
```
settingHelper.getInitialSetting(id,defaultValue[,tabName]);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| defaultValue | int    | The default value							|
| tabName      | string | optional: A tab name (i.e. "ui")			|
If a setting does not exist, you will get the setted default value.
If you don't know the tab name, the settingHelper will search for it.


**Change a setting temporarily**
```
settingHelper.setSetting(id,tabName,newValue)
OR
settingHelper.setSetting(id,newValue);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| defaultValue | int    | The default value							|
| tabName      | string | optional: A tab name (i.e. "ui")			|
If a setting does not exist, you will get the setted default value.
If you don't know the tab name, the settingHelper will search for it - than, you should use the second syntax


**Check, if a setting exists, or is part of a tab**
```
[boolean] settingHelper.isSetting(id[,tabName]);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| tabName      | string | optional: A tab name (i.e. "ui")			|
If you don't know the tabName, the settingHelper will search for it.
If you set the tabName, the function will only check settings in this tab!


**Transfer a settingValue from one tab, to another (and deletes the old one)**
```
[boolean] settingHelper.transferSetting(id,fromTab,toTab);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| fromTab      | string | A tab name (i.e. "ui")					|
| toTab        | string | A tab name (i.e. "performance")			|
You will need this function, if you'll move some old settings (used by rSettingsManager or API), to a new custom tab.
To prevent dublicate ids, you must use this (or the remove function)


**Remove a setting value temporarily (or permanent after settings-save)**
```
[boolean] settingHelper.removeSetting(id[,tabName]);
```
|      KEY     |  TYPE  | 				DESCRIPTION 				|
| ------------ | ------ | ----------------------------------------- |
| id           | string | Identifier of the field 					|
| tabName      | string | A tab name (i.e. "ui")					|
You will need this function, if you'll remove some old settings (used by rSettingsManager or API).


***************************************************************************************************************************************************

####Examples


This will return the current value of **my_setting_key**
```javascript
settingHelper.getSetting('my_setting_key');
```

This will return the current value of **my_setting_key** in the **UI**-Tab
```javascript
settingHelper.getSetting('my_setting_key','ui');
```

This will return the current value of **my_setting_key** with an initial 'default' value **3**
```javascript
model.getInitialSetting('my_setting_key',3);
```

This will change the **my_setting_key** to the new value 5
```javascript
model.setSetting('my_setting_key',3);
```

This will check the **my_setting_key** in the **UI**-Tab
```javascript
model.isSetting('my_setting_key','ui');
```

This will transfer a setting
```javascript
model.transferSetting('my_setting_key','ui','performance');
```
**HINT:** You should use this in the settings-VIEW and save after transfer, to take effect not only temporarily!

This will delete a setting
```javascript
model.removeSetting('my_setting_key');
```
**HINT:** You should use this in the settings-VIEW and save after delete, to take effect not only temporarily!


***************************************************************************************************************************************************


### More information about PA can be found here
http://www.uberent.com/pa/
