<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Design Pro - ديزاين برو</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/5.3.0/fabric.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            direction: rtl;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            display: grid;
            grid-template-columns: 250px 1fr 350px;
            min-height: 90vh;
        }

        /* ===== SIDEBAR LEFT ===== */
        .sidebar-left {
            background: #f8f9fa;
            padding: 20px;
            border-left: 1px solid #ddd;
            overflow-y: auto;
        }

        .sidebar-left h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .templates {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .template-btn {
            padding: 12px 15px;
            border: 2px solid #ddd;
            background: white;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 13px;
            font-weight: 500;
            color: #555;
        }

        .template-btn:hover {
            border-color: #667eea;
            background: #f0f4ff;
            color: #667eea;
        }

        .template-btn.active {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        /* ===== CANVAS AREA ===== */
        .canvas-area {
            background: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            position: relative;
        }

        .toolbar {
            width: 100%;
            background: #f8f9fa;
            padding: 15px;
            border-bottom: 1px solid #ddd;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            border-radius: 8px;
            margin-bottom: 15px;
        }

        .toolbar button {
            padding: 8px 15px;
            border: 1px solid #ddd;
            background: white;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.3s;
            font-weight: 500;
        }

        .toolbar button:hover {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        canvas {
            border: 2px solid #ddd;
            border-radius: 8px;
            background: white;
            cursor: crosshair;
            max-width: 100%;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.1);
        }

        /* ===== SIDEBAR RIGHT (PROPERTIES) ===== */
        .sidebar-right {
            background: #f8f9fa;
            padding: 20px;
            border-left: 1px solid #ddd;
            overflow-y: auto;
        }

        .sidebar-right h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .property-group {
            margin-bottom: 20px;
        }

        .property-group label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-size: 12px;
            font-weight: 600;
            text-transform: uppercase;
        }

        .property-group input[type="text"],
        .property-group input[type="number"],
        .property-group select,
        .property-group input[type="color"] {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 12px;
            transition: all 0.3s;
        }

        .property-group input:focus,
        .property-group select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 5px rgba(102, 126, 234, 0.3);
        }

        .export-buttons {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
            margin-top: 20px;
        }

        .export-btn {
            padding: 10px;
            border: none;
            border-radius: 6px;
            background: #667eea;
            color: white;
            cursor: pointer;
            font-size: 12px;
            font-weight: 600;
            transition: all 0.3s;
        }

        .export-btn:hover {
            background: #764ba2;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .header {
            grid-column: 1 / -1;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 24px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .status {
            font-size: 12px;
            opacity: 0.8;
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 1024px) {
            .container {
                grid-template-columns: 1fr 300px;
            }
            .sidebar-left {
                display: none;
            }
        }

        @media (max-width: 768px) {
            .container {
                grid-template-columns: 1fr;
            }
            .sidebar-right {
                display: none;
            }
            .header {
                flex-direction: column;
                gap: 10px;
            }
        }

        .layers {
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid #ddd;
            border-radius: 6px;
            padding: 8px;
        }

        .layer-item {
            padding: 8px;
            background: white;
            border: 1px solid #ddd;
            border-radius: 4px;
            margin-bottom: 6px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.3s;
        }

        .layer-item:hover {
            background: #f0f4ff;
            border-color: #667eea;
        }

        .layer-item.active {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>✨ Design Pro</h1>
            <div class="status">جاهز للتصميم | Ready to Design</div>
        </div>

        <!-- SIDEBAR LEFT: TEMPLATES -->
        <div class="sidebar-left">
            <h3>📐 القوالب</h3>
            <div class="templates">
                <button class="template-btn active" data-template="instagram">📷 Instagram Post (1080x1080)</button>
                <button class="template-btn" data-template="tiktok">🎬 TikTok Video (1080x1920)</button>
                <button class="template-btn" data-template="youtube">▶️ YouTube Thumbnail (1280x720)</button>
                <button class="template-btn" data-template="facebook">👥 Facebook Post (1200x628)</button>
                <button class="template-btn" data-template="twitter">𝕏 Twitter Post (1024x512)</button>
                <button class="template-btn" data-template="linkedin">💼 LinkedIn Post (1200x627)</button>
                <button class="template-btn" data-template="pinterest">📌 Pinterest Pin (1000x1500)</button>
            </div>
        </div>

        <!-- CANVAS AREA -->
        <div class="canvas-area">
            <div class="toolbar">
                <button onclick="addText()">📝 إضافة نص</button>
                <button onclick="addImage()">🖼️ إضافة صورة</button>
                <button onclick="addShape('rect')">⬜ مربع</button>
                <button onclick="addShape('circle')">⭕ دائرة</button>
                <button onclick="undo()">↶ تراجع</button>
                <button onclick="redo()">↷ إعادة</button>
                <button onclick="clearCanvas()">🗑️ مسح</button>
            </div>
            <canvas id="canvas"></canvas>
        </div>

        <!-- SIDEBAR RIGHT: PROPERTIES -->
        <div class="sidebar-right">
            <h3>⚙️ الخصائص</h3>
            
            <div class="property-group">
                <label>حجم الخط</label>
                <input type="number" id="fontSize" min="10" max="100" value="20" onchange="updateProperty('fontSize')">
            </div>

            <div class="property-group">
                <label>لون النص</label>
                <input type="color" id="fontColor" value="#000000" onchange="updateProperty('fill')">
            </div>

            <div class="property-group">
                <label>لون الخلفية</label>
                <input type="color" id="bgColor" value="#ffffff" onchange="updateCanvasBg()">
            </div>

            <div class="property-group">
                <label>الشفافية</label>
                <input type="number" id="opacity" min="0" max="100" value="100" onchange="updateProperty('opacity')">
            </div>

            <div class="property-group">
                <label>🎯 الطبقات</label>
                <div class="layers" id="layersList"></div>
            </div>

            <div class="export-buttons">
                <button class="export-btn" onclick="downloadCanvas('png')">⬇️ تحميل PNG</button>
                <button class="export-btn" onclick="downloadCanvas('jpg')">⬇️ تحميل JPG</button>
            </div>
        </div>
    </div>

    <script>
        const canvas = new fabric.Canvas('canvas', {
            width: 1080,
            height: 1080,
            backgroundColor: '#ffffff'
        });

        let history = [];
        let historyStep = 0;

        // Template dimensions
        const templates = {
            instagram: { width: 1080, height: 1080 },
            tiktok: { width: 1080, height: 1920 },
            youtube: { width: 1280, height: 720 },
            facebook: { width: 1200, height: 628 },
            twitter: { width: 1024, height: 512 },
            linkedin: { width: 1200, height: 627 },
            pinterest: { width: 1000, height: 1500 }
        };

        // Change template
        document.querySelectorAll('.template-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                document.querySelectorAll('.template-btn').forEach(b => b.classList.remove('active'));
                e.target.classList.add('active');
                const template = e.target.dataset.template;
                const dims = templates[template];
                canvas.setWidth(dims.width);
                canvas.setHeight(dims.height);
                document.getElementById('canvas').style.maxWidth = '100%';
                saveHistory();
            });
        });

        // Add text
        function addText() {
            const text = new fabric.Text('اكتب هنا', {
                left: 100,
                top: 100,
                fontSize: 20,
                fill: '#000000',
                fontFamily: 'Arial'
            });
            canvas.add(text);
            canvas.setActiveObject(text);
            canvas.renderAll();
            saveHistory();
            updateLayers();
        }

        // Add image
        function addImage() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'image/*';
            input.onchange = (e) => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = (event) => {
                    fabric.Image.fromURL(event.target.result, (img) => {
                        img.scale(0.5);
                        img.set({ left: 100, top: 100 });
                        canvas.add(img);
                        canvas.setActiveObject(img);
                        canvas.renderAll();
                        saveHistory();
                        updateLayers();
                    });
                };
                reader.readAsDataURL(file);
            };
            input.click();
        }

        // Add shapes
        function addShape(type) {
            let shape;
            if (type === 'rect') {
                shape = new fabric.Rect({
                    left: 100,
                    top: 100,
                    width: 200,
                    height: 200,
                    fill: '#667eea'
                });
            } else if (type === 'circle') {
                shape = new fabric.Circle({
                    left: 100,
                    top: 100,
                    radius: 100,
                    fill: '#667eea'
                });
            }
            canvas.add(shape);
            canvas.setActiveObject(shape);
            canvas.renderAll();
            saveHistory();
            updateLayers();
        }

        // Update properties
        function updateProperty(prop) {
            const obj = canvas.getActiveObject();
            if (!obj) return;

            if (prop === 'fontSize') {
                obj.fontSize = parseInt(document.getElementById('fontSize').value);
            } else if (prop === 'fill') {
                obj.fill = document.getElementById('fontColor').value;
            } else if (prop === 'opacity') {
                obj.opacity = parseInt(document.getElementById('opacity').value) / 100;
            }
            canvas.renderAll();
            saveHistory();
        }

        // Update canvas background
        function updateCanvasBg() {
            canvas.setBackgroundColor(document.getElementById('bgColor').value);
            canvas.renderAll();
            saveHistory();
        }

        // Undo/Redo
        function saveHistory() {
            history = history.slice(0, historyStep);
            history.push(JSON.stringify(canvas));
            historyStep++;
        }

        function undo() {
            if (historyStep > 0) {
                historyStep--;
                canvas.loadFromJSON(history[historyStep], () => canvas.renderAll());
            }
        }

        function redo() {
            if (historyStep < history.length - 1) {
                historyStep++;
                canvas.loadFromJSON(history[historyStep], () => canvas.renderAll());
            }
        }

        // Clear canvas
        function clearCanvas() {
            if (confirm('هل أنت متأكد؟')) {
                canvas.clear();
                canvas.setBackgroundColor('#ffffff');
                canvas.
