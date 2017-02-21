文明 5 MOD 制作教程 - 第一部分
=========

## 核心概念

### 模组开发是什么？

通过修改硬件或软件来做些有别于设计者所设想的事情，是模组开发的一种更为贴切<!-- slang -->说法。 Firaxis 公司开发的《文明 5》 （包括早先的版本）早已考虑到模组开发。一个模组可以很简单，修改<!-- tweaking -->某些单位和建筑的花费，修改 AI 让它变得不一样或者是向游戏中添加一个新文明。要么是个很复杂、完全不同的 mod，用文明引擎创造一个新的游戏。

这篇文档是用来帮助模组开发者的，通过探讨可行性、了解技术问题、让开发者尽快实现他们的想法以及发布自己的 mod，从而帮助他们更好开发《文明 5》的模组。尽管这篇文章涉及广泛的模组概念，但它无法包含的《文明 5》所能做的每一件事。特别地，源代码模型以及第三方的艺术改动不包含在该教程内。

### 《文明 5》 有哪些改动？

很多游戏都可以添加模组，《文明 4》在设计的时候就基于模组特性制订了一套新的标准。游戏数据库（XML）文件可以用文本编辑器进行修改，脚本语言（python）包含在内让玩家可以构建自己的事件和函数，并且核心 DLL 文件的源代码可以用 c++ 编译器进行修改。《文明 4》不仅仅是款游戏，更是开发模组的基础。

这款引擎开源而且强大，这里列出了几点限制。

1. 对于用户来说，发现、下载和安装模组很难。

2. 没有必要的开发者的整合工作，模组是没法同时运行的。

3. 尽管 python 很强大，它还是会严重影响游戏的性能。

《文明 5》改进了 《文明 4》提供的模组特性，解决了上述三个问题。《文明 5》提供一个游戏内的模组菜单（模组浏览器），允许用户发现、下载和安装他们想要尝试的模组。现在制作一个模组，让玩家可以与其它模组一起运行，非常简单，这允许玩家选择他们想要在游戏中运行的模组。并且用 Lua 替换了 python，Lua 能够与 C++ 核心更好的整合，并且对性能的影响更小。

### 模块设计

《文明 4》的模组开发者可以通过自由更换文件达到修改的目的。如果新的 Civ4Civilizations.xml 文件被添加到模组里面，那么就不会应用基础的 Civ4Civilizaiotns.xml 文件。《文明 5》不会发生这种情况。不是替换文件，所有的模组自动继承基础对象，并且开发者必须删掉那些对象如果他们不再被这个模组所使用（这个教程待会儿会讲到这些内容）。制作 Civil War 版本的模组时，开发者可以创建一个联盟和城邦文明，但还是需要删除基本文明的相关内容，这样就无法使用基本文明的相关内容了（否则在 Civil War 版本的模组里就会看到像法国和俄罗斯这样的文明了）。

这样做的好处在于它让模组模块化了。游戏认为所有的资源都能被使用，除非一个特殊的模组告诉它不要使用所有资源。这样某个模组可能添加加拿大文明，另一个模组可能移除德国文明并用新文明替换，而另一个模组可能添加亚特兰提斯文明。不需要经过开发者的特殊整合，这些模组就能够同时运作。如果模组足够多的话，每个玩家喜爱的模组组合可以大不相同。

### XML

XML 是针对特定数据的一种格式。它不是一门编程语言，而是用来存储信息的语言。在《文明 5》里用 XML 来存储游戏中不同的资源属性。一个单位的花费，国家领导人的名字，一个文明的初始单位都在 XML 中定义。

XML 的好处在于用文本编辑器就能修改，用户不必学一门编程语言就可以用 XML 了。看看下面的有关游戏中移民的 XML 定义：

```xml
<Row>
    <Class>UNITCLASS_SETTLER</Class>
    <Type>UNIT_SETTLER</Type>
    <Moves>2</Moves>
    <Capture>UNITCLASS_WORKER</Capture>
    <HurryCostModifier>33</HurryCostModifier>
    <Domain>DOMAIN_LAND</Domain>
    <DefaultUnitAI>UNITAI_SETTLE</DefaultUnitAI>
    <Description>TXT_KEY_UNIT_SETTLER</Description>
    <Civilopedia>TXT_KEY_CIV5_ANTIQUITY_SETTLER_TEXT</Civilopedia>
    <Strategy>TXT_KEY_UNIT_SETTLER_STRATEGY</Strategy>
    <Help>TXT_KEY_UNIT_HELP_SETTLER</Help>
    <Requirements>TXT_KEY_NO_ACTION_SETTLER_SIZE_LIMIT_HARDCODED</Requirements>
    <Food>true</Food>
    <Found>true</Found>
    <CombatLimit>0</CombatLimit>
    <UnitArtInfo>ART_DEF_UNIT__SETTLER</UnitArtInfo>
    <UnitArtInfoCulturalVariation>true</UnitArtInfoCulturalVariation>
    <PortraitIndex>0</PortraitIndex>
    <IconAtlas>UNIT_ATLAS_1</IconAtlas>
</Row>
```

这个定义中设定了移民的属性。属性存储在元素里，每一个元素都有一个起始标签（`< >` ，比如 `<Moves>`），和一个结束标签（`</ >`，比如 `</Moves>`）。在起始标签和结束标签之间就是数据（2 表示这个移民有 2 格移动力）。要把移民的移动力改成每回合 3 个格子很简单，就把 2 改成 3 就行了。

注意 `<Row>` 和 `</Row>` 也是标签。所以整个移民的定义就是一个包含子元素的元素。

不要纠结上述元素是什么意思。如果是第一次看到 XML 格式的数据，你也许会很惊讶自己能猜想出很多内容。Firaxis 完成了让标签具有良好的可读性这一工作。`<Domain>DOMAIN_LAND</DOMAIN>` 意味着移民是一个陆地单位。`<Capture>UNITCLASS_WORKER</Capture>`意味着当移民被俘获时，他就会变成工人。

元素里面可以包含其它元素。上面的例子中，Row 元素（开始标签是 `<Row>`，结束标签是 `</Row>`的元素）包含了移民的所有元素。这里有个更复杂的例子：

![page5.](civ5_imgs/page5.jpg)

上面的 GameData 元素是整个蓝色区域。Specialists 元素是红色区域。注意 Specialists 元素里面有两个 Row 元素，代表两种不同的伟人。这个伟人有什么特点，Row 元素中就包含哪些元素。第一个 Row 元素是绿色区域，这是对大艺术家的完整定义，粉红区域是这个大艺术家独有的属性。

#### 模式

模式是 XML 元素的定义。在文明中这意味着模式决定 `<Moves>` 标签是否包含一个文本，一个布尔值或是一个整数（Moves 是整数）。它同时为这个属性设定了缺省值，不像《文明 4》那样，开发者要定义每一个属性，《文明 5》的模式里定义了默认值，那么当没有设置它们时，就会使用默认值。

你可能注意到了有很多单位属性没有在移民的定义那一部分中出现。比如它没有说明移民是否能够劫掠。在模式中有个叫做劫掠的属性：

```xml
<Column name="Pillage" type="boolean" default="false"/>
```

这个模式定义表明劫掠元素是布尔型的。它可以是 `<Pillage>true</Pillage>` 或者 `<Pillage>false</Pillage>`，但不能是其它的。我们也可以看到劫掠的默认属性是 false。所以如果它是 false 的话，我们就没必要在单位的定义中添加这个属性。这就是为什么移民不必设置这个属性。

如果你想知道对象有什么属性，你应该查看这个对象类型的模式定义。有时我们会发现模组里面有些属性没有被使用，有些东西能够使用，即使在基础游戏里面不使用这些东西。看看从 CIV5HurryInfos.xml 中节选的：

```xml
<GameData>
    <!-- Table definition -->
    <Table name="HurryInfos">
        <Column name="ID" type="integer" primarykey="true" autoincrement="true"/>
        <Column name="Type" type="text" notnull="true" unique="true"/>
        <Column name="Description" type="text"/>
        <Column name="PolicyPrereq" type="text" reference="Policies(Type)"/>
        <Column name="GoldPerProduction" type="integer" default="0"/>
        <Column name="ProductionPerPopulation" type="integer" default="0"/>
        <Column name="GoldPerBeaker" type="integer" default="0"/>
        <Column name="GoldPerCulture" type="integer" default="0"/>
    </Table>
    <!-- Table data -->
    <HurryInfos>
        <Row>
            <ID>0</ID>
            <Type>HURRY_POPULATION</Type>
            <Description>TXT_KEY_HURRY_POPULATION</Description>
            <PolicyPrereq>NULL</PolicyPrereq>
            <ProductionPerPopulation>60</ProductionPerPopulation>
        </Row>
        <Row>
            <Type>HURRY_GOLD</Type>
            <Description>TXT_KEY_HURRY_GOLD</Description>
            <PolicyPrereq>NULL</PolicyPrereq>
            <GoldPerProduction>6</GoldPerProduction>
        </Row>
    </HurryInfos>
</GameData>
```

在 `<Table name="HurryInfos">` 和 `</Table>` 之间我们定义了一种模式。这个模式定义了 8 种属性，分别是 ID、Type、Description、PolicyPrereq、GoldPerProduction、ProductionPerPopulation、 GoldPerBeaker 以及 GoldPerCulture。

实际上在 `<HurryInfos>` 和 `</HurryInfos>` 标签中间定义为 Row 的是资源。有两种资源， HURRY_POPULATION （牺牲人口加快产能）和 HURRY_GOLD（花费金钱加快产能）。但是 PolicyPrereq、GoldPerBeaker 和 GoldPerCulture 等属性都没有用过。开发者可以在他们的模组里面使用这些属性，尽管在基础游戏里面没有使用它们。

需要注意的是，上述例子中是在文件的开头定义模式，同时定义了模式所控制的资源类型。这与《文明 4》中的处理方式不同，《文明 4》中模式是一个单独的文件。

#### 理解引用

文明里几乎每种资源都与其它的东西有关联。某种单位可能需要相应的科技才能制造，一个文明可能会有这个文明所独有的特色单位，国家领导有相关的特性。这导致删除一个东西会更加复杂，因为需要删除所有与之相关的东西。

举个例子，如果我们想制作一个模组，删除游戏中的石油资源。我们不能简单的在 CIV5Resources.xml 文件中删除石油资源。要是这么做的话，我们在加载这一改进（因为有些单位需要石油才能制造）以及加载这个特性（有些特性需要石油的开采作为引导）的时候就会发生一个错误。

添加新东西到游戏里面也是一样的。如果你添加了一个新文明，但你用了你还没添加到游戏里的领导者，或者用了你还没添加到游戏里的特色单位，在加载模组时，你就会得到一个错误。

要是你想添加一些东西并且想要它正常运作，你可能想要使用已经存在的引用，除非你让这一部分新添加的东西变得特殊。如果你是添加一个文明，你可能要用 LEADER_WASHINGTON 作为国家领导，UNIT_AMERICAN_B17 作为特色单位，这样你能够在添加新的国家领导和特色单位前加载和测试你制作的文明。

#### XML 文件结构

 `<install directory>\assets\Gameplay\XML\` 目录里包含 XML 的文件结构。与《文明 4》不同，这个确切的文件结构并不危险，因为我们不用替换文件。但是知道文件的位置也很重要，这样开发者就能找到当前的设定。一般来说概要定义在文件的开头，所以这也是个查找可用属性的好位置。

![](civ5_imgs/page7.jpg)

**GlobalDefines.xml** - 包含游戏很多设置的默认设定，比如起始年份、城市初始人口以及每个城市的最大建筑数量（默认是无限制）。修改这个文件中的某些值就能轻松改变游戏里的上百条规则。

**AI** - 这个目录包含许多 AI 配置文件。简单改变 XML 文件就能修改 AI 属性，而在《文明 4》里只有修改源代码才可能做到类似的事情。这里的文件是针对城市策略，经济策略，宏观策略（AI 追求何种胜利），军事策略，战术移动，外交和 AI 的一般设定（比如 AI 扩张的速度，它认为黄金有多少价值等等）。

**BasicInfos** - 这个目录包括所有世界性的游戏资源设定。这里也设定了单位的战斗类型，不可见类型，攻击范围等等。一般来说，这些声明很简单（仅仅是一个没有任何属性的类型），并且他们不能被简单修改。但是如果你设置一个单位的不可见类型（举个列子），游戏需要有这个不可见类型设定的位置，也就是在这里。

**Buildings** - 你所能想到的建筑设定保存在这里。这里有两个文件，一个是建筑类别，另一个是具体建筑。

建筑类别是建筑的基本设定，比如兵营。但一个建筑类别里面可以有多个建筑。所以兵营建筑类有两个相关的建筑，兵营和俄罗斯军营。允许一个建筑类中拥有多种建筑是文明中替换建筑的方式。那么一个文明可以建造俄罗斯军营而不是兵营作为特色建筑。但是因为兵营和俄罗斯军营都与兵营类建筑相关，游戏仅能把它视为兵营类别建筑，并且为相应的玩家修建相应形式的军营。

比如所有的文明在开始时都免费拥有一个 BUILDINGCLASS_PALACE。如果你建造一个新文明并且给予它一个特殊的宫殿，你的新建筑就不是 BUILDING_PALACE，但它却和 BUILDINGCLASS_PALACE 息息相关，那么当你的文明提供免费的宫殿时就能拥有自定义的宫殿而不是普通的宫殿了。

**Civilizations** - 文明，城邦，地区和特征都在这里设定。文明或许是大家第一个想要修改、添加的东西，并且稍后这个教程将会讲解关于这些东西的特殊例子。

**Diplomacy**- 这里设定了外交方面的所有文本，比如首次碰面的问候，宣战，拒绝交易等等。

**GameInfo** - 这是 Firaxis 限制所有与其它部分不兼容的东西的地方。这里有以下文件和资源：

- **CIV5ArtStyleTypes.xml** - 城市中可拥有的著作类别列表。这与文明的设定相关。这里没有多少可添加的，因为它仅是名字的标签。

- **CIV5Climates.xml** - 这里有多种多样的气候类型的设定（干旱，寒冷，温和，热带等等），随机地图生成器生成世界时有许多用处。它尽管并没有灵活到创造一个独特的地图，但可以让玩家十分方便地选择有更多或更少丛林、森林、山脉、荒漠等的地图。

- **CIV5CultureLevels.xml** - 这个文件在 Civ5 中已经被废弃并且不再使用了。

- **CIV5Cursors.xml** - 这是游戏中使用的鼠标光标的链接。创建一个新的光标集非常简单，只用添加 .ani 文件并且用新连接更新这个文件就行了。

- **CIV5EmphasizeInfos.xml** - 这个文件在 Civ5 中已经被废弃并且不再使用了。

- **CIV5Eras.xml** - 这里是游戏起始时的设定（设定了你开始时有多少单位，初始金钱，市民是否应该快速增长等等）。但它也确实包含游戏中的某些属性，比如一个修改器加快修造时间，新城市的免费人口，城市装饰的设定（这样你的城市在不同的时代就有不同的外观）。该文件中的 EventChancePerTurn 和 SoundtrackSpace 属性已被废弃。

- **CIV5Flavors.xml** - 这是包含特色种类的其它文件中使用的标签的列表。

- **CIV5ForceControlInfos.xml** - 这个文件在 Civ5 中已经被废弃并且不再使用了。

- **CIV5GameOptions.xml** - 这里是游戏选项的标签（快速战斗，狂暴的野蛮人等）。

- **CIV5GameSpeeds.xml** - 这是游戏速度设定的地方。包括建造速度，人口增长和经济增长的修改。

- **CIV5GoodyHuts.xml** - 包含所有可能的取胜结果。实际上取胜的机会是基于 CIV5HandicapInfos.xml 文件中设置的困难程度。

- **CIV5HandicapInfos.xml** - 这里设定了困难程度。这是一个绝佳的地方，可以看到不同难度程度的区别。这个文件中我们能看到 "EarliestBarbarianReleaseTurn" 在移民的难度中是 10000。大多数开发者不会修改难度调整，但有些开发者会增加 AI 的优势来平衡新机制让人类玩家喜欢。如果能较好的改进 AI，那么他们会降低 AI 的优势，因为 AI 就不需要帮助了。 

- **CIV5HurryInfos.xml** - 这是设定加快产出方法的地方。如果你要修改给定的产出量，用于减速人口增长或者金钱增长以获得大量金钱，这个文件就是用来做这个的。

- **CIV5IconFontMapping.xml** - 这个文件是将图标资源（比如 ICON_BULLET）的集映射到实际的图片。

- **CIV5IconTextureAtlases.xml** - 这是图片集（比如 TECH_ATLAS_1）映射到特殊的文件（比如 TechnologyAtlas.dds）中的地方。如果开发者要修改图标或者想要添加一个新的图片集，查看 dds 文件映射的位置时，这个文件就很有用。

- **CIV5MultiplayerOptions.xml** - 这是设定多人玩家游戏选项的地方。这里只有多人游戏的默认选项和文本字符串。所有的设定实际都在源代码中进行处理。因此向该文件中添加一个新的多人游戏选项将会导致在选择列表中出现一个新选项，但它却没有任何作用。

- **CIV5PlayerOptions.xml**- 这是非多人游戏选项设定的地方。和多人游戏选项一样，这里也没有什么有用的属性。启用或禁用这些选项的实际作用在源代码中实现。

- **CIV5Policies.xml** - 这是所有政策定义的地方。政策都与政策树相关联。这里设定了所有层次的政策，但是你可能要查看这个文件中的若干表项来获取所有信息。

- **CIV5PolicyBranchTypes.xml** - 这是应用政策树的地方。一个分支是否是另一个分支的前提是这里实际存储的配置。在这之外，他们可以被开发者用来添加或移除政策分支，使得在 CIV5Policies.xml 文件中设定的政策可以被应用。

- **CIV5Processes.xml** - 开发者可以修改这个文件中的生成财富和科研的选项，如果他们想要修改生成的比例。

- **CIV5Projects.xml** - 项目在这里设定。注意这里有个模式还没被使用，Project_ResourceQuantityRequirements，这也许能让开发者创造出某系有意思的项目。

- **CIV5SeaLevels.xml** - 这个文件决定了海洋地块的增长率，增长率基于玩家选择的海平面高度。

- **CIV5SmallAwards.xml** - 这个文件在 Civ5 中已经不再使用了。当胜利点数，城市数量和人口达到阈值时，它也能够显示通知。

- **CIV5Specialists.xml** - 这是定义专家的地方。你可以用这个文件修改各种专家提供的产量，添加或删除游戏中的专家，或者修改他们对相应伟人的贡献点数。

- **CIV5Trades.xml** - 这个文件控制 AI 重视贸易的程度（AI 对待科研、文化和金钱的态度）。

- **CIV5TurnTimers.xml** - 这里在多人游戏选项中设置了自动结束回合时定义了回合定时器。

- **CIV5Victories.xml** - 这是设定胜利方式的位置。一般来讲，Civ5 的胜利方式并不需要过多修改。比如征服胜利只用将 Conquest 元素设为 true （`<Conquest>true</Conquest>`）。Conquest 标签的作用是在源代码中设置，而非 XML 文件。但是仔细查看这个文件，可以看到有个模式表没有使用，VictoryPointAwards。Firaxis 在 Civ5 中整合了一个胜利点数系统，但没有使用。开发者可以添加新的胜利方式，不用将 WinsGame 设为真，然后分配点数到 VictoryPointsAwards 表的新方式中。

- **CIV5Votes.xml** - 这个文件在 Civ5 中已经被废弃并且不再使用了。

- **CIV5VoteSources.xml** - 这个文件在 Civ5 中已经被废弃并且不再使用了。

- **CIV5Worlds.xml** - 这里设定了世界的尺寸。格子的宽度和高度（也就是地图的长度和宽度），默认玩家和其它参数在这里设置。向这个文件添加一个新的地图尺寸没有太大作用，因为与地图尺寸相关的很多地图脚本是按照名字来设置变量的。如果新的地图尺寸的名字不是脚本所拥有的，那么脚本就不会正常运行。

**Interface** - 接口的模式，颜色和玩家颜色在这里设定。如果你想要查看你创建的文明分配的颜色，玩家颜色就变得很重要。玩家颜色是主要的颜色，次要颜色和文字颜色都是与玩家颜色相匹配的颜色。比如，玩家颜色中的红色，主要颜色就是红色，次要颜色是白色，文字颜色就是红色。在颜色文件中定义了颜色（比如定义了 “COLOR_RED” 是什么）。Civ5 使用这个来分配更多的颜色，但也有像 COLOR_TECH_TEXT 这样的定义事物的颜色。

**Leaders** - 这个文件夹与其它的有所不同。不是将所有模式和资源放在一个普通文件里，Firaxis 将模式放在一个文件中，将每个首领放在单独的文件里。这不影响模组性，但这让查看首领特性的值变得更加简单，也让比较首领特性的值变得更难。

**Misc** - 这是设定商业和道路（公路和铁路）的地方。开发者可能对修改产量或者公路及铁路上的移动力感兴趣。

**NewText** - 这是指定游戏中所使用的文本对应的具体文字的地方。对于英语玩家， TXT_KEY_CIV_ARABIA_DESC 在这里被解释为 "Arabian Empire"。对于德语（de_DE)，英语（EN_US），西班牙语（es_ES），法语（fr_FR），意大利语（it_IT）和日语（JA_JP）都有单独的字典。英语开发者想要找到一个特定文本指的是什么，会去查看 /XML/NewText/EN_US/ 目录下的文件。

**Technologies** - 这里定义了科技。科技有 GridX 和 GridY 元素。这些决定了科技在科技树中显示的位置。添加科技时你需要更新其它科技的 GridX 和 GridY 属性，好让新科技在科技树中有一席之地。

**Terrain** - 这里设定了特色（包括独有的特色），改造，资源，地形以及产能。

**Units** - 单位和其它与单位相关的东西在这里设定，包括晋升，建造，传教，构造等等。


### SQL 

XML 仅仅是一种特殊的中介语言，它被解释为 SQL 语句，然后在数据库中执行。通常当你运行一次游戏之后，XML 文件没有改变的话，游戏将会直接从 .db 文件加载。这改进了性能。Firaxis 出于熟悉程度的考虑保留了 XML 格式，但它不被游戏直接使用。

![](civ5_imgs/page11.jpg)

实际上 XML 文件是被转化为 SQL 查询语句在数据库中执行。SQL 文件也可以直接运行。SQL 文件有相应的优点和缺点。SQL 的缺陷是开发者必须熟悉这门语言，但它的优点是它可以写成非常复杂的形式，而这种形式无法用 XML 表达。

#### 查看数据库

最简单的查看游戏数据库的方式是安装带有 SQLite Manager 附加组件的 Firefox 浏览器 [https://addons.mozilla.org/en-US/firefox/addon/5817/](https://addons.mozilla.org/en-US/firefox/addon/5817/)。

装好后，打开 Firefox ，进入 Tools 菜单打开 SQLite Manager。（译注：在新版的 Firefox 里需要打开菜单，选择定制将 SQLite Manager 拖放到工具栏中使用）。在应用的最上方的菜单里，找到 Database -> Connect Database。定位到 `<My Documents>/My Games/Sid Meier's Civilization V/cache/` 路径。然后，确保选择的文件类型是 “All Files”，选择 CIV5CoreDatabase.db 文件。确定之后你就能查看数据库的内容了。

![](civ5_imgs/page12.jpg)

cache 文件夹中的文件是游戏运行时会被删除或替换的主体，所以要是你想修改数据的话，在 XML 文件中修改就行了，不要直接修改这些文件。

#### SQL 用例

以下一些例子可以通过你的模组中包含的 sql 文件得到应用。这通常是修改一些设置的更方便的方式，就不用手动修改每个设置了。

-- 让出了移民和侦察兵以外的所有建筑和单位都不能建造

```sql
UPDATE Buildings SET 'PrereqTech' = 'TECH_FUTURE_TECH' WHERE Type <> 'BUILDING_PALACE';
UPDATE Units SET 'PrereqTech' = 'TECH_FUTURE_TECH' WHERE Class <> 'UNITCLASS_SETTLER' and Class <> 'UNITCLASS_SCOUT';
```

-- 屏蔽具体单位建造的另一种方式

```sql
UPDATE UnitClasses SET MaxPlayerInstances = 0 WHERE Type IN ("UNITCLASS_SETTLER","UNITCLASS_ARTIST","UNITCLASS_SCIENTIST","UNITCLASS_MERCHANT","UNITCLASS_ENGINEER");
```

-- 显示所有文明

```lua
for civ in DB.Query("select * from Civilizations") do
    print(civ.Type);
end
```

-- 显示所有花费超过 200 的单位

```lua
for unit in DB.Query("select * from Units where cost > 200") do
    print(unit.Type);
end
```

-- 显示所有玩家主要颜色的 RGB 值

```lua
for color in DB.Query("select Colors.Red, Colors.Green, Colors.Blue from PlayerColors inner join Colors on PlayerColors.PrimaryColor = Color.Type") do
    print(string.Format("R: %f, G: %f, B: %f", color.Red, Color.Green, Color.Blue);
end
```

-- 显示所有美国首领

```lua
local myCiv = "CIVLIZATION_AMERICA";
for leader in DB.Query("select * from Leaders inner join Civilization_Leaders on Leaders.Type = Civilization_Leaders.LeaderheadType where Civilization_Leaders.CivilizationType = ?", myCiv) do
    print(leader.Type);
end
```

### Lua

#### XML 和 LUA 的关系

在 Civ5 中 UI 就是 XML/Lua 对。XML 指定控制和层次，而 Lua 指定逻辑。XML 构建出 UI，它决定按钮出现的位置、表格的外观等等，而 Lua 是一门编程语言，控制你按下按钮后的事件或者如果填写表格的。

#### 参考

有很多 Lua 编程教程。大都能在当地的书店或者从 Amazon 下单。以下是一份在线参考手册（译注：云风已经翻译出了中文版的 [Lua 参考手册](http://www.codingnow.com/2000/download/lua_manual.html)）： 

Lua 5.1 Reference Manual by Roberto Ierusalimschy, Luiz Henrique de Figueiredo, Waldemar Celes: [http://www.lua.org/manual/5.1/manual.html](http://www.lua.org/manual/5.1/manual.html)

#### 脚本事件

Lua 脚本不能直接调用其它 Lua 脚本中的函数。好在有 LuaEvents 接口，它允许 Lua 脚本添加一个全局可用的函数。

如果你想要让函数能被其它的 Lua 脚本调用，你需要创建一个 LuaEvents 函数来实现，比如这样：

```lua
LuaEvents.ToggleHideUnitIcon.Add(
function()
    if (bHideUnitIcon) then
        bHideUnitIcon = false;
    else
        bHideUnitIcon = true;
    end
end);
```

上面的函数已经用 LuaEvents 引擎进行了注册，能够被其它的 Lua 脚本调用：

```lua
LuaEvents.ToggleHideUnitIcon();
```

Which would run the ToggleHideUnitIcon in the first script and change the bHideUnitIcon value for that script.
这会运行第一个脚本中的 ToggleHideUnitIcon 函数，修改相应脚本中的 bHideUnitIcon 值。