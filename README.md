<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Penny Clicker App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        #app {
            max-width: 600px;
            margin: auto;
            text-align: center;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }
        h1 {
            color: #333;
        }
        #click-button, #cash-out-button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 18px;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            margin-top: 10px;
        }
        #click-button:hover, #cash-out-button:hover {
            background-color: #218838;
        }
        #total-earnings {
            font-size: 24px;
            margin-top: 20px;
        }
        #cash-out {
            margin-top: 20px;
        }
        #cash-out-amount {
            width: 80%;
            padding: 10px;
            font-size: 18px;
            margin-top: 10px;
        }
        .paypal-details {
            margin-top: 20px;
            display: none;
            text-align: left;
        }
        .paypal-details input {
            width: calc(80% - 20px);
            padding: 10px;
            margin-top: 10px;
            font-size: 16px;
        }
        #cash-out-message {
            margin-top: 20px;
            font-size: 18px;
        }
        #redirect-message {
            margin-top: 20px;
            font-size: 18px;
            color: #007bff; /* Blue color for redirect message */
        }
    </style>
</head>
<body>

<div id="app">
    <h1>Penny Clicker</h1>
    <button id="click-button">Click Me!</button>
    <div id="total-earnings">Total Earnings: 0p</div>

    <div id="cash-out">
        <input type="number" id="cash-out-amount" placeholder="Enter amount to cash out (in pence)" min="1">
        <button id="cash-out-button">Cash Out</button>
    </div>

    <div class="paypal-details" id="paypal-details">
        <h2>PayPal Email</h2>
        <input type="email" id="paypal-email" placeholder="Enter your PayPal email">
    </div>

    <div id="cash-out-message"></div>
    <div id="redirect-message"></div> <!-- Message for successful redirection -->
</div>

<script>
    let totalEarnings = 0; // Initialize total earnings

    // Function to handle click event
    document.getElementById('click-button').addEventListener('click', function() {
        totalEarnings += 1; // Increment earnings by 1p
        updateEarningsDisplay(); // Update the display
    });

    // Function to update the earnings display
    function updateEarningsDisplay() {
        document.getElementById('total-earnings').textContent = `Total Earnings: ${totalEarnings}p`;
    }

    // Show PayPal email input when cash-out amount is entered
    document.getElementById('cash-out-amount').addEventListener('input', function() {
        const cashOutAmount = parseInt(this.value, 10);
        const paypalDetailsDiv = document.getElementById('paypal-details');
        paypalDetailsDiv.style.display = cashOutAmount > 0 ? 'block' : 'none';
    });

    // Cash out functionality
    document.getElementById('cash-out-button').addEventListener('click', function() {
        const cashOutAmount = parseInt(document.getElementById('cash-out-amount').value, 10);
        const cashOutMessage = document.getElementById('cash-out-message');
        const redirectMessage = document.getElementById('redirect-message');
        const paypalEmail = document.getElementById('paypal-email').value;

        // Clear previous messages
        cashOutMessage.textContent = '';
        redirectMessage.textContent = '';

        // Validate cash-out amount
        if (isNaN(cashOutAmount) || cashOutAmount <= 0) {
            cashOutMessage.textContent = "Please enter a valid amount.";
            cashOutMessage.style.color = "red";
            return;
        }

        // Check if enough earnings to cash out
        if (cashOutAmount > totalEarnings) {
            cashOutMessage.textContent = "Insufficient earnings to cash out.";
            cashOutMessage.style.color = "red";
        } else if (!paypalEmail) {
            cashOutMessage.textContent = "Please enter your PayPal email.";
            cashOutMessage.style.color = "red";
        } else {
            // Deduct the cash-out amount from total earnings
            totalEarnings -= cashOutAmount; 
            updateEarningsDisplay(); // Update the display

            // Simulate redirect to PayPal for payment processing
            redirectMessage.textContent = `Redirecting to PayPal to process ${cashOutAmount}p...`;
            redirectMessage.style.color = "green";

            // Here, you would typically call PayPal's API for processing the payment
            // For this simulation, we'll just clear the input fields after 2 seconds
            setTimeout(() => {
                // Clear input fields
                document.getElementById('cash-out-amount').value = ""; 
                document.getElementById('paypal-email').value = ""; 
                document.getElementById('paypal-details').style.display = 'none'; // Hide PayPal email input
                redirectMessage.textContent = ""; // Clear the redirect message
            }, 2000);
        }
    });
</script>

</body>
</html>
