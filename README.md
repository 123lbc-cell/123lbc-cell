<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>代码练习平台</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
        }

        .container {
            display: flex;
            height: 100vh;
            position: relative;
        }

        .editor-pane, .preview-pane {
            width: 50%;
            height: 100%;
            padding: 20px;
        }

        .editor-container {
            height: 100%;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .code-editor {
            flex: 1;
            padding: 10px;
            border: 2px solid #ccc;
            border-radius: 5px;
            font-family: 'Courier New', monospace;
            resize: none;
        }

        #run-btn {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            padding: 12px 24px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }

        #run-btn:hover {
            background-color: #45a049;
        }

        #preview-frame {
            width: 100%;
            height: 100%;
            border: 2px solid #666;
            border-radius: 5px;
            background-color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="editor-pane">
            <div class="editor-container">
                <textarea class="code-editor" id="html-code" placeholder="输入HTML代码..."></textarea>
                <textarea class="code-editor" id="css-code" placeholder="输入CSS代码..."></textarea>
            </div>
        </div>
        <button id="run-btn">运行 ▶</button>
        <div class="preview-pane">
            <iframe id="preview-frame"></iframe>
        </div>
    </div>

    <script>
        const runBtn = document.getElementById('run-btn');
        const htmlEditor = document.getElementById('html-code');
        const cssEditor = document.getElementById('css-code');
        const previewFrame = document.getElementById('preview-frame');

        function updatePreview() {
            const html = htmlEditor.value;
            const css = cssEditor.value;
            
            const combinedCode = `
                <!DOCTYPE html>
                <html>
                <head>
                    <style>${css}</style>
                </head>
                <body>
                    ${html}
                </body>
                </html>
            `;

            previewFrame.srcdoc = combinedCode;
        }

        runBtn.addEventListener('click', updatePreview);
        // 初始加载示例内容（可选）
        htmlEditor.value = '<h1>欢迎使用代码编辑器</h1>\n<p>点击运行查看效果</p>';
        cssEditor.value = 'body { padding: 20px; }\nh1 { color: #333; }\np { color: #666; }';
        updatePreview();
    </script>
</body>
</html>
