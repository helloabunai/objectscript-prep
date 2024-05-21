# Configuration

Some nice mac tools. Not mentioning work-related docker information.

* [Rectangle](https://rectangleapp.com/): A tool for Windows-esque window snapping to a grid with keyboard shortcuts.
* [MacMediaKeyForwarder](https://github.com/quentinlesceller/macmediakeyforwarder/releases): Override media keys defaulting to iTunes/Apple Music for Spotify control.
* [DeepL](https://www.deepl.com/en/app): Useful + accurate ML-aided translation. CMD+C+C for in-line translation.
* [Karabiner-Elements](https://karabiner-elements.pqrs.org/): Custom keyboard shortcut/replacement macros.
* [iTerm2](https://iterm2.com): Super flexible terminal with lots of nice-to-haves.
* [Homebrew](https://brew.sh): Package management

# Creating a namespace

Assuming you have an instance of IRIS running somewhere, navigate to the management portal in your browser.

```
System Administration
 > Configuration
  > System Configuration
   > Namespaces
    > press button: Create New Namespace
```

On the page for creating namespaces, for local sandboxing:

```
Provide a name
 > Select an existing database for Globals: Create New Database
   > Provide a DB name
   > Database Directory:
    > Navigate to root /iris/databases, press Next
  > Leave defaults
  > Select an existing database for Routines: Choose created DB
  > Save
```

# Connecting to VSCode

To connect a VSCode workspace session to your IRIS server, you'll need the collection of extensions provided by InterSystems:

* InterSystems ObjectScript Extension Pack: `intersystems-community.objectscript-pack`

Containing:

* InterSystems Language Server: `intersystems.language-server`
* Intersystems ObjectScript: `intersystems-community.vscode-objectscript`
* Intersystems Server Manager: `intersystems-community.servermanager`

We need to add our server details to the extension. Select IRIS from the left panel and add a server following the VSCode prompts (your settings will vary depending on server/container):

```
Name of new server definition: free name choice, better to keep it simple
Optional description: self-explanatory
Hostname or IP address of web server: IRIS server address here, for me, localhost
Port: Associated port for traffic to above IRIS server
Optional path prefix of instance: self-explanatory
Username: self-explanatory
Connection type: http
```

On macos, saved IRIS servers will be stored in this file:
`/Users/username.here/Library/Application Support/Code/User/settings.json`. Look for the intersystems.servers json object.

Create a VSCode workspace. Now we want to connect this workspace to our IRIS server. From the IRIS extension panel, within `Explorer` select `Choose server & namespace`. This will raise more VSCode prompts for you to populate with information:

```
Choose folder: Lists vscode workspaces/folders. Choose your recently created one.
Pick intersystems server: Lists servers created in the previous step. Choose your recently created one.
Choose namespace from intersystems server: Lists namespaces available on the server, which we created at the start of this document.
```

Within the workspace, the server should populate a .vscode/settings.json file.

```
{
    "objectscript.conn": {
        "server": "local_server_address_resides_here",
        "ns": "namespace_you_created_resides_here",
        "active": true
    }
}
```

Your IDE is now connected to the IRIS server and will compile objectscript *.cls files on the server.