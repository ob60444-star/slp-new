 <!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>قسم تقويم الكلام واللغة | كلية العلوم الصحية</title>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@300;500;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #4a90e2;
            --secondary-color: #a29bfe;
            --accent-color: #00cec9;
            --bg-dark: #0f172a;
            --card-bg: rgba(255, 255, 255, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Tajawal', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: white;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* تأثيرات الخلفية */
        .background-blob {
            position: fixed;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, var(--primary-color), transparent);
            filter: blur(80px);
            z-index: -1;
            opacity: 0.3;
            animation: float 20s infinite alternate;
        }

        @keyframes float {
            from { transform: translate(-10%, -10%); }
            to { transform: translate(100%, 50%); }
        }

        header {
            padding: 40px 20px;
            text-align: center;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        header h1 {
            font-size: 2.5rem;
            background: linear-gradient(to left, #fff, var(--secondary-color));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .container {
            max-width: 1100px;
            margin: 40px auto;
            padding: 20px;
        }

        h2 {
            margin-bottom: 30px;
            border-right: 5px solid var(--accent-color);
            padding-right: 15px;
        }

        /* قسم المواد */
        .subjects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 60px;
        }

        .subject-card {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .subject-card:hover {
            transform: translateY(-10px);
            background: rgba(255, 255, 255, 0.15);
            border-color: var(--accent-color);
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        /* تفاصيل المحاضرات */
        .lectures-display {
            display: none;
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 25px;
            margin-top: 20px;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .lecture-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            margin-bottom: 10px;
            background: rgba(0,0,0,0.2);
            border-radius: 12px;
        }

        .btn-download {
            background: var(--accent-color);
            color: #000;
            padding: 5px 15px;
            border-radius: 8px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.9rem;
        }

        /* قسم الأخبار */
        .news-section {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
        }

        .news-card {
            position: relative;
            border-radius: 20px;
            overflow: hidden;
            background: rgba(255, 255, 255, 0.05);
            height: 200px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 20px;
            transition: 0.4s;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        .news-card:hover {
            background: linear-gradient(transparent, var(--primary-color));
        }

        .news-tag {
            position: absolute;
            top: 15px;
            right: 15px;
            background: var(--secondary-color);
            padding: 3px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
        }

        footer {
            text-align: center;
            padding: 50px;
            opacity: 0.6;
        }
    </style>
</head>
<body>

    <div class="background-blob"></div>

    <header>
        <h1>كلية العلوم الصحية</h1>
        <p>قسم تقويم الكلام واللغة - البوابة الأكاديمية</p>
    </header>

    <div class="container">
        
        <h2>مواد القسم</h2>
        <div class="subjects-grid">
            <div class="subject-card" onclick="showLectures('علم الأصوات اللغوية')">
                <h3>علم الأصوات اللغوية</h3>
                <p>اضغط لاستعراض المحاضرات</p>
            </div>
            <div class="subject-card" onclick="showLectures('اضطرابات النطق')">
                <h3>اضطرابات النطق</h3>
                <p>اضغط لاستعراض المحاضرات</p>
            </div>
            <div class="subject-card" onclick="showLectures('التطور اللغوي')">
                <h3>التطور اللغوي</h3>
                <p>اضغط لاستعراض المحاضرات</p>
            </div>
            <div class="subject-card" onclick="showLectures(' التشريح والفيزيولوجيا السمعية و النطقية')">
                <h3>تشريح جهاز النطق</h3>
                <p>اضغط لاستعراض المحاضرات</p>
            </div>
        </div>

        <div id="lectures-container" class="lectures-display">
            <h3 id="subject-title" style="color: var(--accent-color); margin-bottom: 20px;"></h3>
            <div id="lectures-list">
                </div>
        </div>

        <hr style="margin: 60px 0; opacity: 0.1;">

        <h2>آخر أخبار الكلية</h2>
        <div class="news-section">
            <div class="news-card">
                <span class="news-tag">إعلان</span>
                <h4>بدء التسجيل للمؤتمر السنوي لصعوبات التعلم</h4>
                <p>التاريخ: 15 فبراير 2026</p>
            </div>
            <div class="news-card">
                <span class="news-tag">هام</span>
                <h4>تعديل جدول الامتحانات العملية للعام الحالي</h4>
                <p>متاح الآن عبر البوابة الإلكترونية</p>
            </div>
            <div class="news-card">
                <span class="news-tag">ندوة</span>
                <h4>ورشة عمل حول استخدام التقنيات الحديثة في علاج التأتأة</h4>
                <p>يوم الأربعاء القادم في المدرج الرئيسي</p>
            </div>
        </div>

    </div>

    <footer>
        <p>حقوق النشر © 2026 - قسم تقويم الكلام واللغة</p>
    </footer>

    <script>
        // بيانات تجريبية للمواد والمحاضرات
        const lecturesData = {
            'علم الأصوات اللغوية': ['المحاضرة 1: مدخل إلى علم الأصوات', 'المحاضرة 2: مخارج الحروف', 'المحاضرة 3: الصفات الصوتية'],
            'اضطرابات النطق': ['المحاضرة 1: تصنيف الاضطرابات', 'المحاضرة 2: التلعثم عند الأطفال', 'المحاضرة 3: آليات الفحص التشخيصي'],
            'التطور اللغوي': ['المحاضرة 1: مراحل اكتساب اللغة', 'المحاضرة 2: النظريات اللغوية', 'المحاضرة 3: اللغة والبيئة'],
            'التشريح والفيزيولوجيا': ['المحاضرة 1: تشريح الحنجرة', 'المحاضرة 2: عضلات الوجه واللسان', 'المحاضرة 3: الجهاز العصبي والكلام']
        };

        function showLectures(subjectName) {
            const container = document.getElementById('lectures-container');
            const title = document.getElementById('subject-title');
            const list = document.getElementById('lectures-list');

            // تحديث المحتوى
            title.innerText = subjectName;
            list.innerHTML = ''; // تفريغ القائمة الحالية

            lecturesData[subjectName].forEach(lec => {
                const item = document.createElement('div');
                item.className = 'lecture-item';
                item.innerHTML = `
                    <span>${lec}</span>
                    <a href="#" class="btn-download" onclick="alert('سيبدأ التحميل قريباً...')">تحميل PDF</a>
                `;
                list.appendChild(item);
            });

            // إظهار القسم مع تأثير
            container.style.display = 'block';
            window.scrollTo({
                top: container.offsetTop - 100,
                behavior: 'smooth'
            });
        }
    </script>
</body>
</html>
