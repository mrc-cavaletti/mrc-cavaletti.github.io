<html>
<head>
    <title>TTRPG Real-Time Sheet</title>
</head>
<body>
    <h1>Character: <span id="char-name">Loading...</span></h1>
    <p>HP: <b id="hp-display">0</b></p>
    
    <input type="number" id="hp-input" placeholder="New HP">
    <button onclick="updateHP()">Update HP</button>
    <img id="char-pic">

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, onSnapshot, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // PASTE YOUR CONFIG HERE
        const firebaseConfig = {
            apiKey: "AIzaSyBrSgmyLYjZ-TD5ecWzLh0SlL0kuKrTHg0",
            authDomain: "ttrpg-manager-792b7.firebaseapp.com",
            projectId: "ttrpg-manager-792b7",
            storageBucket: "ttrpg-manager-792b7.firebasestorage.app",
            messagingSenderId: "180951912564",
            appId: "1:180951912564:web:f99d7273b05fbca111c121"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // Reference to a specific character document
        const charRef = doc(db, "Characters", "Richard Magnusson");

        // 1. REAL-TIME LISTENER (The "Magic" part)
        onSnapshot(charRef, (snapshot) => {
            if (snapshot.exists()) {
                const data = snapshot.data();
                document.getElementById('char-name').innerText = data.Name;
                document.getElementById('hp-display').innerText = data.HP;
                document.getElementById('char-pic').src = data.ImageRef;
            } else {
                console.log("No such character! Create 'players/hero1' in console.");
            }
        });

        // 2. UPDATE DATA
        window.updateHP = async () => {
            const newHP = document.getElementById('hp-input').value;
            await updateDoc(charRef, {
                HP: parseInt(newHP)
            });
        };
    </script>
</body>
</html>
