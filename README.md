
<html>
<head>
  <meta charset="UTF-8">
  <title>A5/1 Cipher - Ergash</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #eef2f5;
    }
    .container {
      max-width: 600px;
      background: white;
      padding: 20px;
      margin: auto;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      border-radius: 10px;
    }
    h2 {
      text-align: center;
    }
    input, textarea, button {
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>A5/1 Soddalashtirilgan - Shifrlash va Deshifrlash</h2>

    <label>Matn (faqat harflar):</label>
    <textarea id="text" rows="3"></textarea>

    <label>64-bitli kalit (0 va 1 dan iborat bo‘lsin):</label>
    <input type="text" id="key" value="1011001110001111000011110000111100001111000011110000111100001111" maxlength="64">

    <button onclick="encrypt()">Shifrlash</button>
    <button onclick="decrypt()">Deshifrlash</button>

    <label>Natija:</label>
    <textarea id="result" rows="4" readonly></textarea>
  </div>

  <script>
    function textToBits(text) {
      return text.toLowerCase().replace(/[^a-z]/g, "").split('').map(c =>
        c.charCodeAt(0).toString(2).padStart(8, '0')
      ).join('');
    }

    function bitsToText(bits) {
      let text = "";
      for (let i = 0; i < bits.length; i += 8) {
        let byte = bits.substr(i, 8);
        text += String.fromCharCode(parseInt(byte, 2));
      }
      return text;
    }

    function xorBits(data, key) {
      let result = "";
      for (let i = 0; i < data.length; i++) {
        let k = key[i % key.length];
        result += data[i] ^ k;
      }
      return result;
    }

    function encrypt() {
      const text = document.getElementById("text").value;
      const key = document.getElementById("key").value;

      if (key.length !== 64 || /[^01]/.test(key)) {
        alert("Kalit faqat 0 va 1 dan iborat 64 bitli bo‘lishi kerak!");
        return;
      }

      const plaintextBits = textToBits(text);
      const keystream = key.repeat(Math.ceil(plaintextBits.length / key.length)).substr(0, plaintextBits.length);
      const cipherBits = xorBits(plaintextBits, keystream);

      document.getElementById("result").value = cipherBits;
    }

    function decrypt() {
      const cipherBits = document.getElementById("text").value.trim();
      const key = document.getElementById("key").value;

      if (/[^01]/.test(cipherBits)) {
        alert("Deshifrlash uchun faqat 0 va 1 lardan iborat shifrlangan matn kiritilsin.");
        return;
      }

      const keystream = key.repeat(Math.ceil(cipherBits.length / key.length)).substr(0, cipherBits.length);
      const plainBits = xorBits(cipherBits, keystream);
      const plaintext = bitsToText(plainBits);

      document.getElementById("result").value = plaintext;
    }
  </script>
</body>
</html>
