# n4pdftool
Free PDF merge tool
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mera PDF Tool - Free Online</title>
    <style>
        /* Yeh CSS hai - Bas colors yahan se change karo */
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .tool-box {
            border: 2px dashed #ccc;
            padding: 40px;
            text-align: center;
            margin: 20px 0;
            border-radius: 5px;
        }
        button {
            background: #667eea;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background: #764ba2;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 Mera PDF Tool</h1>
        <p style="text-align: center;">Apni PDF files yahan upload karo</p>
        
        <div class="tool-box">
            <input type="file" id="pdfInput" accept=".pdf" multiple>
            <br><br>
            <button onclick="mergePDFs()">PDF Merge Karo</button>
        </div>

        <div id="result"></div>
    </div>

    <!-- Yeh library hai PDF process karne ke liye -->
    <script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js"></script>
    
    <script>
        async function mergePDFs() {
            const input = document.getElementById('pdfInput');
            if (input.files.length < 2) {
                alert('Kam se kam 2 PDF files select karo!');
                return;
            }

            // Yeh code PDF merge karta hai
            const { PDFDocument } = PDFLib;
            const mergedPdf = await PDFDocument.create();

            for (let file of input.files) {
                const arrayBuffer = await file.arrayBuffer();
                const pdf = await PDFDocument.load(arrayBuffer);
                const copiedPages = await mergedPdf.copyPages(pdf, pdf.getPageIndices());
                copiedPages.forEach((page) => mergedPdf.addPage(page));
            }

            const pdfBytes = await mergedPdf.save();
            const blob = new Blob([pdfBytes], { type: 'application/pdf' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'merged-file.pdf';
            link.click();
            
            document.getElementById('result').innerHTML = '<h3 style="color: green; text-align: center;">✅ Download Ho Gaya!</h3>';
        }
    </script>
</body>
</html>
