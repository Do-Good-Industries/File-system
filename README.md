<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Flickering Terminal</title>
  <style>
    body {
      margin: 0;
      background: black;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }
    .screen {
      width: 80vw;
      height: 80vh;
      background: #0a0a0a;
      border: 20px solid #808080;
      border-radius: 15px;
      position: relative;
      box-shadow: 0 0 20px rgba(0, 255, 0, 0.2);
      animation: flicker 0.1s infinite alternate;
    }
    @keyframes flicker {
      0% { opacity: 0.95; }
      100% { opacity: 1; }
    }
    .terminal {
      width: 100%;
      height: 100%;
      background: linear-gradient(#333, #222);
      border-radius: 5px;
      display: flex;
      flex-direction: column;
      box-sizing: border-box;
      position: relative;
      overflow: hidden;
    }
    .title-bar {
      height: 30px;
      background: linear-gradient(#444, #333);
      display: flex;
      align-items: center;
      padding: 0 10px;
      border-bottom: 1px solid #111;
      user-select: none;
    }
    .window-buttons {
      display: flex;
      gap: 8px;
    }
    .button {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      border: 1px solid rgba(0, 0, 0, 0.2);
    }
    .close { background: #ff5f57; }
    .minimize { background: #ffbd2e; }
    .maximize { background: #28ca41; }
    .terminal-content {
      flex: 1;
      background: black;
      color: #00ff00;
      font-family: 'Courier New', monospace;
      font-size: 18px;
      padding: 20px;
      overflow: auto;
      white-space: pre-wrap;
      position: relative;
    }
    .terminal-content::before {
      content: '';
      position: absolute;
      top: 30px;
      left: 0;
      width: 100%;
      height: calc(100% - 30px);
      background: linear-gradient(
        to bottom,
        rgba(0, 255, 0, 0.05) 0%,
        rgba(0, 255, 0, 0.05) 2%,
        transparent 2%,
        transparent 100%
      );
      background-size: 100% 4px;
      animation: scanline 8s linear infinite;
      pointer-events: none;
    }
    @keyframes scanline {
      0% { background-position: 0 0; }
      100% { background-position: 0 100%; }
    }
    input {
      background: transparent;
      border: none;
      color: #00ff00;
      font-family: 'Courier New', monospace;
      font-size: 18px;
      outline: none;
      width: 100%;
    }
    .hidden {
      display: none;
    }
    .error {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div class="screen">
    <div class="terminal">
      <div class="title-bar">
        <div class="window-buttons">
          <div class="button close"></div>
          <div class="button minimize"></div>
          <div class="button maximize"></div>
        </div>
      </div>
      <div class="terminal-content" id="terminalContent">
        <p>Anonymous@dogoodindustries</p>
        <p>/home/user&gt; </p>
        <p>$ root@DGI~terminal:</p>
        <input type="text" id="passwordInput" autofocus />
        <p id="secretMessage" class="hidden">safety is an illusion 💜</p>
      </div>
    </div>
  </div>

  <script>
    const passwordInput = document.getElementById('passwordInput');
    const secretMessage = document.getElementById('secretMessage');
    const terminalContent = document.getElementById('terminalContent');

    passwordInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        const input = passwordInput.value.toLowerCase();
        if (input === 'cd/users/home/documents') {
          secretMessage.classList.remove('hidden');
          passwordInput.disabled = true;
        } else {
          // Clear old error messages
          const oldError = document.getElementById('errorMessage');
          if (oldError) oldError.remove();

          // Create a red error message
          const error = document.createElement('p');
          error.textContent = 'Access Denied';
          error.classList.add('error');
          error.id = 'errorMessage';
          terminalContent.appendChild(error);

          passwordInput.value = '';
        }
      }
    });
  </script>
</body>
</html>
