<!DOCTYPE html>
<html lang="ml">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PVG - Photo, Video, GIF & Animation Studio</title>
    <!-- Google Malayalam & Stylish English Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Anek+Malayalam:wght@400;700&family=Gayathri:wght@400;700&family=Manjari:wght@400;700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Anek Malayalam', 'Poppins', sans-serif; background-color: #121212; color: #ffffff; padding: 15px; text-align: center; }
        h1 { color: #ff9800; font-size: 24px; margin-bottom: 5px; }
        p.subtitle { font-size: 13px; color: #aaa; margin-bottom: 20px; }
        
        .editor-container { max-width: 600px; margin: auto; background: #1e1e1e; padding: 15px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.6); }
        
        /* Canvas Workspace */
        .canvas-area { width: 100%; height: 320px; background: #2a2a2a; border-radius: 8px; position: relative; overflow: hidden; display: flex; align-items: center; justify-content: center; margin-bottom: 15px; border: 2px dashed #444; }
        #previewMedia { max-width: 100%; max-height: 100%; object-fit: contain; }
        .draggable-text { position: absolute; color: #ffffff; font-size: 24px; font-weight: bold; cursor: move; text-shadow: 2px 2px 4px #000; padding: 5px; user-select: none; }
        
        /* Control Panel */
        .tools-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; text-align: left; }
        .tool-card { background: #292929; padding: 10px; border-radius: 6px; }
        label { font-size: 12px; color: #ff9800; display: block; margin-bottom: 5px; }
        input[type="text"], select, input[type="file"] { width: 100%; padding: 8px; border-radius: 4px; border: 1px solid #444; background: #181818; color: #fff; font-size: 13px; }
        
        /* Buttons */
        .btn-group { display: flex; gap: 10px; margin-top: 10px; }
        button { flex: 1; padding: 10px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; transition: 0.2s; font-size: 14px; }
        .btn-primary { background: #ff9800; color: #000; }
        .btn-primary:hover { background: #e68900; }
        .btn-secondary { background: #333; color: #fff; }
        .btn-secondary:hover { background: #444; }
        
        /* Templates Section */
        .templates { margin-top: 20px; text-align: left; }
        .templates h3 { font-size: 15px; color: #ff9800; margin-bottom: 10px; }
        .template-list { display: flex; gap: 10px; overflow-x: auto; padding-bottom: 5px; }
        .template-btn { background: #282828; border: 1px solid #444; color: #fff; padding: 8px 12px; border-radius: 20px; font-size: 12px; white-space: nowrap; cursor: pointer; }
    </style>
</head>
<body>

    <h1>PVG Studio</h1>
    <p class="subtitle">Photo • Video • GIF • Animation Editor</p>

    <div class="editor-container">
        <!-- Canvas Preview -->
        <div class="canvas-area" id="canvasArea">
            <span id="placeholderText" style="color:#777;">മീഡിയ അപ്‌ലോഡ് ചെയ്യുക അല്ലെങ്കിൽ ടെംപ്ലേറ്റ് തിരഞ്ഞെടുക്കുക</span>
            <img id="previewMedia" style="display:none;" />
            <div id="overlayText" class="draggable-text" style="display:none;">നിങ്ങളുടെ ടെക്സ്റ്റ്</div>
        </div>

        <!-- Controls -->
        <div class="tools-grid">
            <div class="tool-card">
                <label>1. ഫോട്ടോ/വീഡിയോ/GIF അപ്‌ലോഡ്</label>
                <input type="file" id="mediaInput" accept="image/*,video/*,.gif" onchange="loadMedia(event)">
            </div>
            <div class="tool-card">
                <label>2. മലയാളം/ഇംഗ്ലീഷ് ടെക്സ്റ്റ്</label>
                <input type="text" id="textInput" placeholder="ഇവിടെ ടൈപ്പ് ചെയ്യുക..." oninput="updateText()">
            </div>
            <div class="tool-card">
                <label>3. ഫോണ്ട് ശൈലി (Fonts)</label>
                <select id="fontSelect" onchange="updateFont()">
                    <option value="'Anek Malayalam', sans-serif">Anek Malayalam</option>
                    <option value="'Gayathri', sans-serif">Gayathri (മലയാളം)</option>
                    <option value="'Manjari', sans-serif">Manjari (മലയാളം)</option>
                    <option value="'Poppins', sans-serif">Poppins (English)</option>
                </select>
            </div>
            <div class="tool-card">
                <label>4. ഫിൽട്ടറുകൾ / ഇഫക്റ്റുകൾ</label>
                <select id="filterSelect" onchange="applyFilter()">
                    <option value="none">Normal</option>
                    <option value="grayscale(100%)">Black & White</option>
                    <option value="sepia(100%)">Vintage Sepia</option>
                    <option value="brightness(130%)">High Brightness</option>
                    <option value="contrast(150%)">High Contrast</option>
                </select>
            </div>
        </div>

        <!-- Action Buttons -->
        <div class="btn-group">
            <button class="btn-secondary" onclick="addTextOverlay()">+ ടെക്സ്റ്റ് ചേർക്കുക</button>
            <button class="btn-primary" onclick="exportWork()">സേവ് ചെയ്യുക / ഡൗൺലോഡ്</button>
        </div>

        <!-- Malayalam Poster Templates -->
        <div class="templates">
            <h3>ട്രെൻഡിംഗ് ടെംപ്ലേറ്റുകൾ (Malayalam Editor Mode)</h3>
            <div class="template-list">
                <button class="template-btn" onclick="applyTemplate('കേരളപ്പിറവി ആശംസകൾ')">🌴 കേരളപ്പിറവി</button>
                <button class="template-btn" onclick="applyTemplate('ജന്മദിനാശംസകൾ!')">🎉 ബർത്ത്ഡേ വിഷ്</button>
                <button class="template-btn" onclick="applyTemplate('പ്രത്യേക അറിയിപ്പ്')">📢 പോസ്റ്റർ വിഷ്</button>
                <button class="template-btn" onclick="applyTemplate('ബിസിനസ്സ് ഓഫർ')">🏷️ ഓഫർ ബാാനർ</button>
            </div>
        </div>
    </div>

    <script>
        function loadMedia(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const img = document.getElementById('previewMedia');
                    img.src = e.target.result;
                    img.style.display = 'block';
                    document.getElementById('placeholderText').style.display = 'none';
                }
                reader.readAsDataURL(file);
            }
        }

        function addTextOverlay() {
            const overlay = document.getElementById('overlayText');
            overlay.style.display = 'block';
            updateText();
        }

        function updateText() {
            const text = document.getElementById('textInput').value;
            const overlay = document.getElementById('overlayText');
            overlay.innerText = text || "നിങ്ങളുടെ ടെക്സ്റ്റ്";
        }

        function updateFont() {
            const font = document.getElementById('fontSelect').value;
            document.getElementById('overlayText').style.fontFamily = font;
        }

        function applyFilter() {
            const filter = document.getElementById('filterSelect').value;
            document.getElementById('previewMedia').style.filter = filter;
        }

        function applyTemplate(templateText) {
            document.getElementById('textInput').value = templateText;
            addTextOverlay();
        }

        function exportWork() {
            alert('നിങ്ങളുടെ PVG ആനിമേഷൻ ഡിസൈൻ റെഡിയായിക്കഴിഞ്ഞു!');
        }
    </script>
</body>
</html>
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
