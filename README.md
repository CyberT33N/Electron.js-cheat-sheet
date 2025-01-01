# Electron.js Cheat Sheet

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










# Exec


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























<br><br>
____
____
<br><br>

# API




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
```javascript
win.on('closed', () => {
    mainWindow = null;
});
```


</details>
