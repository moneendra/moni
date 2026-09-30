# smart-parking
Smart Parking System - A modern web application for reserving parking spots
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Parking System</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f1f5f9;
      color: #172033;
    }
    header {
      padding: 18px 7%;
      background: #123b67;
      color: white;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header h1 { margin: 0; font-size: 22px; }
    main { max-width: 1050px; margin: 35px auto; padding: 0 18px; }
    .panel {
      background: white;
      padding: 24px;
      border-radius: 12px;
      box-shadow: 0 5px 20px #17203312;
      margin-bottom: 22px;
    }
    .login { max-width: 420px; margin: 70px auto; }
    input, select, button {
      width: 100%;
      padding: 12px;
      margin: 8px 0;
      border: 1px solid #cbd5e1;
      border-radius: 7px;
      font-size: 15px;
    }
    button {
      border: 0;
      background: #087e8b;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover { background: #066873; }
    button.secondary { background: #64748b; }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 16px;
    }
    .slot {
      border: 1px solid #dbe3ec;
      border-radius: 10px;
      padding: 18px;
    }
    .slot h3 { margin-top: 0; }
    .status { font-weight: bold; }
    .available { color: #16803c; }
    .reserved { color: #b45309; }
    .occupied { color: #c62828; }
    .muted { color: #64748b; font-size: 14px; }
    .hidden { display: none; }
    #message { color: #b91c1c; min-height: 20px; }
  </style>
</head>
<body>
  <header>
    <h1>🅿 Smart Parking System</h1>
    <button id="logoutButton" class="secondary hidden" style="width:auto">Log out</button>
  </header>

  <main>
    <section id="loginPage" class="panel login">
      <h2>Login</h2>
      <p class="muted">Sign in to find and book a parking slot.</p>
      <form id="loginForm">
        <input id="username" placeholder="Username" autocomplete="username" required>
        <input id="password" type="password" placeholder="Password"
               autocomplete="current-password" required>
        <button type="submit">Sign in</button>
      </form>
      <p id="message"></p>
      <p class="muted">Demo credentials: admin / 1234</p>
    </section>

    <section id="homePage" class="hidden">
      <div class="panel">
        <h2>Available parking locations</h2>
        <p class="muted">Reservations last for two hours. Choose a slot below.</p>
        <div id="slots" class="grid"></div>
      </div>
      <div class="panel">
        <h2>Sensor connection</h2>
        <p id="sensorInfo" class="muted">
          Demo mode: the button on each slot simulates an IR sensor detecting a car.
        </p>
      </div>
    </section>
  </main>

  <script>
    // Demo locations and slots. Replace these with data from your backend.
    const parkingSlots = [
      { id: "Taj-1", place: "Taj Mahal, Agra", state: "available" },
      { id: "Gateway-1", place: "Gateway of India, Mumbai", state: "available" },
      { id: "IndiaGate-1", place: "India Gate, New Delhi", state: "available" },
      { id: "Mysore-1", place: "Mysore Palace, Mysuru", state: "available" },
      { id: "Charminar-1", place: "Charminar, Hyderabad", state: "available" },
      { id: "Marina-1", place: "Marina Beach, Chennai", state: "available" }
    ];

    const loginPage = document.getElementById("loginPage");
    const homePage = document.getElementById("homePage");
    const logoutButton = document.getElementById("logoutButton");
    const slotsElement = document.getElementById("slots");

    document.getElementById("loginForm").addEventListener("submit", event => {
      event.preventDefault();

      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      if (username === "admin" && password === "1234") {
        loginPage.classList.add("hidden");
        homePage.classList.remove("hidden");
        logoutButton.classList.remove("hidden");
        renderSlots();
      } else {
        document.getElementById("message").textContent =
          "Incorrect username or password.";
      }
    });

    logoutButton.addEventListener("click", () => {
      homePage.classList.add("hidden");
      loginPage.classList.remove("hidden");
      logoutButton.classList.add("hidden");
    });

    function renderSlots() {
      slotsElement.innerHTML = "";

      parkingSlots.forEach(slot => {
        const card = document.createElement("article");
        card.className = "slot";

        const statusClass = {
          available: "available",
          reserved: "reserved",
          occupied: "occupied"
        }[slot.state];

        let details = "";
        if (slot.state === "reserved" && slot.reservedUntil) {
          details = `<p>Reserved until: ${new Date(slot.reservedUntil).toLocaleTimeString()}</p>
                     <p>Time remaining: <span id="timer-${slot.id}"></span></p>`;
        } else if (slot.state === "occupied") {
          details = `<p>Car detected by IR sensor.</p>
                     <p>Booked at: ${new Date(slot.bookedAt).toLocaleString()}</p>`;
        }

        card.innerHTML = `
          <h3>${slot.place}</h3>
          <p>Slot: ${slot.id}</p>
          <p class="status ${statusClass}">${statusText(slot.state)}</p>
          ${details}
          ${slot.state === "available"
            ? `<button data-book="${slot.id}">Book for 2 hours</button>`
            : ""}
          ${slot.state === "reserved"
            ? `<button data-simulate="${slot.id}">Simulate IR sensor: car arrived</button>`
            : ""}
        `;
        slotsElement.appendChild(card);
      });

      slotsElement.querySelectorAll("[data-book]").forEach(button => {
        button.addEventListener("click", () => bookSlot(button.dataset.book));
      });

      slotsElement.querySelectorAll("[data-simulate]").forEach(button => {
        button.addEventListener("click", () => {
          // Demo only: act as though the IR sensor detected a car.
          onIrSensorUpdate(button.dataset.simulate, true);
        });
      });

      updateTimers();
    }

    function statusText(state) {
      return {
        available: "Available",
        reserved: "Reserved — waiting for car",
        occupied: "Slot booked — car detected"
      }[state];
    }

    function bookSlot(slotId) {
      const slot = parkingSlots.find(item => item.id === slotId);
      if (!slot || slot.state !== "available") return;

      slot.state = "reserved";
      slot.reservedAt = Date.now();
      slot.reservedUntil = Date.now() + 2 * 60 * 60 * 1000;
      renderSlots();
    }

    // Call this function when your real sensor/backend reports a reading.
    // carDetected should be true when the sensor sees a vehicle.
    function onIrSensorUpdate(slotId, carDetected) {
      const slot = parkingSlots.find(item => item.id === slotId);
      if (!slot || slot.state !== "reserved") return;

      if (carDetected) {
        slot.state = "occupied";
        slot.bookedAt = Date.now();
        renderSlots();
      }
    }

    function updateTimers() {
      parkingSlots.forEach(slot => {
        if (slot.state !== "reserved" || !slot.reservedUntil) return;

        const timer = document.getElementById(`timer-${slot.id}`);
        if (!timer) return;

        const remaining = slot.reservedUntil - Date.now();
        if (remaining <= 0) {
          slot.state = "available";
          delete slot.reservedAt;
          delete slot.reservedUntil;
          renderSlots();
          return;
        }

        const hours = Math.floor(remaining / 3600000);
        const minutes = Math.floor((remaining % 3600000) / 60000);
        const seconds = Math.floor((remaining % 60000) / 1000);
        timer.textContent =
          `${String(hours).padStart(2, "0")}:` +
          `${String(minutes).padStart(2, "0")}:` +
          `${String(seconds).padStart(2, "0")}`;
      });
    }

    setInterval(updateTimers, 1000);

    /*
      REAL SENSOR INTEGRATION EXAMPLE:
      Have an ESP32/backend send sensor status to an API, then poll it here.
      Replace the URL and expected JSON with your server's actual API.

      async function readSensorFromServer(slotId) {
        const response = await fetch(`/api/slots/${slotId}/sensor`);
        const data = await response.json(); // Example: { "carDetected": true }
        onIrSensorUpdate(slotId, data.carDetected);
      }

      setInterval(() => {
        readSensorFromServer("Taj-1");
      }, 2000);
    */
  </script>
</body>
</html>
