<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Quick Text Copier</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
      max-width: 800px;
      margin: auto;
    }
    textarea {
      width: 100%;
      font-size: 14px;
      padding: 10px;
      box-sizing: border-box;
      margin-bottom: 10px;
    }
    button {
      padding: 10px 20px;
      font-size: 14px;
      cursor: pointer;
      margin-bottom: 20px;
      margin-right: 10px;
    }
    h2 {
      margin-top: 30px;
    }
  </style>
</head>
<body>

<h2>Message Template</h2>
<textarea id="textArea1" rows="10">
Perfect, thank you!

1.Click on the 3 dots at the bottom right corner >Then click software version >Enter the code 2203 That will get you into the installer toolbox

2. Select the "register panel" and enter this code without the dashes:  XXXXX While that registers can you text me your email?
</textarea>
<br>
<button onclick="copyText('textArea1')">Copy Template Text</button>

<h2>Cleaned Clipboard Output</h2>
<textarea id="textArea2" rows="4" placeholder="Ctrl will paste & clean clipboard content here..."></textarea>
<br>
<button onclick="copyText('textArea2')">Copy Cleaned Text</button>

<script>
  async function insertClipboard() {
    try {
      const clipText = await navigator.clipboard.readText();
      const textarea1 = document.getElementById("textArea1");
      const textarea2 = document.getElementById("textArea2");

      // Insert clipboard into preset message
      const markerStart = "dashes:";
      const markerEnd = " While";
      const original = textarea1.value;
      const startIndex = original.indexOf(markerStart);
      const endIndex = original.indexOf(markerEnd, startIndex);

      if (startIndex !== -1 && endIndex !== -1) {
        const before = original.slice(0, startIndex + markerStart.length);
        const after = original.slice(endIndex);
        const newText = `${before} ${clipText.trim()}${after}`;
        textarea1.value = newText;
      }

      // Clean up for second text field
      let cleaned = clipText
        .replace("⚫ Passcode:", "")
        .replace(/, Expires: .*?⚾ Text message not sent\. No open Case found!/, "")
        .trim();

      textarea2.value = cleaned;

    } catch (err) {
      alert("Clipboard access failed. Please allow clipboard permissions.");
    }
  }

  function copyText(id) {
    const textarea = document.getElementById(id);
    textarea.select();
    document.execCommand("copy");
    alert("Copied to clipboard!");
  }

  // Listen for Ctrl key press
  document.addEventListener("keydown", (e) => {
    if (e.key === "Control") {
      insertClipboard();
    }
  });
</script>

</body>
</html>
