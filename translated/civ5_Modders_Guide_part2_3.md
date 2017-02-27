#### 如何添加首领

添加首领和添加文明差不多。我复制了 Alexander 的所有属性，以便于创造出和 Alexander 性格相近的 Boudica 。然后我替换了 Elizabeth 的关于 ArtDefineTag （首领页面的图画）和 PortraitIndex （她所使用的图标）的图画设定。最后我替换了 Hiawatha 无视森林地形的设定。在后几节中我们会学习如何为 Boudica 创造新的图像和特性。

这是 Boudica 的首领定义。我创建了在相应目录下叫做 `XML/Leaders/CIV5Leader_Bouidica.xml` 的新文件，在模组属性的 actions 标签页设置该文件要更改数据库。

```xml
<GameData>
    <Leaders>
        <Row>
            <Type>LEADER_BOUDICA</Type>
            <Description>TXT_KEY_LEADER_BOUDICA</Description>
            <Civilopedia>TXT_KEY_LEADER_BOUDICA_PEDIA</Civilopedia>
            <CivilopediaTag>TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA</CivilopediaTag>
            <ArtDefineTag>Elizabeth_Scene.xml</ArtDefineTag>
            <VictoryCompetitiveness>8</VictoryCompetitiveness>
            <WonderCompetitiveness>7</WonderCompetitiveness>
            <MinorCivCompetitiveness>3</MinorCivCompetitiveness>
            <Boldness>8</Boldness>
            <DiploBalance>3</DiploBalance>
            <WarmongerHate>2</WarmongerHate>
            <WorkAgainstWillingness>7</WorkAgainstWillingness>
            <WorkWithWillingness>4</WorkWithWillingness>
            <PortraitIndex>6</PortraitIndex>
            <IconAtlas>LEADER_ATLAS</IconAtlas>
        </Row>
            </Leaders>
            <Leader_MajorCivApproachBiases>
            <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_WAR</MajorCivApproachType>
            <Bias>7</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_HOSTILE</MajorCivApproachType>
            <Bias>7</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_DECEPTIVE</MajorCivApproachType>
            <Bias>4</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_GUARDED</MajorCivApproachType>
            <Bias>5</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_AFRAID</MajorCivApproachType>
            <Bias>3</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_FRIENDLY</MajorCivApproachType>
            <Bias>5</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MajorCivApproachType>MAJOR_CIV_APPROACH_NEUTRAL</MajorCivApproachType>
            <Bias>4</Bias>
        </Row>
    </Leader_MajorCivApproachBiases>
    <Leader_MinorCivApproachBiases>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MinorCivApproachType>MINOR_CIV_APPROACH_IGNORE</MinorCivApproachType>
            <Bias>4</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MinorCivApproachType>MINOR_CIV_APPROACH_FRIENDLY</MinorCivApproachType>
            <Bias>5</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MinorCivApproachType>MINOR_CIV_APPROACH_PROTECTIVE</MinorCivApproachType>
            <Bias>3</Bias>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <MinorCivApproachType>MINOR_CIV_APPROACH_CONQUEST</MinorCivApproachType>
            <Bias>8</Bias>
        </Row>
    </Leader_MinorCivApproachBiases>
    <Leader_Flavors>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_OFFENSE</FlavorType>
            <Flavor>8</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_DEFENSE</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_CITY_DEFENSE</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_MILITARY_TRAINING</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_RECON</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_RANGED</FlavorType>
            <Flavor>3</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_MOBILE</FlavorType>
            <Flavor>8</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_NAVAL</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_NAVAL_RECON</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_NAVAL_GROWTH</FlavorType>
            <Flavor>6</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_NAVAL_TILE_IMPROVEMENT</FlavorType>
            <Flavor>6</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_AIR</FlavorType>
            <Flavor>3</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_EXPANSION</FlavorType>
            <Flavor>8</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_GROWTH</FlavorType>
            <Flavor>4</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_TILE_IMPROVEMENT</FlavorType>
            <Flavor>4</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_INFRASTRUCTURE</FlavorType>
            <Flavor>4</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_PRODUCTION</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_GOLD</FlavorType>
            <Flavor>3</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_SCIENCE</FlavorType>
            <Flavor>6</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_CULTURE</FlavorType>
            <Flavor>7</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_HAPPINESS</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_GREAT_PEOPLE</FlavorType>
            <Flavor>6</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_WONDER</FlavorType>
            <Flavor>7</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_RELIGION</FlavorType>
            <Flavor>5</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_DIPLOMACY</FlavorType>
            <Flavor>7</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_SPACESHIP</FlavorType>
            <Flavor>8</Flavor>
        </Row>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <FlavorType>FLAVOR_WATER_CONNECTION</FlavorType>
            <Flavor>6</Flavor>
        </Row>
    </Leader_Flavors>
    <Leader_Traits>
        <Row>
            <LeaderType>LEADER_BOUDICA</LeaderType>
            <TraitType>TRAIT_IGNORE_TERRAIN_IN_FOREST</TraitType>
        </Row>
    </Leader_Traits>
</GameData>
```

在 Boudica 成为一个完整的首领前我们还需要两个东西。首先我们需要把 Celt 文明的首领修改成 Boudica。我们需要修改 “Celt.xml” 文件，把 LEADER_ELIZABETH 修改成 LEADER_BOUDICA。

```xml
<Civilization_Leaders>
    <Row>
        <CivilizationType>CIVILIZATION_CELT</CivilizationType>
        <LeaderheadType>LEADER_BOUDICA</LeaderheadType>
    </Row>
</Civilization_Leaders>
```

其次我们需要向 “GameText.xml” 文件添加以下的文本，作为新首领的描述：

```xml
    <Row Tag="TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA_HEADING_1">
        <Text>History</Text>
    </Row>
    <Row Tag="TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA_TEXT_1">
        <Text>Boudica, formerly known as Boadicea and known in Welsh as "Buddug" was queen of a Celtic tribe who led an uprising against the occupying forces of the Roman Empire.[NEWLINE][NEWLINE]Boudica's husband Prasutagus, ruler of the Iceni tribe who had ruled as a nominally independent ally of Rome, left his kingdom jointly to his daughters and the Roman Emperor in his will. However, when he died his will was ignored. The kingdom was annexed as if conquered, Boudica was flogged and her daughters raped, and Roman financiers called in their loans.[NEWLINE][NEWLINE]In AD 60 or 61, while the Roman governor, Gaius Suetonius Paulinus, was leading a campaign on the island of Anglesey in north Wales, Boudica led the Iceni people, along with the Trinovantes and others, in revolt. They destroyed Camulodunum (modern Colchester), formerly the capital of the Trinovantes, but now a colonia (a settlement for discharged Roman soldiers) and the site of a temple to the former emperor Claudius, which was built and maintained at local expense. They also routed a Roman legion, the IX Hispana, sent to relieve the settlement.[NEWLINE][NEWLINE]On hearing the news of the revolt, Suetonius hurried to Londinium (London), the twenty-year-old commercial settlement that was the rebels' next target. Concluding he did not have the numbers to defend it, Suetonius evacuated and abandoned it. It was burnt to the ground, as was Verulamium (St Albans). An estimated 70,000–80,000 people were killed in the three cities. Suetonius, meanwhile, regrouped his forces in the West Midlands, and despite being heavily outnumbered, defeated the Britons in the Battle of Watling Street. The crisis caused the emperor Nero to consider withdrawing all Roman forces from the island, but Suetonius' eventual victory over Boudica secured Roman control of the province. Boudica then killed herself so she would not be captured, or fell ill and died; Tacitus and Dio differ.[NEWLINE][NEWLINE]The history of these events, as recorded by Tacitus and Cassius Dio, was rediscovered during the Renaissance and led to a resurgence of Boudica's legendary fame during the Victorian era, when Queen Victoria was portrayed as her 'namesake'. Boudica has since remained an important cultural symbol in the United Kingdom. The absence of native British literature during the early part of the first millennium means that Britain owes its knowledge of Boudica's rebellion to the writings of the Romans.</Text>
    </Row>
    <Row Tag="TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA_LIVED">
        <Text>25 - 62 AD</Text>
    </Row>
    <Row Tag="TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA_NAME">
        <Text>Boudica</Text>
    </Row>
    <Row Tag="TXT_KEY_CIVILOPEDIA_LEADERS_BOUDICA_SUBTITLE">
        <Text>Leader of the Celts</Text>
    </Row>
    <Row Tag="TXT_KEY_LEADER_BOUDICA">
        <Text>Boudica</Text>
    </Row>
```

注意，就像文明的标签一样，CivilopediaTag 属性是某些文本的前缀。_NAME 和 _SUBTITLE 对于首领很特殊，他们被展示在百科页面的顶部。就像文明的 _HEADING_1 和 _TEXT_1 是用来创建百科页面的主体部分。还有一个特殊的标签，_LIVED，它包含了首领的生卒年份。

最后，我要换一下 Celt 文明的 Dawn of Man （游戏加载画面）图片，让它加载 Boudica 的一幅图片，而不是现在的 Elizabeth 的图片。当我想创造 Celtic 文明图标来展示我的艺术才能时，我不得不再找一幅 Boudica 的图片。幸运的是 Boudica 在 《文明 4》中有著作：《Warlords expansion》，我认为 Firaxis 不会介意我拿它当作《文明 5》的模组。

Dawn of Man 图片的尺寸是 1024x768，而且得是 DDS 格式的文件。所有图片都可以用。我用 Photoshop 保存了一张叫做 “BoudicaDOM.dds” 的 Boudica 图片，尺寸是 1024x768，然后在模组里添加了这幅图片，并且在 Celt 文明的定义里做了以下变动（蓝色部分）：

```xml
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
        <PortraitIndex>0</PortraitIndex>
        <IconAtlas>CIV_COLOR_ATLAS_LEGENDS</IconAtlas>
        <AlphaIconAtlas>CIV_ALPHA_ATLAS</AlphaIconAtlas>
        <MapImage>MapEngland512.dds</MapImage>
        <DawnOfManQuote>TXT_KEY_CIV5_CELT_TEXT_1</DawnOfManQuote>
        <!-- blue area -->
        <DawnOfManImage>BoudicaDOM.dds</DawnOfManImage>
        <!-- blue area -->
        <DawnOfManAudio/>
    </Row>
</Civilizations>
```

这是我们要创建一个新的、可用的首领的所有东西了。下一节将会降到怎么给 Boudica 添加特性。我们也可以在 Dawn of Man 页面上看到全新的图片。

#### 如何添加特性

目前为止我们借了 Hiawatha 的特性当作 Boudica 的。它很有用，但是如果我们的文明有个独特的特性，它会更有趣。这一节我们将涉及到添加特效的几个基本步骤。

先在模组里面添加 “CIV5Traits.xml” 文件，放在 `/XML/Cilizations` 目录下。添加一个叫做 “Battle Fury” 的特性，让 Melee，Mounted 和 Gun 等单位拥有每回合 2 次攻击的能力，普通单位每回合只能攻击 1 次。这是我们需要的设定：

```xml
<GameData>
    <Traits>
        <Row>
            <Type>TRAIT_BATTLE_FURY</Type>
            <Description>TXT_KEY_TRAIT_BATTLE_FURY</Description>
            <ShortDescription>TXT_KEY_TRAIT_BATTLE_FURY_SHORT</ShortDescription>
        </Row>
    </Traits>
    <Trait_FreePromotionUnitCombats>
        <Row>
            <TraitType>TRAIT_BATTLE_FURY</TraitType>
            <UnitCombatType>UNITCOMBAT_MELEE</UnitCombatType>
            <PromotionType>PROMOTION_SECOND_ATTACK</PromotionType>
        </Row>
        <Row>
            <TraitType>TRAIT_BATTLE_FURY</TraitType>
            <UnitCombatType>UNITCOMBAT_MOUNTED</UnitCombatType>
            <PromotionType>PROMOTION_SECOND_ATTACK</PromotionType>
        </Row>
        <Row>
            <TraitType>TRAIT_BATTLE_FURY</TraitType>
            <UnitCombatType>UNITCOMBAT_GUN</UnitCombatType>
            <PromotionType>PROMOTION_SECOND_ATTACK</PromotionType>
        </Row>
    </Trait_FreePromotionUnitCombats>
</GameData>
```

上述内容添加了新特性。这是很简单的设定，它让这个文明的 Melee、Mounted 和 Gun 单位得到二次攻击的晋升（二次进攻是游戏已经存在的晋升，让单位每回合多进行一次攻击）。

但是我们要修改模组属性，让模组加载时这个文件也能更新数据库，我们需要给使用的文本进行相应的翻译，并把这个特性赋予 Boudica。

你应该很熟悉是如何添加文本的。把这些内容添加到 “GameText.xml” 文件中：

```xml
<Row Tag="TXT_KEY_TRAIT_BATTLE_FURY">
    <Text>Melee, Mounted and Gun units can make 2 attacks per round.</Text>
</Row>
<Row Tag="TXT_KEY_TRAIT_BATTLE_FURY_SHORT">
    <Text>Battle Fury</Text>
</Row>
```

要把这个特性赋予 Boudica，在 Boudica 的首领定义中作以下的修改：

```xml
<Leader_Traits>
    <Row>
        <LeaderType>LEADER_BOUDICA</LeaderType>
        <TraitType>TRAIT_BATTLE_FURY</TraitType>
    </Row>
</Leader_Traits>
```

创造一个新特性就这么简单。确保浏览过有关 Traits 的模式和所有已有的特性的 XML 定义，也许会得到新特性的启发。

![](civ5_imgs/page43.jpg)

#### 如何添加单位

添加单位很简单。你可能注意到在之前的截图中，Celt 有两个特殊单位，这一节我们会讲到如何添加其中一个单位。

这份文档会涉及到技术层面的模组开发，但要添加新特色单位时，讲一些简单的设计思想很有用。当你设计一个特色单位，让这个文明更强大（特色单位通常比他们替换的普通单位更好）。想一想怎么匹配文明的特性，与其它特色单位比较会如何。对于 Celts 我添加的一种特色单位替换了勇士，另一个替换了剑士。这两个都是前期单位，并且与 Boudica 的特性结合的很好。这让 Celt 文明前期的军事力量更加强大。他们没有能帮助他们的基础设施建筑，也没有非常有用的防守能力。但他们将会是前期强大的进攻者。

Gaelic 勇士的花费和攻击力与普通的勇士一样，但 Gaelic 勇士无视地形（对于移动力）的消耗。在森林、丘陵、雨林中移动甚至是渡河，都像在平原一样行动。这让 Gaelic 勇士很勇猛，尤其是配合他们的特色能力，每回合能够进行两次攻击。这让热带地区开局对他们也很有帮助。

![](civ5_imgs/page43-2.jpg)

我们要修改四个地方来添加一种新特色单位。首先在模组里加入 “CIV5Units.xml” 文件。我把它放到了 `XML/Units/` 文件夹下。这个文件需要包含这些单位定义：

```xml
<GameData>
    <Units>
        <Row>
            <Class>UNITCLASS_WARRIOR</Class>
            <Type>UNIT_GAELIC_WARRIOR</Type>
            <Combat>6</Combat>
            <Cost>40</Cost>
            <Moves>2</Moves>
            <CombatClass>UNITCOMBAT_MELEE</CombatClass>
            <Domain>DOMAIN_LAND</Domain>
            <DefaultUnitAI>UNITAI_ATTACK</DefaultUnitAI>
            <Description>TXT_KEY_UNIT_GAELIC_WARRIOR</Description>
            <Civilopedia>TXT_KEY_CIV5_ANTIQUITY_WARRIOR_TEXT</Civilopedia>
            <Strategy>TXT_KEY_UNIT_WARRIOR_STRATEGY</Strategy>
            <Help>TXT_KEY_UNIT_HELP_GAELIC_WARRIOR</Help>
            <MilitarySupport>true</MilitarySupport>
            <MilitaryProduction>true</MilitaryProduction>
            <Pillage>true</Pillage>
            <ObsoleteTech>TECH_METAL_CASTING</ObsoleteTech>
            <GoodyHutUpgradeUnitClass>UNITCLASS_SPEARMAN</GoodyHutUpgradeUnitClass>
            <AdvancedStartCost>10</AdvancedStartCost>
            <XPValueAttack>3</XPValueAttack>
            <XPValueDefense>3</XPValueDefense>
            <Conscription>1</Conscription>
            <UnitArtInfo>ART_DEF_UNIT_GAELIC_WARRIOR</UnitArtInfo>
            <UnitFlagIconOffset>3</UnitFlagIconOffset>
            <IconAtlas>UNIT_ATLAS_1</IconAtlas>
            <PortraitIndex>3</PortraitIndex>
        </Row>
    </Units>
    <Unit_FreePromotions>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <PromotionType>PROMOTION_IGNORE_TERRAIN_COST</PromotionType>
        </Row>
    </Unit_FreePromotions>
    <Unit_AITypes>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <UnitAIType>UNITAI_ATTACK</UnitAIType>
        </Row>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <UnitAIType>UNITAI_DEFENSE</UnitAIType>
        </Row>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <UnitAIType>UNITAI_EXPLORE</UnitAIType>
        </Row>
    </Unit_AITypes>
    <Unit_ClassUpgrades>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <UnitClassType>UNITCLASS_SWORDSMAN</UnitClassType>
        </Row>
    </Unit_ClassUpgrades>
    <Unit_Flavors>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <FlavorType>FLAVOR_RECON</FlavorType>
            <Flavor>1</Flavor>
        </Row>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <FlavorType>FLAVOR_OFFENSE</FlavorType>
            <Flavor>2</Flavor>
        </Row>
        <Row>
            <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
            <FlavorType>FLAVOR_DEFENSE</FlavorType>
            <Flavor>2</Flavor>
        </Row>
    </Unit_Flavors>
</GameData>
```

添加单位的时候，这里有很多表需要被更新。首要的表是 Units，但我们也更新了 Unit_FreePromotions 表 （用来赋予晋升，让单位能够无视地形损耗），Unit_AITypes 表（有趣的是游戏会在 UNITAI_DEFENSE 单位中寻找可用的单位来开局，因为我没有定义这个，因此除非我定义了，Celts 开局时候就没有免费的勇士了），Unit_ClassUpgrades 表（这样我们的新单位可以升级到剑士）以及 Unit_Flavors 表（不同的值代表 AI 首领偏好不同的策略）。

上面的单位使用 UNITCLASS_WARRIOR 作为单位类别，因此不必定义新的单位类别。由于它没有自己的单位类别，我们就知道这是一个特色单位（就是替换文明的勇士单位）。

所有单位属性应该都很容易理解。我给 Gaelic 勇士添加了一个自定义的图片，下一节中将会涉及到。

添加单位的第二步是要修改模组属性，包含新文件，在激活模组时用来更新数据库。

第三布是添加单位需要的文本条目。我没有什么主意，添加了一个自定义的百科条目。就是一些文本，和帮助文本让玩家知道这个单位是做什么的，因此只需要向文件里添加这两个条目：

```xml
<Row Tag="TXT_KEY_UNIT_GAELIC_WARRIOR">
    <Text>Gaelic Warrior</Text>
</Row>
<Row Tag="TXT_KEY_UNIT_HELP_GAELIC_WARRIOR">
    <Text>This unit's ability to ignore terrain allows it to quickly rush into enemy lands.</Text>
</Row>
```

最后一步需要将这个单位添加到 Celt 文明的特色单位。

```xml
<Civilization_UnitClassOverrides>
    <Row>
        <CivilizationType>CIVILIZATION_CELT</CivilizationType>
        <UnitClassType>UNITCLASS_WARRIOR</UnitClassType>
        <UnitType>UNIT_GAELIC_WARRIOR</UnitType>
    </Row>
</Civilization_UnitClassOverrides>
```

这是添加新的特色单位的所有步骤了。下一节里我们会看到怎么给单位设计图片。