<!DOCTYPE html>
<html lang="ml">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PVG - Photo, Video, GIF, Animation</title>
    <style>
        body { font-family: Arial, sans-serif; background: #121212; color: #ffffff; text-align: center; padding: 20px; }
        .container { max-width: 500px; margin: auto; background: #1e1e1e; padding: 20px; border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.5); }
        input, button, textarea { width: 100%; margin: 10px 0; padding: 10px; border-radius: 5px; border: none; }
        button { background: #ff9800; color: white; font-weight: bold; cursor: pointer; }
        button:hover { background: #e68900; }
    </style>
</head>
<body>
    <div class="container">
        <h2>PVG ആനിമേഷൻ ടൂൾ</h2>
        <p>ഫോട്ടോ, വീഡിയോ, GIF എന്നിവ അപ്‌ലോഡ് ചെയ്ത് ആനിമേറ്റ് ചെയ്യാം</p>
        
        <input type="file" id="mediaFile" accept="image/*,video/*">
        <textarea id="promptText" placeholder="നിങ്ങൾക്ക് എന്ത് മാറ്റമാണ് വരുത്തേണ്ടത് എന്ന് ഇവിടെ ടൈപ്പ് ചെയ്യുക..."></textarea>
        <button onclick="processMedia()">ആനിമേഷൻ ചെയ്യുക</button>
        
        <div id="result"></div>
    </div>

    <script>
        function processMedia() {
            const fileInput = document.getElementById('mediaFile').files[0];
            const prompt = document.getElementById('promptText').value;
            if(!fileInput) {
                alert('தயவுசெய்து ഒരു ഫയൽ തിരഞ്ഞെടുക്കൂ!');
                return;
            }
            alert('പ്രോസസ്സിംഗ് ആരംഭിക്കുന്നുവെന്ന് സങ്കൽപ്പിക്കുക! (API കണക്ട് ചെയ്യേണ്ടതുണ്ട്)');
        }
    </script>
</body>
</html>
