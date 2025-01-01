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
