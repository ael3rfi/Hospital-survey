# Hospital-survey
Hospital 

<!-- انسخ الكود من هنا -->
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>استبيان تقييم المستشفى</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f9f9f9;
      margin: 40px;
    }

    .logo {
      text-align: center;
      margin-bottom: 20px;
    }

    .logo img {
      max-width: 200px;
    }

    .section {
      display: none;
    }

    .section.active {
      display: block;
    }

    .question {
      margin-bottom: 20px;
    }

    .question-text {
      margin-bottom: 5px;
      font-weight: bold;
    }

    .answers {
      display: flex;
      gap: 10px;
    }

    .buttons {
      margin-top: 30px;
      display: flex;
      justify-content: space-between;
    }

    button {
      padding: 10px 25px;
      font-size: 16px;
    }

    .rating-option {
      display: flex;
      align-items: center;
    }

    input[type="text"], input[type="date"] {
      width: 100%;
      padding: 8px;
      margin-bottom: 20px;
    }

    #navButtons {
      position: fixed;
      bottom: 20px;
      right: 40px;
      left: 40px;
      display: flex;
      justify-content: space-between;
      background-color: #f9f9f9;
      padding-top: 10px;
      z-index: 999;
      border-top: 1px solid #ccc;
    }
  </style>
</head>
<body>

<form id="surveyForm">

  <!-- الصفحة الأولى -->
  <div class="section active">
    <div class="logo">
      <img src="F162D37E-8D06-4F16-8ACA-B79561F45679.jpeg" alt="شعار الهيئة">
    </div>

    <h2>هيئة اعتماد المؤسسات الصحية ومراقبتها</h2>
    <h3>استبيان تقييم المستشفيات</h3>

    <label>اسم المستشفى:</label>
    <input type="text" name="hospitalName" required>

    <label>تاريخ التقييم:</label>
    <input type="date" name="date" required>

    <label>اسم المقيم:</label>
    <input type="text" name="evaluator" required>
  </div>

</form>

<div id="navButtons">
  <button id="prevBtn" onclick="prevSection()">السابقة</button>
  <button id="nextBtn" onclick="nextSection()">التالي</button>
</div>

<script>
  const form = document.getElementById("surveyForm");
  const questions = [
    {
      title: "القسم الأول: جودة الرعاية والسلامة",
      items: [
        "مدى التزام الفريق الطبي ببروتوكولات السلامة.",
        "سرعة الاستجابة في حالات الطوارئ.",
        "وضوح إجراءات الإبلاغ عن الأخطاء الطبية.",
        "فعالية إدارة العدوى داخل الأقسام.",
        "مدى توفر الأدوية الأساسية بانتظام."
      ]
    },
    {
      title: "القسم الثاني: كفاءة الكادر الطبي",
      items: [
        "مستوى مهارات الأطباء في التشخيص.",
        "تعاون الفريق الطبي مع بعضه البعض.",
        "احترام الطاقم الطبي لخصوصية المرضى.",
        "توفر الأطباء المختصين على مدار 24 ساعة.",
        "وضوح شرح الإجراءات الطبية للمريض."
      ]
    },
    {
      title: "القسم الثالث: المرافق والتجهيزات",
      items: [
        "نظافة الغرف ومرافق الاستقبال.",
        "جودة الأجهزة الطبية وتحديثها.",
        "سهولة الوصول للمرافق.",
        "توفر تجهيزات لذوي الاحتياجات الخاصة.",
        "كفاية عدد الأسرة في الأقسام الحرجة."
      ]
    },
    {
      title: "القسم الرابع: النظافة والتعقيم",
      items: [
        "نظافة دورات المياه والممرات.",
        "التخلص الآمن من النفايات الطبية.",
        "تعقيم الأدوات قبل الاستخدام.",
        "خلو البيئة من الروائح الكريهة.",
        "نظافة أغطية الأسرة والأدوات الشخصية."
      ]
    },
    {
      title: "القسم الخامس: الخدمات الإدارية",
      items: [
        "سرعة إجراءات القبول والاستقبال.",
        "وضوح سياسات الدفع والتأمين.",
        "سرعة معالجة الشكاوى والاقتراحات.",
        "مرونة أوقات الزيارة.",
        "توفر معلومات عن حقوق المريض."
      ]
    },
    {
      title: "القسم السادس: الرعاية الشاملة",
      items: [
        "دقة التعليمات المقدمة عند الخروج.",
        "متابعة الحالة بعد الخروج.",
        "مراعاة الجوانب النفسية للمريض.",
        "توفر برامج توعية صحية.",
        "تقييمك العام لخدمات المستشفى."
      ]
    }
  ];

  let questionCounter = 1;
  questions.forEach((group, index) => {
    const section = document.createElement("div");
    section.classList.add("section");

    section.innerHTML += `<h4>${group.title}</h4>`;

    group.items.forEach(item => {
      const questionDiv = document.createElement("div");
      questionDiv.classList.add("question");
      questionDiv.innerHTML = `
        <div class="question-text">${questionCounter}. ${item}</div>
        <div class="answers">
          ${[1, 2, 3, 4, 5].map(val => `
            <label class="rating-option">
              <input type="radio" name="q${questionCounter}" value="${val}" required> ${val}
            </label>
          `).join('')}
        </div>
      `;
      section.appendChild(questionDiv);
      questionCounter++;
    });

    form.appendChild(section);
  });

  const sections = document.querySelectorAll(".section");
  let current = 0;

  function showSection(index) {
    sections.forEach((sec, i) => sec.classList.toggle("active", i === index));
    document.getElementById("prevBtn").style.visibility = index === 0 ? "hidden" : "visible";
    document.getElementById("nextBtn").innerText = index === sections.length - 1 ? "إرسال" : "التالي";
  }

  function nextSection() {
    if (current < sections.length - 1) {
      current++;
      showSection(current);
    } else {
      submitForm();
    }
  }

  function prevSection() {
    if (current > 0) {
      current--;
      showSection(current);
    }
  }

  function submitForm() {
    const formData = new FormData(document.getElementById("surveyForm"));
    const entries = {};
    formData.forEach((value, key) => entries[key] = value);

    console.log("بيانات الاستبيان:", entries);
    alert("تم إرسال الاستبيان بنجاح!");
    location.reload();
  }

  window.onload = () => showSection(current);
</script>

</body>
</html>
<!-- انتهى -->
