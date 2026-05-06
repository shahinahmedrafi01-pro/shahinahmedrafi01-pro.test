<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>🔮 number telepathy · the mystical reveal</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      background: linear-gradient(145deg, #0b1120 0%, #19233c 100%);
      font-family: 'Segoe UI', system-ui, 'SF Pro Text', 'Inter', 'Poppins', -apple-system, BlinkMacSystemFont, 'Roboto', sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 1.5rem;
    }

    /* main mystical card */
    .card {
      max-width: 620px;
      width: 100%;
      background: rgba(18, 25, 45, 0.75);
      backdrop-filter: blur(14px);
      border-radius: 4rem 2.5rem 4rem 2.5rem;
      box-shadow: 0 25px 45px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(255, 255, 255, 0.08) inset;
      padding: 2rem 2rem 2.8rem;
      transition: all 0.2s ease;
      text-align: center;
    }

    /* sparkling header */
    h1 {
      font-size: 3rem;
      font-weight: 600;
      letter-spacing: -0.3px;
      background: linear-gradient(135deg, #f9f3e2, #e6d5b8);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      display: inline-flex;
      align-items: center;
      gap: 12px;
      flex-wrap: wrap;
      justify-content: center;
    }

    .sparkle {
      font-size: 2.8rem;
      filter: drop-shadow(0 0 4px #ffd966);
    }

    .sub {
      font-size: 1.1rem;
      color: #b9c7e5;
      margin-top: 0.75rem;
      margin-bottom: 2rem;
      font-weight: 400;
      border-bottom: 1px dashed #3e4a6b;
      display: inline-block;
      padding-bottom: 6px;
    }

    /* input and button group */
    .input-group {
      background: #0e1625cc;
      border-radius: 3rem;
      padding: 0.25rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.75rem;
      align-items: center;
      justify-content: center;
      margin: 1.8rem 0 0.8rem;
      border: 1px solid #2f3b5c;
    }

    input {
      flex: 3;
      min-width: 180px;
      background: #0a0f1c;
      border: none;
      padding: 1rem 1.5rem;
      font-size: 1.5rem;
      font-weight: 500;
      font-family: monospace;
      color: #f2e8cf;
      border-radius: 3rem;
      outline: none;
      transition: all 0.2s;
      text-align: center;
      letter-spacing: 1px;
      box-shadow: inset 0 1px 4px #00000030;
    }

    input:focus {
      background: #101725;
      box-shadow: 0 0 0 2px #ffcf6e, inset 0 1px 3px #00000040;
      color: #ffefcf;
    }

    input::placeholder {
      color: #5f6e91;
      font-size: 1.2rem;
      font-family: monospace;
      font-weight: normal;
    }

    button {
      background: linear-gradient(105deg, #f5b042, #ffca6e);
      border: none;
      padding: 1rem 2rem;
      border-radius: 3rem;
      font-weight: bold;
      font-size: 1.25rem;
      font-family: inherit;
      color: #1d2b3b;
      cursor: pointer;
      transition: transform 0.15s, box-shadow 0.2s;
      display: inline-flex;
      align-items: center;
      gap: 10px;
      box-shadow: 0 5px 12px rgba(0, 0, 0, 0.3);
      letter-spacing: 0.3px;
    }

    button:hover {
      transform: scale(1.02);
      background: linear-gradient(105deg, #ffbc4a, #ffd88a);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
    }

    button:active {
      transform: scale(0.97);
    }

    /* waiting chamber — dramatic reveal sequence */
    .waiting-area {
      margin: 2rem 0 1rem;
      background: #03061466;
      border-radius: 3rem;
      padding: 1.2rem;
      min-height: 130px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      transition: all 0.3s;
    }

    .thought-bubble {
      font-size: 1.2rem;
      background: #03050e99;
      border-radius: 2rem;
      padding: 0.9rem 1.6rem;
      display: inline-block;
      backdrop-filter: blur(4px);
      color: #bfd6ff;
    }

    /* animated spinner + suspense */
    .spinner {
      display: inline-block;
      width: 28px;
      height: 28px;
      border: 3px solid rgba(255, 215, 120, 0.3);
      border-radius: 50%;
      border-top-color: #ffcf6e;
      animation: spin 0.8s linear infinite;
      margin-right: 12px;
      vertical-align: middle;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    .reveal-card {
      background: radial-gradient(circle at 20% 30%, #2a2538, #0e1020);
      border-radius: 2rem;
      padding: 1rem 1.5rem;
      margin-top: 0.5rem;
      animation: fadeGlow 0.6s cubic-bezier(0.2, 0.9, 0.4, 1.1);
      border: 1px solid #ffdc97;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.6), 0 0 12px #f7b32b40;
    }

    @keyframes fadeGlow {
      0% { opacity: 0; transform: scale(0.92); filter: blur(3px); }
      100% { opacity: 1; transform: scale(1); filter: blur(0); }
    }

    .final-number {
      font-size: 4rem;
      font-weight: 800;
      font-family: 'Monaco', 'Courier New', monospace;
      background: linear-gradient(145deg, #ffe6b3, #ffbe5e);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      letter-spacing: 4px;
      text-shadow: 0 0 10px #ffb34780;
      word-break: break-word;
    }

    .quote {
      font-size: 0.85rem;
      color: #98a9d4;
      margin-top: 12px;
      font-style: italic;
    }

    /* joke micro interaction */
    .reset-note {
      margin-top: 1.6rem;
      font-size: 0.75rem;
      color: #6f7da0;
      background: #1a233a80;
      border-radius: 30px;
      padding: 6px 12px;
      display: inline-block;
      cursor: pointer;
      transition: 0.1s linear;
      user-select: none;
    }

    .reset-note:hover {
      background: #2a365b;
      color: #ffecb3;
    }

    footer {
      margin-top: 2rem;
      font-size: 0.7rem;
      opacity: 0.6;
      color: #6f7ea8;
    }
  </style>
</head>
<body>

<div class="card">
  <h1>
    <span class="sparkle">✨</span> 
    Think of a number 
    <span class="sparkle">🌀</span>
  </h1>
  <div class="sub">whisper it to the cosmos &nbsp;➡&nbsp; watch the reveal</div>

  <!-- input row -->
  <div class="input-group">
    <input type="number" id="userNumberInput" placeholder="your secret number" autocomplete="off">
    <button id="sendButton">📡 Send number</button>
  </div>

  <!-- dynamic zone: waiting & reveal -->
  <div id="interactiveZone" class="waiting-area">
    <div class="thought-bubble">
      🌙 waiting for your mystical number...
    </div>
  </div>

  <!-- tiny reset helper (just for fun / clear state) -->
  <div id="resetHint" class="reset-note">
    🔄 clear & think again
  </div>
  <footer>🔮 no tricks · your number comes right back</footer>
</div>

<script>
  // ---------- C O R E   M A G I C ----------
  // We don't do any puzzle: just take the number user typed,
  // wait a bit (dramatic pause), then reveal exactly that number.

  const inputElement = document.getElementById('userNumberInput');
  const sendButton = document.getElementById('sendButton');
  const interactiveZone = document.getElementById('interactiveZone');
  const resetHint = document.getElementById('resetHint');

  // Helper: clean and validate number (allow negative, decimal? we'll treat as number but show as given string for versatility)
  // We'll preserve the numeric value as entered, but we also respect floats? but integer mode is fun.
  // Actually we can store raw string representation (but for accuracy use Number).
  // The user expects "the number they thought of" — we return same value.
  
  let currentTimeout = null;        // to cancel pending reveal if needed
  let isWaiting = false;            // prevents overlapping sends

  // smooth clear waiting state
  function abortWaiting(keepMessage = false) {
    if (currentTimeout) {
      clearTimeout(currentTimeout);
      currentTimeout = null;
    }
    isWaiting = false;
    if (!keepMessage) {
      // but we won't overwrite if we are intentionally resetting later
    }
  }

  // render a "waiting" phase with suspense text and animated spinner
  function showWaitingPhase() {
    interactiveZone.innerHTML = `
      <div class="thought-bubble" style="display: flex; align-items: center; gap: 12px; flex-wrap: wrap; justify-content: center;">
        <div class="spinner"></div>
        <span>🔮 reading the vibrations of your mind ...</span>
      </div>
      <div class="quote" style="margin-top: 12px;">⚡ concentrating deeply on the number you chose ⚡</div>
    `;
  }

  // show the final reveal: exact number user sent
  function showReveal(numberValue) {
    // numberValue can be number or string (we'll format nicely)
    // We want to display exactly what they typed? but if they typed float, show float.
    // Use the raw parsed number to avoid scientific notation for big, but for very big ints JS may round,
    // but input type number is limited in browser, plus it's fun. We can also store original input string.
    // For better fidelity, we capture the raw string from input at the moment they click send.
    // We'll store the raw string in a variable before any async wait.
    // Let's adapt send logic to store rawInputString.
    // Implementation: on send we get raw string, then after wait, display exactly that.
    // But the function receives displayNumber (string) .
    
    interactiveZone.innerHTML = `
      <div class="reveal-card">
        <div style="font-size: 1rem; letter-spacing: 2px; color: #fbd67e;">✨ YOU WERE THINKING OF THIS NUMBER ✨</div>
        <div class="final-number">${escapeHtml(numberValue)}</div>
        <div class="quote">📡 transmitted directly — no illusion, just your own thought</div>
      </div>
    `;
  }

  // simple escape to avoid any injection (super safe)
  function escapeHtml(str) {
    return String(str).replace(/[&<>]/g, function(m) {
      if (m === '&') return '&amp;';
      if (m === '<') return '&lt;';
      if (m === '>') return '&gt;';
      return m;
    }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
      return c;
    });
  }

  // show error / or friendly notification if no number
  function showHintMessage(message, isError = false) {
    if (currentTimeout) {
      clearTimeout(currentTimeout);
      currentTimeout = null;
    }
    isWaiting = false;
    const errorColor = isError ? '#ffb79a' : '#c7d6ff';
    interactiveZone.innerHTML = `
      <div class="thought-bubble" style="border-left: 3px solid ${isError ? '#ffaa77' : '#cbaa5e'}; background: #0f1429cc;">
        ${escapeHtml(message)}
      </div>
    `;
    // auto-clear error after 2.5 sec back to idle?
    if (isError) {
      setTimeout(() => {
        // only reset if zone hasn't been overwritten by another action
        if (interactiveZone.innerHTML.includes('no number') || interactiveZone.innerHTML.includes('enter')) {
          resetToIdle();
        }
      }, 2000);
    } else {
      // still set idle after info message for consistency? but keep
      setTimeout(() => {
        if (interactiveZone.innerHTML.includes(message) && !isWaiting) {
          resetToIdle();
        }
      }, 1800);
    }
  }

  function resetToIdle() {
    if (currentTimeout) {
      clearTimeout(currentTimeout);
      currentTimeout = null;
    }
    isWaiting = false;
    interactiveZone.innerHTML = `
      <div class="thought-bubble">
        🌙 waiting for your mystical number...
      </div>
    `;
  }

  // The main magical flow: "Think of a number. The user sends the number to me. After waiting, I show exactly that number."
  function sendNumberAndReveal() {
    // cancel any ongoing suspense
    if (currentTimeout) {
      clearTimeout(currentTimeout);
      currentTimeout = null;
    }
    
    const rawValue = inputElement.value.trim();
    
    // if empty string
    if (rawValue === "") {
      showHintMessage("🔮 please enter a number first (any integer or decimal you're thinking of)", true);
      isWaiting = false;
      return;
    }
    
    // check if it's actually a valid number (numeric)
    const numeric = Number(rawValue);
    if (isNaN(numeric)) {
      showHintMessage("✨ that doesn't look like a number. try digits, like 42 or 3.14 ✨", true);
      isWaiting = false;
      return;
    }
    
    // else valid number — we will preserve the exact literal representation as user typed.
    // that's more charming: if they write "042", we reveal "042"? Actually input[type=number] 
    // won't store leading zero, but they can type text? but we use type=number, so browser may strip leading zero.
    // But aesthetically we want the numeric experience. But the user "thought of a number", so we can display the 
    // normalized numeric string, or the exact typed? for type=number it always casts to number, but .value returns 
    // the string representation as typed? Actually .value returns the numeric value as string but without leading zeros.
    // However, to stay true: we can display the proper "number" that they meant. Additionally we can store the parsed number
    // but convert back to readable form. Great: we display numeric value without trailing .0 if integer.
    let displayNumber;
    if (Number.isInteger(numeric)) {
      displayNumber = numeric.toString();
    } else {
      // keep decimal representation trimmed
      displayNumber = numeric.toString();
    }
    
    // but also keep the original readability: if they typed "7.00" it becomes 7, but that's fine.
    // For the most honest "they were thinking of that number", numeric fidelity works.
    
    // set waiting state
    isWaiting = true;
    showWaitingPhase();
    
    // set suspense timeout — dramatic pause (1.2 seconds feel magical, but wait just a bit)
    currentTimeout = setTimeout(() => {
      // after waiting, show the exact number they sent!
      // we also double-check that input hasn't changed dramatically but we capture displayNumber from moment of send.
      // However, if user changed input while waiting? The number they sent originally is stored in closure variable.
      // We'll use stored displayNumber.
      showReveal(displayNumber);
      currentTimeout = null;
      isWaiting = false;
      
      // Add a little spark: after reveal, we allow new send but we keep the reveal state
      // also user can click reset or input new number and send again.
    }, 1300);  // 1.3 sec of suspense — enough for the 'waiting for a bit' requirement
  }
  
  // event binding for send button
  sendButton.addEventListener('click', () => {
    // prevent spamming the waiting phase while another reveal is pending
    if (isWaiting) {
      // gentle note but not too intrusive — we ignore additional sends until complete
      const tempMsg = document.createElement('div');
      // we can subtle flash, but not overwrite main zone—it's okay to show mini feedback but not disrupt
      // However, we can show notification but without breaking current suspense
      interactiveZone.style.transition = '0.1s';
      interactiveZone.style.transform = 'scale(0.99)';
      setTimeout(() => { if(interactiveZone) interactiveZone.style.transform = ''; }, 150);
      // we could display a small tooltip but not mess with core. just ignore.
      return;
    }
    sendNumberAndReveal();
  });
  
  // allow pressing Enter key in input field
  inputElement.addEventListener('keypress', (event) => {
    if (event.key === 'Enter') {
      event.preventDefault();
      if (!isWaiting) {
        sendNumberAndReveal();
      }
    }
  });
  
  // reset hint: clear input and also reset UI without disturbing active waiting
  resetHint.addEventListener('click', () => {
    // if currently waiting, abort timeout and reset to idle and clear input
    if (currentTimeout) {
      clearTimeout(currentTimeout);
      currentTimeout = null;
    }
    isWaiting = false;
    inputElement.value = '';
    resetToIdle();
    inputElement.focus();
    // optional micro animation
  });
  
  // also ensure that if user starts typing while reveal is shown, it doesn't auto reset, but that's fine.
  // For UX, we provide additional refresh: if you click reset, everything fresh.
  
  // On page load, focus input
  window.addEventListener('load', () => {
    inputElement.focus();
    // set subtle idle message
    resetToIdle();
  });
</script>
</body>
</html>
