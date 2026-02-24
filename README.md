<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Location Check-in</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      background-color: #f9fafb;
      color: #333;
    }
    #status, #coords {
      font-size: 1.2em;
      text-align: center;
      padding: 10px;
    }
    #sendBtn {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 6px;
      background-color: #2563eb;
      color: white;
      display: none;
    }
  </style>
</head>
<body>
  <div id="status">Please allow location access to check in...</div>
  <div id="coords"></div>
  <button id="sendBtn">Send SMS</button>

  <script>
    const phoneNumber = "+18338371430";

    function sendSMS(lat, lon) {
      const message = encodeURIComponent(`Check-in: Lat=${lat}, Lon=${lon}`);
      const smsURL = `sms:${phoneNumber}?body=${message}`;
      window.location.href = smsURL;
    }

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const lat = position.coords.latitude.toFixed(6);
            const lon = position.coords.longitude.toFixed(6);

            document.getElementById("status").textContent = "Location captured!";
            document.getElementById("coords").textContent =
              `Latitude: ${lat} | Longitude: ${lon}`;

            const btn = document.getElementById("sendBtn");
            btn.style.display = "block";

            // Option 1: Auto-send after delay (remove if not needed)
            setTimeout(() => sendSMS(lat, lon), 2000);

            // Option 2: User taps button
            btn.onclick = () => sendSMS(lat, lon);
          },
          (error) => {
            document.getElementById("status").textContent =
              "Unable to get location. Please enable location access and try again.";
          },
          { enableHighAccuracy: true, timeout: 10000 }
        );
      } else {
        document.getElementById("status").textContent =
          "Geolocation not supported on this device.";
      }
    }

    getLocation();
  </script>
</body>
</html>
