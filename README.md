<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>2Bins2Buddies | Trash Bin Sanitation & Curbside Pull</title>
<style>
  body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f9f9f9; color:#333; }
  header { background:#4CAF50; color:white; padding: 40px 20px; text-align:center; }
  header h1 { margin:0; font-size:2.5em; }
  header p { font-size:1.2em; margin-top:10px; }
  section { padding: 40px 20px; max-width:800px; margin:auto; }
  h2 { color:#4CAF50; margin-bottom:20px; }
  .service { border:1px solid #ddd; padding:20px; margin-bottom:20px; border-radius:8px; background:white; }
  button { background:#4CAF50; color:white; padding:12px 20px; border:none; border-radius:5px; cursor:pointer; font-size:1em; }
  button:hover { background:#45a049; }
  form input, form select { width:100%; padding:10px; margin:5px 0 15px; border-radius:5px; border:1px solid #ccc; }
  form p { font-size:0.9em; color:#555; }
  footer { text-align:center; padding:20px; background:#eee; margin-top:40px; font-size:0.9em; }
</style>
</head>
<body>

<header>
  <h1>2Bins2Buddies</h1>
  <p>Professional Trash Bin Sanitation & Weekly Curbside Pulls in Stockbridge, GA</p>
</header>

<section>
  <h2>Our Services</h2>

  <div class="service">
    <h3>One-Time Bin Cleaning</h3>
    <p>Perfect for a deep clean or special occasion. $75 for 1 bin, $150 for 2 bins.</p>
    <button onclick="pay('PRICE_1BIN_ONETIME')">Pay $75 – 1 Bin</button>
    <button onclick="pay('PRICE_2BIN_ONETIME')">Pay $150 – 2 Bins</button>
  </div>

  <div class="service">
    <h3>Weekly Curbside Pull & Sanitation</h3>
    <p>We pull your bins curbside every week and sanitize them monthly. $25/month per bin, $50/month for 2 bins. 12-month commitment.</p>
    <button onclick="pay('PRICE_1BIN_CURBSIDE')">Subscribe $25/mo – 1 Bin</button>
    <button onclick="pay('PRICE_2BIN_CURBSIDE')">Subscribe $50/mo – 2 Bins</button>
  </div>
</section>

<section>
  <h2>Book Your Service</h2>
  <form id="bookingForm">
    <label>Name:</label>
    <input type="text" name="name" required>

    <label>Address:</label>
    <input type="text" name="address" required>

    <label>City:</label>
    <input type="text" name="city" value="Stockbridge" required>

    <label>Phone:</label>
    <input type="text" name="phone" required>

    <label>Pickup Day:</label>
    <select name="pickup" required>
      <option>Wednesday</option>
      <option>Thursday</option>
    </select>

    <label>Service Type:</label>
    <select name="service" required>
      <option value="PRICE_1BIN_ONETIME">One-Time – 1 Bin ($75)</option>
      <option value="PRICE_2BIN_ONETIME">One-Time – 2 Bins ($150)</option>
      <option value="PRICE_1BIN_CURBSIDE">Weekly Curbside Pull – 1 Bin ($25/mo, 1yr)</option>
      <option value="PRICE_2BIN_CURBSIDE">Weekly Curbside Pull – 2 Bins ($50/mo, 1yr)</option>
    </select>

    <button type="submit">Continue to Payment</button>
    <p><em>Note: Weekly Curbside Pull occurs weekly, includes sanitation, and requires a 12-month commitment. Billed monthly.</em></p>
  </form>
</section>

<footer>
  &copy; 2025 2Bins2Buddies | Serving Stockbridge, GA | Contact: info@2bins2buddies.com
</footer>

<script>
async function pay(priceKey) {
  const res = await fetch('/.netlify/functions/create-checkout', {
    method: 'POST',
    body: JSON.stringify({ priceId: priceKey })
  });
  const data = await res.json();
  window.location.href = data.url;
}

document.getElementById('bookingForm').addEventListener('submit', async function(e){
  e.preventDefault();

  const form = new FormData(e.target);
  const priceId = form.get("service");

  const customerData = {
    name: form.get("name"),
    address: form.get("address"),
    city: form.get("city"),
    phone: form.get("phone"),
    pickup: form.get("pickup")
  };

  const res = await fetch('/.netlify/functions/create-checkout', {
    method: 'POST',
    body: JSON.stringify({ priceId, customerData })
  });

  const data = await res.json();
  window.location.href = data.url;
});
</script>

</body>
