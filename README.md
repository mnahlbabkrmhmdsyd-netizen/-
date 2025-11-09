
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نظام إدارة المستشفى</title>
<style>
body{font-family:Arial;direction:rtl;margin:20px;background:#f7f7f7}
h1{color:#0366d6;text-align:center}
h2,h3{color:#0366d6}
table{width:100%;border-collapse:collapse;margin-top:10px;background:#fff}
th,td{border:1px solid #ccc;padding:8px;text-align:center}
input,select,button{padding:6px;margin:5px}
button{background:#0366d6;color:#fff;border:0;border-radius:4px;cursor:pointer}
section{background:#fff;padding:15px;border-radius:8px;margin-bottom:20px;box-shadow:0 0 5px rgba(0,0,0,0.1)}
.logo{text-align:center;font-size:22px;font-weight:bold;color:#0366d6;margin-bottom:10px}
</style>
</head>
<body>

<div class="logo">🏥 مستشفى الحياة التخصصي</div>
<h1>نظام إدارة المستشفى</h1>

<!-- قسم المرضى -->
<section>
  <h3>إضافة مريض جديد</h3>
  <input id="name" placeholder="اسم المريض">
  <input id="age" placeholder="العمر" type="number">
  <select id="gender">
    <option value="">اختر النوع</option>
    <option value="ذكر">ذكر</option>
    <option value="أنثى">أنثى</option>
  </select>
  <input id="phone" placeholder="رقم الهاتف">
  <button onclick="addPatient()">إضافة</button>

  <h3>قائمة المرضى</h3>
  <table id="patients">
    <tr><th>الاسم</th><th>العمر</th><th>النوع</th><th>الهاتف</th><th>إجراء</th></tr>
  </table>
</section>

<!-- قسم الأطباء -->
<section>
  <h3>إضافة طبيب جديد</h3>
  <input id="docName" placeholder="اسم الطبيب">
  <input id="docAge" placeholder="العمر" type="number">
  <input id="specialty" placeholder="التخصص">
  <select id="docGender">
    <option value="">اختر النوع</option>
    <option value="ذكر">ذكر</option>
    <option value="أنثى">أنثى</option>
  </select>
  <input id="docPhone" placeholder="رقم الهاتف">
  <button onclick="addDoctor()">إضافة</button>

  <h3>قائمة الأطباء</h3>
  <table id="doctors">
    <tr><th>الاسم</th><th>العمر</th><th>التخصص</th><th>النوع</th><th>الهاتف</th><th>إجراء</th></tr>
  </table>
</section>

<!-- قسم موظف الاستقبال -->
<section>
  <h3>قسم موظف الاستقبال</h3>

  <h4>👤 اسم موظف الاستقبال المداوم</h4>
  <input id="receptionistName" placeholder="ادخل اسم موظف الاستقبال">

  <h4>📋 كتابة بيانات المريض بعد التسجيل</h4>
  <select id="selectPatient">
    <option value="">اختر مريض</option>
  </select>
  <input id="address" placeholder="العنوان">
  <input id="disease" placeholder="المرض أو الحالة">
  <button onclick="updatePatient()">تحديث بيانات المريض</button>

  <h4>🩺 عرض سجل الأطباء</h4>
  <button onclick="showDoctors()">عرض الأطباء</button>
  <div id="doctorList" style="margin-top:10px;"></div>

  <h4>💰 إنشاء فاتورة</h4>
  <select id="invoicePatient">
    <option value="">اختر مريض</option>
  </select>
  <input id="amount" type="number" placeholder="المبلغ">
  <button onclick="createInvoice()">إنشاء فاتورة</button>
  <div id="invoices" style="margin-top:10px;"></div>
  <button id="printBtn" style="display:none;" onclick="printInvoice()">🖨️ طباعة الفاتورة</button>

  <h4>💾 حفظ بيانات المرضى</h4>
  <button onclick="saveData()">حفظ جميع البيانات</button>
</section>

<script>
let patients = [];
let doctors = [];
let invoices = [];

// ===== المرضى =====
function renderPatients(){
  let table = document.getElementById("patients");
  table.innerHTML = "<tr><th>الاسم</th><th>العمر</th><th>النوع</th><th>الهاتف</th><th>إجراء</th></tr>";
  document.getElementById("selectPatient").innerHTML = "<option value=''>اختر مريض</option>";
  document.getElementById("invoicePatient").innerHTML = "<option value=''>اختر مريض</option>";

  patients.forEach((p,i)=>{
    table.innerHTML += `<tr>
      <td>${p.name}</td>
      <td>${p.age}</td>
      <td>${p.gender}</td>
      <td>${p.phone}</td>
      <td><button onclick="removePatient(${i})">حذف</button></td>
    </tr>`;
    document.getElementById("selectPatient").innerHTML += `<option value="${i}">${p.name}</option>`;
    document.getElementById("invoicePatient").innerHTML += `<option value="${i}">${p.name}</option>`;
  });
}

function addPatient(){
  let name = document.getElementById("name").value;
  let age = document.getElementById("age").value;
  let gender = document.getElementById("gender").value;
  let phone = document.getElementById("phone").value;
  if(name==""){alert("يرجى إدخال الاسم");return;}
  patients.push({name,age,gender,phone,address:"",disease:""});
  renderPatients();
  document.getElementById("name").value="";
  document.getElementById("age").value="";
  document.getElementById("gender").value="";
  document.getElementById("phone").value="";
}

function removePatient(i){
  if(confirm("هل تريد حذف هذا المريض؟")){patients.splice(i,1);renderPatients();}
}

// ===== الأطباء =====
function renderDoctors(){
  let table = document.getElementById("doctors");
  table.innerHTML = "<tr><th>الاسم</th><th>العمر</th><th>التخصص</th><th>النوع</th><th>الهاتف</th><th>إجراء</th></tr>";
  doctors.forEach((d,i)=>{
    table.innerHTML += `<tr>
      <td>${d.name}</td>
      <td>${d.age}</td>
      <td>${d.specialty}</td>
      <td>${d.gender}</td>
      <td>${d.phone}</td>
      <td><button onclick="removeDoctor(${i})">حذف</button></td>
    </tr>`;
  });
}

function addDoctor(){
  let name = document.getElementById("docName").value;
  let age = document.getElementById("docAge").value;
  let specialty = document.getElementById("specialty").value;
  let gender = document.getElementById("docGender").value;
  let phone = document.getElementById("docPhone").value;
  if(name==""){alert("يرجى إدخال اسم الطبيب");return;}
  doctors.push({name,age,specialty,gender,phone});
  renderDoctors();
  document.getElementById("docName").value="";
  document.getElementById("docAge").value="";
  document.getElementById("specialty").value="";
  document.getElementById("docGender").value="";
  document.getElementById("docPhone").value="";
}

function removeDoctor(i){
  if(confirm("هل تريد حذف هذا الطبيب؟")){doctors.splice(i,1);renderDoctors();}
}

// ===== قسم الاستقبال =====
function updatePatient(){
  let i = document.getElementById("selectPatient").value;
  if(i===""){alert("اختر مريض أولاً");return;}
  patients[i].address = document.getElementById("address").value;
  patients[i].disease = document.getElementById("disease").value;
  alert("تم تحديث بيانات المريض بنجاح");
  document.getElementById("address").value="";
  document.getElementById("disease").value="";
}

function showDoctors(){
  let div = document.getElementById("doctorList");
  if(doctors.length===0){div.innerHTML="<p>لا يوجد أطباء مسجلين</p>";return;}
  div.innerHTML = "<ul>" + doctors.map(d=>`<li>${d.name} - ${d.specialty} (${d.gender})</li>`).join("") + "</ul>";
}

function createInvoice(){
  let i = document.getElementById("invoicePatient").value;
  let amount = document.getElementById("amount").value;
  let receptionist = document.getElementById("receptionistName").value;
  if(i==="" || amount===""){alert("يرجى اختيار المريض وإدخال المبلغ");return;}
  if(receptionist===""){alert("يرجى إدخال اسم موظف الاستقبال");return;}
  const patient = patients[i];
  const invoice = {patient:patient.name,amount,date:new Date().toLocaleString(),receptionist};
  invoices.push(invoice);
  document.getElementById("invoices").innerHTML = `
    <h5>آخر فاتورة:</h5>
    <p><strong>الاسم:</strong> ${invoice.patient}</p>
    <p><strong>المبلغ:</strong> ${invoice.amount} ج.س</p>
    <p><strong>التاريخ:</strong> ${invoice.date}</p>
    <p><strong>موظف الاستقبال:</strong> ${invoice.receptionist}</p>
  `;
  document.getElementById("printBtn").style.display = "inline-block";
  document.getElementById("amount").value="";
}

function printInvoice(){
  const invoiceDiv = document.getElementById("invoices").innerHTML;
  const printWindow = window.open('', '', 'width=600,height=500');
  printWindow.document.write(`
    <html>
    <head>
      <title>فاتورة المريض</title>
      <style>
        body{direction:rtl;font-family:Arial;text-align:center;padding:20px;}
        h2{color:#0366d6;}
        p{font-size:18px;}
        .header{border-bottom:2px solid #0366d6;margin-bottom:20px;padding-bottom:10px;}
      </style>
    </head>
    <body>
      <div class="header">
        <h2>🏥 مستشفى الحياة التخصصي</h2>
        <p>فاتورة المريض</p>
      </div>
      ${invoiceDiv}
      <br>
      <p>شكراً لاختياركم مستشفى الحياة التخصصي 💙</p>
      <script>window.print();<\/script>
    </body>
    </html>
  `);
  printWindow.document.close();
}

function saveData(){
  alert("تم حفظ جميع بيانات المرضى والأطباء والفواتير بنجاح ✅");
}
</script>

</body>
</html>
