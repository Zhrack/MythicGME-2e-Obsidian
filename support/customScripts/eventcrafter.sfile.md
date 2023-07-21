Shortcuts for Word Mill Games' Event Crafter. Created by Tana Pigeon, published by Word Mill Games, and licensed for our use with permission from the author. Find out more at [www.wordmillgames.com](http://www.wordmillgames.com/).
__
```
^sfile setup$
```
__
```js

const confirmObjectPath = _inlineScripts.inlineScripts.HelperFncs.confirmObjectPath;

// Root combo list, holding all EC sheets
confirmObjectPath("_inlineScripts.state.sessionState.eventCrafter", { activeSheet: "", sheets: {} });

// Event callback for state.onReset
confirmObjectPath(
	"_inlineScripts.state.listeners.onReset.eventCrafter",
	function()
	{
		expand("eventcrafter reset noconfirm");
	});

```
__
Sets up this shortcut-file


__
```
^sfile shutdown$
```
__
```js

//Event callback removal
delete _inlineScripts?.state?.listeners?.onReset?.eventCrafter;

// State removal
delete _inlineScripts?.state?.sessionState?.eventCrafter;
```
__
Shuts down this shortcut-file

__
```
^eventcrafter reset$
```
__
```js

// Confirm
if (!popups.confirm("Confirm resetting the <b>Event Crafter</b> system"))
{
	return null;
}

// Reset
return expand("eventcrafter reset noconfirm");

```
__
eventcrafter reset - **Deletes all sheets and their content.

__
^eventcrafter reset noconfirm$

__
```js

const confirmObjectPath = _inlineScripts.inlineScripts.HelperFncs.confirmObjectPath;
confirmObjectPath("_inlineScripts.state.sessionState.eventCrafter");

_inlineScripts.state.sessionState.eventCrafter.activeSheet = "";
_inlineScripts.state.sessionState.eventCrafter.sheets = {};

return expFormat("Event Crafter content reset");

```
__
hidden - No-confirm reset.

__
__
```js

const EC_SPACE_SEPARATOR = "__";

function GetSpacedName(name)
{
	return name.replaceAll(EC_SPACE_SEPARATOR, " ");
}

function SheetExists(sheetName)
{
	if(_inlineScripts.state.sessionState.eventCrafter.sheets[sheetName])
	{
		return true;
	}
	
	return false;
}

function CategoryExistsInActiveSheet(categoryName)
{
	const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
	if(activeSheetName === "")
	{
		return false;
	}
	
	const activeSheet = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName];
	
	if(activeSheet.categories[categoryName])
	{
		return true;
	}
	
	return false;
}

function GetUniquesOffset(category, roll)
{
	let uniqueOffset = 0;
	category.uniqueIndicesRemoved.forEach(
		(elem, index, array) => 
		{
			if(elem <= roll)
			{
				uniqueOffset++;
			}
		}
	);

	return uniqueOffset;
}

// if message contains EC_ERROR, it is splitted away and the error text extracted
function CheckAndReturnError(message)
{
	if(message.length > 1)
	{
		return { result: true, error: message[0] };
	}
	return { result: false };
}

function CheckRolledElement(elem, rollWithOffset)
{
	if(elem.unique === 1)
	{
		elem.unique = 2;
		category.uniqueIndicesRemoved.push(rollWithOffset);
	}
}

```
__
Helper scripts

__
```
^eventcrafter meaning ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const MYTHIC2E_MEANING_TABLES = new Map([
["Action", "[[Action Meaning Table 2e#^Mythic2Action1]]"],
["Descriptions", "[[Descriptions Meaning Table 2e#^Mythic2Descriptions1]]"],
["Names", "[[Names#^Mythic2Names]]"],
["Adventure Tone", "[[Adventure Tone#^Mythic2AdventureTone]]"],
["Plot Twists", "[[Plot Twists#^Mythic2PlotTwists]]"],
["Characters", "[[Characters#^Mythic2Characters]]"],
["Character Appearance", "[[Characters Appearance#^Mythic2CharactersAppearance]]"],
["Character Background", "[[Characters Background#^Mythic2CharactersBackground]]"],
["Character Descriptors", "[[Characters Conversations#^Mythic2CharactersConversations]]"],
["Character Identity", "[[Characters Identity#^Mythic2CharactersIdentity]]"],
["Character Motivations", "[[Characters Motivations#^Mythic2CharactersMotivations]]"],
["Character Personality", "[[Characters Personality#^Mythic2CharactersPersonality]]"],
["Character Skills", "[[Characters Skills#^Mythic2CharactersSkills]]"],
["Character Traits & Flaws", "[[Characters Traits & Flaws#^Mythic2CharactersTraitFlaws]]"],
["Characters Actions, General", "[[Characters Actions, General#^Mythic2CharactersGeneral]]"],
["Characters Actions, Combat", "[[Characters Actions, Combat#^Mythic2CharactersCombat]]"],
["Character Conversations", "[[Characters Conversations#^Mythic2CharactersConversations]]"],
["Animal Actions", "[[Animal Actions#^Mythic2AnimalActions]]"],
["Creature Descriptors", "[[Creature Descriptors#^Mythic2CreatureDescriptors]]"],
["Creature Abilities", "[[Creature Abilities#^Mythic2CreatureAbilities]]"],
["Alien Species Descriptors", "[[Alien Species Descriptors#^Mythic2AlienSpecies]]"],
["Creature Mutation Descriptors", "[[Mutation Descriptors#^Mythic2MutationDescriptors]]"],
["Undead Descriptors", "[[Undead Descriptors#^Mythic2UndeadDescriptors]]"],
["Cavern", "[[Cavern Descriptors#^Mythic2CavernDescriptors]]"],
["City", "[[City Descriptors#^Mythic2CityDescriptors]]"],
["Domicile", "[[Domicile Descriptors#^Mythic2DomicileDescriptors]]"],
["Dungeon", "[[Dungeon Descriptors#^Mythic2DungeonDescriptors]]"],
["Forest", "[[Forest Descriptors#^Mythic2ForestDescriptors]]"],
["Locations", "[[Locations#^Mythic2Locations]]"],
["Starships", "[[Locations#^Mythic2Locations]]"],
["Terrain", "[[Terrain Descriptors#^Mythic2TerrainDescriptors]]"],
["Objects", "[[Objects#^Mythic2Objects]]"],
["Dungeon Traps", "[[Dungeon Traps#^Mythic2DungeonTraps]]"],
["Magic Items", "[[Magic Item Descriptors#^Mythic2MagicItemDescriptors]]"],
["Army Descriptors", "[[Army Descriptors#^Mythic2ArmyDescriptors]]"],
["Civilization Descriptors", "[[Civilization Descriptors#^Mythic2CivilizationDescriptors]]"],
["Cryptic Message", "[[Cryptic Message#^Mythic2CrypticMessage]]"],
["Curses", "[[Curses#^Mythic2Curses]]"],
["Gods", "[[Gods#^Mythic2Gods]]"],
["Legends", "[[Legends#^Mythic2Legends]]"],
["Noble Houses", "[[Noble House#^Mythic2NobleHouse]]"],
["Powers", "[[Powers#^Mythic2Powers]]"],
["Scavenging Results", "[[Scavenging Results#^Mythic2ScavengingResults]]"],
["Smells", "[[Smells#^Mythic2Smells]]"],
["Sounds", "[[Sounds#^Mythic2Sounds]]"],
["Spell Effects", "[[Spell Effects#^Mythic2SpellEffects]]"],
["Smells", "[[Smells#^Mythic2Smells]]"],
["Visions & Dreams", "[[Visions & Dreams#^Mythic2VisionsDreams]]"]
]);

let meaningTables = [];
MYTHIC2E_MEANING_TABLES.forEach(function(value, key) {  
	  meaningTables.push(key);
  });
const spacedCategoryName = GetSpacedName($1);
const pick = popups.pick("[" + spacedCategoryName + "] Choose a meaning table to randomly pick from", meaningTables, 0, "adaptive");
if (pick === null)
{
	return expUnformat("Please choose a meaning table");
}

const diceRollerPlugin = app.plugins.getPlugin("obsidian-dice-roller");
const meaningTableRoller = await diceRollerPlugin.getRoller(MYTHIC2E_MEANING_TABLES.get(meaningTables[pick]));

const meaningRoll1 = await meaningTableRoller.roll();
const meaningRoll2 = await meaningTableRoller.roll();
return expUnformat("(" + meaningTables[pick] + "): " + meaningRoll1 + " (of/-ly) " + meaningRoll2);

```
__
hidden - Rolls on MGME2e Meaning Tables.

__
```
^eventcrafter special ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*) (0|1)$
```
__
```js

const EVENT_SPECIAL_ELEMENTS = [ 
[10, "INCREASE", "Roll in the Category again (if you get Special treat it as Expected). When interpreting the Element in this Event increase its intensity, making it something more than you would have."],
[20, "DECREASE", "Roll in the Category again (if you get Special treat it as Expected). When interpreting the Element in this Event decrease its intensity, making it something less than you would have."],
[30, "THIS IS BAD", "Roll in the Category again (if you get Special treat it as Expected). When interpreting the Element in this Event treat it as something unfortunate or negative for the Player Character."],
[40, "THIS IS GOOD", "Roll in the Category again (if you get Special treat it as Expected). When interpreting the Element in this Event treat it as something fortunate or beneficial for the Player Character."],
[60, "MULTI-ELEMENT", "Roll twice in the Category (if you get Special treat it as Expected) and include both Elements in this Event."],
[70, "MOVING ALONG", "Add 3 Progress Points to this Category instead of 1 Point for this roll. Otherwise treat this as an Expected Element."],
[80, "ROLLING BACK", "Subtract 2 Progress Points to this Category and don’t add 1 for this roll. Otherwise treat this as an Expected Element."],
[100, "RANDOM ELEMENT", "Treat this Special Element like a Random Element, rolling on your choice of Meaning Tables twice and interpreting the results."]
];

let result = { eventText: "", type: "", name: "", description: "" };
if($2 === "1")
{
	// No Special flag
	result.type = "std";
	result.eventText = "Expected";
	return result;
}

result.type = "special";
let rolledSpecial;
const diceRollerPlugin = app.plugins.getPlugin("obsidian-dice-roller");
const percentageRoller = await diceRollerPlugin.getRoller("1d100");
const specialRoll = 60;//await percentageRoller.roll();
for (const special of EVENT_SPECIAL_ELEMENTS)
{
	if (specialRoll <= special[0])
	{
		rolledSpecial = special;
		break;
	}
}

// Check if it needs to reroll category
switch(rolledSpecial[1])
{
case "INCREASE":
case "DECREASE":
case "THIS IS BAD":
case "THIS IS GOOD":
	result.special = expand("eventcrafter pick category " + $1 + " " + $2 + " 1");
	break;
case "MULTI-ELEMENT":
	const special1 = expand("eventcrafter pick category " + $1 + " " + $2 + " 1");
	const special2 = expand("eventcrafter pick category " + $1 + " " + $2 + " 1");
	result.multiElement = { special1: special1, special2: special2 };
	break;
case "RANDOM ELEMENT":
	result.eventText = expand("eventcrafter meaning " + $2);
	break;
default:
	break;
}

result.name = "SPECIAL - " + rolledSpecial[1];
result.description = rolledSpecial[2];

return result;

```
__
hidden - Rolls a special event result.

__
```
^eventcrafter pick$
```
__
```js

if(_inlineScripts.state.sessionState.eventCrafter.activeSheet === "")
{
	return expFormat("No active sheet present.");
}

const activeSheet = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
const results = expand("eventcrafter pick " + activeSheet);

// Print table with results for each category
const spacedSheetName = GetSpacedName(activeSheet);
let printedResult = "Picking Sheet __" + spacedSheetName + "__";
for(const result of results)
{
	printedResult += "\n\n";
	
	let info = result.info;
	const category = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheet].categories[result.categoryName];
	const spacedCategoryName = GetSpacedName(result.categoryName);
	printedResult += "- Result for __" + spacedCategoryName + "__ (Progress Points " + category.pp + ", " + (category.randomized ? "Randomized" : "Structured") + ", Rolled: " + info.roll + "):\n";
	
	if(info.name !== "")
	{
		printedResult += "\t__" + info.name + "__:\n\t";
		if(info.description !== "")
		{
			printedResult += "\t" + info.description + "\n\t\n";
		}
	}
	
	if(info.special)
	{
		printedResult += "\tRerolled Category (Rolled " + info.special.roll + "):\n";
		printedResult += "\t\tEVENT " + info.special.name + "\n";
		printedResult += "\t\t" + info.special.description + "\n";
		printedResult += "\t\t" + info.special.eventText;
	}
	
	if(info.multiElement)
	{
		printedResult += "\tFirst Reroll (Rolled " + info.multiElement.special1.roll + ")\n";
		printedResult += "\t\t __" + info.multiElement.special1.name + "__: " + info.multiElement.special1.eventText + "  " + info.multiElement.special1.description + "\n";
		
		printedResult += "\tSecond Reroll (Rolled " + info.multiElement.special2.roll + ")\n";
		printedResult += "\t\t __" + info.multiElement.special2.name + "__: " + info.multiElement.special2.eventText + "  " + info.multiElement.special2.description;
	}
	
	if(info.eventText !== "")
	{
		printedResult += "\t" + info.eventText;
	}
	
}

return expFormat([printedResult]);

```
__
eventcrafter pick - Picks an element from each category in the active sheet.

__
```
^eventcrafter pick category ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*) (0|1)$
```
__
```js

// Data
const EVENT_ELEMENTS = [
[5, "None or Expected", "None or Expected", "None or Expected"],
[8, "Expected", "Expected", "Expected"],
[9, "Random", "Random", "Random", "random"],
[11, "1d10 List or Random", "1d10 List or Random", "1d10 List or Random", "1d10orRandom"],
[12, "None or Expected", "None or Expected", "Conclusion"],
[13, "Special", "Special", "Special", "special"],
[14, "1d10+PP List or Expected", "1d10+PP List or Expected", "1d10+PP List or Expected", "1d10plusPPorExpected"],
[15, "Expected", "Conclusion", "Conclusion"],
[99, "Expected, PP-6", "Expected, PP-6", "Expected, PP-6", "pp-6"],
];

const diceRollerPlugin = app.plugins.getPlugin("obsidian-dice-roller");
const d10Roller = await diceRollerPlugin.getRoller("1d10");
let result = { eventText: "", type: "", name: "", description: "", roll: -1 };

const category = _inlineScripts.state.sessionState.eventCrafter.sheets[$1].categories[$2];
let categoryResult = "";
if(category.randomized)
{
	// Roll on Event Elements Table
	const roll = await d10Roller.roll() + category.pp;
	let rolledEvent = [];
	for (const event of EVENT_ELEMENTS)
	{
		if (roll <= event[0])
		{
			rolledEvent = event;
			break;
		}
	}
	
	if(category.concluding === "nonconcluding")
	{
		result.eventText = rolledEvent[1];
	}
	else if(category.concluding === "concluding")
	{
		result.eventText = rolledEvent[2];
	}
	else // Short
	{
		result.eventText = rolledEvent[3];
	}
	
	// Check special event
	if(rolledEvent[4])
	{
		switch(rolledEvent[4])
		{
		case "1d10orRandom":
			result.type = "1d10orRandom";
			result.name = "1d10 or Random";
			if(category.content.length !== 0)
			{
				let categoryRoll = await d10Roller.roll() - 1;
				categoryRoll += GetUniquesOffset(category, categoryRoll);
				
				if(categoryRoll >= category.content.length)
				{
					// Rolled over
					categoryResult = "Rolled over category, choose one.";
				}
				else
				{
					let rolledElement = category.content[categoryRoll];
					categoryResult = rolledElement.name;
					CheckRolledElement(rolledElement, categoryRoll);
				}
				result.eventText = categoryResult;
				break;
			}
		case "random":
			if(result.type === "")
			{
				result.type = "random";
				result.name = "Random";
			}
			
			result.eventText = expand("eventcrafter meaning " + $2);
			break;
		case "special":
			result = expand("eventcrafter special " + $1 + " " + $2 + " " + $3);
			break;
		case "1d10plusPPorExpected":
			result.type = "1d10plusPPorExpected";
			result.name = "1d10+PP or Expected";
			if(category.content.length !== 0)
			{
				let categoryRoll = await d10Roller.roll() + category.pp - 1;
				categoryRoll += GetUniquesOffset(category, categoryRoll);
				if(categoryRoll >= category.content.length)
				{
					// Rolled over
					categoryResult = "Rolled over category, choose one.";
				}
				else
				{
					let rolledElement = category.content[categoryRoll];
					categoryResult = rolledElement.name;
					CheckRolledElement(rolledElement, categoryRoll);
				}
				result.eventText = categoryResult;
			}
			else
			{
				result.eventText = "Expected";
			}
			break;
		case "pp-6":
			result.type = "pp-6";
			result.eventText = "Expected";
			result.name = "PP-6";
			break;
		default:
			result.type = "std";
			break;
		}
	}
	else
	{
		result.type = "std";
	}
	
	// Update PP
	if(rolledEvent[4])
	{
		if(result.type == "pp-6")
		{
			category.pp = Math.max(0, category.pp - 6);
		}
		else if(result.type == "special")
		{
			if(result.name === "MOVING ALONG")
			{
				category.pp += 3;
			}
			else if(result.name === "ROLLING BACK")
			{
				category.pp = Math.max(0, category.pp - 2);
			}
		}
		else if($3 === "0")
		{
			// Update only if not coming from Multi Element Special
			category.pp++;
		}
	}
	else if($3 === "0")
	{
		// Update only if not coming from Multi Element Special
		category.pp++;
	}
	
	// No need to increase 1, already starting from 1 in this case
	result.roll = roll;
}
else // Structured
{
	// Roll on categories
	result.type = "std";
	if(category.content.length !== 0)
	{
		let categoryRoll = await d10Roller.roll() + category.pp - 1;
		categoryRoll += GetUniquesOffset(category, categoryRoll);
		
		if(categoryRoll >= category.content.length)
		{
			// Rolled over
			result.eventText = "Rolled over category, Expected.";
			if($3 === "0")
			{
				category.pp = Math.max(0, category.pp - 6);
			}
		}
		else
		{
			let rolledElement = category.content[categoryRoll];
			categoryResult = rolledElement.name;
			CheckRolledElement(rolledElement, categoryRoll);
			
			if(categoryResult === "Special")
			{
				result = expand("eventcrafter special " + $1 + " " + $2 + " " + $3);
			}
			else if(categoryResult === "Random")
			{
				result.type = "random";
				result.name = "Random";
				result.eventText = expand("eventcrafter meaning " + $2);
			}
			else
			{
				result.eventText = categoryResult;
			}
			
			if($2 === "0")
			{
				category.pp++;
			}
		}
		
		result.roll = categoryRoll + 1;
	}
}

return result;

```
__
hidden - Picks on category. Whether or not to roll special or treat it as Expected if it's the second

__
```
^eventcrafter pick ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedSheetName = GetSpacedName($1);
// Check sheet existance
if(!SheetExists($1))
{
	return expFormat(["Sheet __\"" + spacedSheetName + "\"__ not found.", errorText]);
}

const sheet = _inlineScripts.state.sessionState.eventCrafter.sheets[$1];
let results = [];
for (let [categoryNameKey, category] of Object.entries(sheet.categories))
{
	let result = expand("eventcrafter pick category " + $1 + " " + categoryNameKey + " 0");
	results.push({categoryName: categoryNameKey, info: result});
}

return results;

```
__
hidden - Picks an element from each category in the given sheet.

__
```
^eventcrafter sheet$
```
__
```js
if(_inlineScripts.state.sessionState.eventCrafter.activeSheet !== "")
{
	return expand("eventcrafter sheet " + _inlineScripts.state.sessionState.eventCrafter.activeSheet);
}
else
{
	return expFormat("No active sheet present.");
}

```
__
eventcrafter sheet - **Prints the active sheet.

__
```
^eventcrafter sheet ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedSheetName = GetSpacedName($1);
// Check sheet existance
if(!SheetExists($1))
{
	return expFormat(["Sheet __\"" + spacedSheetName + "\"__ not found.", errorText]);
}

let result = "Sheet __" + spacedSheetName + "__\n\n";
const categories = _inlineScripts.state.sessionState.eventCrafter.sheets[$1].categories;

// Header row
let sheetHeaders = "| Name | ";
const headerSeparator = ":---:";
let headerSeparatorRow = "| " + headerSeparator + " | ";

for (let [categoryNameKey, category] of Object.entries(categories))
{
	sheetHeaders += GetSpacedName(categoryNameKey) + " | ";
	headerSeparatorRow += headerSeparator + " | ";
}

result += sheetHeaders + "\n";
result += headerSeparatorRow + "\n";

// Find longest category and general data
let categoryMaxLength = 0;
let ppRow = "| ";
let typeRow = "| ";
let concludingRow = "| ";
let emptyRow = "| ";
for (let [categoryNameKey, category] of Object.entries(categories))
{
	categoryMaxLength = Math.max(categoryMaxLength, category.content.length);
	
	ppRow += category.pp + " | ";
	typeRow += (category.randomized ? "Randomized" : "Structured") + " | ";

	let concludingType = "";
	if(category.randomized)
	{
		if(category.concluding === "concluding")
		{
			concludingType = "Concluding";
		}
		else if(category.concluding === "nonconcluding")
		{
			concludingType = "Non-Concluding";
		}
		else
		{
			concludingType = "Concluding, Short";
		}
	}
	
	concludingRow += concludingType + " | ";
	
	emptyRow += " | ";
}
result += "| Progress Points " + ppRow + "\n";
result += "| Type " + typeRow + "\n";
result += "| Concluding Type " + concludingRow + "\n";
result += "| " + emptyRow + "\n";

let tableContent = "";
for(let i = 0; i < categoryMaxLength; ++i)
{
	// Print row
	let rowContent = "| " + (i + 1) + " | ";
	for (let [categoryNameKey, category] of Object.entries(categories))
	{
		if(i < category.content.length)
		{
			// Content available
			const crossed = category.content[i].unique === 2;
			rowContent += (crossed ? "~~" : "") + GetSpacedName(category.content[i].name);
			if(category.content[i].unique !== 0)
			{
				rowContent += " (U)";
			}
			rowContent += (crossed ? "~~" : "") + " | ";
		}
		else
		{
			// Content not available, skip to next category
			rowContent += " | ";
		}
	}
	tableContent += rowContent + "\n";
}
result += tableContent + "\n";
return result;

```
__
eventcrafter sheet {sheet name: name text} - Prints the given sheet.

__
```
^eventcrafter sheets$
```
__
```js

let result = "";
const sheets = _inlineScripts.state.sessionState.eventCrafter.sheets;
for (let sheet in sheets)
{
	if(sheet === _inlineScripts.state.sessionState.eventCrafter.activeSheet)
	{
		result += "__ACTIVE__\n";
	}
	result += expand("eventcrafter sheet " + sheet);
	result += "\n";
}

return result;

```
__
eventcrafter sheets - **Prints all sheets.

__
```
^eventcrafter add sheet ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const finalName = GetSpacedName($1);

if(SheetExists($1))
{
	return expFormat(["Sheet not created.  Name __\"" + finalName + "\"__ unavailable.", errorText]);
}

// Create list
_inlineScripts.state.sessionState.eventCrafter.sheets[$1] = { categories: {} };

return expFormat(["Sheet " + finalName + " added."]);

```
__
eventcrafter add sheet {sheet name: name text} - Create a new empty Event Crafter sheet with the given name. "\_\_" (double  \_) will be substituted with space.

__
```
^eventcrafter rename sheet ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedOldName = GetSpacedName($1);
const spacedNewName = GetSpacedName($2);

if(!SheetExists($1))
{
	return expFormat(["Name __\"" + spacedOldName + "\"__ not found.", errorText]);
}

if(SheetExists($2))
{
	return expFormat(["Name __\"" + spacedNewName + "\"__ already exists.", errorText]);
}

// Copy list
_inlineScripts.state.sessionState.eventCrafter.sheets[$2] = _inlineScripts.state.sessionState.eventCrafter.sheets[$1];

delete _inlineScripts.state.sessionState.eventCrafter.sheets[$1];

return expFormat(["Sheet " + spacedOldName + " renamed to " + spacedNewName]);

```
__
eventcrafter rename sheet {old sheet name: name text} {new sheet name: name text} - Renames a sheet with a new name. "\_\_" (double  \_) will be substituted with space.

__
```
^eventcrafter add category ([_a-zA-Z][_a-zA-Z0-9]*) (0|1|randomized|structured) ?(|nonconcluding|concluding|short)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";

// Check Sheet existance
const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
if(activeSheetName === "")
{
	return expFormat(["No active sheet present. Select one first.", errorText]);
}

const spacedCategoryName = GetSpacedName($1);

let randomizedValue = true;
if($2 === "1" ||
   $2 === "structured")
{
	randomizedValue = false;
}

if(randomizedValue === false && $2 === false)
{
	return expFormat(["Invalid parameters, structured category with randomized-only parameter.", errorText]);
}

if(CategoryExistsInActiveSheet($1))
{
	return expFormat(["This category name already exists in the active sheet.", errorText]);
}

let activeSheet = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName];

// Create category
activeSheet.categories[$1] = { content: [], pp: 0, randomized: randomizedValue, concluding: $3, uniqueIndicesRemoved: [] };

return expFormat(["Category " + spacedCategoryName + " added to Sheet " + activeSheetName + "."]);

```
__
eventcrafter add category {categoryName: category text} {structured: 0 OR randomized to have it randomized. 1 OR structured to make it structured} {type: nonconcluding OR concluding OR short} - Create a new empty Event Crafter category with the given name in the currently active sheet. If {structured} is 1 or "structured", the category will be structured. If anything else, it will be randomized. The type will indicate if the category is concluding, concluding, short or non-concluding (valid only with a randomized category). "\_\_" (double  \_) will be substituted with space.

__
```
^eventcrafter remove category ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

// Generic expansion messages
const SUCCESS_MSG = "__" + $2 + "__ removed from sheet __" + $1 + "__.";
const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedSheetName = GetSpacedName($1);
const spacedCategoryName = GetSpacedName($2);

// Error out if sheet doesn't exist in EC
if(!SheetExists($1))
{
	return expFormat(["Sheet " + spacedSheetName + " doesn't exist.", errorText]);
}

// Error out if category doesn't exist
if(!_inlineScripts.state.sessionState.eventCrafter.sheets[$1].categories[$2])
{
	return expFormat(["Category " + spacedCategoryName + " doesn't exist.", errorText]);
}

// Remove category
delete _inlineScripts.state.sessionState.eventCrafter.sheets[$1].categories[$2];

return expFormat([SUCCESS_MSG]);

```
__
eventcrafter remove category {sheet name: sheet text} {categoryName: category text} - Removes { categoryName } from the given sheet. 

__
```
^eventcrafter rename category ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedOldName = GetSpacedName($1);
const spacedNewName = GetSpacedName($2);

const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
if(activeSheetName === "")
{
	return expFormat(["No active sheet present. Select one first.", errorText]);
}

let activeSheet = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName];

// Copy list
activeSheet.categories[$2] = activeSheet.categories[$1];

delete activeSheet.categories[$1];

return expFormat(["Category " + spacedOldName + " renamed to " + spacedNewName]);

```
__
eventcrafter rename category {old category name: name text} {new category name: name text} - Renames a category with a new name in the active sheet. "\_\_" (double  \_) will be substituted with space.

__
```
^eventcrafter remove sheet ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const finalName = GetSpacedName($1);
// Error out if sheet doesn't exist
if(!SheetExists($1))
{
	return expFormat(["Sheet " + finalName + " doesn't exist.", errorText]);
}

// Remove sheet
delete _inlineScripts.state.sessionState.eventCrafter.sheets[$1];

return expFormat(["Sheet " + finalName + " removed from Event Crafter."]);

```
__
eventcrafter remove sheet {sheet name: sheet text} - Removes the given sheet and all linked categories.

__
```
^eventcrafter activate ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const finalName = GetSpacedName($1);

if(!SheetExists($1))
{
	return expFormat(["Sheet " + finalName + " doesn't exist.", errorText]);
}

// Activate list
_inlineScripts.state.sessionState.eventCrafter.activeSheet = $1;

return expFormat(["Sheet " + finalName + " activated."]);

```
__
eventcrafter activate {sheet name: name text} - Sets the given sheet as the active one for EventCrafter.

__
```
^eventcrafter add element ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*) ?(|U|Unique|unique)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedCategoryName = GetSpacedName($1);
const spacedElementName = GetSpacedName($2);

if(!CategoryExistsInActiveSheet($1))
{
	return expFormat(["Element not created.  Category __\"" + spacedCategoryName + "\"__ doesn't exist in active sheet " + _inlineScripts.state.sessionState.eventCrafter.activeSheet + ".", errorText]);
}

// Add element
const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
_inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName].categories[$1].content.push({name: $2, unique: $3 ? 1 : 0 }); // already used unique is 2

return expFormat(["Element " + spacedElementName + " added to Category " + spacedCategoryName + "."]);

```
__
eventcrafter add element {category name: category text} {item name: item text} {unique: U OR Unique OR unique, default: Not unique} - Adds a new element to the given category in the active sheet, choosing either unique or not. "\_\_" (double  \_) will be substituted with space.

__
```
^eventcrafter remove element ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedCategoryName = GetSpacedName($1);
const spacedElementName = GetSpacedName($2);

if(!CategoryExistsInActiveSheet($1))
{
	return expFormat(["Element not removed.  Category __\"" + spacedCategoryName + "\"__ doesn't exist in active sheet " + _inlineScripts.state.sessionState.eventCrafter.activeSheet + ".", errorText]);
}

// Remove last element
const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
const isCorrectElement = (element) => element.name === $2;
const elementIndex = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName].categories[$1].content.findLastIndex(isCorrectElement);
if(elementIndex !== -1)
{	_inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName].categories[$1].content.splice(elementIndex, 1);
}
else
{
	return expFormat(["Element not found in Category __\"" + spacedCategoryName + "\"__ for active sheet " + _inlineScripts.state.sessionState.eventCrafter.activeSheet + ".", errorText]);
}

return expFormat(["Element " + spacedElementName + " removed from Category " + spacedCategoryName + "."]);

```
__
eventcrafter remove element {category name: category text} {item name: item text} - Removes the given element from the given category in the active sheet. If there are duplicates, removes the last in the list.

__
```
^eventcrafter rename element ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*) ([_a-zA-Z][_a-zA-Z0-9]*)$
```
__
```js

const errorText = expansionInfo.isUserTriggered ? "" : "EC_ERROR";
const spacedOldName = GetSpacedName($1);
const spacedNewName = GetSpacedName($2);
const spacedCategoryName = GetSpacedName($3);

const activeSheetName = _inlineScripts.state.sessionState.eventCrafter.activeSheet;
if(activeSheetName === "")
{
	return expFormat(["No active sheet present. Select one first.", errorText]);
}

if(!CategoryExistsInActiveSheet($3))
{
	return expFormat(["Element not renamed.  Category __\"" + spacedCategoryName + "\"__ doesn't exist in active sheet " + _inlineScripts.state.sessionState.eventCrafter.activeSheet + ".", errorText]);
}

let activeSheet = _inlineScripts.state.sessionState.eventCrafter.sheets[activeSheetName];

const renameElement = (element, index) => 
{
	if(element.name === $1)
	{
		element.name = $2;
	}
};
activeSheet.categories[$3].content.forEach(renameElement);

return expFormat(["All elements " + spacedOldName + " renamed to " + spacedNewName]);

```
__
eventcrafter rename element {old element name: name text} {new element name: name text} {category name: category text} - Renames all elements with a new name in the given category, in the active sheet. "\_\_" (double  \_) will be substituted with space.