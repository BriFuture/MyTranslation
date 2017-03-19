#### 如何用 Lua 禁用单位图标

《文明 5》的单位上方有一个浮动的图标。我们将添加禁用单位图标（Unit Icon）的能力。

![](civ5_imgs/page56.jpg)

先要找到控制单位图标的 Lua 文件。没有简单的引用关系，搜索要花一番功夫。好在 Firaxis 用的文件名相当有含义，单位图标由 “UnitFlagManager.lua” 和 “UnitFlagManager.xml” 文件控制。他们都在 `<Civilization V>\assets\UI\InGame` 目录下。

![](civ5_imgs/page56-2.jpg)

首先要在项目里添加两个新文件。与添加 XML 文件不同的是 Lua 文件要用 XML 一样的文件名，因此分别重命名新文件为 “UnitFlagManager.lua” 和 “UnitFlagManager.xml”，并且删除两个文件里的内容。它们放在哪个文件夹里不重要，但为让其有序，我创建了 Lua 文件夹，并把两个文件添加到里面。

然后是把原始的 “UnitFlagManager.lua” 和 “UnitFlagManager.xml” 文件的内容复制到空的新副本里。Lua 不像 XML 那样易于模块化（InGameUIAddin 允许某些模块化的 Lua 修改，但很有限）因此需要复制整个文件进行修改。

有很多禁用单位图标的方法，方法因人而异。这里有种禁用的方法。用 “UnitFlagManager.lua” 文件里的 UpdateVisibility() 函数：

```lua
UpdateVisibility = function( self )
    if InStrategicView() then
        local bVisible = self.m_IsCurrentlyVisible and self.m_IsGarrisoned and g_GarrisonedUnitFlagsInStrategicView and not self.m_IsInvisible;
        self.m_Instance.Anchor:SetHide(not bVisible);
    else
        self.m_Instance.Anchor:SetHide(not (self.m_IsCurrentlyVisible and not self.m_IsInvisible));
    end
end,
```

观察这段代码，能够发现如果游戏是处于战略视图状态下，那么当单位可见时，就会显示单位图标，单位不可见时单位图标就会隐藏。如果游戏不是战略视图状态，那么不论单位是否可见，都能显示单位图标。

禁用单位图标最简单的方法是做这样的修改：

```lua
UpdateVisibility = function( self )
    if InStrategicView() then
        local bVisible = self.m_IsCurrentlyVisible and self.m_IsGarrisoned and g_GarrisonedUnitFlagsInStrategicView and not self.m_IsInvisible;
        self.m_Instance.Anchor:SetHide(not bVisible);
    else
        -- Modified by Kael 07/17/2010 to disable the Unit Icon
        -- self.m_Instance.Anchor:SetHide(not (self.m_IsCurrentlyVisible and not self.m_IsInvisible));
        self.m_Instance.Anchor:SetHide(true);
        -- End Modify
    end
end,
```

上面的代码里我们只是简单的把单位图标设置成在游戏不是战略视图时永久隐藏。在战略视图里，同样的规则用于显示单位图标。要注意的是我对所作的修改进行了注释，包含谁做了修改，什么时候改的以及为什么修改它。同时用注释保留了原始文件中的一行代码，万一哪天我想撤回修改或者看看做了什么修改（就不用去找原来的文件了）。

我现在能够发布我制作的模组了，它能够禁用单位图标。但这还不是最好的解决方案。如果玩家能够根据自己的喜好设置是否启用单位图标的话就更好了。让我们在 MiniMap Options 面板里添加一个新的菜单选项，这样玩家就能选择是否显示单位图标了。

首先要加入一些代码，这样才能使用变量而不是简单的把 Anchor Hide 属性设置为 true。

```lua
UpdateVisibility = function( self )
    if InStrategicView() then
        local bVisible = self.m_IsCurrentlyVisible and self.m_IsGarrisoned and g_GarrisonedUnitFlagsInStrategicView and not self.m_IsInvisible;
        self.m_Instance.Anchor:SetHide(not bVisible);
    else
        -- Modified by Kael 07/17/2010 to disable the Unit Icon
        -- self.m_Instance.Anchor:SetHide(not (self.m_IsCurrentlyVisible and not self.m_IsInvisible));
        self.m_Instance.Anchor:SetHide(not (self.m_IsCurrentlyVisible and not self.m_IsInvisible) or bHideUnitIcon);
        -- End Modify
    end
end,
```

这个版本就不是简单的设置单位图标在玩家不是策略视图下永远隐藏了，我们用了一个变量来控制它。如果 bHideUnitIcon 是 true 的话，它就会像之前的代码一样，隐藏单位图标。但如果 bHideUnitIcon 是 false 的话，它就能像游戏的原始代码一样显示单位图标。

需要设置 bHideUnitIcon 的默认值，在 “UnitFlagManager.lua” 文件顶部添加下面的代码：

```lua
-- Added by Kael 07/17/2010 to disable the Unit Icon
local bHideUnitIcon = true;
-- End Add
```

注意 bHideUnitIcon 的默认值是 true，这样单位图标一开始就是隐藏的。现在可以加载我们的模组，确认单位图标是否被隐藏。

第五步，需要给菜单按钮添加文本。创建一个新的 XML 文件夹，然后在该文件夹下创建一个 NewText 文件夹，再在 NewText 文件夹下新建文件，就叫 “GameText.xml”。文件结构便于组织项目，它仿照的是《文明 5》的 xml 文件结构。文件名其实也不重要（文件里的 GameData 标签才重要）。即便把它放在项目根目录下，换个 “stuff.xml”的名字，也能正常运行。

第六，用下面的内容替换 “GameText.xml” 文件中的默认文本。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GameData>
    <Language_en_US>
        <Row Tag="TXT_KEY_MAP_OPTIONS_HIDE_UNIT_ICON">
            <Text>Hide Unit Icons</Text>
        </Row>
    </Language_en_US>
</GameData>
```

上面是一个简单的文本替换。当游戏语音是英语，并且遇到 TXT_KEY_MAP_OPTIONS_HIDE_UNIT_ICON 时，它就会被替换成 “Hide Unit Icons”。

第七，添加可以启用或禁用单位图标的选项，需要再添加两个文件 “MiniMapPanel.lua” 和 “MiniMapPanel.xml”。和 UnitFlagManager 文件一样，我们要创造两个空文件，再分别重命名，并把原始文件的内容复制到项目的副本中。

第八，修改文件 “MiniMapPanel.xml”，添加一个新的复选框。

```xml
<Grid ID="OptionsPanel" Anchor="R,B" Size="300,300" Color="White.256" Style="Grid9DetailSix140" Padding="30,46" Hidden="true" ConsumeMouse="1" >
    <Stack Anchor="C,C" Padding="0" Offset="0,5" StackGrowth="Bottom" ID="MainStack" >
        <Stack Anchor="C,T" Offset="0,0" Padding="0" StackGrowth="Bottom" ID="StrategicStack" >
            <Label Anchor="L,T" ColorSet="Beige_Black" Font="TwCenMT18" Offset="0,5" FontStyle="Shadow" ID="OverlayName" String="TXT_KEY_STRAT_OVERLAY"/>
            <PullDown Style="GenericPullDown" ScrollThreshold="256" Offset="0,5" Size="210,27" SpaceForScroll="0" ID="OverlayDropDown"/>
            <Label Anchor="L,T" ColorSet="Beige_Black" Font="TwCenMT18" Offset="0,5" FontStyle="Shadow" ID="IconName" String="TXT_KEY_STRAT_ICON_MODE"/>
            <PullDown Style="GenericPullDown" ScrollThreshold="256" Offset="0,5" Size="210,27" SpaceForScroll="0" ID="IconDropDown"/>
            <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="1" ID="ShowFeatures" String="TXT_KEY_STRAT_FEATURES" />
            <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="1" ID="ShowFogOfWar" String="TXT_KEY_STRAT_FOW" />
            <Box Anchor="C,T" Offset="0,5" Size="175,1" Color="Beige,120" />
        </Stack>
        <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="0" ID="ShowRecommendation" String="TXT_KEY_MAP_OPTIONS_RECOMMENDATIONS" />
        <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="0" ID="ShowResources" String="TXT_KEY_MAP_OPTIONS_RESOURCE_ICONS" />
        <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="0" ID="ShowYield" String="TXT_KEY_MAP_OPTIONS_YIELD_ICONS" />
        <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="0" ID="ShowGrid" String="TXT_KEY_MAP_OPTIONS_HEX_GRID" />
        <!-- Modified by Kael 07/15/2010-->
        <CheckBox Anchor="L,T" TextAnchorSide="Right" Offset="0,0" TextOffset="40,0" IsChecked="1" ID="HideUnitIcon" String="TXT_KEY_MAP_OPTIONS_HIDE_UNIT_ICON" />
        <!-- End Modify-->
        <Box Anchor="C,T" Size="210,1" Color="0,0,0,0" />
    </Stack>
</Grid>
```

注意，上面依然注解了修改，但是 XML 的注释符与 Lua 不同。注释始于“`<!--`”，止于“`-->`”。添加的那行重要的设定是 IsChecked="1"，这意味着复选框默认是勾选的。复选框的 ID 叫 “HideUnitIcon”，显示的文本是 “TXT_KEY_MAP_OPTIONS_HIDE_UNIT_ICON”。

![](civ5_imgs/page60.jpg)

第九，默认是禁用单位图标，我们添加了菜单选项允许玩家启用或禁用单位图标。但是菜单选项还做不了什么事情。当玩家点击“隐藏单位图标”的复选框时，应该触发我们在 “UnitFlagManager.lua” 文件中的 bHideUnit 变量、

![](civ5_imgs/page60-2.jpg)

Lua 脚本不能直接调用其它脚本里的函数。如果要一个 Lua 脚本能调用另一个脚本里的函数，我们要用到 LuaEvenets。在 “UnitFlagManager.lua” 文件的开始，我们需要创建一个新的 LuaEvent，如下：

```lua
-- Added by Kael 07/16/2010
local bHideUnitIcon = true;

LuaEvents.ToggleHideUnitIcon.Add(
function()
    if (bHideUnitIcon) then
        bHideUnitIcon = false;
    else
        bHideUnitIcon = true;
    end
end);
-- End Add
```

现在 LuaEvents.ToggleHideUnitIcon() 函数能够被外部 Lua 脚本调用，并且修改脚本中的值了。

最后，我们需要菜单选项能够调用 LuaEvents.ToggleHideUnitIcon()。向 “MiniMapPanel.lua” 文件添加以下内容即可：

```lua
-- Added by Kael 07/16/2010
function OnHideUnitIconChecked( bIsChecked )
    LuaEvents.ToggleHideUnitIcon();
    Events.StrategicViewStateChanged();
end
Controls.HideUnitIcon:RegisterCheckHandler( OnHideUnitIconChecked );
-- End Add
```

上述代码给 HideUnitIcon 注册了一个触发器（也就是我们在 “MiniMapPanel.xml” 文件中添加的复选框的 ID）。如果被选中，它就调用本地的 OnHideUnitIconChecked 函数。OnHideUnitIconChecked 函数再调用 LuaEvent ToggleHideIcon() 以及 Events.StrategicViewStateChanged() 函数。StrategicViewStateChanged() 强制重画图标，这样游戏不会等到下一次更新单位时才添加或者移除图标，玩家一点击复选框就会更新单位。

这就是一个完整的模组，可以通过小地图面板上的菜单选项启用或禁用单位图标。这里我们讲了怎么添加文本，怎么修改 Lua 脚本，怎么绑定 Lua 函数到 XML 对象，以及如何通过 LuaEvents 从一个 Lua 脚本调用另一个脚本的函数。一旦理解了这些基础，就明白了最难的部分，也就是脚本和函数控制着我们想要改变的流程（我们是如何知道 UnitIcons 是 “UnitFlagManager.lua” 文件控制的）以及调用什么函数能够做到我们想要做的（如何知道 StrategicViewStateChanged() 函数会更新单位图标）。对我而言，这意味着查找搜索了很多文件。

一开始会花很久完成上述事情（它花了我很多时间）。但好在这篇文档明显地缩短了要用的时间。现在我知道我所作的下一个改变就会快得多。

#### 如何用 InGameUIAddin 制作模块化的 UI 变化

通过 InGameUIAddin 可以创造模块化的 UI 变化。尽管它功能有限，但它能添加新的 UI 控件，但不能移除或者修改现有的 UI 控件。要移除或者修改现有的 UI 控件，你得照着 “如何用 Lua 禁用单位图标” 替这一节的做法换 Lua 和 XML 文件

![](civ5_imgs/page61.jpg)

这个例子我们将在界面里添加一个时钟。从添加 “Clock.lua” 和 “Clock.xml” 文件开始。它们不是替换文件，因此文件名没关系。所有 Lua 的 UI 变化都要有配套的 XML 和 Lua 文件。XML文件提供结构和设置，Lua 文件提供代码。

首先在 “Clock.xml” 文件中填入以下内容：

```xml
<Context ColorSet="Beige_Black" Font="TwCenMT20" FontStyle="Shadow" >
    <Label Anchor="C,T" Offset="0,10" Font="TwCenMT20" ColorSet="Beige_Black_Alpha" FontStyle="Shadow" ID="ClockLabel"/>
</Context>
```

The above defines a new label with the ID of "ClockLabel". We can see the font (TwCenMT20), color (Beige_Black_Alpha) and location (centered at the top of the screen, with an offset 0 to the left and 10 down) for the label.
以上定义了一个叫做 “ClockLabel” 的新标签。可以看到标签的字体（font，TwCenMT20）,颜色（color，Beige_Black_Alpha）和位置（location，屏幕上方中央，距离左边为 0 ，距下边为 10）。（注： Label 中的 Anchor 指明位置，C 表示 Center、中央，T 表示 Top、顶部）

但这还只是个空标签，需要 “Clock.lua” 来同步时间。“Clock.lua” 的内容如下：

```lua
ContextPtr:SetUpdate(function()
    local t = os.date("%I:%M");
    Controls.ClockLabel:SetString(t);
end);
```

ContextPtr:SetUpdate 在屏幕画面更新时调用。上述代码添加了同步时间的函数。它将时和分赋给 “t” 字符串，然后设置成 XML 文件中定义的 ClockLabel 显示的文本。

下一步是当模组加载时，要加载 UI 的变化，与 XML 需要在模组属性的 actions 页设置 OnModActivated 和 UpdateDatabase 操作类似。对于 InGameUIAddin 变化，我们要转到模组属性的 Content 页，指定这是 InGameUIAddin 变化，以及我们想要激活的文件（相应的 XML 文件不需要指定）。

![](civ5_imgs/page63.jpg)

下面这幅截图中能够看到我们添加的时钟（红色框内的是添加的部分，让它在截图中更容易看到，红色框不是模组添加的）。

![](civ5_imgs/page63-2.jpg)

#### 如何用 Lua 添加一个新窗口

没有办法在进行游戏时知道你加载了什么模组（注：在 BNW 版本中已经可以查看了）。这一节将会在额外的信息菜单中添加一个新窗口，可以列出所有加载的模组。

在添加新窗口前，还有一些问题需要解决。在前一节里，我们用了 InGameUIAddin 的例子。InGameUIAddin 允许添加新的 UI。但不允许修改现有的 UI 控件。要修改现有的控件，需要替换那些原始的文件。替换 Lua 文件的问题是这可能会与其它替换同样文件的模组冲突。

要让模组工作，得替换以下文件：

- **DiploCorner.lua** -  该文件负责屏幕右上角的 UI 管理。我们将会修改这个文件，添加菜单按钮。
- **DiploCorner.xml** - 这个 XML 文件负责 XML 层面的外交界面管理。
- **InGame.xml** - 这是注册 Lua 文件的主要地方。
- **NotificationLogPopup.lua** - 它控制通知的弹出菜单，我们将会通过这个事件添加一个新的事件。

![](civ5_imgs/page64.jpg)

现在添加两个文件用来控制新窗口：

- **ModList.lua** - 控制窗口的 Lua 文件

- **ModList.xml** - 负责设定模组列表窗口的 XML 文件

1. 第一步是像上面的截图那样，在 Lua 目录下创建相应的文件。从 “`<Civ5 install directory>\Assets\UI\`” 目录下分别复制 “DiploCorner.lua”，“DiploCorner.xml”，“InGame.xml” 以及 “NotificationLogPopup.lua” 等文件的内容到我们新创建的文件里面。

第一个小改动是修改 “InGame.xml” 文件，使之能加载添加的 Lua 文件：

```xml
<LuaContext FileName="Assets/UI/InGame/Popups/SetCityName" ID="SetCityName" Hidden="True" />
<LuaContext FileName="Assets/UI/InGame/Popups/ProductionPopup" ID="ProductionPopup" Hidden="True" />
<!-- Added by Kael 09/17/2010 -->
<LuaContext FileName="Lua/ModList" ID="ModList" Hidden="True" />
<!-- End Add -->
<LuaContext FileName="Assets/UI/InGame/TopPanel" ID="TopPanel" />
```

上面所作的是在加载模组时，同时运行我们的 “ModList.lua” 文件。稍后会介绍更多关于 “ModList.lua” 的信息。

2. 下一步是修改 “DiploCorner.lua” 和 “DiploCorner.xml” 添加新的菜单选项。下面是在 “DiploCorner.lua” 文件里添加的内容：

```lua
local g_MultiPullInfo = {};
g_MultiPullInfo[0] = { text="TXT_KEY_ADVISOR_SCREEN_TECH_TREE_DISPLAY", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_TECH_TREE } ); end };
g_MultiPullInfo[1] = { text="TXT_KEY_DIPLOMACY_OVERVIEW", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_DIPLOMATIC_OVERVIEW } ); end };
g_MultiPullInfo[2] = { text="TXT_KEY_MILITARY_OVERVIEW", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_MILITARY_OVERVIEW } ); end };
g_MultiPullInfo[3] = { text="TXT_KEY_ECONOMIC_OVERVIEW", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_ECONOMIC_OVERVIEW } ); end };
g_MultiPullInfo[4] = { text="TXT_KEY_VP_TT", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_VICTORY_INFO} ); end };
g_MultiPullInfo[5] = { text="TXT_KEY_DEMOGRAPHICS", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_DEMOGRAPHICS} ); end };
g_MultiPullInfo[6] = { text="TXT_KEY_POP_NOTIFICATION_LOG", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_NOTIFICATION_LOG, Data1 = Game.GetActivePlayer() } ); end };

-- Added by Kael 09/17/2010
g_MultiPullInfo[7] = { text="TXT_KEY_MOD_LIST", call=function() Events.SerialEventGameMessagePopup( { Type = ButtonPopupTypes.BUTTONPOPUP_NOTIFICATION_LOG, Data1 = 999 } ); end };
-- End Add
```

这里往 multipull 菜单里添加了新的选项。如果它被选中，它将会把 BUTTONPOPUP_NOTIFICATION_LOG 和 Data1 里的 999 传递出去。然后我们就要捕捉事件，触发弹出菜单。

![](civ5_imgs/page65.jpg)

以上完成了在选项菜单里添加新菜单项的工作。但我们想调整下拉框的大小，让新选项有足够的空间。XML 控制着 UI 的尺寸和形式，因此需要修改 “DiploCorner.xml” 调整大小。

```xml
<!-- ==========================================================================================================-->
<!-- Notification Log DropDownButtons -->
<!-- ==========================================================================================================-->
<PullDown ConsumeMouse="1" Offset="-6,0" Anchor="R,T" Size="45,45" AutoSizePopUp="0" SpaceForScroll="0" ScrollThreshold="900" ID="MultiPull" >
    <ButtonData>
        <Button Anchor="R,T" Size="45.45" Texture="assets\UI\Art\Notification\NotificationNotes.dds" ToolTip="TXT_KEY_DIPLO_ADDITIONAL" >
            <ShowOnMouseOver>
                <Image Anchor="R,T" Offset="0,0" Size="45.45" Texture="assets\UI\Art\Notification\NotificationNotes.dds" />
                <AlphaAnim Anchor="R,T" Offset="0,0" Size="45.45" TextureOffset="0.0" Texture="assets\UI\Art\Notification\NotificationNotesHL.dds" Pause="0" Cycle="Bounce" Speed="2" AlphaStart="0.95" AlphaEnd="0.55"/>
            </ShowOnMouseOver>
        </Button>
    </ButtonData>
    <GridData Anchor="L,T" Offset="-24.-41" AnchorSide="O,I" Style="Grid9DetailTwo140" Padding="0,0" >

<!-- Modified by Kael 09/17/2010
        <Box Color="Black.0" Size="200.260" /> -->
        <Box Color="Black.0" Size="200.280" />
<!-- End Modify -->
```

这把下拉框的大小从 260 调到了 280.

3. 这里用了在 “DiploConer.lua” 文件中添加的 BUTTONPOPUP_NOTIFICATION_LOG 函数。因此当我们选中添加的菜单选项时，它就触发了，就像是 BUTTONPOPUP_NOTIFICATION_LOG 函数被调用了一样。如果能够使用游戏开发包的话，还能给模组列表创建一个 BUTTONPOPUP 设定。没有这个定义就得想其它方法了。因此我们把 999 传给了调用的 BUTTONPOPUP_NOTIFICATION_LOG（通常是传递玩家号码，但也不会接近 999）。

由于利用了 BUTTONPOPUP_NOTIFICATION_LOG 函数，需要屏蔽掉普通的 BUTTONPOPUP_NOTIFICATION_LOG 对话框。这就是在项目中添加 “NotificationLogPopup.lua” 的原因。

记住 Lua 函数不能直接调用其它文件的 Lua 函数。除了带有统一事件处理的 Lua 注册函数以及监控何时响应事件的监控器。这是 “NotificationLogPopup.lua” 文件中普通的 OnPopup 函数：

```lua
function OnPopup( popupInfo )
    -- red area
    if( popupInfo.Type ~= ButtonPopupTypes.BUTTONPOPUP_NOTIFICATION_LOG ) then
        return;
    end
    -- end of red area

    CivIconHookup( Game.GetActivePlayer(), 64, Controls.CivIcon, Controls.CivIconBG, Controls.CivIconShadow, false, true );
    m_PopupInfo = popupInfo;
    g_NotificationInstanceManager:ResetInstances();
    local player = Players[Game.GetActivePlayer()];
    local numNotifications = player:GetNumNotifications();
    for i = 0, numNotifications - 1
    do
        local str = player:GetNotificationStr((numNotifications - 1) - i);
        local index = player:GetNotificationIndex((numNotifications - 1) - i);
        local turn = player:GetNotificationTurn((numNotifications - 1) - i);
        local dismissed = player:GetNotificationDismissed((numNotifications - 1) - i);
        AddNotificationButton(index, str, turn, dismissed);
    end
    Controls.NotificationButtonStack:CalculateSize();
    Controls.NotificationButtonStack:ReprocessAnchoring();
    Controls.NotificationScrollPanel:CalculateInternalSize();
    if( m_PopupInfo.Data1 == 1 ) then
        if( ContextPtr:IsHidden() == false ) then
            OnClose();
        else
            UIManager:QueuePopup( ContextPtr, PopupPriority.eUtmost );
        end
    else
        UIManager:QueuePopup( ContextPtr, PopupPriority.NotificationLog );
    end
end
-- blue area
Events.SerialEventGameMessagePopup.Add( OnPopup );
-- end of blue area
```

上面的代码定义了 OnPopup 函数，它被注册到 SerialEventGameMessagePopup 事件中（蓝色区域）。onPopup 函数里的第一个判断是验证传到 SerialEventGameMessagePopup 中的这个事件是否要被处理。因此检查 popupInfo.Type 是不是 BUTTONPOPUP_NOTIFICATION_LOG （红色区域）。如果不是的话，就会返回并且结束函数。

但我们要让 BUTTONPOPUP_NOTIFICATION_LOG 触发别的函数，不希望触发弹出窗口。因此要在 “NotificationLogPopup.lua” 文件里做些修改：

```lua
function OnPopup( popupInfo )
    if( popupInfo.Type ~= ButtonPopupTypes.BUTTONPOPUP_NOTIFICATION_LOG ) then
        return;
    end

    -- Added by Kael 09/17/2010
    if( popupInfo.Data1 == 999 ) then
        return;
    end
    -- End Add

    CivIconHookup( Game.GetActivePlayer(), 64, Controls.CivIcon, Controls.CivIconBG, Controls.CivIconShadow, false, true );
    m_PopupInfo = popupInfo;
    g_NotificationInstanceManager:ResetInstances();
    local player = Players[Game.GetActivePlayer()];
    local numNotifications = player:GetNumNotifications();
    for i = 0, numNotifications - 1
    do
        local str = player:GetNotificationStr((numNotifications - 1) - i);
        local index = player:GetNotificationIndex((numNotifications - 1) - i);
        local turn = player:GetNotificationTurn((numNotifications - 1) - i);
        local dismissed = player:GetNotificationDismissed((numNotifications - 1) - i);
        AddNotificationButton(index, str, turn, dismissed);
    end
    Controls.NotificationButtonStack:CalculateSize();
    Controls.NotificationButtonStack:ReprocessAnchoring();
    Controls.NotificationScrollPanel:CalculateInternalSize();
    if( m_PopupInfo.Data1 == 1 ) then
        if( ContextPtr:IsHidden() == false ) then
            OnClose();
        else
            UIManager:QueuePopup( ContextPtr, PopupPriority.eUtmost );
        end
    else
        UIManager:QueuePopup( ContextPtr, PopupPriority.NotificationLog );
    end
end
Events.SerialEventGameMessagePopup.Add( OnPopup );
```

以上代码添加了另一个判断。就像如果 type 不是 BUTTONPOPUP_NOTIFICATION_LOG 的判断一样，如果 Data1 的值不是 999，这个判断也会退出函数，这样我们就能使用 BUTTONPOPUP_NOTIFICATION_LOG 函数，不用担心和其它的冲突了。

4. 最后需要为新窗口写代码。首先需要用 XML 定义窗口。这是我在 “ModList.xml” 文件中的设定：

```xml
<Context Font="TwCenMT14" FontStyle="Base" Color="Beige" Color1="Black" >
    <Instance Offset="0,0" Name="NotificationButton" Size="890,60" >
        <Button Size="890,60" Offset="0,0" StateOffsetIncrement="0" ID="Button">
            <Button Anchor="L,C" Offset="0,0" Size="64.64" Texture="assets\UI\Art\WorldView\ActionItemsButton.dds" Hidden="1" ID="GenericButton" />
            <Label Anchor="R,B" Offset="5,8" Font="TwCenMT24" ColorSet="Beige_Black_Alpha" FontStyle="Shadow" String="Turn" ID="NotificationTurnText" />
            <Stack ID="TextStack" Anchor="L,T" Padding="10">
                <Label Anchor="L,T" Offset="16,5" LeadingOffset="-8" WrapWidth="780" Font="TwCenMT18" ColorSet="Beige_Black_Alpha" FontStyle="Shadow" ID="NotificationText" String="Text" />
                <Image Anchor="C,B" Offset="0,0" TextureOffset="0.0" Texture="bar900x2.dds" Size="900.1" />
            </Stack>
            <ShowOnMouseOver>
                <AlphaAnim ID="TextAnim" Anchor="C,C" Offset="10,0" Size="890,64" Pause="0" Cycle="Bounce" Speed="1" AlphaStart="2" AlphaEnd="1">
                    <Grid ID="TextHL" Size="890,64" Offset="0,0" Padding="0,0" Style="Grid9FrameTurnsHL" />
                </AlphaAnim>
            </ShowOnMouseOver>
        </Button>
    </Instance>
    <Box Style="BGBlock_ClearTopBar" />

    <Instance Name="ListingButtonInstance">
        <Button Anchor="L,T" Size="896,32" Offset="0,0" Color="Black.0" StateOffsetIncrement="0,0" ID="Button">
            <ShowOnMouseOver>
                <AlphaAnim Anchor="L,T" Size="896,72" Pause="0" Cycle="Bounce" Speed="1" AlphaStart="1.5" AlphaEnd="1" ID="SelectHighlight">
                    <Grid Size="896,36" Offset="0,0" Padding="0,0" Style="Grid9FrameTurnsHL" />
                </AlphaAnim>
            </ShowOnMouseOver>
            <Stack StackGrowth="Right" Offset="0,0" Padding="0">
                <Box Anchor="L,T" Offset="8,8" Size="175,24" Color="Black.0">
                    <Label Anchor="L,T" Offset="10,0" Font="TwCenMT18" ColorSet="Beige_Black_Alpha" FontStyle="Shadow" String="MOD TITLE" ID="Title" />
                </Box>
            </Stack>
            <Image Anchor="L,B" Offset="0,0" TextureOffset="0.0" Texture="bar900x2.dds" Size="900.1" />
        </Button>
    </Instance>

    <Grid Size="960,658" Anchor="C,C" Offset="0,36" Padding="0,0" Style="Grid9DetailFive140" ConsumeMouse="0">
        <Image Anchor="L,T" Offset="17,90" Texture="HorizontalTrim.dds" Size="926.2" />
        <ScrollPanel Anchor="L,T" ID="ListingScrollPanel" Vertical="1" Size="912,440" Offset="13,94" AutoScrollBar="1">
        <ScrollBar Anchor="R,T" AnchorSide="O,I" Offset="0,18" Length="404" Style="VertSlider"/>
            <UpButton Anchor="R,T" AnchorSide="O,I" Offset="0,0" Style="ScrollBarUp"/>
            <DownButton Anchor="R,B" AnchorSide="O,I" Offset="0,0" Style="ScrollBarDown"/>
            <Stack ID="ListingStack" StackGrowth="B" Offset="4,0" Padding="0" />
        </ScrollPanel>
        <Label String="TXT_KEY_POP_MOD_LIST" ID="Notifications Label" Anchor="C,T" Offset="0,19" Font="TwCenMT20" Color0="30.50.80.255" Color1="133.184.186.255" Color2="133.184.186.255" FontStyle="SoftShadow" />
        <Box Anchor="C,B" AnchorSide="I.I" Offset="0,54" Size="910,56" Color="255,255,255,0" >
            <GridButton Anchor="L,B" Style="SmallButton" Size="150,32" Offset="14,0" StateOffsetIncrement="0,0" ID="CloseButton" >
                <Label Anchor="C,C" Offset="0,0" String="TXT_KEY_CLOSE" Font="TwCenMT18" ColorSet="Beige_Black_Alpha" FontStyle="Shadow" />
            </GridButton>
        </Box>
        <Image Anchor="C,B" Offset="0,110" Texture="HorizontalTrim.dds" Size="926.5" />
    </Grid>
</Context>
```

 在空文件里写出这么多东西很难。我确信有些人能够做到，但显然我不是其中一员。我在窗口文件中寻找和我想要做的窗口差不多的 XML 定义，然后复制，修改，添加，直到它看起来就是我想要的。我用 Notification 窗口作为这个窗口的基础，然后从 ModBrowser 相关文件中复制出我需要的 XML 片段（也就是你在主菜单里选择模组的地方）。由于《文明 5》所有的 UI 都是用 Lua 绘制的，因此能借鉴相应文件中的控件，制作自己的窗口。

以上定义了窗口，但还不能完成逻辑操作。这就是要用到 Lua。“ModList.lua” 文件包含了这些内容：

```lua
include( "InstanceManager" );
g_InstanceManager = InstanceManager:new( "ListingButtonInstance", "Button", Controls.ListingStack );

function OnPopup( popupInfo )
    -- <red>
    if( popupInfo.Type ~= ButtonPopupTypes.BUTTONPOPUP_NOTIFICATION_LOG ) then
        return;
    end
    if( popupInfo.Data1 ~= 999 ) then
        return;
    end
    -- </red>

    local unsortedInstalledMods = Modding.GetInstalledMods();
    g_InstanceManager:ResetInstances();
    for k, v in pairs(unsortedInstalledMods) do
        local modinfo = v;
        if(modinfo == nil) then
            break;
        end
        if(modinfo.Enabled) then
            local listing = g_InstanceManager:GetInstance();
            local str = modinfo.Name;
            if(modinfo.Version ~= "") then
                local version = string.format("version %s (%s)", modinfo.Version or "", modinfo.Stability or "");
                str = string.format("%s - %s", str, version)
            end
            listing.Title:SetText(str);
        end
    end

    UIManager:QueuePopup( ContextPtr, PopupPriority.NotificationLog );
end
-- <blue>
Events.SerialEventGameMessagePopup.Add( OnPopup );
-- </blue>

function OnClose ()
    UIManager:DequeuePopup( ContextPtr );
end
Controls.CloseButton:RegisterCallback( Mouse.eLClick, OnClose );
```

注意，以上代码很像我们修改的 Notification log 代码。像 Notification log popup 一样，它也在 SerialEventGameMessagePopup 中注册了（蓝色区域）。但是本例中,只有当类型是 BUTTONPOPUP_NOTIFICATION_LOG 并且 Data1 是 999 的时候（红色区域），我们才用 OnPopup 函数进行处理。

代码很简单。我从 ModBroser 的 Lua 文件中查找我需要的函数及其调用（这是我找到 Modding.GetInstalledMods() 函数以及返回 .Name 和 .Enabled 属性的方法）。实际上在显示窗口时 OnPopup 函数为每一个已安装并在该局游戏启用的模组添加了一条分割线。显示了每个模组名称，版本号和稳定性（实验性的，初始版，测试版，最终版）。

唯一添加的函数是控制关闭按钮的代码。这就是一个非常简单的添加新窗口的例子了。

![](civ5_imgs/page70.jpg)


### WorldBuilder

![](civ5_imgs/page70-2.jpg)

WorldBuilder 是针对《文明 5》而推出的一款地图编辑器。它可以帮你快速创造地图，也能帮助刚起步的团队开发复杂的场景。这一节将会讲一讲 Worldbuildier 里的选项，这样开发者就能快速编辑他们的地图场景了。

在文件栏顶部有一个 description 选项，这让你能够给地图命名，给它作个简单的介绍。

![](civ5_imgs/page71.jpg)

然后是队伍编辑器（team editor）。它允许场景创造者给该场景分配可用的文明以及起始科技，政策，金钱等等。在城市之前或者不是野蛮人的单位在该地图里可以被替换掉，需要在队伍编辑器中分配队伍。

![](civ5_imgs/page71-2.jpg)

WorldBuilder 里的地图编辑器（map editor）清楚直白。地图编辑器的工具箱包含各种工具，可以用这些标签页来创造一个有意思的地图：

- **Edit Plot** - 这允许选中的地块拥有任意地形，地貌，资源以及基础设施。这也是你设置玩家的开局地点的地方。
- **Plopper** - 这个标签页允许特征，资源基础设施，自然奇观和道路能够在该地块快速绘制。
- **Paint** - 这个标签页区分笔刷尺寸，设置地形，地貌，大洲设定，文化边界以及可见地形。
- **Rivers** - 如果你熟悉《文明 4》 WorldBuilder 中设置河流的过程（我敢担保完成它得烧支高香），那么你就能发现《文明 5》里得到了极大的改良。所有六边形的交界处支持设置河流，点击交界处就能决定河流是否应该流经这一边界。
- **Continents** - 设置大洲（美洲，亚洲，非洲和欧洲）的地方。
- **Units** - 只能放置野蛮人的单位，除非在队伍编辑器中添加了玩家。如果有玩家，那么这个标签页允许在该地图上设置所有单位，以及单位的状态（血量，强化，航运模式）。
- **Cities** - 需要在队伍编辑器中设置玩家，才能用这个标签。如果设置了玩家，就能在地图上放置城市，设置该城市的人口以及建筑。
- **Misc** - Misc 标签页允许覆盖世界，设置随机资源。

完成地图后，点击菜单的保存（Save）按钮来保存它。也能重新加载这个地图，做更多改变。WorldBuilder 的地图保存在 “`..\<My Games>\Sid Meier's Civilization V\Maps\`” 这个目录。

#### 保存文明 5 的地图

如果你喜欢正在使用的地图，或者认为它能够作为场景的良好基础，你可以直接在游戏里保存它。

![](civ5_imgs/page72.jpg)

移到保存游戏菜单，这里有个按钮 “Save Map”。

这将打开一个保存地图（Save Map）窗口，可以输入名字。点击保存（save）就能保存地图了。这个地图会被保存到 “`..\<My Games>\Sid Meier's Civilization V\Maps\`” 目录（WorldBuilder 保存文件的目录）。

![](civ5_imgs/page72-2.jpg)

#### 向模组中添加地图

把 ModBuddy 中的地图文件复制到你的模组中，就添加到模组里了。这个地图的名称和位置不重要。我建议在你项目的根目录下创建一个 “Map” 目录来存放地图文件，尽管这不是必须的。

![](civ5_imgs/page73.jpg)

当模组成功加载后，可以在选择菜单中选择你的地图。它看起来像游戏的普通地图。这个截图 Ireland 是模组中带的一个地图。

如果你在队伍编辑器中设置了玩家，那你会在选择窗口中看到一个“加载场景（Load Scenario）”的选项。如果该选项未被选中，加载地图时就没有自定的玩家，单位，城市等等。仅仅会用到这幅地图。如果选中了“加载场景”，那么只会加载该地图中的文明和首领。

![](civ5_imgs/page73-2.jpg)

### 发布模组

你需要用 ModBuddy 进行“构建（Build）”才能测试模组。构建就是取出你模组里的所有东西，然后将它们转换成游戏能够识别的格式。如果你想要让最新的修改生效的话，每次修改后都要构建模组。当你做出修改后，要经常构建和测试（而不是修改了一堆文件后，再来查看是哪个改动让游戏无法加载模组）。

要是你准备好与他人分享模组，就可以用两种不同的方式进行发布。

第一种方式是在游戏外部分发文件。模组浏览器是个很好的资源，但不是分享模组所必需的。你的模组会被放在 "`..\< My Games>\Sid Meier's Civilization V\MODS`" 目录下。你可以打包模组目录，然后分享给别人（那么这个人需要把这个压缩包解压到 "`..\< My Games>\Sid Meier's Civilization V\MODS`" 目录下）。

第二种方式是通过 ModBuddy 工具菜单下的“在线服务（Online Services）”选项。这会将你的模组上传到模组数据库中，然后别人就能在模组浏览器中看到它。

![](civ5_imgs/page74.jpg)

点击在线服务之后，通过这几个步骤就能上传模组了：

![](civ5_imgs/page74-2.jpg)

1. 登陆 Gamespy（如果需要的话）。

2. 点击在线服务窗口的上传（Upload）按钮。注意这个窗口会显示上传模组的状态，包括较早的版本。

![](civ5_imgs/page74-3.jpg)

3. 在选择模组窗口点击 “...” 按钮，浏览你要上传的模组。

4. 选择模组包。默认是在 `../My Documents/Firaxis ModBuddy/<ModName>/Packages` 目录下。如果该模组有多个版本，它们都会显示在这里。选择你想要上传的版本（一般是最新的版本）。

![](civ5_imgs/page74-4.jpg)

5. 等待模组上传。

6. 选择包含模组的目录。

好了，你的模组就被上传并能被其它玩家下载了。

要更新模组，修改模组属性里的版本号然后按照上述步骤重新上传就行了。

#### 兼容性

《文明 5》中的模组有三种级别的兼容性。

- **Nonexclusive** - 其它任何模组都可以与你的模组共同使用（默认）。
- **Partially Exclusive** - 被你的模组引用或者是引用了你的模组的其它所有模组都能被启用。
- **Totally Exclusive** - 只能启用你的模组所引用的模组。

大多数模组都是非排他的。这意味着他们可以与其它非排他性模组共用（部分排他或者完全排他的模组筛选这些模组）。如果你要做一个模组，添加新文明和新单位，那么你很有可能要做成一个非排他的模组。

部分排他性是整个转换模组的合适设定。如果你制作一个战争模组，又不想玩家使用西班牙文明，或者想要制作一个添加了 Firaxis 这个世界奇观的模组。如果你的模组删除了游戏的某些基础资源（移除了一个资源或者科技等等），你可能会想让这个模组是部分排他的，这是因为这些资源可能会被其它模组所使用。如果你或者其它模组作者设置了模组能够与你的这个模组一起运行，那么这些模组就能一起运行。

完全排他性是模组控制的最后一部分。你的模组不能与其它任何模组一起运行，除非你指定能够一起使用的模组。与部分排他性相比，它的唯一不同是防止其它模组作者让他们的模组能够与你的一起运行。这个设置用的很少，只有当与其它模组作者存在标记兼容性或者导致兼容性的问题才使用。

### 排除故障

如果你像我一样是个不善言谈的人,你会花 10% 的时间制作模组，然后用 90% 的时间找出它不能正常运行的问题。在 Firaxis 帮助我们制作模组提供的所有文件中，我在 debug 日志上花的时间最多。

debug 日志在 "..\My Games\Sid Meier's Civilization 5\Logs\" 目录下。这里有很多文件，我发现按照修改时间来排序文件很有效，能够看到哪个文件是最近修改的。我不会把这里的所有文件都讲到，讲的是我最常用到的两个文件。

#### Database.log

这个文件包含了游戏加载时出现的所有异常。这通常是指 XML 引用问题。如果一个文明引用了一个根本不存在的首领，或者一个单位引用了一个已经被移除的资源，那么 “Database.log” 就是一个很好的能找到问题所在的地方。

这也是个寻找错误类型的好地方。如果你拼写错了 LEADER_ELIZABETH，游戏加载模组时就会崩溃，这个日志文件会提供线索，告诉你是什么出错了。这里是 “Database.log” 文件中的一个错误案例：

```
[207898.601] Invalid Reference on Civilizations.Civilopedia - "TXT_KEY_CIV_RUSSIA_PEDIA" does not exist in Language_en_US
[207898.616] Invalid Reference on Feature_YieldChanges.FeatureType - "FEATURE_RIVER" does not exist in Features
```

#### Lua.log

就像 “Database.log” 文件帮你找出 XML 问题一样，“Lua.log” 帮你找错 Lua 错误。注意大多数情况（像下面这个例子），“Lua.log” 文件给出导致错误的文件以及行号。这对想要找出模组不能正常运行的原因很有帮助。

```
[202778.570] Syntax Error: [string "Lua/ModList.lua"]:165: 'end' expected (to close 'function' at line 14) near '<eof>'
[202778.570] Runtime Error: Error loading Lua/ModList.lua.
```
