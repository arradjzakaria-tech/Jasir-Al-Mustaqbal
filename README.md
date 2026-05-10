<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة معدلي: البيام والبكالوريا</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;800&display=swap');

        :root {
            --primary: #1e3a8a; /* أزرق داكن */
            --primary-light: #3b82f6; /* أزرق فاتح */
            --gold: #d97706; /* ذهبي */
            --gold-light: #fef3c7; /* ذهبي فاتح */
            --bg-color: #f3f4f6;
            --surface: #ffffff;
            --text-main: #1f2937;
            --text-muted: #6b7280;
            --success: #16a34a;
            --danger: #dc2626;
            --border: #e5e7eb;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            line-height: 1.6;
            padding-bottom: 40px;
        }

        /* Header */
        header {
            background: linear-gradient(135deg, var(--primary), var(--primary-light));
            color: white;
            text-align: center;
            padding: 2.5rem 1rem;
            border-bottom: 5px solid var(--gold);
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        header h1 { font-size: 2.2rem; font-weight: 800; margin-bottom: 0.5rem; }
        header p { font-size: 1.1rem; color: var(--gold-light); font-weight: 600; }

        /* Container */
        .container {
            max-width: 900px;
            margin: -20px auto 0;
            padding: 0 15px;
            position: relative;
            z-index: 10;
        }

        /* Views */
        .view { display: none; animation: fadeIn 0.4s ease; }
        .view.active { display: block; }

        /* Home Cards */
        .home-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .home-card {
            background-color: var(--surface);
            border-radius: 16px;
            padding: 3rem 2rem;
            text-align: center;
            cursor: pointer;
            border: 2px solid var(--border);
            transition: all 0.3s ease;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }

        .home-card:hover {
            transform: translateY(-5px);
            border-color: var(--gold);
            box-shadow: 0 15px 20px -3px rgba(217, 119, 6, 0.2);
        }

        .home-card h2 { color: var(--primary); font-size: 1.8rem; margin-bottom: 1rem; }
        .home-card p { color: var(--text-muted); font-size: 1.1rem; font-weight: 600; }
        
        .icon-circle {
            width: 80px; height: 80px;
            background-color: var(--gold-light);
            color: var(--gold);
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            margin: 0 auto 1.5rem;
        }

        /* Card Section */
        .card {
            background-color: var(--surface);
            border-radius: 12px;
            padding: 2rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            margin-bottom: 1.5rem;
            border: 1px solid var(--border);
        }

        .top-bar {
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 2rem; padding-bottom: 1rem; border-bottom: 2px dashed var(--border);
        }

        .btn-back {
            background: none; border: none; color: var(--primary); font-weight: 800;
            font-size: 1.1rem; cursor: pointer; display: flex; align-items: center; gap: 0.5rem;
        }

        .btn-back:hover { color: var(--gold); }

        .section-title { color: var(--primary); font-size: 1.5rem; font-weight: 800; margin-bottom: 1.5rem; border-right: 4px solid var(--gold); padding-right: 10px; }

        /* Form Elements */
        .form-group { margin-bottom: 1.5rem; }
        .form-group label { display: block; font-weight: 800; margin-bottom: 0.5rem; color: var(--primary); }
        
        select {
            width: 100%; padding: 1rem; border: 2px solid var(--border); border-radius: 8px;
            font-size: 1.1rem; font-weight: 600; background-color: var(--surface); color: var(--text-main); cursor: pointer;
        }
        select:focus { border-color: var(--gold); outline: none; }

        .exemptions-container {
            display: flex; gap: 1.5rem; flex-wrap: wrap; background-color: #f8fafc;
            padding: 1rem; border-radius: 8px; border: 1px solid var(--border);
        }
        .checkbox-label { display: flex; align-items: center; gap: 0.5rem; font-weight: 600; cursor: pointer; }
        .checkbox-label input { width: 1.2rem; height: 1.2rem; accent-color: var(--gold); }

        /* Grid Inputs */
        .subjects-grid {
            display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.25rem; margin-bottom: 2rem;
        }

        .input-group { display: flex; flex-direction: column; gap: 0.5rem; transition: opacity 0.3s; }
        .input-group.disabled { opacity: 0.5; pointer-events: none; }
        .input-group label { font-weight: 600; display: flex; justify-content: space-between; align-items: center; font-size: 0.95rem;}
        
        .coef-badge {
            background-color: var(--gold-light); color: var(--gold); font-size: 0.8rem;
            padding: 0.2rem 0.6rem; border-radius: 20px; font-weight: 800;
        }

        .input-group input {
            padding: 0.8rem; border: 2px solid var(--border); border-radius: 8px;
            font-size: 1.1rem; text-align: center; transition: border 0.3s;
        }
        .input-group input:focus { border-color: var(--gold); outline: none; }
        .input-group input.error { border-color: var(--danger); background-color: #fef2f2; }

        /* Buttons */
        .actions { display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center; }
        button {
            padding: 1rem 2rem; border: none; border-radius: 8px; font-size: 1.1rem;
            font-weight: 800; cursor: pointer; transition: all 0.3s; display: inline-flex;
            align-items: center; justify-content: center; gap: 0.5rem; flex: 1; min-width: 200px;
        }
        .btn-calc { background-color: var(--primary); color: white; }
        .btn-calc:hover { background-color: #1e40af; transform: translateY(-2px); }
        .btn-clear { background-color: white; color: var(--danger); border: 2px solid var(--danger); }
        .btn-clear:hover { background-color: #fee2e2; }
        .btn-whatsapp { background-color: #25D366; color: white; width: 100%; margin-top: 1.5rem; }
        .btn-whatsapp:hover { background-color: #1ebc56; }

        /* Results */
        .result-section { display: none; animation: fadeIn 0.5s; text-align: center; }
        .result-box {
            background: linear-gradient(135deg, var(--surface), var(--bg-color));
            border: 3px solid var(--gold); border-radius: 12px; padding: 2rem; margin-bottom: 1.5rem;
        }
        .average-score { font-size: 4rem; font-weight: 800; line-height: 1; margin: 1rem 0; direction: ltr; }
        
        .status-badge {
            display: inline-block; padding: 0.5rem 2rem; border-radius: 30px;
            font-size: 1.3rem; font-weight: 800; color: white; margin-bottom: 1rem;
        }
        .status-pass { background-color: var(--success); }
        .status-fail { background-color: var(--danger); }
        .status-rescue { background-color: var(--primary); }
        
        .motivation { font-size: 1.2rem; color: var(--text-main); font-weight: 700; margin-bottom: 1rem; }
        
        .totals-info {
            display: flex; justify-content: space-around; background-color: var(--surface);
            padding: 1rem; border-radius: 8px; border: 1px solid var(--border); font-weight: 700; flex-wrap: wrap; gap: 1rem;
        }

        .rescue-info {
            background-color: #e0f2fe; border: 2px dashed var(--primary-light);
            padding: 1rem; border-radius: 8px; margin-top: 1rem; font-weight: 700; color: var(--primary);
        }

        /* Table */
        .table-responsive { overflow-x: auto; margin-top: 1.5rem; }
        table { width: 100%; border-collapse: collapse; text-align: right; }
        th, td { padding: 0.8rem; border-bottom: 1px solid var(--border); }
        th { background-color: var(--primary); color: white; font-weight: 600; }
        tr:nth-child(even) { background-color: #f8fafc; }

        /* Footer */
        footer { text-align: center; margin-top: 3rem; padding: 1rem; color: var(--text-muted); font-size: 0.95rem; line-height: 1.8; }
        footer strong { color: var(--primary); font-size: 1.1rem;}

        /* Toast */
        .toast {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            background-color: #1f2937; color: white; padding: 1rem 2rem; border-radius: 8px;
            font-weight: 700; box-shadow: 0 4px 15px rgba(0,0,0,0.2); z-index: 1000;
            display: none; opacity: 0; transition: opacity 0.3s; text-align: center; width: 90%; max-width: 400px;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        @media (max-width: 600px) {
            header h1 { font-size: 1.8rem; }
            .card { padding: 1.5rem; }
            .average-score { font-size: 3rem; }
            th, td { padding: 0.6rem; font-size: 0.9rem; }
            .home-card { padding: 2rem 1.5rem; }
        }
    </style>
</head>
<body>

    <header>
        <h1>حاسبة معدلي</h1>
        <p>البيام والبكالوريا - حسب البرنامج الجزائري</p>
    </header>

    <div class="container">
        
        <!-- ================= HOME VIEW ================= -->
        <div id="view-home" class="view active">
            <div class="home-grid">
                <div class="home-card" onclick="switchView('view-bem')">
                    <div class="icon-circle">
                        <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"></path></svg>
                    </div>
                    <h2>شهادة التعليم المتوسط (BEM)</h2>
                    <p>حساب المعدل العام ومعدل الإنقاذ</p>
                </div>
                
                <div class="home-card" onclick="switchView('view-bac')">
                    <div class="icon-circle">
                        <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 10v6M2 10l10-5 10 5-10 5z"></path><path d="M6 12v5c3 3 9 3 12 0v-5"></path></svg>
                    </div>
                    <h2>شهادة البكالوريا (BAC)</h2>
                    <p>حساب المعدل لجميع الشعب بخصائص الإعفاء</p>
                </div>
            </div>
        </div>

        <!-- ================= BEM VIEW ================= -->
        <div id="view-bem" class="view">
            <div class="card">
                <div class="top-bar">
                    <h2 style="color: var(--primary);">حساب معدل BEM</h2>
                    <button class="btn-back" onclick="switchView('view-home')">
                        العودة للرئيسية
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 14 4 9 9 4"></polyline><path d="M20 20v-7a4 4 0 0 0-4-4H4"></path></svg>
                    </button>
                </div>

                <div class="section-title">نقاط المواد في الشهادة</div>
                <div class="subjects-grid" id="bem-subjects-container"></div>

                <div class="section-title" style="margin-top: 1rem;">معدلات الفصول (لخيار الإنقاذ)</div>
                <div class="subjects-grid">
                    <div class="input-group">
                        <label>معدل الفصل الأول</label>
                        <input type="number" id="bem-t1" min="0" max="20" step="0.01" placeholder="مثال: 12.50" oninput="saveData('bem')">
                    </div>
                    <div class="input-group">
                        <label>معدل الفصل الثاني</label>
                        <input type="number" id="bem-t2" min="0" max="20" step="0.01" placeholder="مثال: 13.00" oninput="saveData('bem')">
                    </div>
                    <div class="input-group">
                        <label>معدل الفصل الثالث</label>
                        <input type="number" id="bem-t3" min="0" max="20" step="0.01" placeholder="مثال: 14.25" oninput="saveData('bem')">
                    </div>
                </div>

                <div class="actions">
                    <button class="btn-calc" onclick="calculateBEM()">احسب المعدل</button>
                    <button class="btn-clear" onclick="clearData('bem')">مسح البيانات</button>
                </div>
            </div>

            <!-- BEM Result -->
            <div class="card result-section" id="bem-result">
                <div class="result-box">
                    <h2 style="color: var(--primary);">معدل شهادة التعليم المتوسط</h2>
                    <div class="average-score" id="bem-final-avg">00.00</div>
                    <div class="status-badge" id="bem-status-badge">...</div>
                    <div class="motivation" id="bem-motivation">...</div>
                    
                    <div id="bem-rescue-box" class="rescue-info" style="display:none;">
                        <div>المعدل السنوي: <span id="bem-annual-avg"></span></div>
                        <div>معدل الإنقاذ: <span id="bem-rescue-avg"></span></div>
                        <div style="margin-top: 5px; color: var(--danger); font-size: 0.9em;">(معدل الإنقاذ = (معدل الشهادة + المعدل السنوي) ÷ 2)</div>
                    </div>
                </div>
                
                <h3 style="text-align: right; color: var(--primary);">تفاصيل الحساب:</h3>
                <div class="table-responsive">
                    <table id="bem-table">
                        <thead><tr><th>المادة</th><th>النقطة</th><th>المعامل</th><th>المجموع</th></tr></thead>
                        <tbody id="bem-table-body"></tbody>
                    </table>
                </div>
                <button class="btn-whatsapp" onclick="shareBEM()">مشاركة النتيجة عبر واتساب</button>
            </div>
        </div>

        <!-- ================= BAC VIEW ================= -->
        <div id="view-bac" class="view">
            <div class="card">
                <div class="top-bar">
                    <h2 style="color: var(--primary);">حساب معدل البكالوريا</h2>
                    <button class="btn-back" onclick="switchView('view-home')">
                        العودة للرئيسية
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 14 4 9 9 4"></polyline><path d="M20 20v-7a4 4 0 0 0-4-4H4"></path></svg>
                    </button>
                </div>

                <div class="form-group">
                    <label>اختر الشعبة:</label>
                    <select id="bac-stream" onchange="renderBacSubjects(); saveData('bac')">
                        <option value="علوم تجريبية">علوم تجريبية</option>
                        <option value="رياضيات">رياضيات</option>
                        <option value="تقني رياضي">تقني رياضي</option>
                        <option value="تسيير واقتصاد">تسيير واقتصاد</option>
                        <option value="آداب وفلسفة">آداب وفلسفة</option>
                        <option value="لغات أجنبية">لغات أجنبية</option>
                    </select>
                </div>

                <div class="exemptions-container" style="margin-bottom: 2rem;">
                    <label class="checkbox-label">
                        <input type="checkbox" id="bac-exempt-sport" onchange="toggleBacExemptions(); saveData('bac')"> معفى من التربية البدنية
                    </label>
                    <label class="checkbox-label">
                        <input type="checkbox" id="bac-exempt-amazigh" onchange="toggleBacExemptions(); saveData('bac')"> معفى من اللغة الأمازيغية
                    </label>
                </div>

                <div class="section-title">نقاط المواد</div>
                <div class="subjects-grid" id="bac-subjects-container"></div>

                <div class="actions">
                    <button class="btn-calc" onclick="calculateBAC()">احسب المعدل</button>
                    <button class="btn-clear" onclick="clearData('bac')">مسح البيانات</button>
                </div>
            </div>

            <!-- BAC Result -->
            <div class="card result-section" id="bac-result">
                <div class="result-box">
                    <h2 style="color: var(--primary);">المعدل النهائي للبكالوريا</h2>
                    <div class="average-score" id="bac-final-avg">00.00</div>
                    <div class="status-badge" id="bac-status-badge">...</div>
                    <div class="motivation" id="bac-motivation">...</div>
                    
                    <div class="totals-info">
                        <div>مجموع المعاملات: <span id="bac-total-coefs" style="color: var(--gold);">0</span></div>
                        <div>النقاط الموزونة: <span id="bac-total-points" style="color: var(--primary);">0</span></div>
                    </div>
                </div>

                <h3 style="text-align: right; color: var(--primary);">الجدول التفصيلي:</h3>
                <div class="table-responsive">
                    <table id="bac-table">
                        <thead><tr><th>المادة</th><th>النقطة</th><th>المعامل</th><th>المجموع</th></tr></thead>
                        <tbody id="bac-table-body"></tbody>
                    </table>
                </div>
                <button class="btn-whatsapp" onclick="shareBAC()">مشاركة النتيجة عبر واتساب</button>
            </div>
        </div>

    </div>

    <footer>
        <p>صاحب الفكرة والإنجاز: <br><strong>الأستاذ عراج زكرياء</strong></p>
        <p>مؤسسة جسر المستقبل - عين البيضاء السانية وهران</p>
    </footer>

    <div class="toast" id="toast"></div>

    <script>
        // ================= DATA CONFIGURATION =================
        
        // مواد البيام
        const bemSubjectsList = [
            { id: 'bem-arb', name: 'اللغة العربية', coef: 5 },
            { id: 'bem-math', name: 'الرياضيات', coef: 4 },
            { id: 'bem-fr', name: 'اللغة الفرنسية', coef: 3 },
            { id: 'bem-eng', name: 'اللغة الإنجليزية', coef: 2 },
            { id: 'bem-sci', name: 'العلوم الطبيعية', coef: 2 },
            { id: 'bem-phy', name: 'العلوم الفيزيائية', coef: 2 },
            { id: 'bem-his', name: 'التاريخ والجغرافيا', coef: 2 },
            { id: 'bem-isl', name: 'التربية الإسلامية', coef: 2 },
            { id: 'bem-civ', name: 'التربية المدنية', coef: 1 },
            { id: 'bem-spo', name: 'التربية البدنية', coef: 1 },
            { id: 'bem-art', name: 'التربية الفنية/الموسيقية', coef: 0, isBonus: true }
        ];

        /* * يمكن تعديل المعاملات هنا حسب آخر قرار رسمي من وزارة التربية الوطنية.
         */
        const branches = {
            "علوم تجريبية": [
                { id: "math", name: "الرياضيات", coef: 5 },
                { id: "science", name: "علوم الطبيعة والحياة", coef: 6 },
                { id: "physics", name: "العلوم الفيزيائية", coef: 5 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 3 },
                { id: "french", name: "اللغة الفرنسية", coef: 2 },
                { id: "english", name: "اللغة الإنجليزية", coef: 2 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 2 },
                { id: "philosophy", name: "الفلسفة", coef: 2 }
            ],
            "رياضيات": [
                { id: "math", name: "الرياضيات", coef: 7 },
                { id: "physics", name: "العلوم الفيزيائية", coef: 6 },
                { id: "science", name: "علوم الطبيعة والحياة", coef: 2 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 3 },
                { id: "french", name: "اللغة الفرنسية", coef: 2 },
                { id: "english", name: "اللغة الإنجليزية", coef: 2 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 2 },
                { id: "philosophy", name: "الفلسفة", coef: 2 }
            ],
            "تقني رياضي": [
                { id: "math", name: "الرياضيات", coef: 6 },
                { id: "physics", name: "العلوم الفيزيائية", coef: 6 },
                { id: "tech", name: "التكنولوجيا (هندسة)", coef: 6 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 3 },
                { id: "french", name: "اللغة الفرنسية", coef: 2 },
                { id: "english", name: "اللغة الإنجليزية", coef: 2 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 2 },
                { id: "philosophy", name: "الفلسفة", coef: 2 }
            ],
            "تسيير واقتصاد": [
                { id: "accounting", name: "التسيير المحاسبي والمالي", coef: 6 },
                { id: "economics", name: "الاقتصاد والمناجمنت", coef: 5 },
                { id: "math", name: "الرياضيات", coef: 5 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 4 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 3 },
                { id: "law", name: "القانون", coef: 2 },
                { id: "french", name: "اللغة الفرنسية", coef: 2 },
                { id: "english", name: "اللغة الإنجليزية", coef: 2 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "philosophy", name: "الفلسفة", coef: 2 }
            ],
            "آداب وفلسفة": [
                { id: "philosophy", name: "الفلسفة", coef: 6 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 6 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 4 },
                { id: "french", name: "اللغة الفرنسية", coef: 3 },
                { id: "english", name: "اللغة الإنجليزية", coef: 3 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "math", name: "الرياضيات", coef: 2 }
            ],
            "لغات أجنبية": [
                { id: "foreign3", name: "اللغة الأجنبية الثالثة", coef: 5 },
                { id: "arabic", name: "اللغة العربية وآدابها", coef: 5 },
                { id: "french", name: "اللغة الفرنسية", coef: 5 },
                { id: "english", name: "اللغة الإنجليزية", coef: 5 },
                { id: "history", name: "التاريخ والجغرافيا", coef: 2 },
                { id: "philosophy", name: "الفلسفة", coef: 2 },
                { id: "islamic", name: "التربية الإسلامية", coef: 2 },
                { id: "math", name: "الرياضيات", coef: 2 }
            ]
        };

        const exemptableSubjects = [
            { id: "bac-sport", name: "التربية البدنية والرياضية", coef: 1 },
            { id: "bac-amazigh", name: "اللغة الأمازيغية", coef: 2 }
        ];

        // Global State for Sharing
        let currentBemResult = null;
        let currentBacResult = null;

        // ================= UI & NAVIGATION =================

        function switchView(viewId) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function showToast(msg) {
            const toast = document.getElementById('toast');
            toast.textContent = msg;
            toast.style.display = 'block';
            void toast.offsetWidth;
            toast.style.opacity = '1';
            setTimeout(() => {
                toast.style.opacity = '0';
                setTimeout(() => toast.style.display = 'none', 300);
            }, 3000);
        }

        function getMotivationMessage(avg) {
            if (avg >= 16) return "ممتاز جدًا، واصل التألق والنجاح! 🌟";
            if (avg >= 14) return "جيد جدًا، نتيجة رائعة وعمل مميز! 👏";
            if (avg >= 12) return "جيد، أداء طيب ويمكنك تحسينها أكثر! 👍";
            if (avg >= 10) return "مقبول، مبارك لك النجاح تحتاج لدعم أكثر مستقبلاً. ✅";
            return "تحتاج إلى مراجعة جدية وخطة عمل، لا تستسلم! 💪";
        }

        // ================= BEM LOGIC =================

        function renderBemSubjects() {
            const container = document.getElementById('bem-subjects-container');
            container.innerHTML = '';
            bemSubjectsList.forEach(sub => {
                const coefText = sub.isBonus ? 'بدون' : sub.coef;
                container.innerHTML += `
                    <div class="input-group">
                        <label for="${sub.id}">${sub.name} <span class="coef-badge">معامل: ${coefText}</span></label>
                        <input type="number" id="${sub.id}" min="0" max="20" step="0.25" placeholder="0-20" oninput="saveData('bem')">
                    </div>
                `;
            });
        }

        function calculateBEM() {
            let totalPoints = 0;
            let totalCoefs = 0;
            let hasError = false;
            const tableBody = document.getElementById('bem-table-body');
            tableBody.innerHTML = '';

            // Calculate BEM standard score
            for (let sub of bemSubjectsList) {
                const input = document.getElementById(sub.id);
                input.classList.remove('error');
                let val = input.value.trim();

                if (val === '') { input.classList.add('error'); hasError = true; continue; }
                
                let grade = parseFloat(val);
                if (isNaN(grade) || grade < 0 || grade > 20) { input.classList.add('error'); hasError = true; continue; }

                if (sub.isBonus) {
                    let bonus = grade > 10 ? grade - 10 : 0;
                    totalPoints += bonus;
                    tableBody.innerHTML += `<tr style="color:var(--success)"><td>${sub.name}</td><td>${grade}</td><td>إضافي</td><td>+ ${bonus}</td></tr>`;
                } else {
                    let pts = grade * sub.coef;
                    totalPoints += pts;
                    totalCoefs += sub.coef;
                    tableBody.innerHTML += `<tr><td>${sub.name}</td><td>${grade}</td><td>${sub.coef}</td><td>${pts.toFixed(2)}</td></tr>`;
                }
            }

            if (hasError) { showToast("الرجاء إدخال نقاط صحيحة بين 0 و 20 لجميع مواد الشهادة."); return; }

            const bemAvg = totalPoints / (totalCoefs || 24);
            
            // Check Trimesters
            const t1Input = document.getElementById('bem-t1');
            const t2Input = document.getElementById('bem-t2');
            const t3Input = document.getElementById('bem-t3');
            
            t1Input.classList.remove('error'); t2Input.classList.remove('error'); t3Input.classList.remove('error');
            
            let v1 = t1Input.value.trim(), v2 = t2Input.value.trim(), v3 = t3Input.value.trim();
            let annualAvg = null;
            let needsAnnual = bemAvg < 10;

            if (needsAnnual) {
                if(v1===''||v2===''||v3==='' || v1<0||v1>20 || v2<0||v2>20 || v3<0||v3>20){
                     t1Input.classList.add(v1===''||v1<0||v1>20?'error':'');
                     t2Input.classList.add(v2===''||v2<0||v2>20?'error':'');
                     t3Input.classList.add(v3===''||v3<0||v3>20?'error':'');
                     showToast("معدلك في الشهادة أقل من 10. الرجاء إدخال معدلات الفصول لحساب الإنقاذ.");
                     return;
                }
                annualAvg = (parseFloat(v1) + parseFloat(v2) + parseFloat(v3)) / 3;
            }

            // Determine Status
            let finalStatus = '';
            let isPass = false;
            let isRescue = false;
            let rescueAvg = 0;

            if (bemAvg >= 10) {
                finalStatus = "ناجح مباشرة";
                isPass = true;
            } else {
                rescueAvg = (bemAvg + annualAvg) / 2;
                if (rescueAvg >= 10) {
                    finalStatus = "ناجح بالإنقاذ";
                    isPass = true;
                    isRescue = true;
                } else {
                    finalStatus = "راسب";
                    isPass = false;
                }
            }

            // Update UI
            document.getElementById('bem-final-avg').textContent = bemAvg.toFixed(2);
            document.getElementById('bem-final-avg').style.color = bemAvg >= 10 ? 'var(--success)' : (isPass ? 'var(--primary)' : 'var(--danger)');
            
            const badge = document.getElementById('bem-status-badge');
            badge.textContent = finalStatus;
            badge.className = 'status-badge ' + (isRescue ? 'status-rescue' : (isPass ? 'status-pass' : 'status-fail'));

            document.getElementById('bem-motivation').textContent = getMotivationMessage(isRescue ? rescueAvg : bemAvg);

            const rescueBox = document.getElementById('bem-rescue-box');
            if (needsAnnual) {
                document.getElementById('bem-annual-avg').textContent = annualAvg.toFixed(2);
                document.getElementById('bem-rescue-avg').textContent = rescueAvg.toFixed(2);
                rescueBox.style.display = 'block';
            } else {
                rescueBox.style.display = 'none';
            }

            currentBemResult = { avg: bemAvg.toFixed(2), status: finalStatus };
            document.getElementById('bem-result').style.display = 'block';
            document.getElementById('bem-result').scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        function shareBEM() {
            if(!currentBemResult) return;
            const txt = `حاسبة معدلي 🎓\nالنوع: شهادة التعليم المتوسط (BEM)\nالمعدل: ${currentBemResult.avg}\nالنتيجة: ${currentBemResult.status}\n\nصاحب الفكرة: الأستاذ عراج زكرياء\nجسر المستقبل - وهران`;
            window.open(`https://api.whatsapp.com/send?text=${encodeURIComponent(txt)}`, '_blank');
        }

        // ================= BAC LOGIC =================
        let currentBacActiveSubjects = [];

        function renderBacSubjects() {
            const stream = document.getElementById('bac-stream').value;
            const container = document.getElementById('bac-subjects-container');
            container.innerHTML = '';
            
            currentBacActiveSubjects = [...branches[stream], ...exemptableSubjects];

            currentBacActiveSubjects.forEach(sub => {
                container.innerHTML += `
                    <div class="input-group" id="grp-${sub.id}">
                        <label for="inp-${sub.id}">${sub.name} <span class="coef-badge">معامل: ${sub.coef}</span></label>
                        <input type="number" id="inp-${sub.id}" min="0" max="20" step="0.25" placeholder="0-20" oninput="saveData('bac')">
                    </div>
                `;
            });
            toggleBacExemptions();
            document.getElementById('bac-result').style.display = 'none';
        }

        function toggleBacExemptions() {
            const noSport = document.getElementById('bac-exempt-sport').checked;
            const noAmazigh = document.getElementById('bac-exempt-amazigh').checked;

            const sportGrp = document.getElementById('grp-bac-sport');
            const sportInp = document.getElementById('inp-bac-sport');
            if(sportGrp) {
                if(noSport) { sportGrp.classList.add('disabled'); sportInp.disabled=true; sportInp.value=''; sportInp.classList.remove('error');}
                else { sportGrp.classList.remove('disabled'); sportInp.disabled=false; }
            }

            const amzGrp = document.getElementById('grp-bac-amazigh');
            const amzInp = document.getElementById('inp-bac-amazigh');
            if(amzGrp) {
                if(noAmazigh) { amzGrp.classList.add('disabled'); amzInp.disabled=true; amzInp.value=''; amzInp.classList.remove('error');}
                else { amzGrp.classList.remove('disabled'); amzInp.disabled=false; }
            }
        }

        function calculateBAC() {
            let pts = 0, coefs = 0, hasErr = false;
            const noSport = document.getElementById('bac-exempt-sport').checked;
            const noAmazigh = document.getElementById('bac-exempt-amazigh').checked;
            const tbody = document.getElementById('bac-table-body');
            tbody.innerHTML = '';

            for(let sub of currentBacActiveSubjects) {
                if(sub.id === 'bac-sport' && noSport) continue;
                if(sub.id === 'bac-amazigh' && noAmazigh) continue;

                const inp = document.getElementById(`inp-${sub.id}`);
                inp.classList.remove('error');
                let val = inp.value.trim();

                if(val === '' || isNaN(val) || val<0 || val>20) { inp.classList.add('error'); hasErr=true; continue; }
                
                let grade = parseFloat(val);
                let wg = grade * sub.coef;
                pts += wg; coefs += sub.coef;

                tbody.innerHTML += `<tr><td>${sub.name}</td><td>${grade}</td><td>${sub.coef}</td><td style="font-weight:700">${wg.toFixed(2)}</td></tr>`;
            }

            if(hasErr) { showToast("الرجاء عدم ترك أي مادة أساسية فارغة، وإدخال نقاط بين 0 و 20."); return; }
            if(coefs === 0) return;

            const avg = pts / coefs;
            const isPass = avg >= 10;
            const statusTxt = isPass ? 'ناجح 🎉' : 'غير ناجح';

            document.getElementById('bac-final-avg').textContent = avg.toFixed(2);
            document.getElementById('bac-final-avg').style.color = isPass ? 'var(--success)' : 'var(--danger)';
            
            const badge = document.getElementById('bac-status-badge');
            badge.textContent = statusTxt;
            badge.className = 'status-badge ' + (isPass ? 'status-pass' : 'status-fail');
            
            document.getElementById('bac-motivation').textContent = getMotivationMessage(avg);
            document.getElementById('bac-total-coefs').textContent = coefs;
            document.getElementById('bac-total-points').textContent = pts.toFixed(2);

            currentBacResult = { avg: avg.toFixed(2), status: statusTxt, stream: document.getElementById('bac-stream').value };
            
            document.getElementById('bac-result').style.display = 'block';
            document.getElementById('bac-result').scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        function shareBAC() {
            if(!currentBacResult) return;
            const txt = `حاسبة معدلي 🎓\nالنوع: بكالوريا (${currentBacResult.stream})\nالمعدل: ${currentBacResult.avg}\nالنتيجة: ${currentBacResult.status}\n\nصاحب الفكرة: الأستاذ عراج زكرياء\nجسر المستقبل - وهران`;
            window.open(`https://api.whatsapp.com/send?text=${encodeURIComponent(txt)}`, '_blank');
        }

        // ================= LOCALSTORAGE & UTILS =================

        function saveData(mode) {
            try {
                if (mode === 'bem') {
                    let data = { t1: document.getElementById('bem-t1').value, t2: document.getElementById('bem-t2').value, t3: document.getElementById('bem-t3').value, subs: {} };
                    bemSubjectsList.forEach(s => data.subs[s.id] = document.getElementById(s.id).value);
                    localStorage.setItem('calc_bem_data', JSON.stringify(data));
                } else if (mode === 'bac') {
                    let data = { 
                        stream: document.getElementById('bac-stream').value,
                        noSport: document.getElementById('bac-exempt-sport').checked,
                        noAmz: document.getElementById('bac-exempt-amazigh').checked,
                        subs: {}
                    };
                    currentBacActiveSubjects.forEach(s => {
                        let inp = document.getElementById(`inp-${s.id}`);
                        if(inp) data.subs[s.id] = inp.value;
                    });
                    localStorage.setItem('calc_bac_data', JSON.stringify(data));
                }
            } catch(e) { console.warn("Storage restricted"); }
        }

        function loadData() {
            try {
                // Load BEM
                let bData = JSON.parse(localStorage.getItem('calc_bem_data'));
                if(bData) {
                    if(bData.t1) document.getElementById('bem-t1').value = bData.t1;
                    if(bData.t2) document.getElementById('bem-t2').value = bData.t2;
                    if(bData.t3) document.getElementById('bem-t3').value = bData.t3;
                    bemSubjectsList.forEach(s => { if(bData.subs[s.id]) document.getElementById(s.id).value = bData.subs[s.id]; });
                }
                
                // Load BAC
                let bacData = JSON.parse(localStorage.getItem('calc_bac_data'));
                if(bacData) {
                    if(bacData.stream) document.getElementById('bac-stream').value = bacData.stream;
                    renderBacSubjects(); // render inputs for stream
                    
                    document.getElementById('bac-exempt-sport').checked = bacData.noSport || false;
                    document.getElementById('bac-exempt-amazigh').checked = bacData.noAmz || false;
                    toggleBacExemptions();

                    currentBacActiveSubjects.forEach(s => {
                        let inp = document.getElementById(`inp-${s.id}`);
                        if(inp && bacData.subs[s.id]) inp.value = bacData.subs[s.id];
                    });
                } else {
                    renderBacSubjects(); // Default render
                }
            } catch(e) { 
                renderBacSubjects(); // Ensure BAC renders if storage fails
                console.warn("Storage restricted"); 
            }
        }

        function clearData(mode) {
            if(mode === 'bem') {
                document.getElementById('bem-t1').value = ''; document.getElementById('bem-t2').value = ''; document.getElementById('bem-t3').value = '';
                bemSubjectsList.forEach(s => { 
                    let el = document.getElementById(s.id);
                    el.value = ''; el.classList.remove('error');
                });
                document.getElementById('bem-result').style.display = 'none';
                localStorage.removeItem('calc_bem_data');
            } else {
                currentBacActiveSubjects.forEach(s => {
                    let el = document.getElementById(`inp-${s.id}`);
                    if(el) { el.value = ''; el.classList.remove('error'); }
                });
                document.getElementById('bac-result').style.display = 'none';
                // Reset exemptions? Let's leave checkboxes as is, just clear inputs
                saveData('bac'); // saves empty state
            }
            showToast("تم مسح البيانات.");
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Initialize App
        window.onload = () => {
            renderBemSubjects();
            loadData(); // Will also render BAC subjects based on storage or default
        };

    </script>
</body>
</html>
