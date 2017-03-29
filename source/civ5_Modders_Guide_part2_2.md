#### How to: Add a Civilization

In this section we will go through the entire process of adding a new civilization to the game. First let's look at the schema definition for a civilization from the Civ5Civilizations.xml file:

```xml
<Table name="Civilizations">
    <Column name="ID" type="integer" primarykey="true" autoincrement="true"/>
    <Column name="Type" type="text" notnull="true" unique="true"/>
    <Column name="Description" type="text"/>
    <Column name="Civilopedia" type="text"/>
    <Column name="CivilopediaTag" type="text"/>
    <Column name="Strategy" type="text" default="Ask Paul"/>
    <Column name="Playable" type="boolean" default="true"/>
    <Column name="AIPlayable" type="boolean" default="true"/>
    <Column name="ShortDescription" type="text" default="NULL"/>
    <Column name="Adjective" type="text" default="NULL"/>
    <Column name="DefaultPlayerColor" type="text" default="NULL"/>
    <Column name="ArtDefineTag" type="text" default="NULL"/>
    <Column name="ArtStyleType" type="text" default="NULL"/>
    <Column name="ArtStyleSuffix" type="text" default="NULL"/>
    <Column name="ArtStylePrefix" type="text" default="NULL"/>
    <Column name="DerivativeCiv" type="text" default="NULL"/>
    <Column name="PortraitIndex" type="integer" default="-1"/>
    <Column name="IconAtlas" type="text" default="NULL" reference="IconTextureAtlases(Atlas)"/>
    <Column name="AlphaIconAtlas" type="text" default="NULL" reference="IconTextureAtlases(Atlas)"/>
    <Column name="MapImage" type="text" default="NULL"/>
    <Column name="DawnOfManQuote" type="text" default="NULL"/>
    <Column name="DawnOfManImage" type="text" default="NULL"/>
    <Column name="DawnOfManAudio" type="text" default="NULL"/>
</Table>
<Table name="Civilization_BuildingClassOverrides">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="BuildingClassType" type="text" notnull="true" reference="BuildingClasses(Type)"/>
    <Column name="BuildingType" type="text" reference="Buildings(Type)"/>
</Table>
<Table name="Civilization_CityNames">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="CityName" type="text" notnull="true"/>
</Table>
<Table name="Civilization_DisableTechs">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="TechType" type="text" reference="Technologies(Type)"/>
</Table>
<Table name="Civilization_FreeBuildingClasses">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="BuildingClassType" type="text" reference="BuildingClasses(Type)"/>
</Table>
<Table name="Civilization_FreeTechs">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="TechType" type="text" reference="Technologies(Type)"/>
</Table>
<Table name="Civilization_FreeUnits">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="UnitClassType" type="text" reference="UnitClasses(Type)"/>
    <Column name="UnitAIType" type="text" reference="UnitAIInfos(Type)"/>
    <Column name="Count" type="integer"/>
</Table>
<Table name="Civilization_Leaders">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="LeaderheadType" type="text" reference="Leaders(Type)"/>
</Table>
<Table name="Civilization_UnitClassOverrides">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="UnitClassType" type="text" notnull="true" reference="UnitClasses(Type)"/>
    <Column name="UnitType" type="text" reference="Units(Type)"/>
</Table>
<Table name="Civilization_Start_Along_Ocean">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="StartAlongOcean" type="boolean" default="false"/>
</Table>
<Table name="Civilization_Start_Along_River">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="StartAlongRiver" type="boolean" default="false"/>
</Table>
<Table name="Civilization_Start_Region_Priority">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="RegionType" type="text" reference="Regions(Type)"/>
</Table>
<Table name="Civilization_Start_Region_Avoid">
    <Column name="CivilizationType" type="text" reference="Civilizations(Type)"/>
    <Column name="RegionType" type="text" reference="Regions(Type)"/>
</Table>
```

Let's look at each of these values in detail:

- **ID** - This is the database starter row. It is set as 0 on the first entry but shouldn't be modified.

- **Type** - This is the key we will use to reference this civilization. We typically use `CIVILIZATION_<name>` (so CIVILIZATION_CANADA for Canada). We keep the object type as the first part of the name so that it doesn't get confused with another object type with the same name, and is easy to read and identify. Types have to be unique (you can't have two CIVILIZATION_CANADA's). If two objects with the same Type are loaded the one that is loaded second will replace the first one.

- **Description** - This is the text string description of the civ, for America it is "American Empire".

- **Civilopedia** - This isn't used in Civ5.

- **CivilopediaTag** - The starting text string for the pedia entry.

- **Strategy** - This isn't used in Civ5 (though Firaxis was having some fun with their default setting).

- **Playable** - If true then humans can select this as an available civilization. If false then it won't be available. Note that the default is true, so civilizations will be playable unless the modmaker specifically disables them.

- **AIPlayable** - If this is true then the AI can pick this civilization when randomly picking a civ when the game begins. If it is set to false then the AI will never pick this civilization. As with Playable this default to true unless the modder changes it. Setting both the Playable and AIPlayable attribute to false for a civilization is a good way to remove a civilization from the game without actually deleting the assets (so you won't break other mods that refer to it).

- **ShortDescription** - The short text string for this civilization. For America it is "America".

- **Adjective**- The text string for the qualifier to things that belong to this civilization. For Ameican it is "American", as in an American Spearman.

- **DefaultPlayerColor** - The default color of this civilizations borders, etc. This entry has to be specified in the CIV5PlayerColors.xml file. It is only the default because if the same civilization is in a game twice a random color will be assigned to the second instance (so players can tell them apart).

- **ArtDefineTag** - This isn't used in Civ5.

- **ArtStyleType** - Defines the art style for the buildings that are used in this Civilization's cities.

- **ArtStyleSuffix** - Used to pick different flavors of improvements and wonder art.

- **ArtStylePrefix** - Used to pick different flavors of improvements and wonder art.

- **DervativeCiv** - This isn't used in Civ5.

- **PortraitIndex** - The number of the icon in the icon atlas that is used for this Civilization.

- **IconAtlas** - The icon atlas that holds the icon for this civilization.

- **AlphaIconAtlas** - The icon atlas that has the alpha layer for this icon.

- **MapImage** - The picture (as a dds file) that is displayed when this civ is selected from the civilization selection menu. Typically this is a map of the Civilization.

- **DawnOfManQuote**- The text that is displayed on the Dawn of Man (ie: loading) screen.

- **DawnOfManImage** - The picture (as a dds file) that is show on the dawn on man screen.

- **DawnOfManAudio** - The audio file that is played on the Dawn of Man screen, typically this is a reading of the Dawn of Man quote.

- **Civilization_BuildingClassOverrides** - This is how unique buildings are implemented for a civilization. This can be used to block a civilization from being able to build a building such as this change which keeps minor civilizations from being able to build the Sydney Opera House:

```xml
<Row>
    <CivilizationType>CIVILIZATION_MINOR</CivilizationType>
    <BuildingClassType>BUILDINGCLASS_SYDNEY_OPERA_HOUSE</BuildingClassType>
    <BuildingType/>
</Row>
```

Or this change which switched the Castle to the Mughal fort for India:

```xml
<Row>
    <CivilizationType>CIVILIZATION_INDIA</CivilizationType>
    <BuildingClassType>BUILDINGCLASS_CASTLE</BuildingClassType>
    <BuildingType>BUILDING_MUGHAL_FORT</BuildingType>
</Row>
```

- **Civilization_CityNames**- This is a list of the available city names for a civilization.
- **Civilization_DisableTechs**- Any techs set here for a specific civilization won't be available for that civilization to research.
- **Civilization_FreeBuildingClasses**- This is the free buildings available to a civilization when they found their first city. By default all civilizations get a free palace.
- **Civilization_FreeTechs**- These are the free techs a civilization starts with.
- **Civilization_FreeUnits**- These are the units the civilization starts with. By default all civilizations are set to start with a free settler.
- **Civilization_Leaders**-This is where leaders are associated to their civilizations. In Civ5 each civilization can only have 1 leader.
- **Civilization_UnitClassOverrides**- Much like the building class overrides this is where unique units are set with the unit they replace for this civilization.
- **Civilization_Start_Along_Ocean**- If a civilization has this set the game will attempt to start them along a coastal tile. This defaults to false in schema, so it doesn't need to be set unless the modder is setting it to true.
- **Civilization_Start_Along_River**- If a civilization has this set the game will attempt to start them along a coastal tile. This defaults to false in schema, so it doesn't need to be set unless the modder is setting it to true.
- **Civilization_Start_Region_Priority**- If a civilization has this set the game will attempt to start them in the specified region. For example Arabia has Desert set for this.
- **Civilization_Start_Region_Avoid**- If a civilization has this set the game will attempt to avoid these regions when deciding the civilizations starting location. For example Egypt is set to avoid starts in Jungles.

Unfortunately there is no complete reference for what all the attributes for all the objects types in civilization. But as you can see from the above schema most are fairly easy to figure out. You can also look for examples in the XML definitions to give hints. If you were, for example. wondering what Civilization_FreeUnits is it's helpful to see that every civilization has one for the settler.

Now that we understand the attributes that are required for a civilization we are ready to add a new civilization to the game.

1. Create the file structure. The filename and file structure doesn't matter. But I prefer to mirror Firaxis's XML folders for consistency. Create an XML folder from the root of the project, and a Civilizations folder beneath it. Within the Civilizations folder create a new file called Civ_Celt.xml.<br /><br />
I prefer to have each civilization have their own file to make it easier for me to find data and make changes. But it's just as viable to create a single Civilizations file as Firaxis did, or to put all your xml objects in a single file.

2. Fill in the Civ_Celt.xml file with the following information:

```xml
<GameData>
    <Civilizations>
        <Row>
            <Type>CIVILIZATION_CELT</Type>
            <Description>TXT_KEY_CIV_CELT_DESC</Description>
            <ShortDescription>TXT_KEY_CIV_CELT_SHORT_DESC</ShortDescription>
            <Adjective>TXT_KEY_CIV_CELT_ADJECTIVE</Adjective>
            <Civilopedia>TXT_KEY_CIV_CELT_PEDIA</Civilopedia>
            <CivilopediaTag>TXT_KEY_CIV5_CELT</CivilopediaTag>
            <DefaultPlayerColor>PLAYERCOLOR_DARK_GREEN</DefaultPlayerColor>
            <ArtDefineTag>ART_DEF_CIVILIZATION_ENGLAND</ArtDefineTag>
            <ArtStyleType>ARTSTYLE_EUROPEAN</ArtStyleType>
            <ArtStyleSuffix>_EURO</ArtStyleSuffix>
            <ArtStylePrefix>EUROPEAN </ArtStylePrefix>
            <PortraitIndex>6</PortraitIndex>
            <IconAtlas>CIV_COLOR_ATLAS</IconAtlas>
            <AlphaIconAtlas>CIV_ALPHA_ATLAS</AlphaIconAtlas>
            <MapImage>MapEngland512.dds</MapImage>
            <DawnOfManQuote>TXT_KEY_CIV5_CELT_TEXT_1</DawnOfManQuote>
            <DawnOfManImage>DOM_Elizabeth.dds</DawnOfManImage>
        </Row>
    </Civilizations>
    <Civilization_CityNames>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <CityName>TXT_KEY_CITY_NAME_BIBRACTE</CityName>
        </Row>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <CityName>TXT_KEY_CITY_NAME_VIENNE</CityName>
        </Row>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <CityName>TXT_KEY_CITY_NAME_TOLOSA</CityName>
        </Row>
    </Civilization_CityNames>
    <Civilization_FreeBuildingClasses>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <BuildingClassType>BUILDINGCLASS_PALACE</BuildingClassType>
        </Row>
    </Civilization_FreeBuildingClasses>
    <Civilization_FreeTechs>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <TechType>TECH_AGRICULTURE</TechType>
        </Row>
    </Civilization_FreeTechs>
    <Civilization_FreeUnits>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <UnitClassType>UNITCLASS_SETTLER</UnitClassType>
            <Count>1</Count>
            <UnitAIType>UNITAI_SETTLE</UnitAIType>
        </Row>
    </Civilization_FreeUnits>
    <Civilization_Leaders>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <LeaderheadType>LEADER_ELIZABETH</LeaderheadType>
        </Row>
    </Civilization_Leaders>
    <Civilization_Start_Region_Priority>
        <Row>
            <CivilizationType>CIVILIZATION_CELT</CivilizationType>
            <RegionType>REGION_FOREST</RegionType>
        </Row>
    </Civilization_Start_Region_Priority>
</GameData>
```

The above is the definition for our new Celtic civilization. Because we haven't created a new leader yet I'm using Elizabeth as a leader. We haven't created a unique buildings (Civilization_BuildingClassOverrides) or unique units (Civilization_UnitClassOverrides) so they aren't set. I did set the Celt's to prefer start positions in forested regions. I selected PLAYERCOLOR_DARK_GREEN as the default player color for the civilization, you can find the full list of available player colors in CIV5PlayerColors.xml (which is also where you would add new ones).

I only used 3 city names in the above example just to simplify this document. For a real civilization we would want more than three city names.

3. Define the text strings. We used a lot of text strings in the civ definition. We will have to define those in XML as well. Create a New Text directory under XML and add a new file under it. I have called mine GameText.xml.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page31.jpg)

The pedia entries use a special format. The prefix for the pedia entries is whatever we put in the CivilopediaTag attribute (TXT_KEY_CIV5_CELT). But the pedia is built form matching pairs of _HEADING_# and _TEXT_# entries. If we add a text entry for TXT_KEY_CIV5_CELT_HEADING_1, then that will be the heading for the for section of the pedia, TXT_KEY_CIV5_CELT_TEXT_1 will be the pedia entry under that heading. This way modders can add as many pedia entries as they want for a civilization (the same system is used for leaders).

Being lazy, I also used TXT_KEY_CIV5_CELT_TEXT_1 as the Dawn of Man quote.

4. Add the following to the text file:

```xml
<GameData>
    <Language_en_US>
        <Row Tag="TXT_KEY_CITY_NAME_BIBRACTE">
            <Text>Bibracte</Text>
        </Row>
        <Row Tag="TXT_KEY_CITY_NAME_VIENNE">
            <Text>Vienne</Text>
        </Row>
        <Row Tag="TXT_KEY_CITY_NAME_TOLOSA">
            <Text>Tolosa</Text>
        </Row>
        <Row Tag="TXT_KEY_CIV_CELT_ADJECTIVE">
            <Text>Celtic</Text>
        </Row>
        <Row Tag="TXT_KEY_CIV_CELT_DESC">
            <Text>Celtic Empire</Text>
        </Row>
        <Row Tag="TXT_KEY_CIV_CELT_SHORT_DESC">
            <Text>Celtia</Text>
        </Row>
        <Row Tag="TXT_KEY_CIV5_CELT_HEADING_1">
            <Text>History</Text>
        </Row>
        <Row Tag="TXT_KEY_CIV5_CELT_TEXT_1">
            <Text>The Celts were a diverse group of tribal societies in Iron Age and Roman-era Europe who spoke Celtic languages.[NEWLINE][NEWLINE]The earliest archaeological culture commonly accepted as Celtic, or rather Proto-Celtic, was the central European Hallstatt culture (ca. 800-450 BC), named for the rich grave finds in Hallstatt, Austria. By the later La Tène period (ca. 450 BC up to the Roman conquest), this Celtic culture had expanded over a wide range of regions, whether by diffusion or migration: to the British Isles (Insular Celts), the Iberian Peninsula (Celtiberians, Celtici ), much of Central Europe, (Gauls) and following the Gallic invasion of the Balkans in 279 BC as far east as central Anatolia (Galatians).[NEWLINE][NEWLINE]The earliest directly attested examples of a Celtic language are the Lepontic inscriptions, beginning from the 6th century BC. Continental Celtic languages are attested only in inscriptions and place-names. Insular Celtic is attested from about the 4th century AD in ogham inscriptions, although it is clearly much earlier. Literary tradition begins with Old Irish from about the 8th century. Coherent texts of Early Irish literature, such as the Táin Bó Cúailnge (The Cattle Raid of Cooley), survive in 12th-century recensions. According to the theory of John T. Koch and others, the Tartessian language may have been the earliest directly attested Celtic language with the Tartessian written script used in the inscriptions based on a version of a Phoenician script in use around 825 BC.[NEWLINE][NEWLINE]By the early 1st millennium AD, following the expansion of the Roman Empire and the Great Migrations (Migration Period) of Germanic peoples, Celtic culture had become restricted to the British Isles (Insular Celtic), and the Continental Celtic languages ceased to be widely used by the 6th century.</Text>
        </Row>
    </Language_en_US>
</GameData>           
```

5. Lastly we have to make sure our modified files update the database. On the Actions tab on the mod properties we have to add the following entries to get the game to convert our xml files to sql and write them to the game database when the mod is loaded. This is one of the few places where the file path is important, if we change our directory or file names we will have to update the entry here on the Actions tab.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page32.jpg)

After that we have a new civilization in the game. We could use some art assets to make it look better, a new leader to go with it, and a unique unit and building. All of those will be covered in later sections.

#### How to: Add an Icon

Now we need an icon for our civilization.

Firaxis has helped us out by providing icon templates in the `<SDK install directory>\Art\` directory as .psd files. There are seven templates files, one for 32x32 icons, one for 45x45, 64x64, 80x80, 128x128, 214x214 and 256x256.

Firaxis also provided a readme for icon sizes that are required for all the asset types.

|  |  |
| :--------------- |  :-------------------  |
|  Promotions       |  256,64, 45, 32           | 
|  Buildings        |  256,128, 64, 45          | 
|  Citizens         |  256,128, 64, 45, 32      | 
|  Civilizations    |  256,128, 80, 64, 45, 32  |
|  Difficulties     |  128,64, 32               |
|  GameSpeeds       |  128,64, 32               | 
|  Leaders          |  256,128, 64              |   
|  Natural Wonders  |  256,128, 80, 64, 45      |     
|  Policies         |  256,64                   |
|  Resources        |  256,80, 64, 45           |    
|  Technologies     |  256,214, 128, 80, 64, 45 |    
|  Terrain          |  256,64                   |
|  Unit Actions     |  64, 45                   |
|  Units            |  256,128, 80, 64, 45      |    
|  Unit Flags       |  32                       |
|  World Sizes      |  128,64, 32               |
|  World Types      |  128,64, 32               | 
| | |

The above means that civilizations, for example, need a 256x256, 128x128, 80x80, 64x64, 45x45 and 32x32 icon. So we need to create six icons for different sizes for our civilization. Loading the IconAtlas256.psd I can use Photoshop to create an icon in the first slot.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page33.jpg)

I prefer Photoshop, but many modders like to use Gimp, which has the considerable advantage of being free. The art tool doesn't matter, as long as it can read the .psd template and save the file as a .dds file.

I prefer to create the largest icon size first, since we will shrink this file to save the smaller versions. It's also important that our icon be clear and distinctive, not only at the 256x256 size, but at the 32x32 size. So avoid designs that are to complex (the celtic knot design I used is probably too complex to look that good at 32x32, but it will do).

There is also an alpha layer in the psd file that defaults to the same round circles you see in the rest of the icon spaces above. The alpha layer controls what is displayed, what is white in the alpha layer is shown as part of the icon, what is black isn't. It is how the game knows what the edges of the icon are.

All the base game civilization icons are simple circles, but I got a little fancy with mine and had it extend outside of the circle, so I had to adjust the alpha layer to match.

Once you have the icon created in the template, save it as a dds file. You may need to download special plugins for your art tool of choice to be able to save dds files. I selected to call my file CivSymbolsColorLegends256.dds.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page34.jpg)

The 256x256 template is 2048x2048 pixels (eight 256 width icons across, eight 256 height icons from top to bottom). If we resize our image from 2048x2048 to 1024x1024 this will reduce our icons to 128x128, then we can save CivSymbolsColorLegends128.dds, resize to 640x640 (80 x 8) and save CivSymbolsColorLegends80.dds. And on to create a 64x64, 45x45 and 32x32 icon size dds file.

Once that is done we can add the files to our mod by creating an Art folder (though this isn't nessesary, I create it to help organize the project) and dragging and dropping our files into it.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page34-2.jpg)

Once all of our art files our added to the project we need to be able to reference them. to do that add a new asset type we need to add, IconTextureAtlases. Add the GameInfo folder underneath the XML folder and add an XML file called CIV5IconTextureAtlases.xml that contains the following:

```xml
<GameData>
    <IconTextureAtlases>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>256</IconSize>
            <Filename>CivSymbolsColorLegends256.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>128</IconSize>
            <Filename>CivSymbolsColorLegends128.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>80</IconSize>
            <Filename>CivSymbolsColorLegends80.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>64</IconSize>
            <Filename>CivSymbolsColorLegends64.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>45</IconSize>
            <Filename>CivSymbolsColorLegends45.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
        <Row>
            <Atlas>CIV_COLOR_ATLAS_LEGENDS</Atlas>
            <IconSize>32</IconSize>
            <Filename>CivSymbolsColorLegends32.dds</Filename>
            <IconsPerRow>8</IconsPerRow>
            <IconsPerColumn>8</IconsPerColumn>
        </Row>
    </IconTextureAtlases>
</GameData>
```

The above defines the dds files we added. The important part for each is that the Icon size specifies what size icons it holds, and the IconsPerRow and IconsPerColumm tells it how the icons are laid out. Also note that all the Atlases have the same name, what distinguishes them is what size icon the game needs. So that if we tell the game to get the 12th icon in the CIV_COLOR_ATLAS_LEGENDS it will already know that it needs it for a 128x128 size display, so it will look in CivSymbolsColorLegends128.dds and since it knows that the icons are in a 8x8 grid it knows the 12th icon is 3rd icon in the 2nd row (counting starts at 0).

Lastly we need to modify our civilization definition to use our new icon. back in our Celt.xml file we need to make the changes marked in blue:

```xml
<GameData>
    <Civilizations>
        <Row>
            <Type>CIVILIZATION_CELT</Type>
            <Description>TXT_KEY_CIV_CELT_DESC</Description>
            <ShortDescription>TXT_KEY_CIV_CELT_SHORT_DESC</ShortDescription>
            <Adjective>TXT_KEY_CIV_CELT_ADJECTIVE</Adjective>
            <Civilopedia>TXT_KEY_CIV_CELT_PEDIA</Civilopedia>
            <CivilopediaTag>TXT_KEY_CIV5_CELT</CivilopediaTag>
            <DefaultPlayerColor>PLAYERCOLOR_DARK_GREEN</DefaultPlayerColor>
            <ArtDefineTag>ART_DEF_CIVILIZATION_ENGLAND</ArtDefineTag>
            <ArtStyleType>ARTSTYLE_EUROPEAN</ArtStyleType>
            <ArtStyleSuffix>_EURO</ArtStyleSuffix>
            <ArtStylePrefix>EUROPEAN </ArtStylePrefix>
            <blue><PortraitIndex>0</PortraitIndex></blue>
            <blue><IconAtlas>CIV_COLOR_ATLAS_LEGENDS</IconAtlas></blue>
            <AlphaIconAtlas>CIV_ALPHA_ATLAS</AlphaIconAtlas>
            <MapImage>MapEngland512.dds</MapImage>
            <DawnOfManQuote>TXT_KEY_CIV5_DAWN_CELT_TEXT</DawnOfManQuote>
            <DawnOfManImage>DOM_Elizabeth.dds</DawnOfManImage>
            <DawnOfManAudio/>
        </Row>
...etc
```

The above tells the civilization to use the new atlas we defined and use Icon 0 (the first icon in that atlas) the civ icon. Loading up the mod we can take a look at our new icon.

![](https://github.com/GitFuture/MyTranslation/blob/master/translated/civ5_imgs/page36.jpg)
