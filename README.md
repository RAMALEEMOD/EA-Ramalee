# EA-Ramalee
<!DOCTYPE html>
<html lang=”en”>
<head>
    <meta charset=”UTF-8”>
    <meta name=”viewport” content=”width=device-width, initial-scale=1.0”>
    <title>Login – MT5 Trading EA</title>
    <script src=https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js></script>
    <script src=https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js></script>
    <script src=https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js></script>
</head>
<body>
    <h2>Login</h2>
    <div>
        <input type=”email” id=”email” placeholder=”Enter email” />
        <input type=”password” id=”password” placeholder=”Enter password” />
        <button onclick=”login()”>Login</button>
    </div>
    <div>
        <p>Don’t have an account? <a href=”signup.html”>Sign Up</a></p>
    </div>

    <script>
        Const firebaseConfig = {
            apiKey: “YOUR_API_KEY”,
            authDomain: “YOUR_AUTH_DOMAIN”,
            projectId: “YOUR_PROJECT_ID”,
            storageBucket: “YOUR_STORAGE_BUCKET”,
            messagingSenderId: “YOUR_MESSAGING_SENDER_ID”,
            appId: “YOUR_APP_ID”
        };
        
        Const app = firebase.initializeApp(firebaseConfig);
        Const auth = firebase.auth();

        Function login() {
            Const email = document.getElementById(“email”).value;
            Const password = document.getElementById(“password”).value;
            
            Auth.signInWithEmailAndPassword(email, password)
                .then(() => {
                    Window.location.href = “settings.html”;  // Redirect to settings page
                })
                .catch((error) => {
                    Alert(error.message);
                });
        }
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang=”en”>
<head>
    <meta charset=”UTF-8”>
    <meta name=”viewport” content=”width=device-width, initial-scale=1.0”>
    <title>Sign Up – MT5 Trading EA</title>
</head>
<body>
    <h2>Sign Up</h2>
    <div>
        <input type=”email” id=”email” placeholder=”Enter email” />
        <input type=”password” id=”password” placeholder=”Enter password” />
        <button onclick=”signUp()”>Sign Up</button>
    </div>
    <div>
        <p>Already have an account? <a href=”index.html”>Login</a></p>
    </div>

    <script>
        Const firebaseConfig = {
            apiKey: “YOUR_API_KEY”,
            authDomain: “YOUR_AUTH_DOMAIN”,
            projectId: “YOUR_PROJECT_ID”,
            storageBucket: “YOUR_STORAGE_BUCKET”,
            messagingSenderId: “YOUR_MESSAGING_SENDER_ID”,
            appId: “YOUR_APP_ID”
        };
        
        Const app = firebase.initializeApp(firebaseConfig);
        Const auth = firebase.auth();

        Function signUp() {
            Const email = document.getElementById(“email”).value;
            Const password = document.getElementById(“password”).value;
            
            Auth.createUserWithEmailAndPassword(email, password)
                .then(() => {
                    Window.location.href = “index.html”;  // Redirect to login page
                })
                .catch((error) => {
                    Alert(error.message);
                });
        }
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang=”en”>
<head>
    <meta charset=”UTF-8”>
    <meta name=”viewport” content=”width=device-width, initial-scale=1.0”>
    <title>EA Settings – MT5 Trading EA</title>
</head>
<body>
    <h2>EA Settings</h2>
    <form id=”settingsForm”>
        <label for=”lotSize”>Lot Size</label>
        <input type=”number” id=”lotSize” name=”lotSize” required />

        <label for=”strategy”>Trading Strategy</label>
        <select id=”strategy” name=”strategy”>
            <option value=”crossover”>MA Crossover</option>
            <option value=”breakout”>Breakout</option>
        </select>

        <button type=”submit”>Save Settings</button>
    </form>

    <script>
        Const firebaseConfig = {
            apiKey: “YOUR_API_KEY”,
            authDomain: “YOUR_AUTH_DOMAIN”,
            projectId: “YOUR_PROJECT_ID”,
            storageBucket: “YOUR_STORAGE_BUCKET”,
            messagingSenderId: “YOUR_MESSAGING_SENDER_ID”,
            appId: “YOUR_APP_ID”
        };

        Const app = firebase.initializeApp(firebaseConfig);
        Const auth = firebase.auth();
        Const db = firebase.firestore();

        // Listen for user login and load their settings
        Auth.onAuthStateChanged((user) => {
            If (!user) {
                Window.location.href = “index.html”; // Redirect to login if not logged in
            }

            // Load and display current settings from Firestore (if available)
            Const settingsRef = db.collection(‘settings’).doc(user.uid);
            settingsRef.get().then(doc => {
                if (doc.exists) {
                    const data = doc.data();
                    document.getElementById(“lotSize”).value = data.lotSize || 0.1;
                    document.getElementById(“strategy”).value = data.strategy || “crossover”;
                }
            });
        });

        // Save settings to Firestore
        Document.getElementById(“settingsForm”).addEventListener(“submit”, function(event) {
            Event.preventDefault();
            Const lotSize = document.getElementById(“lotSize”).value;
            Const strategy = document.getElementById(“strategy”).value;

            Db.collection(‘settings’).doc(auth.currentUser.uid).set({
                lotSize: lotSize,
                strategy: strategy
            }).then(() => {
                Alert(“Settings saved!”);
            }).catch((error) => {
                Alert(error.message);
            });
        });
    </script>
</body>
</html>
