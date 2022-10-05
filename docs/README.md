# BBjAppLayout Widget

<p>
  <a href="http://www.basis.cloud/downloads">
    <img src="https://img.shields.io/badge/BBj-v22.04-blue" alt="BBj v22.04" />
  </a>
  <a href="https://github.com/BBj-Plugins/BBjAppLayout/blob/master/README.md">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="BBjAppLayout is released under the MIT license." />
  </a>
  <a href="https://github.com/necolas/issue-guidelines/blob/master/CONTRIBUTING.md#pull-requests">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome!" />
  </a>
   <a href="https://basishub.github.io/basis-next/#/dwc/bbj-app-layout">
    <img src="https://img.shields.io/badge/Component-bbj--app--layout-%23006aff" alt="Tag Name">
  </a>
</p>

<!-- Document Links: -->

[bbjtoplevelwindow]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjtoplevelwindow/bbjtoplevelwindow.htm
[bbjchildwindow]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjchildwindow/bbjchildwindow.htm
[bbj-icon-button]: https://basishub.github.io/basis-next/#/dwc/bbj-icon-button
[css-vars]: https://basishub.github.io/basis-next/#/theme-engine/css-variables
[viewport-meta-tag]: https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag
[media-query]: https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries
[bbjcontrol]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjwindow/bbjwindow.htm

`BBjAppLayout` is a custom BBj Widget for the DWC (Dynamic Web Client) which allows for building common application layouts.
The layout is responsive and it adjusts itself to fit the different screens sizes.

It consist of a header, a footer, a collapsible drawer and a content area. Each one of these sections is an instance of [`BBjChildWindow`][bbjchildwindow].
Usually the header and the footer are fixed, the drawer slides in and out of the viewport
and the content area is scrollable.

?> **Note:** A DWC application can have one and only one `BBjAppLayout` instance. Having multiple instances is not supported. Also the [`BBjTopLevelWindow`][bbjtoplevelwindow] should set the `$00001000$` flag to fill the viewport.

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

!> **Tip:** The demos uses the [`bbj-icon-button`][bbj-icon-button] web component to create a drawer toggle button. The button has the `data-drawer-toggle` attribute which instructs the `BBjAppLayout` to listen to click events coming from that component to toggle the drawer state.

```bbj
use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout

app! = new BBjAppLayout(wnd!)
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("bbj-toolbar")

REM - Add a toggle button which works independently in the browser
toolbar!.addStaticText("
:<html>
: <bbj-icon-button name='menu-2' data-drawer-toggle>
: </bbj-icon-button>
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
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--bbj-space-m) 0;
:    margin-bottom: var(--bbj-space-m);
:    border-bottom: thin solid var(--bbj-color-default)
: }
:
: .bbj-logo img {
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
toolbar!.addStyle("bbj-toolbar")
toolbar!.addStaticText("<html><bbj-icon-button name='menu-2' data-drawer-toggle></bbj-icon-button></html>")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("bbj-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")

empty! = drawer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<bbj-icon name='dashboard'></bbj-icon>      Dashboard"    , empty!)
drawerMenu!.addTab("<bbj-icon name='shopping-cart'></bbj-icon>  Orders"       , empty!)
drawerMenu!.addTab("<bbj-icon name='users'></bbj-icon>          Customers"    , empty!)
drawerMenu!.addTab("<bbj-icon name='box'></bbj-icon>            Products"     , empty!)
drawerMenu!.addTab("<bbj-icon name='files'></bbj-icon>          Documents"    , empty!)
drawerMenu!.addTab("<bbj-icon name='checklist'></bbj-icon>      Tasks"        , empty!)
drawerMenu!.addTab("<bbj-icon name='chart-dots-2'></bbj-icon>   Analytics"    , empty!)

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
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-drawer {
:   padding-top: var(--bbj-space-m)    
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .bbj-logo img {
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
toolbar!.addStyle("bbj-toolbar")
toolbar!.addStaticText("<html><bbj-icon-button name='menu-2' data-drawer-toggle></bbj-icon-button></html>")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("bbj-drawer")

empty! = drawer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<bbj-icon name='dashboard'></bbj-icon>      Dashboard"    , empty!)
drawerMenu!.addTab("<bbj-icon name='shopping-cart'></bbj-icon>  Orders"       , empty!)
drawerMenu!.addTab("<bbj-icon name='users'></bbj-icon>          Customers"    , empty!)
drawerMenu!.addTab("<bbj-icon name='box'></bbj-icon>            Products"     , empty!)
drawerMenu!.addTab("<bbj-icon name='files'></bbj-icon>          Documents"    , empty!)
drawerMenu!.addTab("<bbj-icon name='checklist'></bbj-icon>      Tasks"        , empty!)
drawerMenu!.addTab("<bbj-icon name='chart-dots-2'></bbj-icon>   Analytics"    , empty!)

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
toolbar!.addStyle("bbj-toolbar")

toolbar!.addStaticText("
:<html>
: <bbj-icon-button name='menu-2' data-drawer-toggle>
: </bbj-icon-button>
:</html>")

toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
empty! = secondToolbar!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())

menu! = secondToolbar!.addTabCtrl()
menu!.setAttribute("nobody","true")
menu!.setAttribute("borderless","true")
menu!.addTab("<bbj-icon name='report-money'></bbj-icon> Sales", empty!)
menu!.addTab("<bbj-icon name='building'></bbj-icon> Enterprise", empty!)
menu!.addTab("<bbj-icon name='credit-card'></bbj-icon> Payments", empty!)
menu!.addTab("<bbj-icon name='history'></bbj-icon> History", empty!)
```

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/multi-toolbars.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--bbj-space-m) 0;
:    margin-bottom: var(--bbj-space-m);
:    border-bottom: thin solid var(--bbj-color-default)
: }
:
: .bbj-logo img {
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
firstToolbar!.addStyle("bbj-toolbar")
firstToolbar!.addStaticText("<html><bbj-icon-button name='menu-2' data-drawer-toggle></bbj-icon-button></html>")
firstToolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
empty! = secondToolbar!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
secondToolbarMenu! = secondToolbar!.addTabCtrl()
secondToolbarMenu!.setAttribute("nobody","true")
secondToolbarMenu!.setAttribute("borderless","true")
secondToolbarMenu!.addTab("<bbj-icon name='report-money'></bbj-icon>  Sales"        , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='building'></bbj-icon>      Enterprise"   , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='credit-card'></bbj-icon>   Payments"     , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='history'></bbj-icon>       History"      , empty!)

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("bbj-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")

empty! = drawer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT, "onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<bbj-icon name='dashboard'></bbj-icon>      Dashboard"    , empty!)
drawerMenu!.addTab("<bbj-icon name='shopping-cart'></bbj-icon>  Orders"       , empty!)
drawerMenu!.addTab("<bbj-icon name='users'></bbj-icon>          Customers"    , empty!)
drawerMenu!.addTab("<bbj-icon name='box'></bbj-icon>            Products"     , empty!)
drawerMenu!.addTab("<bbj-icon name='files'></bbj-icon>          Documents"    , empty!)
drawerMenu!.addTab("<bbj-icon name='checklist'></bbj-icon>      Tasks"        , empty!)
drawerMenu!.addTab("<bbj-icon name='chart-dots-2'></bbj-icon>   Analytics"    , empty!)

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

It is possible to create a sticky toolbars using the [CSS custom property][css-vars] `--bbj-app-layout-header-collapse-height` and the
`BBjAppLayout::HeaderReveal` option.

When `BBjAppLayout::HeaderReveal` option is set to true then the header will be visible at first render then hidden when the user starts scrolling down. Once the user starts scrolling up again the header will be revealed.

With the help of the CSS custom property `--bbj-app-layout-header-collapse-height` it is possible to control how much of the header navbar will be hidden.

<div class="demo">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/sticky-toolbar.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: :root {   
:   --bbj-app-layout-header-collapse-height: 45px;
: }
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--bbj-space-m) 0;
:    margin-bottom: var(--bbj-space-m);
:    border-bottom: thin solid var(--bbj-color-default)
: }
:
: .bbj-logo img {
:    max-width: 100px;
: }
: 
: .bbj-content {
:   max-width: 600px;
:   margin: 0 auto; 
: }
:
: .bbj-card {
:    padding: var(--bbj-space-m);
:    margin: var(--bbj-space-m) 0;
:    border: thin solid var(--bbj-color-default);
:    border-radius: var(--bbj-border-radius-m);
:    background-color: var(--bbj-surface-3);
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
firstToolbar!.addStyle("bbj-toolbar")
firstToolbar!.addStaticText("<html><bbj-icon-button name='menu-2' data-drawer-toggle></bbj-icon-button></html>")
firstToolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

secondToolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
empty! = secondToolbar!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
secondToolbarMenu! = secondToolbar!.addTabCtrl()
secondToolbarMenu!.setAttribute("nobody","true")
secondToolbarMenu!.setAttribute("borderless","true")
secondToolbarMenu!.addTab("<bbj-icon name='report-money'></bbj-icon>  Sales"        , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='building'></bbj-icon>      Enterprise"   , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='credit-card'></bbj-icon>   Payments"     , empty!)
secondToolbarMenu!.addTab("<bbj-icon name='history'></bbj-icon>       History"      , empty!)

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("bbj-drawer")

logo! = drawer!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")

empty! = drawer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<bbj-icon name='dashboard'></bbj-icon>      Dashboard"    , empty!)
drawerMenu!.addTab("<bbj-icon name='shopping-cart'></bbj-icon>  Orders"       , empty!)
drawerMenu!.addTab("<bbj-icon name='users'></bbj-icon>          Customers"    , empty!)
drawerMenu!.addTab("<bbj-icon name='box'></bbj-icon>            Products"     , empty!)
drawerMenu!.addTab("<bbj-icon name='files'></bbj-icon>          Documents"    , empty!)
drawerMenu!.addTab("<bbj-icon name='checklist'></bbj-icon>      Tasks"        , empty!)
drawerMenu!.addTab("<bbj-icon name='chart-dots-2'></bbj-icon>   Analytics"    , empty!)

rem Content
rem ==================
content! = app!.getContent()
content!.addStyle("bbj-content")
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
    card!.addStyle("bbj-card")
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
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .bbj-logo img {
:    height: 24px;
: }
:
: @media (max-width: 600px) {  
:  bbj-tab::part(title) { 
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
toolbar!.addStyle("bbj-toolbar")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Footer
rem ==================
footer! = app!.getFooter()
footer!.addStyle("bbj-footer")

empty! = footer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
footerMenu! = footer!.addTabCtrl()
footerMenu!.setCallback(footerMenu!.ON_TAB_SELECT, "onPageChanged")
footerMenu!.setAttribute("nobody","true")
footerMenu!.setAttribute("borderless","true")
footerMenu!.setAttribute("placement","bottom")
footerMenu!.setAttribute("alignment","stretch")
footerMenu!.addTab("<bbj-icon expanse='s' name='dashboard'></bbj-icon>      <span part='title'>Dashboard</span>"    , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='shopping-cart'></bbj-icon>  <span part='title'>Orders</span>"       , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='users'></bbj-icon>          <span part='title'>Customers</span>"    , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='box'></bbj-icon>            <span part='title'>Products</span>"     , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='files'></bbj-icon>          <span part='title'>Documents</span>"    , empty!)

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
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
: }
:
: .bbj-logo img {
:    height: 24px;
: }
:
: .bbj-content {
:   max-width: 600px;
:   margin: 0 auto;
: }
:
: .bbj-card {
:    padding: var(--bbj-space-m);
:    margin: var(--bbj-space-m) 0;
:    border: thin solid var(--bbj-color-default);
:    border-radius: var(--bbj-border-radius-m);
:    background-color: var(--bbj-surface-3);
: }
:
: @media (max-width: 600px) {
:  bbj-tab::part(title) {
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
toolbar!.addStyle("bbj-toolbar")
logo! = toolbar!.addImageCtrl(imageManager!.loadImageFromFile("./assets/logo.png"))
logo!.addStyle("bbj-logo")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Footer
rem ==================
footer! = app!.getFooter()
footer!.addStyle("bbj-footer")

empty! = footer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
footerMenu! = footer!.addTabCtrl()
footerMenu!.setCallback(footerMenu!.ON_TAB_SELECT, "onPageChanged")
footerMenu!.setAttribute("nobody","true")
footerMenu!.setAttribute("borderless","true")
footerMenu!.setAttribute("placement","bottom")
footerMenu!.setAttribute("alignment","stretch")
footerMenu!.addTab("<bbj-icon expanse='s' name='dashboard'></bbj-icon>      <span part='title'>Dashboard</span>"    , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='shopping-cart'></bbj-icon>  <span part='title'>Orders</span>"       , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='users'></bbj-icon>          <span part='title'>Customers</span>"    , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='box'></bbj-icon>            <span part='title'>Products</span>"     , empty!)
footerMenu!.addTab("<bbj-icon expanse='s' name='files'></bbj-icon>          <span part='title'>Documents</span>"    , empty!)

rem Content
rem ==================
content! = app!.getContent()
content!.addStyle("bbj-content")
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
    card!.addStyle("bbj-card")
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