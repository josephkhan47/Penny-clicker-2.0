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
        .bank-details {
            margin-top: 20px;
            display: none;
            text-align: left;
        }
        .bank-details input {
            width: calc(80% - 20px);
            padding: 10px;
            margin-top: 10px;
            font-size: 16px;
        }
        #cash-out-message {
            margin-top: 20px;
            font-size: 18px;
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

    <div class="bank-details" id="bank-details">
        <h2>Bank Transfer Details</h2>
        <input type="text" id="account-number" placeholder="Account Number" maxlength="8">
        <input type="text" id="sort-code" placeholder="Sort Code" maxlength="6">
        <input type="text" id="account-name" placeholder="Account Name">
    </div>

    <div id="cash-out-message"></div>
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

    // Show bank details when cash-out amount is entered
    document.getElementById('cash-out-amount').addEventListener('input', function() {
        const cashOutAmount = parseInt(this.value, 10);
        const bankDetailsDiv = document.getElementById('bank-details');
        bankDetailsDiv.style.display = cashOutAmount > 0 ? 'block' : 'none';
    });

    // Cash out functionality
    document.getElementById('cash-out-button').addEventListener('click', function() {
        const cashOutAmount = parseInt(document.getElementById('cash-out-amount').value, 10);
        const cashOutMessage = document.getElementById('cash-out-message');
        const accountNumber = document.getElementById('account-number').value;
        const sortCode = document.getElementById('sort-code').value;
        const accountName = document.getElementById('account-name').value;

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
        } else if (accountNumber.length !== 8 || sortCode.length !== 6) {
            cashOutMessage.textContent = "Please enter valid bank details.";
            cashOutMessage.style.color = "red";
        } else {
            // Here you would typically call your backend API to process the cash-out request
            totalEarnings -= cashOutAmount; // Deduct the cash-out amount from total earnings
            updateEarningsDisplay(); // Update the display
            cashOutMessage.textContent = `Successfully cashed out ${cashOutAmount}p to your bank!`;
            cashOutMessage.style.color = "green";
            document.getElementById('cash-out-amount').value = ""; // Clear input field
            document.getElementById('account-number').value = ""; // Clear bank details
            document.getElementById('sort-code').value = "";
            document.getElementById('account-name').value = "";
            document.getElementById('bank-details').style.display = 'none'; // Hide bank details
        }
    });
</script>

</body>
</html>
