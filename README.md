<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vehicle Rental System</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Vehicle Rental System</h1>
    </header>

    <main>
        <section id="available-vehicles">
            <h2>Available Vehicles</h2>
            <ul id="vehicle-list">
                <!-- Vehicle list will be dynamically populated here -->
            </ul>
        </section>

        <section id="rental-form">
            <h2>Rent a Vehicle</h2>
            <form id="rent-vehicle-form">
                <label for="vehicle-select">Select Vehicle:</label>
                <select id="vehicle-select" name="vehicle-select">
                    <!-- Vehicle options will be dynamically populated here -->
                </select>

                <label for="customer-id">Customer ID:</label>
                <input type="number" id="customer-id" name="customer-id" required>

                <label for="start-date">Start Date:</label>
                <input type="date" id="start-date" name="start-date" required>

                <label for="end-date">End Date:</label>
                <input type="date" id="end-date" name="end-date" required>

                <button type="submit">Rent Vehicle</button>
                <p id="rental-message"></p>
            </form>
        </section>

        <section id="current-rentals">
            <h2>Current Rentals</h2>
            <ul id="rental-list">
                <!-- Current rental list will be dynamically populated here -->
            </ul>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 Vehicle Rental System</p>
    </footer>

    <script src="script.js"></script>
</body>
</html>
/Вот здесь я представил код и заскриптовал код используя технологию html
/* style.css */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
    color: #333;
}

header {
    background-color: #333;
    color: #fff;
    padding: 1em 0;
    text-align: center;
}

main {
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

section {
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin-bottom: 20px;
    width: 80%;
}

section h2 {
    text-align: center;
    margin-bottom: 15px;
    color: #333;
}

ul {
    list-style: none;
    padding: 0;
}

ul li {
    padding: 10px;
    border-bottom: 1px solid #eee;
}

ul li:last-child {
    border-bottom: none;
}

#rental-form {
    width: 60%;
}

#rent-vehicle-form {
    display: flex;
    flex-direction: column;
}

label {
    margin-top: 10px;
    font-weight: bold;
}

input[type="number"],
input[type="date"],
select {
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 10px;
    border-radius: 4px;
    border: 1px solid #ccc;
}

button {
    background-color: #5cb85c;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: #449d44;
}

#rental-message {
    margin-top: 10px;
    font-weight: bold;
    text-align: center;
}

footer {
    text-align: center;
    padding: 1em 0;
    background-color: #333;
    color: #fff;
}
///Вот здесь я внешне представил для более удобного использования приложения

/* script.js */

// Sample Data (replace with a database or API calls in a real application)
const vehicles = [
    { id: 1, make: "Toyota", model: "Camry", licensePlate: "ABC-123", rentalRatePerDay: 35, type: "Car" },
    { id: 2, make: "Ford", model: "F-150", licensePlate: "DEF-456", rentalRatePerDay: 50, type: "Truck" },
    { id: 3, make: "Honda", model: "CR-V", licensePlate: "GHI-789", rentalRatePerDay: 40, type: "SUV" }
];

let rentals = [];

// DOM Elements
const vehicleList = document.getElementById("vehicle-list");
const vehicleSelect = document.getElementById("vehicle-select");
const rentVehicleForm = document.getElementById("rent-vehicle-form");
const rentalList = document.getElementById("rental-list");
const rentalMessage = document.getElementById("rental-message");

// Functions

// Populate Available Vehicles List
function populateVehicleList() {
    vehicleList.innerHTML = "";
    vehicles.forEach(vehicle => {
        if (isVehicleAvailable(vehicle.id)) {
            const listItem = document.createElement("li");
            listItem.textContent = `${vehicle.make} ${vehicle.model} (${vehicle.licensePlate}) - $${vehicle.rentalRatePerDay}/day`;
            vehicleList.appendChild(listItem);
        }
    });
}

// Populate Vehicle Select Options
function populateVehicleSelect() {
    vehicleSelect.innerHTML = "";
    vehicles.forEach(vehicle => {
        if (isVehicleAvailable(vehicle.id)) {
            const option = document.createElement("option");
            option.value = vehicle.id;
            option.textContent = `${vehicle.make} ${vehicle.model}`;
            vehicleSelect.appendChild(option);
        }
    });
}

// Check if a Vehicle is Available
function isVehicleAvailable(vehicleId) {
    return !rentals.some(rental => rental.vehicleId === vehicleId && rental.returned === false);
}

// Rent a Vehicle
function rentVehicle(vehicleId, customerId, startDate, endDate) {
    const vehicle = vehicles.find(v => v.id === vehicleId);

    if (!vehicle) {
        throw new Error("Vehicle not found.");
    }

    const newRental = {
        rentalId: rentals.length > 0 ? rentals[rentals.length - 1].rentalId + 1 : 1, // Generate a new rental ID
        vehicleId: vehicleId,
        customerId: customerId,
        startDate: startDate,
        endDate: endDate,
        rentalRatePerDay: vehicle.rentalRatePerDay,
        returned: false,  // Initially, the vehicle is not returned
        totalCost: calculateRentalCost(vehicle.rentalRatePerDay, startDate, endDate)
    };

    rentals.push(newRental);
    updateRentalList();
    populateVehicleList();
    populateVehicleSelect();
    return newRental;
}

// Calculate Rental Cost
function calculateRentalCost(rentalRatePerDay, startDate, endDate) {
    const start = new Date(startDate);
    const end = new Date(endDate);
    const duration = (end.getTime() - start.getTime()) / (1000 * 60 * 60 * 24); // Duration in days
    return duration * rentalRatePerDay;
}

// Update Rental List
function updateRentalList() {
    rentalList.innerHTML = "";
    rentals.forEach(rental => {
        if (!rental.returned) { // Only show current, non-returned rentals
            const listItem = document.createElement("li");
            listItem.innerHTML = `
                Rental ID: ${rental.rentalId} | Vehicle ID: ${rental.vehicleId} | Customer ID: ${rental.customerId} | Start Date: ${rental.startDate} | End Date: ${rental.endDate} | Total Cost: $${rental.totalCost}
                <button class="return-button" data-rental-id="${rental.rentalId}">Return Vehicle</button>
            `;
            rentalList.appendChild(listItem);
        }
    });

    // Add event listeners to the return buttons
    document.querySelectorAll(".return-button").forEach(button => {
        button.addEventListener("click", returnVehicle);
    });
}

// Handle Return Vehicle Click
function returnVehicle(event) {
    const rentalId = parseInt(event.target.dataset.rentalId);
    const rental = rentals.find(r => r.rentalId === rentalId);

    if (rental) {
        rental.returned = true;
        updateRentalList();
        populateVehicleList();
        populateVehicleSelect();
    }
}

// Event Listeners

// Rent Vehicle Form Submission
rentVehicleForm.addEventListener("submit", function (event) {
    event.preventDefault();

    const vehicleId = parseInt(vehicleSelect.value);
    const customerId = parseInt(document.getElementById("customer-id").value);
    const startDate = document.getElementById("start-date").value;
    const endDate = document.getElementById("end-date").value;

    try {
        const newRental = rentVehicle(vehicleId, customerId, startDate, endDate);
        rentalMessage.textContent = `Vehicle rented successfully! Rental ID: ${newRental.rentalId}, Total Cost: $${newRental.totalCost}`;
    } catch (error) {
        rentalMessage.textContent = `Error: ${error.message}`;
    }
});

// Initial Population
populateVehicleList();
populateVehicleSelect();
updateRentalList();
///Здесь представлен код где подробно объясняется клиенту,как будет проявляться работа приложения в успешном случае и ошибочном,и показывает на то что надо поменять своё действие
