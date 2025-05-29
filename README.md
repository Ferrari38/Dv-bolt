<script>
  function calculate() {
    const date = document.getElementById('date').value;
    const grab = parseFloat(document.getElementById('grab').value) || 0;
    const bolt = parseFloat(document.getElementById('bolt').value) || 0;
    const tip = parseFloat(document.getElementById('tip').value) || 0;
    const extraIncome = parseFloat(document.getElementById('extraIncome').value) || 0;
    const oil = parseFloat(document.getElementById('oil').value) || 0;
    const otherExpense = parseFloat(document.getElementById('otherExpense').value) || 0;
    const startKm = parseFloat(document.getElementById('startKm').value) || 0;
    const endKm = parseFloat(document.getElementById('endKm').value) || 0;

    const distance = endKm - startKm;
    if (distance < 0) {
      alert("กรุณากรอกเลขกิโลเมตรให้ถูกต้อง (เลิกงานต้องมากกว่าเริ่มงาน)");
      return;
    }

    const boltCommission = bolt * 0.18;
    const boltAfter = bolt - boltCommission;
    const totalIncomeBeforeMaintenance = grab + boltAfter + extraIncome;
    const maintenance = totalIncomeBeforeMaintenance * 0.10;

    const totalExpenses = oil + otherExpense + maintenance + boltCommission;
    const netIncome = totalIncomeBeforeMaintenance - totalExpenses + tip;

    const halfIncomePlusMaintenance = (netIncome / 2 + maintenance).toFixed(2);
    const costPerKm = distance > 0 ? ((boltAfter + extraIncome) / distance).toFixed(2) : "0.00";

    const format = n => n.toLocaleString(undefined, { minimumFractionDigits: 2 });

    const resultHTML = \`
      <strong>สรุปผลประจำวันที่: \${date}</strong><br><br>
      <strong>รายรับ</strong><br>
      GRAB: \${format(grab)} บาท<br>
      BOLT: \${format(bolt)} บาท (หัก 18%: \${format(boltCommission)} บาท เหลือ: \${format(boltAfter)} บาท)<br>
      รายรับอื่นๆ: \${format(extraIncome)} บาท<br>
      รวมรายรับก่อนหักค่าซ่อม: \${format(totalIncomeBeforeMaintenance)} บาท<br><br>

      <strong>รายจ่าย</strong><br>
      ค่าซ่อม 10%: \${format(maintenance)} บาท<br>
      ค่าน้ำมัน: \${format(oil)} บาท<br>
      รายจ่ายอื่นๆ: \${format(otherExpense)} บาท<br>
      ค่าคอมมิชชั่น BOLT: \${format(boltCommission)} บาท<br>
      รวมรายจ่ายทั้งหมด: \${format(totalExpenses)} บาท<br><br>

      <strong>ทิป:</strong> \${format(tip)} บาท<br>
      <strong>รายได้สุทธิ:</strong> \${format(netIncome)} บาท<br>
      <strong>หาร 2 + ค่าซ่อม:</strong> \${halfIncomePlusMaintenance} บาท<br><br>

      <strong>ระยะทางที่ใช้:</strong> \${distance} กม.<br>
      <strong>บาทต่อกิโลเมตร:</strong> \${costPerKm} บาท/กม.
    \`;

    document.getElementById('result').innerHTML = resultHTML;
  }
</script>
