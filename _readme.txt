
lilsag (Lilly SameGame) - V1.0.XXX (RC)
---------------------------------------------------------
Produced by Hypersurge for Lilly
For more details please contact robert.fell@hypersurge.com


FILES
-----
This folder contains (* denotes not required for deployment):

"assets/*"                  - graphics, audio, font binary assets loaded by the game
"assets/__Config.xml"       - xml file containing in game text and configuration details - see below on how to optionally load this at runtime
"js/game.js"                - js file for the game (minified)
"js/pixijs.min.js"          - js library for display methods used by game.js (minified)
"js/simple-popup.min.js"    - js library for ui debug features (minified)
"_readme.txt" *             - this file
"index.html"                - html file containg the game canvas and style sheet
"manifest.appcache" *       - a list of all files used in this game - by default the manifest is disabled (to enable it set manifest="manifest.appcache" in index.html's <html> tag and ensure server is configured with correct mime type)
"manifest.json"             - a web-app manifest tells the browser about your Progressive Web App (PWA) and how it should behave when installed on the user's desktop or mobile device


All xml should be saved as unicode format (utf-8) for advanced ascii characters.


VERSION & ISSUE TRACKING
------------------------
To find the accurate version number (and corresponding repository revision) run the game and view the Developer Console in your desktop browser (usually F12).  The version will be visible at the top.


CONFIGURATION
-------------
By default the game has all configuration baked into its code.
This default configuration can be overridden at runtime by loading settings from a relative, same domain, file (e.g. "assets/__Config.xml" ).
To do so add a html attribute to index.html's "gameCanvas" element:

    config="assets/__Config.xml"

If the page is loaded with an additional "?lang=%languageCode%" parameter the game will ignore the attribute and attempt to load Config from the path: "assets/__Config_%languageCode%.xml".
For example, "index.html?lang=fr" will load "assets/__Config_fr.xml".  This allows multiple, language specific, config files to be loaded at runtime on demand.  Add more languages as required.

Due to security restrictions in most browsers local file loading (and crossdomain loading) is disabled.  Therefore custom configs will only work when the files are loaded from a webserver (i.e. http:// or https://).  For a lightweight local webserver search for "Mongoose webserver".
It is possible to chain multiple config files sequentially using the "settings.joinXml" node.  Each subsequent file will override existing nodes.
FullScreen mode is enabled by default for mobile devices, and disabled by default on desktop devices.  It attempts to resize the game canvas to 100%.  To override this add an XML node settings.fullScreen or add an html attribute to the gameCanvas.  Options are all, default, mobile, desktop.

    fullScreen="all"

NativeExperience mode is enabled by default for mobile devices.  It attempts to force the game into the JS FullScreen API, and lock orientation of the device (browser and device permitting).  To override this add an XML node settings.nativeExperience or add an html attribute to the gameCanvas.  Options are true, false.

    nativeExperience="true"

The JS FullScreen API will likely fail if using iframe(s) to embed the game into a website.  To fix this add the following attributes to all parent iframes:

    allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true" oallowfullscreen="true" msallowfullscreen="true"


TEXT FORMATTING
---------------
All text in the "__Config.xml" (and subsequent xml files) can be modified to suit your requirements.  The following characters are embedded as standard in the webfonts:

    abcdefghijklmnopqrstuvwxyz
    ABCDEFGHIJKLMNOPQRSTUVWXYZ
    1234567890
    \!"$%&*()-+=;:@'#,.?/
    ™®ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÑÒÓÔÕÖÙÚÛÜßàáâãäåæçèéêëìíîïñòóôõöùúûüŒœøØåÅ¿

For extended character ranges, new webfonts should be included via CSS and assigned by name in node "settings.font.name".  Please contact Hypersurge for creation of such files.
Please note each node's approximate length, and try and match - check thoroughly in game once changes have been made.
CDATA nodes can be used to fully embrace a node's value - e.g. <node><![CDATA[Insert text here]]></node>
Basic HTML tags can be used to extend the formatting of each text field.  E.g. for italics wrap a node's value with <i>italicised</i> (it is sometimes recommended to add an extra space before italicised text for clarity).  Use CDATA or escape the HTML - e.g.:

    <node>Normal text &lt;i&gt;italic text&lt;/i&gt;</node>


SPECIFIC NODES
--------------
The following nodes are required configuration settings (rather than text display).  They have the following names, purposes and ranges:

    "settings.joinXml"                            - chain xml files together (optional): will load xml file named in node after this current file is completed (keep on same domain)
    "settings.font.name"                          - the name of the font used in the game (must be a valid webfont and loaded from CSS)
    "settings.font.bold"                          - set to "true" to adjust the font used in the game to bold
    "settings.urls.website"                       - the url for all client website links
    "settings.googleAnalytics"                    - set this node's id attribute to a valid Google Analytics ID if tracking is required, and set enabled attribute to true
    "settings.audioHoldDelay"                     - how long an iOS device will wait for a touch event to initialise the audio system after preloader is completed (integer in ms)
    "settings.instructionsCompulsory"             - set enabled to "false" to allow players to skip having to read the instructions on first play
    "settings.sponsor"                            - set enabled to "false" to disable the in-game sponsor logo (the T1 logo)
    "settings.plainButtons"                       - set enabled to "false" to adopt more tactile and clearer to read game designed buttons
    "settings.audio"                              - set effects to "false" to mute sound effects on start, set music to "false" to mute music on start


TESTING
-------

Enable TestMode from the SelectLevel scene by holding keys "[" & "]" down while clicking the "level" button.  Test mode unlocks the following features:

1) "Finish Game" button ends the game as-is



GOOGLE ANALYTICS EVENTS
-----------------------

Google Analytics is only active if there is a valid GA tracker ID in "settings.googleAnalytics" XML node and "enabled" is "true".
Please ensure that Google Analytics Tracker code is also embedded in the html page.

    "scene"                                     - for each new scene (postfixed with scene type)
    "gameStart"                                 - game is played (postfixed with level type)
    "gameOver"                                  - game is finished (postfixed with level type and stars type)
    "gotoUrl"                                   - the CTA to website was called

