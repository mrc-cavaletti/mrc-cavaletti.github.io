<!DOCTYPE html>
<html>
<head>
    <title>TTRPG Real-Time Sheet</title>
</head>
<body>
    <h1>Character: <span id="char-name">Loading...</span></h1>
    <p>HP: <b id="hp-display">0</b></p>
    
    <input type="number" id="hp-input" placeholder="New HP">
    <button onclick="updateHP()">Update HP</button>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, onSnapshot, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // PASTE YOUR CONFIG HERE
        const firebaseConfig = {
            apiKey: "AIzaSyAuXd9f6syxohcRQwXvFYcyuJCIyv_enTI",
            authDomain: "charactersheetmanager-ee5e8.firebaseapp.com",
            projectId: "charactersheetmanager-ee5e8",
            storageBucket: "charactersheetmanager-ee5e8.firebasestorage.app",
            messagingSenderId: "1095802341255",
            appId: "1:1095802341255:web:8e3dd6ce82c82d185d7522"
            };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // Reference to a specific character document
        // (Firestore: Collection "players", Document "hero1")
        const charRef = doc(db, "Players", "Hero1");

        // 1. REAL-TIME LISTENER (The "Magic" part)
        onSnapshot(charRef, (snapshot) => {
            if (snapshot.exists()) {
                const data = snapshot.data();
                document.getElementById('char-name').innerText = data.Name;
                document.getElementById('hp-display').innerText = data.HP;
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
