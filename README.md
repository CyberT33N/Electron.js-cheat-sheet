# Electron.js Cheat Sheet

# package.json

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









<br><br>
____
____
<br><br>

# API

<br><br>

## ipcMain
- https://www.electronjs.org/docs/latest/api/ipc-main
- The ipcMain module is an Event Emitter. When used in the main process, it handles asynchronous and synchronous messages sent from a renderer process (web page). Messages sent from a renderer will be emitted to this module.

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











<br><br>
<br><br>

## app
- https://www.electronjs.org/docs/latest/api/app

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















<br><br>
<br><br>

## BrowserWindow
- https://www.electronjs.org/docs/latest/api/browser-window
```javascript
// In the main process.
const { BrowserWindow } = require('electron')

const win = new BrowserWindow({ width: 800, height: 600 })

// Load a remote URL
win.loadURL('https://github.com')

// Or load a local HTML file
win.loadFile('index.html')
```



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
