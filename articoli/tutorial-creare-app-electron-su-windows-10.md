# Come creare un'app di Electron su Windows 10

Questa guida per creare un'app di Electron è tratta dal [Getting started](https://electronjs.org/docs/tutorial/first-app) del sito ufficiale di Electron.

## Prerequisiti

Per creare un'app di [Electron](https://electronjs.org/), occorre innanzitutto installare: 
* [Download Node.js](https://nodejs.org/)
* [Download Git Bash](https://git-scm.com/download/win)
* [Download Visual Studio Code](https://code.visualstudio.com) _(opzionale)_

In questa guida ipotizzeremo di usare _Visual Studio Code_ come nostro editor. Da _Git Bash_ esso è avviabile lanciando il comando `code .` che lo avvierà con il workspace settato sulla cartella corrente.

## Creazione del file di configurazione dell'app

Supponiamo di creare un'app denominata 'myapp' e di inserirla in una omonima cartella.

Aprire _Git Bash_, recarsi nella propria directory dei progetti, ad esempio `/c/progetti` e digitare:

```bash
mkdir myapp
cd myapp
npm init
```

Il comando `npm init` creerà un file `package.json` all'interno della directory (il comando vi chiederà alcune info in input). 

Questo file generato, verrà usato come file di configurazione dell'app. Il contenuto finale del file dovrà essere simile al seguente:

```json
{
    "name": "myapp",
    "version": "1.0.0",
    "main": "main.js",
    "scripts": {
      "start": "electron ."
    }
  }
```

Se qualcosa - in genere gli scripts - non è corretto come nell'esempio, lanciare il vostro editor e modificare il contenuto di `package.json`:

```bash
code package.json
```

Salvate e chiudete il file nell'editor, in modo che sia modificabile da _npm install_ che ora lanceremo.

## Installare Electron

Per installare Electron con una versione specifica per questo progetto, lanciare:

```bash
npm install --save-dev electron
```

## Creazione dei files di base per far funzionare l'app

In Electron, i files di base che fanno compongono un'app sono:
- `package.json`
- `main.js`
- `index.html`

Il primo file viene creato automaticamente da `npm init`, gli altri possiamo crearli noi manualmente.

### Esempio: Hello world App 

Un'app Hello world in Electron, può essere creata, con il seguente codice.

#### Contenuto di `main.js`

```javascript
  const {app, BrowserWindow} = require('electron')
  const path = require('path')
  const url = require('url')
  
  // Keep a global reference of the window object, if you don't, the window will
  // be closed automatically when the JavaScript object is garbage collected.
  let win
  
  function createWindow () {
    // Create the browser window.
    win = new BrowserWindow({width: 800, height: 600})
  
    // and load the index.html of the app.
    win.loadURL(url.format({
      pathname: path.join(__dirname, 'index.html'),
      protocol: 'file:',
      slashes: true
    }))
  
    // Open the DevTools.
    win.webContents.openDevTools()
  
    // Emitted when the window is closed.
    win.on('closed', () => {
      // Dereference the window object, usually you would store windows
      // in an array if your app supports multi windows, this is the time
      // when you should delete the corresponding element.
      win = null
    })
  }
  
  // This method will be called when Electron has finished
  // initialization and is ready to create browser windows.
  // Some APIs can only be used after this event occurs.
  app.on('ready', createWindow)
  
  // Quit when all windows are closed.
  app.on('window-all-closed', () => {
    // On macOS it is common for applications and their menu bar
    // to stay active until the user quits explicitly with Cmd + Q
    if (process.platform !== 'darwin') {
      app.quit()
    }
  })
  
  app.on('activate', () => {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (win === null) {
      createWindow()
    }
  })
  
  // In this file you can include the rest of your app's specific main process
  // code. You can also put them in separate files and require them here.
```

#### Contenuto di `index.html`:

In una comune pagina HTML, creare un elemento paragrafo e per verificare che funzioni, inserire un tag SCRIPT con all'interno i seguenti comandi:

```javascript
  // Versione di Node
  document.write(process.versions.node)
  // Versione di Chrome
  document.write(process.versions.chrome)
  // Versione di Electron 
  document.write(process.versions.electron)
```

## Come eseguire l'app

Da _Git Bash_ o dal terminale di _Visual Studio Code_ (attivabile con la combinazione <kbd>CTRL</kbd>+<kbd>ò</kbd>), eseguire:

```bash
npm start
```
