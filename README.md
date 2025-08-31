<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tesla - PDF Document Manager</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=SF+Pro+Display:wght@300;400;500;600;700&family=SF+Pro+Text:wght@300;400;500;600&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'SF Pro Text', -apple-system, BlinkMacSystemFont, sans-serif;
            background: #161616;
            color: #ffffff;
            min-height: 100vh;
            line-height: 1.4;
        }

        .app-container {
            max-width: 390px;
            margin: 0 auto;
            background: #000000;
            min-height: 100vh;
            position: relative;
            box-shadow: 0 0 30px rgba(0,0,0,0.5);
            overflow: hidden; /* Bu satırı ekledim */
        }

        /* Tesla Status Bar */
        .status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 20px 8px;
            font-size: 14px;
            font-weight: 600;
            color: #ffffff;
        }

        .status-left {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .status-right {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        /* Tesla Header */
        .tesla-header {
            padding: 10px 10px 1px;
            text-align: center;
            background: linear-gradient(180deg, #000000 0%, #000000 100%);
            /* Üst köşelerde border-radius ekledim */
            border-top-left-radius: 24px;
            border-top-right-radius: 24px;
        }

        .tesla-logo-svg {
            width: 80px;
            height: 80px;
            margin: 0 auto 0px;
        }

        .tesla-logo-svg img {
            display: block;
            margin: 0 auto;
        }

        .tesla-wordmark svg {
            width: 120px;
            height: auto;
        }

        /* Tesla Card Style */
        .tesla-card {
            background: #1c1c1e;
            border-radius: 16px;
            margin: 16px 20px;
            padding: 0;
            overflow: hidden;
            border: 0.5px solid #2c2c2e;
        }

        .card-header {
            padding: 20px 20px 16px;
            border-bottom: 0.5px solid #2c2c2e;
        }

        .card-title {
            font-size: 18px;
            font-weight: 600;
            color: #ffffff;
            margin-bottom: 4px;
        }

        .card-subtitle {
            font-size: 14px;
            color: #8e8e93;
            font-weight: 400;
        }

        .card-content {
            padding: 20px;
        }

        /* Upload Section */
        .upload-area {
            background: #2c2c2e;
            border: 2px dashed #3a3a3c;
            border-radius: 12px;
            padding: 40px 20px;
            text-align: center;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .upload-area:hover, .upload-area.dragover {
            border-color: #e31e24;
            background: rgba(227, 30, 36, 0.1);
        }

        .upload-icon {
            width: 48px;
            height: 48px;
            margin: 0 auto 16px;
            background: #e31e24;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .upload-text {
            font-size: 16px;
            font-weight: 500;
            color: #ffffff;
            margin-bottom: 4px;
        }

        .upload-subtext {
            font-size: 14px;
            color: #8e8e93;
        }

        .file-input {
            position: absolute;
            opacity: 0;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        /* Stats Grid */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .stat-item {
            background: #2c2c2e;
            border-radius: 12px;
            padding: 16px;
            text-align: center;
        }

        .stat-number {
            font-size: 24px;
            font-weight: 600;
            color: #e31e24;
            margin-bottom: 4px;
        }

        .stat-label {
            font-size: 12px;
            color: #8e8e93;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            font-weight: 500;
        }

        /* Progress Section */
        .progress-card {
            display: none;
        }

        .progress-card.show {
            display: block;
        }

        .progress-content {
            text-align: center;
            padding: 30px 20px;
        }

        .loading-spinner {
            width: 40px;
            height: 40px;
            margin: 0 auto 20px;
            border: 3px solid #2c2c2e;
            border-top: 3px solid #e31e24;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .progress-bar {
            width: 100%;
            height: 4px;
            background: #2c2c2e;
            border-radius: 2px;
            overflow: hidden;
            margin: 16px 0;
        }

        .progress-fill {
            height: 100%;
            background: #e31e24;
            width: 0%;
            transition: width 0.3s ease;
            border-radius: 2px;
        }

        .progress-text {
            font-size: 14px;
            color: #8e8e93;
            font-weight: 500;
        }

        /* Results List */
        .results-list {
            max-height: 400px;
            overflow-y: auto;
        }

        .file-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px 20px;
            border-bottom: 0.5px solid #2c2c2e;
            transition: background 0.2s ease;
        }

        .file-row:hover {
            background: #2c2c2e;
        }

        .file-row:last-child {
            border-bottom: none;
        }

        .file-info {
            flex: 1;
            margin-right: 12px;
        }

        .file-name {
            font-size: 15px;
            font-weight: 500;
            color: #ffffff;
            margin-bottom: 2px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .file-meta {
            font-size: 13px;
            color: #8e8e93;
            font-weight: 400;
        }

        .file-badges {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .position-badge {
            background: rgba(227, 30, 36, 0.15);
            color: #e31e24;
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 12px;
            font-weight: 600;
        }

        .digit-badge {
            background: #e31e24;
            color: #ffffff;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            font-weight: 600;
        }

        /* Tesla Buttons */
        .tesla-button {
            width: 100%;
            background: #e31e24;
            color: #ffffff;
            border: none;
            border-radius: 12px;
            padding: 16px;
            font-size: 16px;
            font-weight: 600;
            font-family: 'SF Pro Text', sans-serif;
            cursor: pointer;
            transition: all 0.2s ease;
            margin-bottom: 12px;
        }

        .tesla-button:hover {
            background: #c41e24;
        }

        .tesla-button:active {
            background: #a51e24;
            transform: scale(0.98);
        }

        .tesla-button:disabled {
            background: #2c2c2e;
            color: #8e8e93;
            cursor: not-allowed;
        }

        .tesla-button.secondary {
            background: #2c2c2e;
            color: #ffffff;
        }

        .tesla-button.secondary:hover {
            background: #3a3a3c;
        }

        /* Messages */
        .message {
            margin: 16px 20px;
            padding: 16px;
            border-radius: 12px;
            font-size: 14px;
            font-weight: 500;
        }

        .error-message {
            background: rgba(255, 59, 48, 0.1);
            color: #ff3b30;
            border: 1px solid rgba(255, 59, 48, 0.2);
        }

        .success-message {
            background: rgba(52, 199, 89, 0.1);
            color: #34c759;
            border: 1px solid rgba(52, 199, 89, 0.2);
        }

        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 4px;
        }

        ::-webkit-scrollbar-track {
            background: transparent;
        }

        ::-webkit-scrollbar-thumb {
            background: #3a3a3c;
            border-radius: 2px;
        }

        /* Error state for files */
        .file-row.error {
            background: rgba(255, 59, 48, 0.05);
        }

        .file-row.error .digit-badge {
            background: #ff3b30;
            font-size: 12px;
        }

        .file-row.error .position-badge {
            background: rgba(255, 59, 48, 0.15);
            color: #ff3b30;
        }

        /* Desktop version */
        @media (min-width: 768px) {
            .app-container {
                max-width: 800px;
                border-radius: 24px;
                
                
            }

            /* Desktop için header'ın üst border-radius'unu ayarla */
            .tesla-header {
                border-top-left-radius: 24px;
                border-top-right-radius: 24px;
            }

            .main-grid {
                display: grid;
                grid-template-columns: 1fr 1fr;
                gap: 0;
            }

            .main-grid .tesla-card {
                margin: 16px 10px;
            }

            .main-grid .tesla-card:first-child {
                margin-left: 20px;
            }

            .main-grid .tesla-card:last-child {
                margin-right: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="app-container">

        <!-- Tesla Header with Logo -->
        <div class="tesla-header">
            <!-- Tesla T Logo -->
            <div class="tesla-logo-svg">
                <img src="https://images.seeklogo.com/logo-png/36/1/tesla-motors-logo-png_seeklogo-365011.png" 
                     alt="Tesla" 
                     style="width: 80px; height: 80px; object-fit: contain;"
                     onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';">
                <div style="display: none; width: 80px; height: 80px; background: #e31e24; border-radius: 12px; margin: 0 auto; color: white; font-size: 48px; font-weight: bold; text-align: center; line-height: 80px; font-family: 'SF Pro Display', sans-serif;">T</div>
            </div>
        </div>

        <!-- Instructions Card -->
        <div class="tesla-card">
            <div class="card-header">
                <div class="card-title">How It Works</div>
                <div class="card-subtitle">Automated VIN number processing</div>
            </div>
            <div class="card-content">
                <div style="display: flex; flex-direction: column; gap: 12px;">
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <div style="width: 24px; height: 24px; background: #e31e24; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: 600;">1</div>
                        <div style="font-size: 14px; color: #ffffff;">Select PDF files containing VIN numbers</div>
                    </div>
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <div style="width: 24px; height: 24px; background: #e31e24; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: 600;">2</div>
                        <div style="font-size: 14px; color: #ffffff;">System extracts VIN numbers automatically</div>
                    </div>
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <div style="width: 24px; height: 24px; background: #e31e24; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: 600;">3</div>
                        <div style="font-size: 14px; color: #ffffff;">Sorted by last digit (0→9)</div>
                    </div>
                    <div style="display: flex; align-items: center; gap: 12px;">
                        <div style="width: 24px; height: 24px; background: #e31e24; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: 600;">4</div>
                        <div style="font-size: 14px; color: #ffffff;">Combined into single document</div>
                    </div>
                </div>
                
                <div style="background: rgba(227, 30, 36, 0.1); border-radius: 8px; padding: 12px; margin-top: 16px;">
                    <div style="color: #e31e24; font-weight: 600; font-size: 13px; margin-bottom: 8px;">Example:</div>
                    <div style="font-family: 'SF Mono', monospace; font-size: 12px; color: #8e8e93;">
                        <div>XP7YGCFS0SB123450 → Last digit: 0</div>
                        <div>XP7YGCFS0SB123451 → Last digit: 1</div>
                        <div>XP7YGCFS0SB123459 → Last digit: 9</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Upload and Stats in Mobile Grid -->
        <div class="main-grid">
            <!-- Upload Card -->
            <div class="tesla-card">
                <div class="card-header">
                    <div class="card-title">Upload Files</div>
                    <div class="card-subtitle">Select PDF documents</div>
                </div>
                <div class="card-content">
                    <div class="upload-area" id="uploadZone">
                        <input type="file" id="fileInput" class="file-input" multiple accept=".pdf">
                        <div class="upload-icon">📄</div>
                        <div class="upload-text" id="uploadText">Select PDFs</div>
                        <div class="upload-subtext">Tap to browse</div>
                    </div>
                </div>
            </div>

            <!-- Stats Card -->
            <div class="tesla-card">
                <div class="card-header">
                    <div class="card-title">Statistics</div>
                    <div class="card-subtitle">Processing overview</div>
                </div>
                <div class="card-content">
                    <div class="stats-grid">
                        <div class="stat-item">
                            <div class="stat-number" id="totalFiles">0</div>
                            <div class="stat-label">Files</div>
                        </div>
                        <div class="stat-item">
                            <div class="stat-number" id="processedFiles">0</div>
                            <div class="stat-label">Processed</div>
                        </div>
                        <div class="stat-item">
                            <div class="stat-number" id="errorFiles">0</div>
                            <div class="stat-label">Errors</div>
                        </div>
                        <div class="stat-item">
                            <div class="stat-number" id="totalPages">0</div>
                            <div class="stat-label">Pages</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Progress Card -->
        <div class="tesla-card progress-card" id="progressCard">
            <div class="card-header">
                <div class="card-title">Processing</div>
                <div class="card-subtitle">Analyzing documents</div>
            </div>
            <div class="progress-content">
                <div class="loading-spinner"></div>
                <div class="progress-bar">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <div class="progress-text" id="progressText">0 / 0 files processed</div>
            </div>
        </div>

        <!-- Results Card -->
        <div class="tesla-card" id="resultsCard" style="display: none;">
            <div class="card-header">
                <div class="card-title">Sorted Files</div>
                <div class="card-subtitle">Organized by VIN number</div>
            </div>
            <div class="results-list" id="resultsList"></div>
        </div>

        <!-- Action Buttons -->
        <div style="padding: 20px; display: none;" id="actionSection">
            <button class="tesla-button" id="combineBtn" onclick="createCombinedPDF()">
                Generate Combined PDF
            </button>
            <button class="tesla-button secondary" id="downloadListBtn" onclick="downloadSortedList()">
                Download Report
            </button>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
    <script>
        let sortedFiles = [];
        let processedData = { total: 0, processed: 0, errors: 0, totalPages: 0 };

        // File input and drag/drop setup
        const fileInput = document.getElementById('fileInput');
        const uploadZone = document.getElementById('uploadZone');

        fileInput.addEventListener('change', handleFiles);

        // Drag and drop functionality
        ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
            uploadZone.addEventListener(eventName, preventDefaults, false);
        });

        ['dragenter', 'dragover'].forEach(eventName => {
            uploadZone.addEventListener(eventName, highlight, false);
        });

        ['dragleave', 'drop'].forEach(eventName => {
            uploadZone.addEventListener(eventName, unhighlight, false);
        });

        uploadZone.addEventListener('drop', handleDrop, false);

        function preventDefaults(e) {
            e.preventDefault();
            e.stopPropagation();
        }

        function highlight() {
            uploadZone.classList.add('dragover');
        }

        function unhighlight() {
            uploadZone.classList.remove('dragover');
        }

        function handleDrop(e) {
            const dt = e.dataTransfer;
            const files = Array.from(dt.files).filter(file => 
                file.type === 'application/pdf' || file.name.toLowerCase().endsWith('.pdf')
            );
            
            if (files.length > 0) {
                const dataTransfer = new DataTransfer();
                files.forEach(file => dataTransfer.items.add(file));
                fileInput.files = dataTransfer.files;
                handleFiles({ target: fileInput });
            }
        }

        async function handleFiles(event) {
            const files = Array.from(event.target.files);
            if (files.length === 0) return;

            // Reset data
            sortedFiles = [];
            processedData = { total: 0, processed: 0, errors: 0, totalPages: 0 };

            // Update UI
            document.getElementById('progressCard').classList.add('show');
            document.getElementById('resultsCard').style.display = 'none';
            document.getElementById('actionSection').style.display = 'none';
            
            updateStats();

            try {
                await processFiles(files);
                sortFilesByLastDigit();
                displaySortedResults();
                document.getElementById('actionSection').style.display = 'block';
            } catch (error) {
                showError('Error processing files: ' + error.message);
            } finally {
                document.getElementById('progressCard').classList.remove('show');
            }
        }

        async function processFiles(files) {
            processedData.total = files.length;
            updateStats();

            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                try {
                    const result = await processFile(file);
                    sortedFiles.push(result);
                    processedData.processed++;
                    processedData.totalPages += result.pageCount;
                } catch (error) {
                    console.error(`Error processing ${file.name}:`, error);
                    processedData.errors++;
                    
                    sortedFiles.push({
                        name: file.name,
                        saseNumber: 'Not Found',
                        lastDigit: 999,
                        last6Digits: 'N/A',
                        file: file,
                        pageCount: 0,
                        error: error.message,
                        sortOrder: 999
                    });
                }

                // Update progress
                const progress = ((i + 1) / files.length) * 100;
                document.getElementById('progressFill').style.width = progress + '%';
                document.getElementById('progressText').textContent = `${i + 1} / ${files.length} files processed`;
                updateStats();

                if (i % 5 === 0) {
                    await new Promise(resolve => setTimeout(resolve, 10));
                }
            }
        }

        async function processFile(file) {
            const arrayBuffer = await file.arrayBuffer();
            const pdfDoc = await PDFLib.PDFDocument.load(arrayBuffer);
            
            let saseNumber = null;
            
            try {
                const form = pdfDoc.getForm();
                const fields = form.getFields();
                
                for (const field of fields) {
                    const fieldName = field.getName().toLowerCase();
                    if (fieldName.includes('şase') || fieldName.includes('sase') || fieldName.includes('VIN')) {
                        try {
                            if (field.constructor.name === 'PDFTextField') {
                                saseNumber = field.getText();
                                if (saseNumber && saseNumber.trim()) break;
                            }
                        } catch (e) {
                            continue;
                        }
                    }
                }
            } catch (e) {
                // Form extraction failed
            }

            if (!saseNumber || !saseNumber.trim()) {
                const textPatterns = [
                    file.name.match(/([A-Z0-9]{10,})/g),
                    file.name.match(/\b([A-Z]{2,3}\d{6,}[A-Z0-9]*)\b/g)
                ];
                
                for (const matches of textPatterns) {
                    if (matches && matches.length > 0) {
                        saseNumber = matches[0];
                        break;
                    }
                }
            }

            if (!saseNumber || !saseNumber.trim()) {
                throw new Error('VIN number not found');
            }

            const digits = saseNumber.replace(/[^0-9]/g, '');
            if (digits.length < 6) {
                throw new Error('Insufficient digits in VIN number');
            }

            const last6Digits = digits.slice(-6);
            const lastDigit = parseInt(last6Digits[5]);

            return {
                name: file.name,
                saseNumber: saseNumber.trim(),
                lastDigit: lastDigit,
                last6Digits: last6Digits,
                file: file,
                pdfDoc: pdfDoc,
                pageCount: pdfDoc.getPageCount(),
                sortOrder: lastDigit
            };
        }

        function sortFilesByLastDigit() {
            sortedFiles.sort((a, b) => {
                if (a.sortOrder === 999 && b.sortOrder === 999) {
                    return a.name.localeCompare(b.name);
                }
                if (a.sortOrder === 999) return 1;
                if (b.sortOrder === 999) return -1;
                
                if (a.lastDigit !== b.lastDigit) {
                    return a.lastDigit - b.lastDigit;
                }
                
                const aNum = parseInt(a.last6Digits);
                const bNum = parseInt(b.last6Digits);
                if (aNum !== bNum) {
                    return aNum - bNum;
                }
                
                return a.name.localeCompare(b.name);
            });
        }

        function displaySortedResults() {
            const resultsList = document.getElementById('resultsList');
            
            const validFiles = sortedFiles.filter(f => f.sortOrder !== 999);
            const errorFiles = sortedFiles.filter(f => f.sortOrder === 999);

            let html = '';

            validFiles.forEach((fileData, index) => {
                html += `
                    <div class="file-row">
                        <div class="file-info">
                            <div class="file-name">${fileData.name}</div>
                            <div class="file-meta">VIN: ${fileData.saseNumber} • ${fileData.pageCount} pages</div>
                        </div>
                        <div class="file-badges">
                            <div class="position-badge">#${index + 1}</div>
                            <div class="digit-badge">${fileData.lastDigit}</div>
                        </div>
                    </div>
                `;
            });

            errorFiles.forEach((fileData, index) => {
                html += `
                    <div class="file-row error">
                        <div class="file-info">
                            <div class="file-name">${fileData.name}</div>
                            <div class="file-meta">Error: ${fileData.error}</div>
                        </div>
                        <div class="file-badges">
                            <div class="position-badge">#${validFiles.length + index + 1}</div>
                            <div class="digit-badge">!</div>
                        </div>
                    </div>
                `;
            });

            resultsList.innerHTML = html;
            document.getElementById('resultsCard').style.display = 'block';
        }

        async function createCombinedPDF() {
            try {
                const combineBtn = document.getElementById('combineBtn');
                combineBtn.disabled = true;
                combineBtn.textContent = 'Processing...';

                const combinedPdf = await PDFLib.PDFDocument.create();
                await addTitlePage(combinedPdf);

                let currentProgress = 0;
                const validFiles = sortedFiles.filter(f => f.sortOrder !== 999);

                for (let i = 0; i < validFiles.length; i++) {
                    const fileData = validFiles[i];
                    
                    try {
                        const indices = Array.from({length: fileData.pageCount}, (_, i) => i);
                        const copiedPages = await combinedPdf.copyPages(fileData.pdfDoc, indices);
                        
                        copiedPages.forEach(page => combinedPdf.addPage(page));
                        
                        currentProgress++;
                        const progress = (currentProgress / validFiles.length) * 100;
                        document.getElementById('progressFill').style.width = progress + '%';
                        
                        combineBtn.textContent = `Processing... ${currentProgress}/${validFiles.length}`;
                            
                    } catch (error) {
                        console.error(`Error adding ${fileData.name} to combined PDF:`, error);
                    }

                    if (i % 3 === 0) {
                        await new Promise(resolve => setTimeout(resolve, 10));
                    }
                }

                // Generate and download the PDF
                const pdfBytes = await combinedPdf.save();
                const blob = new Blob([pdfBytes], { type: 'application/pdf' });
                const url = URL.createObjectURL(blob);
                
                const a = document.createElement('a');
                a.href = url;
                a.download = `Tesla_Combined_PDF_${new Date().toISOString().split('T')[0]}.pdf`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                
                URL.revokeObjectURL(url);
                
                showSuccess(`Success! ${validFiles.length} PDF files combined and downloaded.`);
                
            } catch (error) {
                showError('Error during PDF combination: ' + error.message);
            } finally {
                const combineBtn = document.getElementById('combineBtn');
                combineBtn.disabled = false;
                combineBtn.textContent = 'Generate Combined PDF';
                document.getElementById('progressFill').style.width = '0%';
            }
        }

        async function addTitlePage(pdfDoc) {
            const titlePage = pdfDoc.addPage([595, 842]);
            const { width, height } = titlePage.getSize();
            
            let font;
            try {
                font = await pdfDoc.embedFont(PDFLib.StandardFonts.Helvetica);
            } catch (e) {
                font = await pdfDoc.embedFont(PDFLib.StandardFonts.Courier);
            }
            
            // Tesla-style title page
            titlePage.drawText('TESLA PDF MERGER REPORT', {
                x: 50,
                y: height - 100,
                size: 24,
                color: PDFLib.rgb(0.89, 0.12, 0.14),
                font: font
            });

            titlePage.drawText(`Generated: ${new Date().toLocaleString('en-US')}`, {
                x: 50,
                y: height - 140,
                size: 12,
                color: PDFLib.rgb(0.4, 0.4, 0.4),
                font: font
            });

            const validFiles = sortedFiles.filter(f => f.sortOrder !== 999);
            const summary = [
                `PROCESSING SUMMARY`,
                `Total Files: ${processedData.total}`,
                `Successfully Processed: ${processedData.processed}`,
                `Error Files: ${processedData.errors}`,
                `Total Pages: ${processedData.totalPages}`,
                ``,
                `SORTING METHOD:`,
                `Ascending order by last digit of VIN number`,
                `(0 -> 1 -> 2 -> ... -> 9)`,
                ``,
                `FILE LIST (First 20 files):`,
            ];

            let yPos = height - 200;
            summary.forEach(line => {
                titlePage.drawText(line, {
                    x: 50,
                    y: yPos,
                    size: 12,
                    color: PDFLib.rgb(0.2, 0.2, 0.2),
                    font: font
                });
                yPos -= 20;
            });

            validFiles.slice(0, 20).forEach((fileData, index) => {
                if (yPos > 50) {
                    titlePage.drawText(`${index + 1}. ${fileData.name} (Last digit: ${fileData.lastDigit})`, {
                        x: 50,
                        y: yPos,
                        size: 10,
                        color: PDFLib.rgb(0.4, 0.4, 0.4),
                        font: font
                    });
                    yPos -= 15;
                }
            });

            if (validFiles.length > 20) {
                titlePage.drawText(`... and ${validFiles.length - 20} more files`, {
                    x: 50,
                    y: yPos,
                    size: 10,
                    color: PDFLib.rgb(0.6, 0.6, 0.6),
                    font: font
                });
            }
        }

        function downloadSortedList() {
            const validFiles = sortedFiles.filter(f => f.sortOrder !== 999);
            const errorFiles = sortedFiles.filter(f => f.sortOrder === 999);
            
            let report = `TESLA PDF DOCUMENT MANAGER - SORTING REPORT\n`;
            report += `Generated: ${new Date().toLocaleString('en-US')}\n`;
            report += `=`.repeat(60) + `\n\n`;
            
            report += `PROCESSING STATISTICS:\n`;
            report += `- Total Files: ${processedData.total}\n`;
            report += `- Successfully Processed: ${processedData.processed}\n`;
            report += `- Error Files: ${processedData.errors}\n`;
            report += `- Total Pages: ${processedData.totalPages}\n\n`;

            report += `SORTING METHOD:\n`;
            report += `Ascending order by last digit of VIN number\n`;
            report += `(0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9)\n\n`;

            report += `SORTED FILE LIST:\n`;
            report += `=`.repeat(60) + `\n`;

            validFiles.forEach((file, index) => {
                report += `${String(index + 1).padStart(3, '0')}. ${file.name}\n`;
                report += `     VIN No: ${file.saseNumber}\n`;
                report += `     Last 6 Digits: ${file.last6Digits}\n`;
                report += `     Last Digit: ${file.lastDigit}\n`;
                report += `     Page Count: ${file.pageCount}\n\n`;
            });

            if (errorFiles.length > 0) {
                report += `\nERROR FILES:\n`;
                report += `=`.repeat(30) + `\n`;
                errorFiles.forEach((file, index) => {
                    report += `${index + 1}. ${file.name}\n`;
                    report += `   Error: ${file.error}\n\n`;
                });
            }

            const blob = new Blob([report], { type: 'text/plain;charset=utf-8' });
            const url = URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = `Tesla_Sorting_Report_${new Date().toISOString().split('T')[0]}.txt`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            
            URL.revokeObjectURL(url);
            showSuccess('Sorting report downloaded successfully!');
        }

        function updateStats() {
            document.getElementById('totalFiles').textContent = processedData.total;
            document.getElementById('processedFiles').textContent = processedData.processed;
            document.getElementById('errorFiles').textContent = processedData.errors;
            document.getElementById('totalPages').textContent = processedData.totalPages;
        }

        function showError(message) {
            const existingErrors = document.querySelectorAll('.error-message');
            existingErrors.forEach(el => el.remove());

            const errorDiv = document.createElement('div');
            errorDiv.className = 'error-message message';
            errorDiv.textContent = message;
            
            const container = document.querySelector('.app-container');
            container.appendChild(errorDiv);
            
            setTimeout(() => errorDiv.remove(), 5000);
        }

        function showSuccess(message) {
            const existingSuccess = document.querySelectorAll('.success-message');
            existingSuccess.forEach(el => el.remove());

            const successDiv = document.createElement('div');
            successDiv.className = 'success-message message';
            successDiv.textContent = message;
            
            const actionSection = document.getElementById('actionSection');
            actionSection.appendChild(successDiv);
            
            setTimeout(() => successDiv.remove(), 5000);
        }

        // Update upload text when files selected
        fileInput.addEventListener('change', function() {
            const fileCount = this.files.length;
            const uploadText = document.getElementById('uploadText');
            if (fileCount > 0) {
                uploadText.textContent = `${fileCount} PDFs selected`;
                uploadZone.style.borderColor = '#e31e24';
                uploadZone.style.background = 'rgba(227, 30, 36, 0.1)';
            } else {
                uploadText.textContent = 'Select PDFs';
                uploadZone.style.borderColor = '#3a3a3c';
                uploadZone.style.background = '#2c2c2e';
            }
        });

        // Smooth page load
        window.addEventListener('load', () => {
            document.body.style.opacity = '0';
            document.body.style.transition = 'opacity 0.5s ease';
            setTimeout(() => {
                document.body.style.opacity = '1';
            }, 100);
        });
    </script>
</body>
</html>
