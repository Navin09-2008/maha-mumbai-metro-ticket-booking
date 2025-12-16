<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Maha Mumbai Metro Ticket</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#e3f2fd;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
}

.box{
  background:white;
  width:380px;
  padding:20px;
  border-radius:12px;
  box-shadow:0 10px 20px rgba(0,0,0,0.2);
}

h2{
  text-align:center;
  color:#0a7cff;
}

label{
  font-weight:bold;
  display:block;
  margin-top:10px;
}

input,select,button{
  width:100%;
  padding:8px;
  margin-top:5px;
  border-radius:6px;
  border:1px solid #ccc;
}

button{
  margin-top:15px;
  background:#0a7cff;
  color:white;
  border:none;
  font-size:16px;
  cursor:pointer;
}

button:hover{
  background:#055ec4;
}

.ticket{
  display:none;
  margin-top:20px;
  border:2px dashed #0a7cff;
  padding:12px;
  background:#f5faff;
}

.qr{
  margin-top:10px;
  text-align:center;
  display:none;
}
</style>
</head>

<body>

<div class="box">
<h2>Maha Mumbai Metro</h2>

<label>From Station</label>
<input type="text" value="Diamond Garden" readonly>

<label>To Station</label>
<select id="to">
  <option value="">Select Destination</option>
  <option value="Shivaji Chowk" data-fare="10">Shivaji Chowk – ₹10</option>
  <option value="Deonar" data-fare="20">Deonar – ₹20</option>
  <option value="Mankhurd" data-fare="20">Mankhurd – ₹20</option>
  <option value="Maharashtra Nagar Mandale" data-fare="20">
    Maharashtra Nagar Mandale – ₹20
  </option>
</select>

<label>Ticket Type</label>
<select id="type">
  <option value="SJT">Single Journey Ticket (SJT)</option>
  <option value="RJT">Return Journey Ticket (RJT)</option>
</select>

<label>Quantity</label>
<input type="number" id="qty" value="1" min="1">

<label>Journey Date</label>
<input type="date" id="date">

<button onclick="book()">Pay & Book Ticket</button>

<div class="ticket" id="ticket">
  <h3>🎫 Ticket Confirmed</h3>
  <p id="details"></p>

  <div class="qr" id="qrBox">
    <img id="qrImg" width="150">
  </div>

  <b>Valid for current date only</b>
</div>
</div>

<script>
let today = new Date().toISOString().split("T")[0];
document.getElementById("date").value = today;
document.getElementById("date").min = today;
document.getElementById("date").max = today;

function book(){
  let to = document.getElementById("to");
  let opt = to.options[to.selectedIndex];

  if(opt.value === ""){
    alert("Please select destination");
    return;
  }

  let qty = Number(document.getElementById("qty").value);
  let type = document.getElementById("type").value;
  let fare = Number(opt.dataset.fare);

  if(type === "RJT") fare *= 2;

  let total = fare * qty;

  let ticketText =
    "From: Diamond Garden\n" +
    "To: " + opt.value + "\n" +
    "Type: " + type + "\n" +
    "Qty: " + qty + "\n" +
    "Fare: ₹" + total + "\n" +
    "Date: " + today;

  document.getElementById("details").innerHTML =
    ticketText.replace(/\n/g,"<br>");

  // SHOW TICKET
  document.getElementById("ticket").style.display = "block";

  // GENERATE QR AFTER BOOKING
  let qrURL =
    "https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=" +
    encodeURIComponent(ticketText);

  document.getElementById("qrImg").src = qrURL;
  document.getElementById("qrBox").style.display = "block";
}
</script>

</body>
</html>
