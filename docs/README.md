# BBjAppLayout Widget

<p>
  <a href="http://www.basis.cloud/downloads">
    <img src="https://img.shields.io/badge/BBj-v24.00-blue" alt="BBj v24.00" />
  </a>
  <a href="https://github.com/BBj-Plugins/BBjAppLayout/blob/master/README.md">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="BBjAppLayout is released under the MIT license." />
  </a>
  <a href="https://github.com/necolas/issue-guidelines/blob/master/CONTRIBUTING.md#pull-requests">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome!" />
  </a>
   <a href="https://basishub.github.io/basis-next/#/dwc/dwc-app-layout">
    <img src="https://img.shields.io/badge/Component-dwc--app--layout-%23006aff" alt="Tag Name">
  </a>
</p>

<!-- Document Links: -->

[bbjtoplevelwindow]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjtoplevelwindow/bbjtoplevelwindow.htm
[bbjchildwindow]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjchildwindow/bbjchildwindow.htm
[bbj-icon-button]: https://basishub.github.io/basis-next/#/dwc/dwc-icon-button
[css-vars]: https://basishub.github.io/basis-next/#/theme-engine/css-variables
[viewport-meta-tag]: https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag
[media-query]: https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries
[bbjcontrol]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjwindow/bbjwindow.htm

`BBjAppLayout` is a custom BBj Widget for the DWC (Dynamic Web Client) which allows for building common application layouts.
The layout is responsive and it adjusts itself to fit the different screens sizes.

It consist of a header, a footer, a collapsible drawer and a content area. Each one of these sections is by default an instance of [`BBjChildWindow`][bbjchildwindow].
Usually the header and the footer are fixed, the drawer slides in and out of the viewport
and the content area is scrollable.

?> **Note:** A DWC application can have one and only one `BBjAppLayout` instance. Having multiple instances is not supported. 

?> **Note:** The [`BBjTopLevelWindow`][bbjtoplevelwindow] should set the `$00001000$` flag to fill the viewport.
## Installation

- Clone the [project](https://github.com/BBj-Plugins/BBjAppLayout) locally , then add `BBjAppLayout` to your BBj paths
- Or [Use the plugins manager](https://www.bbj-plugins.com/en/get-started)

## Features

- Easy to set up
- Easy to customize
- Responsive
- Multiple layout options
- Works with the DWC Dark mode 🌒

And much more !

## The gist

The following sample shows how to use the `BBjAppLayout` widget. Each
part of the layout is an instance of [`BBjChildWindow`][bbjchildwindow] which can contain any valid [BBjControl][bbjcontrol].

For best results, The application should include a [viewport meta tag][viewport-meta-tag] which contains `viewport-fit=cover`. The meta tag causes the viewport to be scaled to fill the device display.

```bbj
web! = BBjAPI().getWebManager()
meta$ = "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no"
web!.setMeta("viewport", meta$)
```

!> **Tip:** The demos uses the [`dwc-icon-button`][dwc-icon-button] web component to create a drawer toggle button. The button has the `data-drawer-toggle` attribute which instructs the `BBjAppLayout` to listen to click events coming from that component to toggle the drawer state.

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

app! = new BBjAppLayout(wnd!)
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")

REM - Add a toggle button which works independently in the browser
toolbar!.addStaticText("
:<html>
: <dwc-icon-button name='menu-2' data-drawer-toggle>
: </dwc-icon-button>
:</html>")

toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")
```

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/basis-layout.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--dwc-space-m) 0;
:    margin-bottom: var(--dwc-space-m);
:    border-bottom: thin solid var(--dwc-color-default)
: }
:
: .dwc-logo img {
:    max-width: 100px;
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)

rem Header
rem ==================
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")
toolbar!.addStaticText("<html><dwc-icon-button name='menu-2' data-drawer-toggle></dwc-icon-button></html>")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("dwc-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")

drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<dwc-icon name='dashboard'></dwc-icon>      Dashboard"    , -1)
drawerMenu!.addTab("<dwc-icon name='shopping-cart'></dwc-icon>  Orders"       , -1)
drawerMenu!.addTab("<dwc-icon name='users'></dwc-icon>          Customers"    , -1)
drawerMenu!.addTab("<dwc-icon name='box'></dwc-icon>            Products"     , -1)
drawerMenu!.addTab("<dwc-icon name='files'></dwc-icon>          Documents"    , -1)
drawerMenu!.addTab("<dwc-icon name='checklist'></dwc-icon>      Tasks"        , -1)
drawerMenu!.addTab("<dwc-icon name='chart-dots-2'></dwc-icon>   Analytics"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStaticText("<html><h1>Application Title</h1></html>")
pageContent! = content!.addStaticText("<html><p>Content goes here</p></html>")

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageContent!.setText("<html><p>Content for " + title$ + " goes here</p></html>")
return

eoj:
release
```

  </div>
</div>

## Full width navbars

By default. `BBjAppLayout` renders the header and the footer in the `off-screen` mode. The `off-screen` mode means that the header and the footer position will be
shifted to fit beside the opened drawer. Disabling this mode will cause the header and footer to take the full available space and shift the drawer top and bottom position to fit with the header and the footer.

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

app! = new BBjAppLayout(wnd!)
app!.setHeaderOffscreen(0)
app!.setFooterOffscreen(0)
```

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/full-header.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
: }
:
: .dwc-drawer {
:   padding-top: var(--dwc-space-m)    
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .dwc-logo img {
:    height: 24px;
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)
app!.setHeaderOffscreen(0)

rem Header
rem ==================
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")
toolbar!.addStaticText("<html><dwc-icon-button name='menu-2' data-drawer-toggle></dwc-icon-button></html>")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("dwc-drawer")

drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<dwc-icon name='dashboard'></dwc-icon>      Dashboard"    , -1)
drawerMenu!.addTab("<dwc-icon name='shopping-cart'></dwc-icon>  Orders"       , -1)
drawerMenu!.addTab("<dwc-icon name='users'></dwc-icon>          Customers"    , -1)
drawerMenu!.addTab("<dwc-icon name='box'></dwc-icon>            Products"     , -1)
drawerMenu!.addTab("<dwc-icon name='files'></dwc-icon>          Documents"    , -1)
drawerMenu!.addTab("<dwc-icon name='checklist'></dwc-icon>      Tasks"        , -1)
drawerMenu!.addTab("<dwc-icon name='chart-dots-2'></dwc-icon>   Analytics"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStaticText("<html><h1>Application Title</h1></html>")
pageContent! = content!.addStaticText("<html><p>Content goes here</p></html>")

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageContent!.setText("<html><p>Content for " + title$ + " goes here</p></html>")
return

eoj:
release
```
  </div>
</div>

## Multiple toolbars

The navbar has no limit to the number of toolbars you can add. A toolbar is only a [`BBjChildWindow`][bbjchildwindow].

The following demo shows how to use two toolbars, The first one houses the drawer's toggle button and the application's title. The second toolbar houses a secondary navigation menu.

!> **Tip:** For applications that require multilevel or hierarchical navigation, use the drawer to house the first level and the secondary navigation items can be placed in a secondary toolbar inside the navbar.

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

app! = new BBjAppLayout(wnd!)
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")

toolbar!.addStaticText("
:<html>
: <dwc-icon-button name='menu-2' data-drawer-toggle>
: </dwc-icon-button>
:</html>")

toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
empty! = secondToolbar!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())

menu! = secondToolbar!.addTabCtrl()
menu!.setAttribute("nobody","true")
menu!.setAttribute("borderless","true")
menu!.addTab("<dwc-icon name='report-money'></dwc-icon> Sales", empty!)
menu!.addTab("<dwc-icon name='building'></dwc-icon> Enterprise", empty!)
menu!.addTab("<dwc-icon name='credit-card'></dwc-icon> Payments", empty!)
menu!.addTab("<dwc-icon name='history'></dwc-icon> History", empty!)
```

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/multi-toolbars.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--dwc-space-m) 0;
:    margin-bottom: var(--dwc-space-m);
:    border-bottom: thin solid var(--dwc-color-default)
: }
:
: .dwc-logo img {
:    max-width: 100px;
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)

rem Header
rem ==================
header! = app!.getHeader()
firstToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
firstToolbar!.addStyle("dwc-toolbar")
firstToolbar!.addStaticText("<html><dwc-icon-button name='menu-2' data-drawer-toggle></dwc-icon-button></html>")
firstToolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
secondToolbarMenu! = secondToolbar!.addTabCtrl()
secondToolbarMenu!.setAttribute("nobody","true")
secondToolbarMenu!.setAttribute("borderless","true")
secondToolbarMenu!.addTab("<dwc-icon name='report-money'></dwc-icon>  Sales"        , -1)
secondToolbarMenu!.addTab("<dwc-icon name='building'></dwc-icon>      Enterprise"   , -1)
secondToolbarMenu!.addTab("<dwc-icon name='credit-card'></dwc-icon>   Payments"     , -1)
secondToolbarMenu!.addTab("<dwc-icon name='history'></dwc-icon>       History"      , -1)

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("dwc-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")

drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT, "onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<dwc-icon name='dashboard'></dwc-icon>      Dashboard"    , -1)
drawerMenu!.addTab("<dwc-icon name='shopping-cart'></dwc-icon>  Orders"       , -1)
drawerMenu!.addTab("<dwc-icon name='users'></dwc-icon>          Customers"    , -1)
drawerMenu!.addTab("<dwc-icon name='box'></dwc-icon>            Products"     , -1)
drawerMenu!.addTab("<dwc-icon name='files'></dwc-icon>          Documents"    , -1)
drawerMenu!.addTab("<dwc-icon name='checklist'></dwc-icon>      Tasks"        , -1)
drawerMenu!.addTab("<dwc-icon name='chart-dots-2'></dwc-icon>   Analytics"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStaticText("<html><h1>Application Title</h1></html>")
pageContent! = content!.addStaticText("<html><p>Content goes here</p></html>")

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageContent!.setText("<html><p>Content for " + title$ + " goes here</p></html>")
return

eoj:
release
```
  </div>
</div>

## Sticky toolbars

A sticky toolbar is a toolbar that remains visible at the top of the page when the user scrolls down but the navbar height is collapsed to make more space available for the page's content. Usually this kind of toolbar contains a fixed navigation menu which is relevant to the current page.

It is possible to create a sticky toolbars using the [CSS custom property][css-vars] `--dwc-app-layout-header-collapse-height` and the
`BBjAppLayout::HeaderReveal` option.

When `BBjAppLayout::HeaderReveal` option is set to true then the header will be visible at first render then hidden when the user starts scrolling down. Once the user starts scrolling up again the header will be revealed.

With the help of the CSS custom property `--dwc-app-layout-header-collapse-height` it is possible to control how much of the header navbar will be hidden.

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/sticky-toolbar.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: :root {   
:   --dwc-app-layout-header-collapse-height: 45px;
: }
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--dwc-space-m) 0;
:    margin-bottom: var(--dwc-space-m);
:    border-bottom: thin solid var(--dwc-color-default)
: }
:
: .dwc-logo img {
:    max-width: 100px;
: }
: 
: .dwc-content {
:   max-width: 600px;
:   margin: 0 auto; 
: }
:
: .dwc-card {
:    padding: var(--dwc-space-m);
:    margin: var(--dwc-space-m) 0;
:    border: thin solid var(--dwc-color-default);
:    border-radius: var(--dwc-border-radius-m);
:    background-color: var(--dwc-surface-3);
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)
app!.setHeaderReveal(1)

rem Header
rem ==================
header! = app!.getHeader()
firstToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
firstToolbar!.addStyle("dwc-toolbar")
firstToolbar!.addStaticText("<html><dwc-icon-button name='menu-2' data-drawer-toggle></dwc-icon-button></html>")
firstToolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
secondToolbarMenu! = secondToolbar!.addTabCtrl()
secondToolbarMenu!.setAttribute("nobody","true")
secondToolbarMenu!.setAttribute("borderless","true")
secondToolbarMenu!.addTab("<dwc-icon name='report-money'></dwc-icon>  Sales"        , -1)
secondToolbarMenu!.addTab("<dwc-icon name='building'></dwc-icon>      Enterprise"   , -1)
secondToolbarMenu!.addTab("<dwc-icon name='credit-card'></dwc-icon>   Payments"     , -1)
secondToolbarMenu!.addTab("<dwc-icon name='history'></dwc-icon>       History"      , -1)

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("dwc-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")

drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<dwc-icon name='dashboard'></dwc-icon>      Dashboard"    , -1)
drawerMenu!.addTab("<dwc-icon name='shopping-cart'></dwc-icon>  Orders"       , -1)
drawerMenu!.addTab("<dwc-icon name='users'></dwc-icon>          Customers"    , -1)
drawerMenu!.addTab("<dwc-icon name='box'></dwc-icon>            Products"     , -1)
drawerMenu!.addTab("<dwc-icon name='files'></dwc-icon>          Documents"    , -1)
drawerMenu!.addTab("<dwc-icon name='checklist'></dwc-icon>      Tasks"        , -1)
drawerMenu!.addTab("<dwc-icon name='chart-dots-2'></dwc-icon>   Analytics"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStyle("dwc-content")
pageHeader! = content!.addStaticText("<html><h1>Application Title</h1></html>")

lorem! = "
:Lorem Ipsum is simply dummy text of the printing and typesetting 
:industry. Lorem Ipsum has been the industry's standard dummy 
:text ever since the 1500s when an unknown printer took a galley 
:of type and scrambled it to make a type specimen book. It has 
:survived not only five centuries, but also the leap into electronic 
:typesetting, remaining essentially unchanged. It was popularised 
:in the 1960s with the release of Letraset sheets containing Lorem 
:Ipsum passages, and more recently with desktop publishing software
: like Aldus PageMaker including versions of Lorem Ipsum.
:"

for i=1 to 10
    card! = content!.addChildWindow("", $00108000$, sysgui!.getAvailableContext())
    card!.addStyle("dwc-card")
    cardTitle! = card!.addStaticText("<html><h2>What is Lorem Ipsum (" + str(i) +")?</h2></html>")
    cardContent! = card!.addStaticText("<html><p>" + lorem! + "</p></html>")
next 

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageHeader!.setText("<html><h1>" + title$ + " Page</h1></html>")
return

eoj:
release
```

  </div>
  </div>

## Mobile Navigation Layout

The bottom navbar can be used to provide a different version of the navigation at the bottom of application.
This type of navigation is specifically popular in mobile apps.

!> **Tip:** Notice how the drawer is hidden in the following demo. The `BBjAppLayout` widget supports three drawer positions: `DRAWER_PLACEMENT_LEFT`, `DRAWER_PLACEMENT_RIGHT` and `DRAWER_PLACEMENT_HIDDEN`.

<div class="demo demo--phone ">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/mobile-footer.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons demo__buttons--hasFooter">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .dwc-logo img {
:    height: 24px;
: }
:
: @media (max-width: 600px) {  
:  dwc-tab::part(title) { 
:     display: none;
:   }    
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)
app!.setHeaderOffscreen(0)
app!.setDrawerPlacement(BBjAppLayout.DRAWER_PLACEMENT_HIDDEN)

rem Header
rem ==================
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Footer
rem ==================
footer! = app!.getFooter()
footer!.addStyle("dwc-footer")

footerMenu! = footer!.addTabCtrl()
footerMenu!.setCallback(footerMenu!.ON_TAB_SELECT, "onPageChanged")
footerMenu!.setAttribute("nobody","true")
footerMenu!.setAttribute("borderless","true")
footerMenu!.setAttribute("placement","bottom")
footerMenu!.setAttribute("alignment","stretch")
footerMenu!.addTab("<dwc-icon expanse='s' name='dashboard'></dwc-icon>      <span part='title'>Dashboard</span>"    , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='shopping-cart'></dwc-icon>  <span part='title'>Orders</span>"       , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='users'></dwc-icon>          <span part='title'>Customers</span>"    , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='box'></dwc-icon>            <span part='title'>Products</span>"     , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='files'></dwc-icon>          <span part='title'>Documents</span>"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStaticText("<html><h1>Application Title</h1></html>")
pageContent! = content!.addStaticText("<html><p>Content goes here</p></html>")

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageContent!.setText("<html><p>Content for " + title$ + " goes here</p></html>")
return

eoj:
release
```
  </div>
</div>

## Footer Reveal

Same as `BBjAppLayout:HeaderReveal`, `BBjAppLayout:FooterReveal` is supported. When `BBjAppLayout::FooterReveal` option is set to true then the footer will be visible at first render then hidden when the user starts scrolling up. Once the user starts scrolling down again the footer will be revealed.

<div class="demo demo--phone">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/footer-reveal.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons demo__buttons--hasFooter">
    <a  data-action-toggle-code>
      <dwc-icon name="chevron-down"></dwc-icon> Show code
    </a>
    <a data-action-open-link>
      <dwc-icon name="external-link"></dwc-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .dwc-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--dwc-space-m);
:    padding: 0 var(--dwc-space-m);
:    background-color: var(--dwc-color-primary);
:    color: var(--dwc-color-on-primary-text);
: }
:
: .dwc-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .dwc-logo img {
:    height: 24px;
: }
:
: .dwc-content {
:   max-width: 600px;
:   margin: 0 auto; 
: }
:    
: .dwc-card {
:    padding: var(--dwc-space-m);
:    margin: var(--dwc-space-m) 0;
:    border: thin solid var(--dwc-color-default);
:    border-radius: var(--dwc-border-radius-m);
:    background-color: var(--dwc-surface-3);
: }
:    
: @media (max-width: 600px) {  
:  dwc-tab::part(title) { 
:     display: none;
:   }    
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")
imageManager! = sysgui!.getImageManager()

wnd! = sysgui!.addWindow("BBjAppLayout", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)
app!.setFooterReveal(1)
app!.setFooterShadow(BBjAppLayout.SHADOW_SCROLL)
app!.setDrawerPlacement(BBjAppLayout.DRAWER_PLACEMENT_HIDDEN)

rem Header
rem ==================
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("dwc-toolbar")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("dwc-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Footer
rem ==================
footer! = app!.getFooter()
footer!.addStyle("dwc-footer")

footerMenu! = footer!.addTabCtrl()
footerMenu!.setCallback(footerMenu!.ON_TAB_SELECT, "onPageChanged")
footerMenu!.setAttribute("nobody","true")
footerMenu!.setAttribute("borderless","true")
footerMenu!.setAttribute("placement","bottom")
footerMenu!.setAttribute("alignment","stretch")
footerMenu!.addTab("<dwc-icon expanse='s' name='dashboard'></dwc-icon>      <span part='title'>Dashboard</span>"    , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='shopping-cart'></dwc-icon>  <span part='title'>Orders</span>"       , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='users'></dwc-icon>          <span part='title'>Customers</span>"    , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='box'></dwc-icon>            <span part='title'>Products</span>"     , -1)
footerMenu!.addTab("<dwc-icon expanse='s' name='files'></dwc-icon>          <span part='title'>Documents</span>"    , -1)

rem Content
rem ==================
content! = app!.getContent()
content!.addStyle("dwc-content")
pageHeader! = content!.addStaticText("<html><h1>Application Title</h1></html>")

lorem! = "
:Lorem Ipsum is simply dummy text of the printing and typesetting 
:industry. Lorem Ipsum has been the industry's standard dummy 
:text ever since the 1500s when an unknown printer took a galley 
:of type and scrambled it to make a type specimen book. It has 
:survived not only five centuries, but also the leap into electronic 
:typesetting, remaining essentially unchanged. It was popularised 
:in the 1960s with the release of Letraset sheets containing Lorem 
:Ipsum passages, and more recently with desktop publishing software
: like Aldus PageMaker including versions of Lorem Ipsum.
:"

for i=1 to 10
    card! = content!.addChildWindow("", $00108000$, sysgui!.getAvailableContext())
    card!.addStyle("dwc-card")
    cardTitle! = card!.addStaticText("<html><h2>What is Lorem Ipsum (" + str(i) +")?</h2></html>")
    cardContent! = card!.addStaticText("<html><p>" + lorem! + "</p></html>")
next 

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageHeader!.setText("<html><h1>" + title$ + " Page</h1></html>")
return


eoj:
release
```

  </div>
</div>

## Drawer Breakpoint

Be default, when the screen width is `800px` or less , the drawer will be switched to `popover` mode. This is called the breakpoint. The popover mode means that the drawer will pop over the content area with an overlay. It is possible to configure the breakpoint by using the `BBjAppLayout:setDrawerBreakpoint` method and the breakpoint must be a valid [media query][media-query].

For instance, in the following sample. we configure the drawer breakpoint to be `500px` or less.

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

app! = new BBjAppLayout(wnd!)
app!.setDrawerBreakpoint("(max-width: 500px)")
```

<div class="demo demo--phone">
  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/drawer-breakpoint.html"></iframe>
    </div>
  </div>
</div>

## Events

The `BBjAppLayout` supports three events:

1. `ON_DRAWER_OPENED`: Fired when the drawer is opened.
2. `ON_DRAWER_CLOSED`: Fired when the drawer is closed.
3. `ON_DRAWER_TOGGLED`: Fired when the drawer is toggled.

The event's payload is an instance of the `::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayoutEvent`

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayoutEvent

app! = new BBjAppLayout(wnd!)
app!.setCallback(BBjAppLayout.ON_DRAWER_TOGGLED, "onDrawerToggled")

onDrawerToggled:
  declare auto BBjAppLayoutEvent payload!
  event! = sysgui!.getLastEvent()
  payload! = event!.getObject()

  let x = MSGBOX("Is Opened = " + str(payload!.isDrawerOpened()), 0, "Drawer Toggled")
return 
```