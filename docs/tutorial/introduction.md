const { app, BrowserWindow } = require('electron');
const path = require('path');
const { spawn } = require('child_process');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 700,
    webPreferences: {
      nodeIntegration: false,
    },
  });

  // Abrir o app React buildado
  mainWindow.loadFile(path.join(__dirname, 'public', 'index.html'));

  // Abrir DevTools se quiser:
  // mainWindow.webContents.openDevTools();
}

// Iniciar backend Node com yt-dlp quando app abre
function startBackend() {
  spawn('node', ['backend.js'], {
    cwd: __dirname,
    shell: true,
    stdio: 'inherit',
  });
}

app.whenReady().then(() => {
  startBackend();
  createWindow();
});

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit();
});
