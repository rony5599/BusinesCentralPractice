Create a Business Central AL extension that adds a “Hello World” action to the Customer List page and shows a Message() popup when clicked.

Step-by-step plan (pseudocode)

Install VS Code + AL Language extension.

Create a new AL project with AL: Go! (targets your BC cloud/sandbox).

Download symbols (AL uses them to compile against the Base App).

Add a pageextension that extends Customer List and injects an action.

In the action trigger, call Message('Hello world...').

Press F5 (or AL: Publish) to deploy to your sandbox and test.

References: Microsoft “Get started with AL” + dev overview , adding actions to a page , Message method , launch.json purpose

Do this in VS Code

Install tooling

Install VS Code

Install the AL Language extension (ms-dynamics-smb.al)

Create a project

Ctrl+Shift+P → AL: Go!

Pick Business Central Cloud (or your target)

Sign in to your sandbox tenant

Download symbols

Ctrl+Shift+P → AL: Download Symbols

Add the Hello World action
Create a new file, e.g. src/CustomerListHelloWorld.PageExt.al, with the code below.

Run

Press F5 (uses .vscode/launch.json)

Open Customers list → click Hello World action → popup appears

// File: app.json
// NOTE: If you used "AL: Go!", keep the generated app.json values (runtime/application/platform).
// Only ensure name/publisher/version are what you want.
{
  "id": "b7c7e6e3-7af4-4b7a-b2e7-2c1d1b1f2a10",
  "name": "CC Hello World",
  "publisher": "CodeCopilot",
  "version": "1.0.0.0",
  "brief": "Hello World action on Customer List",
  "description": "Adds a Hello World action to the Customer List page and shows a message.",
  "privacyStatement": "",
  "EULA": "",
  "help": "",
  "url": "",
  "logo": "",
  "dependencies": [],
  "screenshots": [],
  "platform": "25.0.0.0",
  "application": "25.0.0.0",
  "idRanges": [
    {
      "from": 50100,
      "to": 50149
    }
  ],
  "resourceExposurePolicy": {
    "allowDebugging": true,
    "allowDownloadingSource": true,
    "includeSourceInSymbolFile": true
  },
  "runtime": "14.0",
  "target": "Cloud"
}

// File: .vscode/launch.json
// NOTE: If you used "AL: Go!", keep the generated launch.json and just update environment/tenant as needed.
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Business Central Sandbox",
      "type": "al",
      "request": "launch",
      "environmentType": "Sandbox",
      "environmentName": "Sandbox",
      "tenant": "common",
      "startupObjectType": "Page",
      "startupObjectId": 22,
      "breakOnError": "All",
      "launchBrowser": true
    }
  ]
}

// File: src/CustomerListHelloWorld.PageExt.al
pageextension 50100 "CC Customer List Hello" extends "Customer List"
{
    actions
    {
        addlast(Processing)
        {
            action("Hello World")
            {
                ApplicationArea = All;
                Caption = 'Hello World';
                Image = Information;

                trigger OnAction()
                begin
                    Message('Hello world from Business Central!');
                end;
            }
        }
    }
}