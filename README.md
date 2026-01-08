<!DOCTYPE html>
<html>
<head>
    <title>Savannah Sugar Company</title>
    <script src="https://sdk.minepi.com/pi-sdk.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background-color: #f4f4f4; margin: 0; padding: 20px; }
        .product-grid { display: flex; justify-content: center; gap: 20px; flex-wrap: wrap; margin-top: 30px; }
        .card { background: white; border-radius: 10px; padding: 20px; width: 200px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .card img { width: 100%; border-radius: 5px; }
        .card h3 { margin: 10px 0; font-size: 1.2em; }
        .price { color: #555; font-weight: bold; margin-bottom: 15px; display: block; }
        button { background-color: #673ab7; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-weight: bold; }
        button:hover { background-color: #512da8; }
    </style>
</head>
<body>
    <h1>Savannah Sugar Company</h1>
    <p id="user-info">Connect your wallet to start shopping.</p>
    <button id="login-btn" onclick="login()">Connect Pi Wallet</button>

    <div class="product-grid">
        <div class="card">
            <img src="https://via.placeholder.com/150" alt="Brown Sugar">
            <h3>Pure Brown Sugar</h3>
            <span class="price">3.14 Pi</span>
            <button onclick="buyProduct('Brown Sugar', 3.14)">Buy Now</button>
        </div>
        <div class="card">
            <img src="https://via.placeholder.com/150" alt="White Sugar">
            <h3>Granulated White Sugar</h3>
            <span class="price">2.50 Pi</span>
            <button onclick="buyProduct('White Sugar', 2.50)">Buy Now</button>
        </div>
    </div>

    <script>
        Pi.init({ version: "2.0", sandbox: true });

        async function login() {
            try {
                const scopes = ['username', 'payments'];
                const auth = await Pi.authenticate(scopes, onIncompletePaymentFound);
                document.getElementById('user-info').innerHTML = `Welcome, <b>${auth.user.username}</b>!`;
                document.getElementById('login-btn').style.display = 'none';
            } catch (err) { alert("Please use the Pi Browser."); }
        }

        function buyProduct(name, price) {
            const paymentData = {
                amount: price,
                memo: `Purchase of ${name} from Savannah Sugar`,
                metadata: { productId: name.toLowerCase().replace(" ", "-") }
            };

            const callbacks = {
                onReadyForServerApproval: (paymentId) => { console.log("Approved ID:", paymentId); },
                onReadyForServerCompletion: (paymentId, txid) => { console.log("Completed TX:", txid); },
                onCancel: (paymentId) => { console.log("Cancelled"); },
                onError: (error, payment) => { console.error("Error", error); },
            };

            Pi.createPayment(paymentData, callbacks);
        }

        function onIncompletePaymentFound(payment) { /* Logic to handle unfinished orders */ };
    </script>
</body>
</html>
