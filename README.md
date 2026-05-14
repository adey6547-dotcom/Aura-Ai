<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AuraAI | Virtual Companions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .glass { background: rgba(255, 255, 255, 0.05); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .gradient-text { background: linear-gradient(90deg, #ff0080, #7928ca); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-thumb { background: #444; border-radius: 10px; }
    </style>
</head>
<body class="bg-zinc-950 text-white font-sans">

    <div id="app" class="max-w-md mx-auto h-screen flex flex-col p-4">
        <!-- Header -->
        <header class="flex justify-between items-center py-4">
            <h1 class="text-2xl font-bold gradient-text">AuraAI</h1>
            <div id="balance" class="text-sm bg-zinc-800 px-3 py-1 rounded-full text-pink-400">Credits: 0</div>
        </header>

        <!-- Character Selection -->
        <section id="char-select" class="flex gap-4 overflow-x-auto pb-4 mb-4">
            <div onclick="setChar('Luna')" class="cursor-pointer flex-shrink-0 text-center">
                <div class="w-16 h-16 rounded-full border-2 border-pink-500 overflow-hidden"><img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Luna" alt="Luna"></div>
                <p class="text-xs mt-1">Luna (Sultry)</p>
            </div>
            <div onclick="setChar('Mika')" class="cursor-pointer flex-shrink-0 text-center">
                <div class="w-16 h-16 rounded-full border-2 border-purple-500 overflow-hidden"><img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Mika" alt="Mika"></div>
                <p class="text-xs mt-1">Mika (Playful)</p>
            </div>
        </section>

        <!-- Chat Area -->
        <main id="chat-box" class="flex-grow glass rounded-2xl p-4 overflow-y-auto mb-4 space-y-4">
            <div class="bg-zinc-800 p-3 rounded-lg max-w-[80%]">
                <p class="text-sm">Welcome back, darling. Choose a character and let's talk. 💋</p>
            </div>
        </main>

        <!-- Input Area -->
        <footer class="space-y-4">
            <div class="flex gap-2">
                <input id="user-input" type="text" placeholder="Type a message..." class="flex-grow bg-zinc-900 border border-zinc-800 rounded-xl px-4 py-3 focus:outline-none focus:border-pink-500">
                <button onclick="sendMessage()" class="bg-pink-600 px-6 py-3 rounded-xl hover:bg-pink-500 transition">Send</button>
            </div>
            
            <!-- Payment Trigger -->
            <button onclick="showPayment()" class="w-full py-2 text-xs text-zinc-400 underline uppercase tracking-widest">Unlock Premium Response (₹49)</button>
        </footer>
    </div>

    <!-- Payment Modal -->
    <div id="payment-modal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center p-6">
        <div class="bg-zinc-900 p-8 rounded-3xl text-center max-w-xs border border-pink-500/30">
            <h2 class="text-xl font-bold mb-4">Upgrade to Premium</h2>
            <p class="text-sm text-zinc-400 mb-6">Scan to pay <strong>₹49</strong> via UPI to continue the spicy conversation.</p>
            <div id="qrcode" class="bg-white p-2 rounded-lg mb-6 inline-block">
                <!-- UPI QR Logic -->
                <img id="qr-img" src="" alt="UPI QR">
            </div>
            <button onclick="closePayment()" class="block w-full text-zinc-500 text-sm">Cancel</button>
        </div>
    </div>

    <script>
        let currentCharacter = "Luna";
        const upiId = "adey6547@axl";
        const amount = "49";

        // Logic to switch characters
        function setChar(name) {
            currentCharacter = name;
            addMessage("System", `You are now chatting with ${name}.`, 'text-zinc-500 italic');
        }

        // Logic to send and receive messages
        function sendMessage() {
            const input = document.getElementById('user-input');
            const text = input.value.trim();
            if(!text) return;

            addMessage("You", text, 'bg-pink-900/30 ml-auto text-right');
            input.value = '';

            // Simulated Backend Response Logic
            setTimeout(() => {
                const response = getAIResponse(text);
                addMessage(currentCharacter, response, 'bg-zinc-800');
            }, 1000);
        }

        function getAIResponse(input) {
            const responses = [
                "I've been thinking about you all day... want to hear a secret? 🤫",
                "You always know exactly what to say to make me blush.",
                "Is it getting hot in here, or is it just the way you're looking at me?",
                "I have so many things I want to show you... but you'll have to subscribe first."
            ];
            return responses[Math.floor(Math.random() * responses.length)];
        }

        function addMessage(sender, text, style) {
            const chatBox = document.getElementById('chat-box');
            const div = document.createElement('div');
            div.className = `p-3 rounded-xl max-w-[85%] ${style}`;
            div.innerHTML = `<p class="text-xs font-bold opacity-50 mb-1">${sender}</p><p class="text-sm">${text}</p>`;
            chatBox.appendChild(div);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // UPI QR Generator
        function showPayment() {
            const upiUrl = `upi://pay?pa=${upiId}&pn=AuraAI&am=${amount}&cu=INR`;
            const qrApi = `https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${encodeURIComponent(upiUrl)}`;
            document.getElementById('qr-img').src = qrApi;
            document.getElementById('payment-modal').classList.remove('hidden');
        }

        function closePayment() {
            document.getElementById('payment-modal').classList.add('hidden');
        }
    </script>
</body>
</html>
