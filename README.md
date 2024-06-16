<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel</title>
    <style>
        .section 
        {
            margin-bottom: 20px;
        }
        .seat 
        {
            width: 30px;
            height: 30px;
            margin: 2px;
            background-color: green;
            display: inline-block;
        }
        .booked 
        {
            background-color: red;
        }
    </style>
</head>
<body>
    <h1>Admin Panel</h1>

    <div class="section" id="screenManagement">
        <h2>Screen Management</h2>
        <label for="screenSelect">Select Screen: </label>
        <select id="screenSelect">
            <option value="1">Screen 1</option>
            <option value="2">Screen 2</option>
            <option value="3">Screen 3</option>
        </select>
        <button onclick="loadScreenConfiguration()">Load Configuration</button>

        <div id="screenConfig" style="display: none;">
            <h3>Configure Screen</h3>
            <label for="numSeats">Number of Seats: </label>
            <input type="number" id="numSeats" min="1" max="100">
            <button onclick="saveScreenConfiguration()">Save Configuration</button>
            <div id="seatLayout"></div>
        </div>
    </div>

    <div class="section" id="bookingManagement">
        <h2>Booking Management</h2>
        <label for="screenBookingSelect">Select Screen: </label>
        <select id="screenBookingSelect">
            <option value="1">Screen 1</option>
            <option value="2">Screen 2</option>
            <option value="3">Screen 3</option>
        </select>
        <button onclick="loadBookings()">Load Bookings</button>
        <div id="bookingDetails"></div>
    </div>

    <div class="section" id="foodMenuManagement">
        <h2>Food Menu Management</h2>
        <div id="foodMenu"></div>
        <button onclick="addFoodItem()">Add Item</button>
    </div>

    <script>
        const screens = 
        {
            1: { seats: 60 },
            2: { seats: 50 },
            3: { seats: 40 }
        };

        const foodMenu = [];

        function loadScreenConfiguration() 
        {
            const screenId = document.getElementById('screenSelect').value;
            document.getElementById('numSeats').value = screens[screenId].seats;
            document.getElementById('screenConfig').style.display = 'block';
        }

        function saveScreenConfiguration() 
        {
            const screenId = document.getElementById('screenSelect').value;
            const numSeats = document.getElementById('numSeats').value;
            screens[screenId].seats = numSeats;
            alert(`Screen ${screenId} configuration saved with ${numSeats} seats.`);
        }

        function loadBookings() 
       {
            const screenId = document.getElementById('screenBookingSelect').value;
            const bookings = [
                // This should be fetched from the server
                { seat: 1, status: 'booked' },
                { seat: 2, status: 'available' },
                { seat: 3, status: 'booked' }
            ];
            const bookingDetails = document.getElementById('bookingDetails');
            bookingDetails.innerHTML = '';
            for (let i = 1; i <= screens[screenId].seats; i++) 
            {
                const seatDiv = document.createElement('div');
                seatDiv.classList.add('seat');
                const booking = bookings.find(b => b.seat === i);
                if (booking && booking.status === 'booked') 
                {
                    seatDiv.classList.add('booked');
                }
                seatDiv.innerText = i;
                bookingDetails.appendChild(seatDiv);
            }
        }

        function addFoodItem() 
        {
            const foodName = prompt('Enter food item name:');
            const foodPrice = prompt('Enter food item price:');
            if (foodName && foodPrice) 
            {
                foodMenu.push({ name: foodName, price: foodPrice });
                renderFoodMenu();
            }
        }

        function renderFoodMenu() 
        {
            const foodMenuDiv = document.getElementById('foodMenu');
            foodMenuDiv.innerHTML = '';
            foodMenu.forEach(item => 
            {
                const itemDiv = document.createElement('div');
                itemDiv.innerText = `${item.name} - $${item.price}`;
                foodMenuDiv.appendChild(itemDiv);
            });
        }

        
        renderFoodMenu();
    </script>
</body>
</html>
