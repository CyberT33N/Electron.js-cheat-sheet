# Electron.js Cheat Sheet



## Good 2 Know
- **Use directly [electron-vite](https://github.com/CyberT33N/electron-vite-cheat-sheet/blob/main/README.md)**














<br><br>
<br><br>
___
<br><br>
<br><br>



# package.json

<details><summary>Click to expand..</summary>

```javascript
{
  "name": "secure-file-vault",
  "version": "1.0.0",
  "description": "A modern Electron.js application for secure file encryption and compression",
  "main": "main.js",
  "scripts": {
    "start": "DISPLAY=:0 electron . --no-sandbox",
    "dev": "electron . --no-sandbox --debug"
  },
  "devDependencies": {
    "electron": "^28.1.0"
  },
  "dependencies": {
    "material-components-web": "^14.0.0",
    "@mdi/font": "^7.4.47",
    "node-7z": "^3.0.0"
  }
}
```
- On ubuntu you may must use DISPLAY=:0 in order that the app is starting


</details>
































<br><br>
<br><br>
________
________
<br><br>
<br><br>

# Debugging

<details><summary>Click to expand..</summary>

# Chrome Extensions

<details><summary>Click to expand..</summary>


You can use:
- https://github.com/MarshallOfSound/electron-devtools-installer

Or for local import:
```typescript
try {
    const extensionPathBrowserTools = path.join(__dirname, '../../chrome-extension/browser-tools')
    await session.defaultSession.loadExtension(extensionPathBrowserTools, {
        allowFileAccess: true
    })

    console.info('Extension geladen')
} catch (err) {
    console.error('Fehler beim Laden der Extension:', err)
}
```

</details>





</details>





















<br><br>
<br><br>
________
________
<br><br>
<br><br>

# External dependencies

<details><summary>Click to expand..</summary>







# electron-devtools-installer
- https://github.com/MarshallOfSound/electron-devtools-installer

<details><summary>Click to expand..</summary>

Electron DevTools Installer
---------------------------

![CircleCI](https://img.shields.io/circleci/build/github/MarshallOfSound/electron-devtools-installer?style=for-the-badge)
[![npm](https://img.shields.io/npm/v/electron-devtools-installer?style=for-the-badge)](https://www.npmjs.com/package/electron-devtools-installer)
![npm](https://img.shields.io/npm/dt/electron-devtools-installer?style=for-the-badge)
[![license](https://img.shields.io/github/license/GPMDP/electron-devtools-installer.svg?maxAge=2592000&style=for-the-badge)](https://github.com/GPMDP/electron-devtools-installer/blob/master/LICENSE)
[![CFA Enabled](https://img.shields.io/badge/CFA-Enabled-success?style=for-the-badge)](https://github.com/continuousauth)

This is an easy way to install DevTool extensions into Electron.  You shouldn't
have to mess around with downloading the extension, finding the right folder and
then configuring the path for everyone's machines.

## Install

```
npm install electron-devtools-installer --save-dev
```
or
```
yarn add electron-devtools-installer -D
```

## Usage
All you have to do now is this in the **main** process of your application.

```js
import { installExtension, REDUX_DEVTOOLS } from 'electron-devtools-installer';
// Or if you can not use ES6 imports
/**
const { default: installExtension, REACT_DEVELOPER_TOOLS } = require('electron-devtools-installer');
*/
const { app } = require('electron');

app.whenReady().then(() => {
    installExtension(REDUX_DEVTOOLS)
        .then((ext) => console.log(`Added Extension:  ${ext.name}`))
        .catch((err) => console.log('An error occurred: ', err));
});
```

To install multiple extensions, `installExtension` takes an array.

```typescript
app.whenReady().then(() => {
    installExtension([REDUX_DEVTOOLS, REACT_DEVELOPER_TOOLS])
        .then(([redux, react]) => console.log(`Added Extensions:  ${redux.name}, ${react.name}`))
        .catch((err) => console.log('An error occurred: ', err));
});
```

### Local Files

If you want your DevTools extensions to work on local `file://` URLs (e.g. loaded via `browserWindow.loadFile()`), don't forget to set `allowFileAccess` in the options passed to `installExtension`.

```typescript
installExtension(REDUX_DEVTOOLS, { loadExtensionOptions: { allowFileAccess: true } })
```

For more information see the [Electron documentation](https://www.electronjs.org/docs/latest/api/session#sesloadextensionpath-options).

## What extensions can I use?

Technically you can use whatever extension you want.  Simply find the ChromeStore ID
of the extension you want to install, and call `installExtension('YOUR_ID_HERE')`.  We
offer a few extension ID's inside the package so you can easily import them to install without
having to find them yourselves.

```js
import {
  installExtension,
  EMBER_INSPECTOR, REACT_DEVELOPER_TOOLS,
  BACKBONE_DEBUGGER, JQUERY_DEBUGGER,
  VUEJS_DEVTOOLS, VUEJS_DEVTOOLS_BETA,
  REDUX_DEVTOOLS, MOBX_DEVTOOLS,
  APOLLO_DEVELOPER_TOOLS,
} from 'electron-devtools-installer';
```

## How does it work?

Well, you know those steps over in the [Electron Docs](https://github.com/electron/electron/blob/master/docs/tutorial/devtools-extension.md)
that involve downloading, copying, checking paths, Etc.

This does all of that for you, it downloads the chrome extension directly from
the Chrome WebStore.  Then it extracts it to your applications `userData` directory
before loading it into Electron.

</details>


















<br><br>
<br><br>

# Framework

## electron-vite
- https://electron-vite.org/guide/



















<br><br>
<br><br>


# Distrubution

## electron-builder
- https://github.com/CyberT33N/electron-builder-cheat-sheet




















<br><br>
<br><br>

# Hot-Reload

## electron-reloader (HOT Reload)
- **It is recommended to use electron-vite instead which supports hot reload**

<details><summary>Click to expand..</summary>

- Add to your main file:
```javascript
// Enable hot reload for development
try {
  require('electron-reloader')(module, {
    debug: true,
    watchRenderer: true
  });
} catch (_) { console.log('Error loading electron-reloader'); }
```

Then as usually run npm start:
```javascript
 "scripts": {
    "start": "DISPLAY=:0 electron . --no-sandbox",
    "dev": "DISPLAY=:0 electron . --no-sandbox --inspect"
  },
```

</details>



























<br><br>
<br><br>

# Storage


## electron-store

<details><summary>Click to expand..</summary>

## 📦 Installation
```bash
npm install electron-store
```

## 💡 Funktionsweise
- Speichert automatisch die Fensterposition und -größe
- Stellt die letzte Position beim Neustart wieder her
- Verwendet `electron-store` für persistente Speicherung
- Reagiert auf 'resize' und 'move' Events

## 🔧 Implementierung

```javascript
import { BrowserWindow, ipcMain, screen } from 'electron'
import Store from 'electron-store'
import path from 'path'

export class WindowManager {
    constructor(isDev, preloadPath) {
        this.mainWindow = null
        this.isDev = isDev
        this.preloadPath = preloadPath
        // Store-Instanz initialisieren
        this.store = new Store()
    }

    createWindow() {
        const primaryDisplay = screen.getPrimaryDisplay()
        const { width, height } = primaryDisplay.workAreaSize

        // Gespeicherte Fensterposition laden
        const windowBounds = this.store.get('windowBounds', {
            width: width,
            height: height,
            x: undefined,
            y: undefined
        })

        // Fenster mit gespeicherten Bounds erstellen
        this.mainWindow = new BrowserWindow({
            ...windowBounds,
            frame: false,
            webPreferences: {
                nodeIntegration: false,
                contextIsolation: true,
                preload: this.preloadPath
            }
        })

        // Position bei Änderungen speichern
        ['resize', 'move'].forEach(eventName => {
            this.mainWindow.on(eventName, () => {
                const bounds = this.mainWindow.getBounds()
                this.store.set('windowBounds', bounds)
            })
        })

        // ... Rest der Methode
    }
}
```

## 📝 Wichtige Komponenten

### 1. Store Initialisierung
```javascript
import Store from 'electron-store'
this.store = new Store()
```

### 2. Fensterposition Laden
```javascript
const windowBounds = this.store.get('windowBounds', {
    width: width,
    height: height,
    x: undefined,
    y: undefined
})
```

### 3. Position Speichern
```javascript
['resize', 'move'].forEach(eventName => {
    this.mainWindow.on(eventName, () => {
        const bounds = this.mainWindow.getBounds()
        this.store.set('windowBounds', bounds)
    })
})
```

## 🔍 Gespeicherte Daten
Die Fensterdaten werden in folgendem Format gespeichert:
```javascript
{
    windowBounds: {
        width: number,    // Fensterbreite
        height: number,   // Fensterhöhe
        x: number,       // horizontale Position
        y: number        // vertikale Position
    }
}
```

## 💪 Vorteile
- Automatische Persistenz
- Nahtlose Benutzererfahrung
- Einfache Implementation
- Kein manuelles State Management nötig

## 🚀 Verwendung
Einfach die `WindowManager`-Klasse instanziieren und `createWindow()` aufrufen - der Rest geschieht automatisch!


</details>




















</details>



























<br><br>
<br><br>
________
________
<br><br>
<br><br>



# Exec

<details><summary>Click to expand..</summary>
  
<br><br>

## Run external cli with sudo and ask user for password prompt
- Will ask for password. You can use this aswell in electron.js
```javascript
const sudo = require('sudo-prompt');

const sudoOptions = {
    name: 'Secure File Vault'
};

// Promisified sudo exec
function sudoExec(command) {
    return new Promise((resolve, reject) => {
        sudo.exec(command, sudoOptions, (error, stdout, stderr) => {
            if (error) reject(error);
            else resolve(stdout);
        });
    });
}

await sudoExec(`veracrypt --text --create "${containerPath}" --size "${containerSize}" --password "${password}" --encryption AES --hash sha512 --filesystem FAT --non-interactive`);
```

</details>





















<br><br>
____
____
<br><br>

# API

<details><summary>Click to expand..</summary>




## dialog
- https://www.electronjs.org/docs/latest/api/dialog
- Display native system dialogs for opening and saving files, alerting, etc.

<details><summary>Click to expand..</summary>

```javascript
  mainWindow.on('close', async(event) => {
        if (hasQuit) return // Skip if we've already started quitting
        event.preventDefault() // Prevent window from closing immediately
        
        // Show native dialog box
        await dialog.showMessageBox(mainWindow, {
            type: 'warning',
            title: 'Administrator Rechte',
            message: 'Administratorrechte werden benötigt um VeraCrypt Einträge zu bereinigen',
            buttons: ['OK'],
            defaultId: 0,
            noLink: true
        })
        
        hasQuit = true
        app.quit()
    })

```


</details>















  
<br><br>
<br><br>

## shell
- https://www.electronjs.org/docs/latest/api/shell

<details><summary>Click to expand..</summary>



# showItemInFolder
- https://www.electronjs.org/docs/latest/api/shell#shellshowiteminfolderfullpath

<details><summary>Click to expand..</summary>

1. preload.js - Bridge zwischen Renderer und Main Process
```javascript
const { contextBridge, ipcRenderer, shell } = require('electron')

contextBridge.exposeInMainWorld('electron', {
    // Andere API Methoden...
    send: (channel, data) => {
        ipcRenderer.send(channel, data)
    },
    receive: (channel, func) => {
        const subscription = (event, ...args) => func(...args)
        ipcRenderer.on(channel, subscription)
        return () => ipcRenderer.removeListener(channel, subscription)
    }
})
```

2. main.js - Main Process Handler
```javascript
import { app, ipcMain, shell } from 'electron'
import fs from 'fs-extra'

// IPC Handler für das Öffnen von Ordnern
ipcMain.on('open-folder', async (event, filePath) => {
    try {
        if (!fs.existsSync(filePath)) {
            console.error('Datei existiert nicht:', filePath)
            event.reply('open-folder-error', 'Datei existiert nicht')
            return
        }
        shell.showItemInFolder(filePath)
    } catch (error) {
        console.error('Fehler beim Öffnen des Ordners:', error)
        event.reply('open-folder-error', error.message)
    }
})
```

3. React Komponente
```javascript
import React from 'react'

export const MyComponent = React.memo(({ filePath }) => {
    // Error Handler für Fehler beim Öffnen
    React.useEffect(() => {
        if (!window.electron) return

        const cleanup = window.electron.receive('open-folder-error', (errorMessage) => {
            console.error('Fehler beim Öffnen des Ordners:', errorMessage)
        })

        return () => cleanup && cleanup()
    }, [])

    // Click Handler für den Button
    const handleOpenFolder = React.useCallback((path) => {
        if (!window.electron) {
            console.error('Electron API nicht verfügbar')
            return
        }
        window.electron.send('open-folder', path)
    }, [])

    return (
        <button
            onClick={() => handleOpenFolder(filePath)}
            title="Open containing folder"
        >
            <i className="fas fa-folder-open"></i>
        </button>
    )
})
```

4. Font Awesome einbinden (für das Icon)
```css
// In index.html:
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

// 5. Optional: CSS für den Button
.open-folder-button {
    background: linear-gradient(145deg, rgba(64, 192, 255, 0.1), rgba(64, 192, 255, 0.2));
    color: #40c0ff;
    border: 1px solid rgba(64, 192, 255, 0.3);
    padding: 0.4em 0.8em;
    border-radius: 0.3em;
    font-size: 0.8rem;
    transition: all 0.3s ease;
    cursor: pointer;
}

.open-folder-button:hover {
    background: linear-gradient(145deg, rgba(64, 192, 255, 0.2), rgba(64, 192, 255, 0.3));
    box-shadow: 0 0 15px rgba(64, 192, 255, 0.2);
}
```
</details>





</details>
















  
<br><br>
<br><br>

## ipcRenderer
- https://www.electronjs.org/de/docs/latest/api/ipc-renderer
- The ipcRenderer module is an EventEmitter. It provides a few methods so you can send synchronous and asynchronous messages from the render process (web page) to the main process. You can also receive replies from the main process.

<details><summary>Click to expand..</summary>


# Example for drag and drop
- When you click the button #process-button the click event listener will trigger and use `ipcRenderer.send()` to send a message to the event `process-files` which ist started inside of main.js

<details><summary>Click to expand..</summary>

main.js
```
const { app, BrowserWindow, ipcMain } = require('electron');
const path = require('path');
const { exec } = require('child_process');
const fs = require('fs');

let mainWindow = null;

const createWindow = () => {
    mainWindow = new BrowserWindow({
        width: 1200,
        height: 800,
        webPreferences: {
            nodeIntegration: true,
            contextIsolation: false,
            enableRemoteModule: true
        },
        backgroundColor: '#000000',
        show: false,
        frame: false
    });

    mainWindow.loadFile('index.html');
    mainWindow.once('ready-to-show', () => {
        mainWindow.show();
        mainWindow.focus();
    });

    mainWindow.on('closed', () => {
        mainWindow = null;
    });
};

if (app) {
    app.whenReady().then(createWindow);

    app.on('window-all-closed', () => {
        if (process.platform !== 'darwin') {
            app.quit();
        }
    });

    app.on('activate', () => {
        if (mainWindow === null) {
            createWindow();
        }
    });

    // Handle window controls
    ipcMain.on('minimize-window', () => {
        if (mainWindow) mainWindow.minimize();
    });

    ipcMain.on('maximize-window', () => {
        if (mainWindow) {
            if (mainWindow.isMaximized()) {
                mainWindow.unmaximize();
            } else {
                mainWindow.maximize();
            }
        }
    });

    ipcMain.on('close-window', () => {
        if (mainWindow) mainWindow.close();
    });

    // Handle file encryption and compression
    ipcMain.on('process-files', async (event, { files, containerSize }) => {
        try {
            // Implementation for VeraCrypt container creation and file processing
            // This is where we'll add the VeraCrypt CLI integration
            event.reply('process-status', { status: 'Processing files...' });
        } catch (error) {
            event.reply('process-error', { error: error.message });
        }
    });
}
```

index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>Secure File Vault</title>
    <link href="css/styles.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</head>
<body>
    <div class="app-container">
        <div class="header">
            <h1>CMCU</h1>
            <div class="window-controls">
                <button class="control-button minimize">−</button>
                <button class="control-button maximize">□</button>
                <button class="control-button close">×</button>
            </div>
        </div>

        <div class="main-content">
            <div class="upload-section">
                <div class="dropzone" id="dropzone">
                    <div class="dropzone-content">
                        <div class="upload-icon"></div>
                        <p>Drag and drop files here</p>
                        <p>or</p>
                        <button class="upload-button">Choose Files</button>
                    </div>
                </div>
                <div class="file-info">
                    <div class="size-display">
                        <span>Total Size:</span>
                        <span id="total-size">0 B</span>
                    </div>
                    <div class="file-list" id="file-list"></div>
                </div>
            </div>

            <div class="settings-panel">
                <h2>Cloud Integration</h2>
                <div class="cloud-providers">
                    <button class="provider-button coming-soon" disabled>
                        <span class="provider-icon google-drive"></span>
                        Google Drive
                        <span class="coming-soon-badge">Coming Soon</span>
                    </button>
                    <button class="provider-button coming-soon" disabled>
                        <span class="provider-icon dropbox"></span>
                        Dropbox
                        <span class="coming-soon-badge">Coming Soon</span>
                    </button>
                </div>
            </div>

            <div class="action-buttons">
                <button id="process-button" class="primary-button" disabled>
                    Process Files
                    <div class="button-gradient"></div>
                </button>
            </div>
        </div>
    </div>
    <script src="js/renderer.js"></script>
</body>
</html>
```

style.css
```
:root {
    --primary-gradient: linear-gradient(135deg, #FFA500, #FFD700);
    --hover-gradient: linear-gradient(135deg, #FFD700, #FFA500);
    --background-color: #000000;
    --text-color: #FFFFFF;
    --border-radius: 12px;
    --transition-speed: 0.3s;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Inter', sans-serif;
}

body {
    background-color: var(--background-color);
    color: var(--text-color);
    height: 100vh;
    overflow: hidden;
}

.app-container {
    height: 100vh;
    display: flex;
    flex-direction: column;
}

.header {
    -webkit-app-region: drag;
    padding: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: rgba(255, 255, 255, 0.05);
}

.window-controls {
    -webkit-app-region: no-drag;
}

.control-button {
    background: none;
    border: none;
    color: var(--text-color);
    padding: 0.5rem;
    cursor: pointer;
    transition: var(--transition-speed);
}

.main-content {
    flex: 1;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.dropzone {
    border: 2px dashed rgba(255, 165, 0, 0.3);
    border-radius: var(--border-radius);
    padding: 3rem;
    text-align: center;
    transition: var(--transition-speed);
    background: rgba(255, 255, 255, 0.02);
}

.dropzone.drag-over {
    border-color: #FFA500;
    background: rgba(255, 165, 0, 0.1);
}

.upload-button {
    background: var(--primary-gradient);
    border: none;
    padding: 1rem 2rem;
    border-radius: var(--border-radius);
    color: var(--background-color);
    font-weight: 600;
    cursor: pointer;
    transition: var(--transition-speed);
    position: relative;
    overflow: hidden;
}

.upload-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 15px rgba(255, 165, 0, 0.3);
}

.file-info {
    margin-top: 2rem;
    background: rgba(255, 255, 255, 0.05);
    padding: 1rem;
    border-radius: var(--border-radius);
}

.settings-panel {
    background: rgba(255, 255, 255, 0.02);
    padding: 2rem;
    border-radius: var(--border-radius);
}

.cloud-providers {
    display: flex;
    gap: 1rem;
    margin-top: 1rem;
}

.provider-button {
    background: rgba(255, 255, 255, 0.05);
    border: none;
    padding: 1rem;
    border-radius: var(--border-radius);
    color: var(--text-color);
    display: flex;
    align-items: center;
    gap: 0.5rem;
    cursor: not-allowed;
    position: relative;
    overflow: hidden;
}

.coming-soon-badge {
    background: var(--primary-gradient);
    padding: 0.2rem 0.5rem;
    border-radius: 12px;
    font-size: 0.8rem;
    color: var(--background-color);
}

.primary-button {
    background: var(--primary-gradient);
    border: none;
    padding: 1rem 3rem;
    border-radius: var(--border-radius);
    color: var(--background-color);
    font-weight: 600;
    cursor: pointer;
    transition: var(--transition-speed);
    position: relative;
    overflow: hidden;
}

.primary-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.button-gradient {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: var(--hover-gradient);
    opacity: 0;
    transition: var(--transition-speed);
}

.primary-button:hover .button-gradient {
    opacity: 1;
}

@keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.05); }
    100% { transform: scale(1); }
}

.upload-icon {
    width: 64px;
    height: 64px;
    margin: 0 auto 1rem;
    background: var(--primary-gradient);
    mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M9 16h6v-6h4l-7-7-7 7h4v6zm-4 2h14v2H5v-2z'/%3E%3C/svg%3E");
    -webkit-mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M9 16h6v-6h4l-7-7-7 7h4v6zm-4 2h14v2H5v-2z'/%3E%3C/svg%3E");
    mask-size: contain;
    -webkit-mask-size: contain;
    mask-repeat: no-repeat;
    -webkit-mask-repeat: no-repeat;
}
```

renderer.js
```
const { ipcRenderer } = require('electron');

let selectedFiles = [];

// Dropzone functionality
const dropzone = document.getElementById('dropzone');
const fileList = document.getElementById('file-list');
const totalSizeDisplay = document.getElementById('total-size');
const processButton = document.getElementById('process-button');

// Window control buttons
document.querySelector('.minimize').addEventListener('click', () => {
    ipcRenderer.send('minimize-window');
});

document.querySelector('.maximize').addEventListener('click', () => {
    ipcRenderer.send('maximize-window');
});

document.querySelector('.close').addEventListener('click', () => {
    ipcRenderer.send('close-window');
});

// Prevent default drag behaviors
['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
    dropzone.addEventListener(eventName, preventDefaults, false);
    document.body.addEventListener(eventName, preventDefaults, false);
});

function preventDefaults(e) {
    e.preventDefault();
    e.stopPropagation();
}

// Highlight drop zone when dragging files over it
['dragenter', 'dragover'].forEach(eventName => {
    dropzone.addEventListener(eventName, highlight, false);
});

['dragleave', 'drop'].forEach(eventName => {
    dropzone.addEventListener(eventName, unhighlight, false);
});

function highlight(e) {
    dropzone.classList.add('drag-over');
}

function unhighlight(e) {
    dropzone.classList.remove('drag-over');
}

// Handle dropped files
dropzone.addEventListener('drop', handleDrop, false);

function handleDrop(e) {
    const dt = e.dataTransfer;
    const files = dt.files;
    handleFiles(files);
}

// Handle file selection via button
document.querySelector('.upload-button').addEventListener('click', () => {
    const input = document.createElement('input');
    input.type = 'file';
    input.multiple = true;
    input.onchange = e => {
        handleFiles(e.target.files);
    };
    input.click();
});

function handleFiles(files) {
    selectedFiles = [...files];
    updateFileList();
    updateTotalSize();
    processButton.disabled = selectedFiles.length === 0;
}

function updateFileList() {
    fileList.innerHTML = '';
    selectedFiles.forEach(file => {
        const fileElement = document.createElement('div');
        fileElement.className = 'file-item';
        fileElement.textContent = `${file.name} (${formatSize(file.size)})`;
        fileList.appendChild(fileElement);
    });
}

function updateTotalSize() {
    const totalSize = selectedFiles.reduce((acc, file) => acc + file.size, 0);
    totalSizeDisplay.textContent = formatSize(totalSize);
}

function formatSize(bytes) {
    const units = ['B', 'KB', 'MB', 'GB'];
    let size = bytes;
    let unitIndex = 0;
    
    while (size >= 1024 && unitIndex < units.length - 1) {
        size /= 1024;
        unitIndex++;
    }
    
    return `${size.toFixed(2)} ${units[unitIndex]}`;
}

// Process files button
processButton.addEventListener('click', () => {
    const totalSize = selectedFiles.reduce((acc, file) => acc + file.size, 0);
    ipcRenderer.send('process-files', {
        files: selectedFiles.map(f => f.path),
        containerSize: totalSize * 1.1 // 10% larger than total file size
    });
});

// Handle process status updates
ipcRenderer.on('process-status', (event, { status }) => {
    // Update UI with status
    console.log(status);
});

ipcRenderer.on('process-error', (event, { error }) => {
    // Show error in UI
    console.error(error);
});
```

</details>











</details>






















<br><br>
<br><br>

## ipcMain
- https://www.electronjs.org/docs/latest/api/ipc-main
- The ipcMain module is an Event Emitter. When used in the main process, it handles asynchronous and synchronous messages sent from a renderer process (web page). Messages sent from a renderer will be emitted to this module.

<details><summary>Click to expand..</summary>



## Custom Listener
```
// Handle file encryption and compression
    ipcMain.on('process-files', async (event, { files, password }) => {
        try {
            //..
            event.reply('process-status', { 
                status: 'complete',
                message: 'Files have been successfully encrypted and compressed.'
            });
            
        } catch (error) {
            console.error('Error in process-files:', error);
            event.reply('process-error', { 
                error: error.message,
                details: error.stack
            });
        }
    });
```
- You can use `event` to display alerts in the UI










<br><br>

### minimize-window
```javascript
// Handle window controls
ipcMain.on('minimize-window', () => {
    if (mainWindow) mainWindow.minimize();
});
```

<br><br>

### maximize-window
```javascript
ipcMain.on('maximize-window', () => {
    if (mainWindow) {
        if (mainWindow.isMaximized()) {
            mainWindow.unmaximize();
        } else {
            mainWindow.maximize();
        }
    }
});
```


<br><br>

### close-window
```javascript
ipcMain.on('close-window', () => {
    if (mainWindow) mainWindow.close();
});
```


</details>



















<br><br>
<br><br>

## app
- https://www.electronjs.org/docs/latest/api/app

<details><summary>Click to expand..</summary>

```javascript
const { app } = require('electron')
app.on('window-all-closed', () => {
  app.quit()
})
```

<br><br>


### Events

#### before-quit
- Do something when electron app is closed
```javascript
 app.on('before-quit', () => {
    try {
        if (fs.existsSync('/tmp/CMCU')) {
            fs.removeSync('/tmp/CMCU');
        }
    } catch (error) {
        console.error('Error cleaning up /tmp/CMCU:', error);
    }
});
```

<br><br>

#### window-all-closed
```javascript
app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') {
        app.quit();
    }
});
```

<br><br>

#### activate [MAC]
- https://www.electronjs.org/docs/latest/api/app#event-activate-macos
- Emitted when the application is activated. Various actions can trigger this event, such as launching the application for the first time, attempting to re-launch the application when it's already running, or clicking on the application's dock or taskbar icon.
```javascript
app.on('activate', () => {
    if (mainWindow === null) {
        createWindow();
    }
});
```




<br><br>

### whenReady
```javascript
app.whenReady().then(fn);
```



</details>















<br><br>
<br><br>
<br><br>
<br><br>

## contextBridge
- https://www.electronjs.org/docs/latest/api/context-bridge
- Create a safe, bi-directional, synchronous bridge across isolated contexts


<details><summary>Click to expand..</summary>

# Electron.js Cheat Sheet: Context Bridge

## Was ist `contextBridge`?

Die `contextBridge` API von Electron ermöglicht eine sichere Kommunikation zwischen der **Main Process** und der **Renderer Process**, indem sie eine kontrollierte Schnittstelle für den Zugriff auf Node.js-Funktionen bereitstellt.

## Warum `contextBridge` nutzen?

- **Verhindert direkte Node.js-Zugriffe** im Renderer (bessere Sicherheit)
- **Ermöglicht eine sichere IPC-Kommunikation** zwischen Main und Renderer
- **Schützt vor unsicheren Code-Einschleusungen** (z. B. durch `remote`)

---

## Beispiel: Sichere Kommunikation mit `contextBridge`

### 1. **Preload-Skript (preload.js)**
Hier definieren wir eine `window.api` Schnittstelle, die vom Renderer sicher genutzt werden kann.

```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('api', {
  sendMessage: (channel, data) => ipcRenderer.send(channel, data),
  onMessage: (channel, callback) => ipcRenderer.on(channel, (event, ...args) => callback(...args))
});
```

---

### 2. **Main Process (main.js)**
Hier reagieren wir auf IPC-Events, die vom Renderer über `contextBridge` gesendet werden.

```javascript
const { app, BrowserWindow, ipcMain } = require('electron');

let mainWindow;

app.whenReady().then(() => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: __dirname + '/preload.js',
      contextIsolation: true, // Muss aktiviert sein
      enableRemoteModule: false, // Empfohlen zur Sicherheit - Should be default in latest versions
      nodeIntegration: false // Kein direkter Zugriff auf Node.js
    }
  });

  mainWindow.loadURL('file://' + __dirname + '/index.html');
});

ipcMain.on('message-from-renderer', (event, data) => {
  console.log('Nachricht vom Renderer:', data);
  event.reply('reply-from-main', 'Antwort vom Main-Prozess');
});
```

---

### 3. **Renderer Process (index.html + script.js)**
Hier nutzen wir die `window.api`-Schnittstelle, um mit dem Main Process zu kommunizieren.

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>Electron ContextBridge</title>
</head>
<body>
  <button id="send">Nachricht senden</button>
  <p id="response"></p>

  <script>
    document.getElementById('send').addEventListener('click', () => {
      window.api.sendMessage('message-from-renderer', 'Hallo Main!');
    });

    window.api.onMessage('reply-from-main', (response) => {
      document.getElementById('response').innerText = response;
    });
  </script>
</body>
</html>
```

---

## Fazit

✅ `contextBridge` verbessert die Sicherheit, indem es eine isolierte API für Renderer bereitstellt.  
✅ Es verhindert unsicheren direkten Zugriff auf Node.js im Renderer.  
✅ Die Kombination mit `ipcRenderer` und `ipcMain` ermöglicht eine sichere Kommunikation.  

Nutze **`contextBridge`** immer, wenn der Renderer mit dem Main Process interagieren muss! 🚀


</details>









































<br><br>
<br><br>
<br><br>
<br><br>

## BrowserWindow
- https://www.electronjs.org/docs/latest/api/browser-window


<details><summary>Click to expand..</summary>

```javascript
// In the main process.
const { BrowserWindow } = require('electron')

const win = new BrowserWindow({ width: 800, height: 600 })

// Load a remote URL
win.loadURL('https://github.com')

// Or load a local HTML file
win.loadFile('index.html')
```






# Start full width and height
```javascript
let mainWindow = null

function createWindow() {
    const primaryDisplay = screen.getPrimaryDisplay()
    const { width, height } = primaryDisplay.workAreaSize

    mainWindow = new BrowserWindow({
        width: width,
        height: height,
        frame: false,
        webPreferences: {
            nodeIntegration: false,
            contextIsolation: true,
            preload: path.join(__dirname, 'preload.js')
        }
    })

    // Load the app
    if (isDev) {
        mainWindow.loadURL('http://localhost:5173')
        mainWindow.webContents.openDevTools()
    } else {
        mainWindow.loadFile(path.join(__dirname, 'dist', 'index.html'))
    }
}
```





<br><br>

# BrowserWindow Constructor Options

## webPreferences (WebPreferences) - Optional
Settings of web page's features:

- **devTools** (boolean) - Enable DevTools. Default: `true`
- **nodeIntegration** (boolean) - Enable node integration. Default: `false`
- **nodeIntegrationInWorker** (boolean) - Enable node integration in web workers. Default: `false`
- **nodeIntegrationInSubFrames** (boolean) - Experimental option for Node.js support in sub-frames (iframes/child windows)
- **preload** (string) - Script loaded before other page scripts. Requires absolute file path
- **sandbox** (boolean) - Sandbox the renderer, making it compatible with Chromium OS-level sandbox
- **session** (Session) - Sets page session. Preferred over partition option
- **partition** (string) - Sets session by partition string. Use `persist:` prefix for persistent sessions
- **zoomFactor** (number) - Default page zoom factor (1.0 = 100%). Default: `1.0`
- **javascript** (boolean) - Enable JavaScript support. Default: `true`
- **webSecurity** (boolean) - Enable same-origin policy. Default: `true`
- **allowRunningInsecureContent** (boolean) - Allow HTTPS pages to run content from HTTP. Default: `false`
- **images** (boolean) - Enable image support. Default: `true`
- **imageAnimationPolicy** (string) - Image animation behavior (`animate`|`animateOnce`|`noAnimation`). Default: `animate`
- **textAreasAreResizable** (boolean) - Make TextArea elements resizable. Default: `true`
- **webgl** (boolean) - Enable WebGL support. Default: `true`
- **plugins** (boolean) - Enable plugins. Default: `false`
- **experimentalFeatures** (boolean) - Enable Chromium experimental features. Default: `false`
- **scrollBounce** (boolean) - Enable scroll bounce on macOS. Default: `false`
- **enableBlinkFeatures** (string) - Comma-separated feature list to enable
- **disableBlinkFeatures** (string) - Comma-separated feature list to disable

#### Default Font Settings
- **defaultFontFamily** (Object)
  - `standard`: Times New Roman
  - `serif`: Times New Roman
  - `sansSerif`: Arial
  - `monospace`: Courier New
  - `cursive`: Script
  - `fantasy`: Impact
  - `math`: Latin Modern Math

#### Font Sizes
- **defaultFontSize** (Integer) - Default: `16`
- **defaultMonospaceFontSize** (Integer) - Default: `13`
- **minimumFontSize** (Integer) - Default: `0`

#### Additional Settings
- **defaultEncoding** (string) - Default: `ISO-8859-1`
- **backgroundThrottling** (boolean) - Throttle background animations/timers. Default: `true`
- **contextIsolation** (boolean) - Run Electron APIs in separate JavaScript context. Default: `true`
- **webviewTag** (boolean) - Enable `<webview>` tag. Default: `false`
- **spellcheck** (boolean) - Enable built-in spellchecker. Default: `true`
- **enableWebSQL** (boolean) - Enable WebSQL API. Default: `true`

#### Security & Dialog Options
- **safeDialogs** (boolean) - Enable consecutive dialog protection. Default: `false`
- **safeDialogsMessage** (string) - Message for consecutive dialog protection
- **disableDialogs** (boolean) - Disable all dialogs. Default: `false`
- **navigateOnDragDrop** (boolean) - Navigate on file/link drag and drop. Default: `false`

#### Advanced Options
- **v8CacheOptions** (string) - V8 code caching policy (`none`|`code`|`bypassHeatCheck`|`bypassHeatCheckAndEagerCompile`)
- **enablePreferredSizeMode** (boolean) - Enable preferred size mode. Default: `false`
- **transparent** (boolean) - Enable background transparency. Default: `true`




















<br><br>
<br><br>

### Events

<br><br>

#### ready-to-show
```javascript
win.once('ready-to-show', () => {
    mainWindow.show();
    mainWindow.focus();
});
```

<br><br>

#### close
- Will be triggered when the x button is pressed. The Windows session is programmatacily still avaialble and can be used
```javascript
win.on('closed', () => {
    mainWindow = null;
});
```








</details>



</details>




























<br><br>
<br><br>
___
<br><br>
<br><br>




# Distrubution


<details><summary>Click to expand..</summary>





Der Unterschied zwischen **Electron Forge** und **electron-builder** liegt hauptsächlich in ihrem Ansatz für das Packaging und die Distribution von Electron-Apps:

### **Electron Forge**  
🔹 **Ziel**: All-in-One-Tool zur Entwicklung, Verpackung und Veröffentlichung von Electron-Apps.  
🔹 **Vorteile**:
   - Bietet eine **strukturierte Projektvorlage** mit Boilerplates.
   - Unterstützt **verschiedene Kompilationssysteme** (z. B. Webpack, TypeScript).
   - Integrierte **Updater- und Publisher-Funktionen** für GitHub, S3, etc.
   - **Modular aufgebaut**: Plugins für verschiedene Build-Systeme.  
🔹 **Nachteile**:
   - Weniger Anpassungsmöglichkeiten bei den generierten Installern.  
   - **Nicht so ausgereift** wie electron-builder für komplexe Deployment-Szenarien.  

---

### **electron-builder**  
🔹 **Ziel**: Hochgradig konfigurierbares Tool zur Paketierung und Verteilung von Electron-Apps.  
🔹 **Vorteile**:
   - **Unterstützt viele Formate**: `.exe`, `.dmg`, `.deb`, `.AppImage`, Snap, etc.  
   - Sehr **anpassbare Konfiguration** über `package.json` oder `electron-builder.yml`.  
   - **Integrierter Auto-Update-Support** für eigene Server, GitHub Releases, S3, etc.  
   - **Schneller und stabiler** bei der Erstellung großer Builds.  
🔹 **Nachteile**:
   - Keine vorgefertigten Templates oder Dev-Tools wie Electron Forge.  
   - **Komplexere Konfiguration** erforderlich.  

---

### **Fazit**  
📌 **Nutze Electron Forge**, wenn du eine **einfache, standardisierte Lösung** suchst, um eine Electron-App zu entwickeln, zu packagen und zu veröffentlichen.  
📌 **Nutze electron-builder**, wenn du eine **maximal anpassbare Lösung** für die Erstellung von Installern und die Verteilung deiner App brauchst.  

💡 **Kombination möglich**: Electron Forge kann **electron-builder als Plugin** nutzen, um die besten Features beider Tools zu vereinen! 🚀












<br><br>
<br><br>


## electron-builder
- https://www.npmjs.com/package/electron-builder
- https://www.electron.build/



<details><summary>Click to expand..</summary>

1. You can create a configuration file electron-builder.yml for electron-builder with the content below.
```yaml
appId: com.electron.app
productName: vue-ts
directories:
  buildResources: build
files:
  - '!**/.vscode/*'
  - '!src/*'
  - '!electron.vite.config.{js,ts,mjs,cjs}'
  - '!{.eslintignore,.eslintrc.cjs,.prettierignore,.prettierrc.yaml,dev-app-update.yml,CHANGELOG.md,README.md}'
  - '!{.env,.env.*,.npmrc,pnpm-lock.yaml}'
  - '!{tsconfig.json,tsconfig.node.json,tsconfig.web.json}'
asarUnpack:
  - resources/**
afterSign: build/notarize.js
win:
  executableName: vue-ts
nsis:
  artifactName: ${name}-${version}-setup.${ext}
  shortcutName: ${productName}
  uninstallDisplayName: ${productName}
  createDesktopShortcut: always
mac:
  entitlementsInherit: build/entitlements.mac.plist
  extendInfo:
    - NSCameraUsageDescription: Application requests access to the device's camera.
    - NSMicrophoneUsageDescription: Application requests access to the device's microphone.
    - NSDocumentsFolderUsageDescription: Application requests access to the user's Documents folder.
    - NSDownloadsFolderUsageDescription: Application requests access to the user's Downloads folder.
dmg:
  artifactName: ${name}-${version}.${ext}
linux:
  target:
    - AppImage
    - snap
    - deb
  maintainer: electronjs.org
  category: Utility
appImage:
  artifactName: ${name}-${version}.${ext}
npmRebuild: false
publish:
  provider: generic
  url: https://example.com/auto-updates
```

2. Add the scripts key to the package.json:
```javascript
"scripts": {
  "build:win": "npm run build && electron-builder --win --config",
  "build:mac": "npm run build && electron-builder --mac --config",
  "build:linux": "npm run build && electron-builder --linux --config"
}
```



</details>











<br><br>
<br><br>



## Electron Forge 
- https://www.electronforge.io/


<details><summary>Click to expand..</summary>

1. You can create a configuration file forge.config.cjs for Electron Forge with the content below.
```javascript
module.exports = {
  packagerConfig: {
    ignore: [
      /^\/src/,
      /(.eslintrc.json)|(.gitignore)|(electron.vite.config.ts)|(forge.config.cjs)|(tsconfig.*)/,
    ],
  },
  rebuildConfig: {},
  makers: [
    {
      name: '@electron-forge/maker-squirrel',
      config: {},
    },
    {
      name: '@electron-forge/maker-zip',
      platforms: ['darwin'],
    },
    {
      name: '@electron-forge/maker-deb',
      config: {},
    },
    {
      name: '@electron-forge/maker-rpm',
      config: {},
    },
  ],
};
```

2. Add the scripts and dependencies to the package.json:
```javascript
"main": "./dist/main/index.js",
"scripts": {
  "start": "electron-vite preview --outDir=dist",
  "dev": "electron-vite dev --outDir=dist",
  "package": "electron-vite build --outDir=dist && electron-forge package",
  "make ": "electron-vite build --outDir=dist && electron-forge make"
},
"devDependencies": {
  "@electron-forge/cli": "^6.2.1",
  "@electron-forge/maker-deb": "^6.2.1",
  "@electron-forge/maker-rpm": "^6.2.1",
  "@electron-forge/maker-squirrel": "^6.2.1",
  "@electron-forge/maker-zip": "^6.2.1",
}
```


  
</details>






</details>






































<br><br>
<br><br>
___
<br><br>
<br><br>





# Testing

<details><summary>Click to expand..</summary>





<br><br>
<br><br>


# E2E testing


<details><summary>Click to expand..</summary>

# Playwright
- https://playwright.dev/docs/api/class-electron
- https://www.electronjs.org/es/docs/latest/tutorial/automated-testing

<details><summary>Click to expand..</summary>

# 📦 Installation
```
# Windows
set PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1; npm install --save-dev playwright  @playwright/test

# Linux
PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1 npm install --save-dev playwright  @playwright/test

```


## 🚀 App starten

```js
const { _electron: electron } = require('playwright');
const { test } = require('@playwright/test');

test('launch app', async () => {
  const electronApp = await electron.launch({ args: ['main.js'] });
  await electronApp.close();
});
```

---

## 🔍 Main Process Zugriff

```js
test('get isPackaged', async () => {
  const electronApp = await electron.launch({ args: ['main.js'] });

  const isPackaged = await electronApp.evaluate(async ({ app }) => {
    return app.isPackaged;
  });

  console.log(isPackaged); // false in Dev-Mode
  await electronApp.close();
});
```

---

## 🖼️ Screenshot der ersten Window

```js
test('save screenshot', async () => {
  const electronApp = await electron.launch({ args: ['main.js'] });
  const window = await electronApp.firstWindow();

  await window.screenshot({ path: 'intro.png' });
  await electronApp.close();
});
```

---

## ✅ Komplettes Testbeispiel

`example.spec.js`

```js
const { _electron: electron } = require('playwright');
const { test, expect } = require('@playwright/test');

test('example test', async () => {
  const electronApp = await electron.launch({ args: ['.'] });

  const isPackaged = await electronApp.evaluate(async ({ app }) => {
    return app.isPackaged;
  });

  expect(isPackaged).toBe(false);

  const window = await electronApp.firstWindow();
  await window.screenshot({ path: 'intro.png' });

  await electronApp.close();
});
```

---

## 🧪 Tests ausführen

```bash
npx playwright test
```



## 📦 `electron.launch(options?)`

### 🔧 Options (optional)

| Option              | Type                                                                                         | Description                               |
| ------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------- |
| `args`              | `string[]`                                                                                   | Main script args (e.g. `['main.js']`)     |
| `cwd`               | `string`                                                                                     | Working directory                         |
| `env`               | `Record<string, string>`                                                                     | Environment vars (`process.env` default)  |
| `executablePath`    | `string`                                                                                     | Custom Electron binary path               |
| `acceptDownloads`   | `boolean`                                                                                    | Auto-accept attachments (default: `true`) |
| `bypassCSP`         | `boolean`                                                                                    | Bypass CSP (default: `false`)             |
| `colorScheme`       | `"light" \| "dark" \| "no-preference" \| null`                                               | Emulates media feature                    |
| `extraHTTPHeaders`  | `Record<string, string>`                                                                     | Extra HTTP headers                        |
| `geolocation`       | `{ latitude: number, longitude: number, accuracy?: number }`                                 | Fake location                             |
| `httpCredentials`   | `{ username: string, password: string, origin?: string, send?: 'unauthorized' \| 'always' }` | HTTP Auth                                 |
| `ignoreHTTPSErrors` | `boolean`                                                                                    | Ignore HTTPS errors (default: `false`)    |
| `locale`            | `string`                                                                                     | e.g. `en-GB`, `de-DE`                     |
| `offline`           | `boolean`                                                                                    | Emulate offline (default: `false`)        |
| `recordHar`         | `object`                                                                                     | HAR file recording                        |
| `recordVideo`       | `{ dir: string, size?: { width: number, height: number } }`                                  | Video recording                           |
| `timeout`           | `number`                                                                                     | Launch timeout in ms (default: `30000`)   |
| `timezoneId`        | `string`                                                                                     | Change time zone                          |
| `tracesDir`         | `string`                                                                                     | Save traces here                          |

---

## 🧪 Common Methods

| Method                      | Description                        |
| --------------------------- | ---------------------------------- |
| `electron.launch()`         | Starts Electron with given options |
| `electronApp.evaluate()`    | Run JS in main process             |
| `electronApp.firstWindow()` | Gets the first created window      |
| `window.title()`            | Gets title of the window           |
| `window.screenshot()`       | Saves screenshot                   |
| `window.click(selector)`    | Clicks element                     |
| `window.on('console', cb)`  | Listen to console events           |
| `electronApp.close()`       | Terminates the app                 |

---

## 🗂️ Tips

* Console logs from Electron's renderer can be piped directly via:

  ```js
  window.on('console', console.log);
  ```
* Always `await browserContext.close()` to flush HAR/Video/Trace.


---

## Clean Start
- Die folgmäßig wird immer die gleiche Elektroneninstanz benutzt und daher werden Cookies usw. gespeichert. Für einen **clean Start** können wir Folgendes machen:
```typescript
// ===== Imports =====
import * as fs from 'fs'
import * as os from 'os'
import * as path from 'path'
import { _electron as electron, type ElectronApplication, type Page } from '@playwright/test'
import dotenv from 'dotenv'

// Load environment variables
dotenv.config() // Load default .env
dotenv.config({ path: '.env.e2e', override: true }) // Load .env.e2e

/** This class is used to setup the app for the tests.
 * @author test
 * @version 1.0.0
 * @since 2025-05-21
 */
export class AppSetup {
    private static readonly _buildedAppPathConst: string = 'out/main/index.js'

    public electronApp!: ElectronApplication
    public page!: Page
    public readonly patientenTableSelector: string = 'div.ag-center-cols-container'
    public readonly calendarViewSelector: string = 'div#daily-timeline.mbsc-eventcalendar'
    public readonly pvs: string = 'dampsoft'
    public readonly user: string = 'TEST'
    private _tempUserDataPath: string | null = null

    public constructor(pvs = 'dampsoft') {
        this.pvs = pvs
    }

    /** This method initializes the electron app
     * @returns {Promise<void>}
    */
    public async init(): Promise<void> {
        this._tempUserDataPath = path.join(os.tmpdir(), `test-playwright-electron-${String(Date.now())}`)
        // Ensure the directory exists, Electron might create it, but it's safer to ensure.
        // fs.mkdirSync(this._tempUserDataPath, { recursive: true }); // Electron usually handles this.

        this.electronApp = await electron.launch({
            args: [`--user-data-dir=${this._tempUserDataPath}`, AppSetup._buildedAppPathConst],
            env: {
                ...process.env,
                // eslint-disable-next-line @typescript-eslint/naming-convention
                ELECTRON_ENABLE_LOGGING: '1',
                PVS: this.pvs // Use the instance PVS property
            }
        })

        this.page = await this.electronApp.firstWindow()
        await this.page.setViewportSize({ width: 1920, height: 1080 })
    }

    /** This method returns true if the app is packaged
     * @returns {Promise<boolean>}
    */
    public async getIsPackaged(): Promise<boolean> {
        // eslint-disable-next-line @typescript-eslint/prefer-readonly-parameter-types
        return this.electronApp.evaluate(({ app }) => {
            // This code runs in the Electron main process
            return app.isPackaged
        })
    }

    /** This method ensures the patients page is loaded
     * @returns {Promise<void>}
    */
    public async ensurePatientsLoaded(): Promise<void> {
        await this.page.waitForSelector(this.patientenTableSelector, { state: 'visible', timeout: 15000 })
    }

    /** This method ensures the appointments page is loaded
     * @returns {Promise<void>}
    */
    public async ensureAppointmentsPageLoaded(): Promise<void> {
        await this.page.waitForSelector(this.calendarViewSelector, { state: 'visible', timeout: 15000 })
    }

    /** This method closes the electron app
     * @returns {Promise<void>}
    */
    public async close(): Promise<void> {
        await this.electronApp.close()
        if (typeof this._tempUserDataPath === 'string' && fs.existsSync(this._tempUserDataPath)) {
            try {
                fs.rmSync(this._tempUserDataPath, { recursive: true, force: true })
            } catch (error) {
                console.error(`Failed to remove temp user data dir: ${this._tempUserDataPath}`, error)
            }
        }
    }
}
```

---

## 📚 Weitere Ressourcen

* [📖 Electron Support Docs](https://playwright.dev/docs/api/class-electronapplication)
* [🧪 Third-Party Test Runner Guide](https://playwright.dev/docs/test-runners)

  









<br><br>
<br><br>


## Test generator
- https://playwright.dev/docs/codegen

<details><summary>Click to expand..</summary>


# Option 1 - Playwright inspector
- Usefully for e.g. electron apps

1. Create record.spec.ts
```
// In a test file, e.g., test/e2e/record-my-feature.e2e.ts
import { test } from '@playwright/test'
import { AppSetup } from './app.base.setup.ts'

test.describe('Recording Session', () => {
    let appSetup: AppSetup

    test.beforeAll(async() => {
        appSetup = new AppSetup(/* pass pvs if needed */)
        await appSetup.init() // This launches Electron, and it loads your UI
    })

    test.afterAll(async() => {
        await appSetup.close()
    })

    test.only('Record interactions here', async() => {
        // The webServer from playwright.config.ts has started your Vite server.
        // appSetup.init() has launched Electron, and Electron has loaded the UI.
        
        // Now, pause to open the Playwright Inspector connected to the Electron window
        await appSetup.page.pause() 

        // After you're done recording with the inspector, you can stop the test.
        // The recorded steps will appear in the Inspector window.
    })
})
```

2. The Playwright Inspector window will also appear due to await appSetup.page.pause();
   
3. Use the "Record" button in the Playwright Inspector (not the browser extension).
  





</details>

</details>

</details>
































<br><br>
<br><br>
______

<br><br>
<br><br>


# Unit testing


<details><summary>Click to expand..</summary>

# Mocking
Then you write unit test and try to run something like `ìmport { app } from electron`. This will not work because you must mock everything from electron
  - https://stackoverflow.com/a/38550760/8822860
  - https://stackoverflow.com/a/38550760/8822860

</details>

</details>

























































<br><br>
<br><br>
___
<br><br>
<br><br>






# FAQ

<details><summary>Click to expand..</summary>

# sandbox fix
```
sudo chown root:root node_modules/electron/dist/chrome-sandbox && sudo chmod 4755 node_modules/electron/dist/chrome-sandbox
```
- Related to (https://electron-vite.org/guide/cli#preview-options) `The --noSandbox option will force Electron run without sandboxing. It is commonly used to enable Electron to run as root on Linux.`


<br><br>
<br><br>

# Error

<details><summary>Click to expand..</summary>

# The requested module 'electron' is a CommonJS module, which
may not support all module.exports as named exports.
```plaintext
SyntaxError: Named export 'BrowserWindow' not found. The requested module 'electron' is a CommonJS module, which
may not support all module.exports as named exports.
CommonJS modules can always be imported via the default export, for example using:

import pkg from 'electron';
const { app, session, ipcMain, BrowserWindow } = pkg;
```
- This will mostly happen, when you try to load the electron packages outside of the electron context. This means e.g. when you write unit test and try to run something like `ìmport { app } from electron`. This will not work because you must mock everything from electron
  - https://stackoverflow.com/a/38550760/8822860
  - https://stackoverflow.com/a/38550760/8822860

</details>





  
</details>







