<Centro de Controle>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel de Controle do Comandante</title>
    <!-- Inclui o Tailwind CSS para estilização moderna e responsiva -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        .status-Pendente { background-color: #fcd34d; color: #78350f; }
        .status-Aprovado { background-color: #34d399; color: #065f46; }
        .status-Negado { background-color: #f87171; color: #991b1b; }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-top: 4px solid #3b82f6;
            border-radius: 50%;
            width: 3rem;
            height: 3rem;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="p-6 md:p-12">
    <div class="bg-white rounded-3xl shadow-3d p-6 md:p-10 max-w-5xl mx-auto border-4 border-gray-800">
        <h1 class="text-3xl font-extrabold text-center text-gray-800 mb-8">Painel de Controle do Comandante</h1>
        
        <div id="requests-list" class="flex flex-col gap-6">
            <!-- As solicitações serão renderizadas aqui pelo JavaScript -->
        </div>

        <div id="loading-indicator" class="flex items-center justify-center py-10">
            <div class="spinner"></div>
            <span class="ml-4 text-gray-600">Carregando solicitações...</span>
        </div>

        <div id="no-requests" class="hidden text-center text-gray-500 py-10">
            <p>Nenhuma solicitação de permuta pendente no momento.</p>
        </div>
    </div>

    <script type="module">
        // Firebase Imports
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getFirestore, collection, onSnapshot, doc, updateDoc, query } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";

        // Global variables provided by the environment
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        let userId = null;
        let isAuthReady = false;

        // HTML elements
        const requestsListElement = document.getElementById('requests-list');
        const loadingIndicator = document.getElementById('loading-indicator');
        const noRequestsMessage = document.getElementById('no-requests');

        // Authenticate the user
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                userId = user.uid;
                isAuthReady = true;
                console.log("Comandante autenticado. UID:", userId);
                await fetchRequests();
            } else {
                console.log("Tentando autenticação...");
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }
            }
        });

        // Function to format the date
        function formatDate(dateString) {
            const date = new Date(dateString + 'T00:00:00');
            return new Intl.DateTimeFormat('pt-BR').format(date);
        }

        // Function to render a single request card
        function renderRequest(doc) {
            const request = doc.data();
            const card = document.createElement('div');
            card.className = `bg-gray-100 p-6 rounded-2xl shadow-lg border-2 border-gray-200 transition-all duration-300 transform hover:scale-101`;
            card.innerHTML = `
                <div class="flex items-center justify-between mb-4">
                    <h3 class="text-xl font-bold text-gray-800">Solicitação de Permuta</h3>
                    <span class="px-4 py-1 rounded-full font-semibold text-sm status-${request.status.replace(' ', '')}">
                        ${request.status}
                    </span>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-gray-700">
                    <p><strong>Localização:</strong> ${request.localizacao}</p>
                    <p><strong>Data de Envio:</strong> ${new Date(request.timestamp.seconds * 1000).toLocaleString('pt-BR')}</p>
                    <p><strong>PM Substituído:</strong> ${request.pmSubstituido.nome} - ID: ${request.pmSubstituido.id}</p>
                    <p><strong>PM Substituto:</strong> ${request.pmSubstituto.nome} - ID: ${request.pmSubstituto.id}</p>
                    <p class="md:col-span-2"><strong>Serviço Permutado:</strong> ${formatDate(request.servicoPermutado.inicio)} a ${formatDate(request.servicoPermutado.fim)}</p>
                    <p class="md:col-span-2"><strong>Serviço de Pagamento:</strong> ${request.pagamentoServico.naoHaPagamento ? 'Não há pagamento de serviço' : `${formatDate(request.pagamentoServico.inicio)} a ${formatDate(request.pagamentoServico.fim)}`}</p>
                </div>
                <div class="flex justify-end gap-4 mt-6 no-print">
                    <button id="approve-${doc.id}" class="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-6 rounded-lg shadow-md transition-transform transform hover:scale-105">
                        Aprovar
                    </button>
                    <button id="deny-${doc.id}" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-6 rounded-lg shadow-md transition-transform transform hover:scale-105">
                        Negar
                    </button>
                </div>
            `;
            
            // Adiciona os event listeners para os botões
            const approveButton = card.querySelector(`#approve-${doc.id}`);
            const denyButton = card.querySelector(`#deny-${doc.id}`);

            if (request.status === 'Pendente') {
                approveButton.addEventListener('click', () => updateRequestStatus(doc.id, 'Aprovado'));
                denyButton.addEventListener('click', () => updateRequestStatus(doc.id, 'Negado'));
            } else {
                // Desativa os botões se o status não for "Pendente"
                approveButton.disabled = true;
                denyButton.disabled = true;
                approveButton.classList.add('opacity-50', 'cursor-not-allowed');
                denyButton.classList.add('opacity-50', 'cursor-not-allowed');
            }
            
            requestsListElement.appendChild(card);
        }

        // Function to update the status of a request in Firestore
        async function updateRequestStatus(docId, status) {
            if (!isAuthReady) {
                console.error("Autenticação não finalizada. Tente novamente.");
                return;
            }
            try {
                const docRef = doc(db, `/artifacts/${appId}/public/data/permuta_requests`, docId);
                await updateDoc(docRef, { status: status });
                console.log(`Status do documento ${docId} atualizado para ${status}`);
            } catch (error) {
                console.error("Erro ao atualizar status do documento: ", error);
            }
        }

        // Listen for real-time changes to the requests
        async function fetchRequests() {
            const requestsCollection = collection(db, `/artifacts/${appId}/public/data/permuta_requests`);
            const q = query(requestsCollection);

            onSnapshot(q, (querySnapshot) => {
                // Clear the list before rendering
                requestsListElement.innerHTML = '';
                loadingIndicator.classList.add('hidden');
                
                if (querySnapshot.empty) {
                    noRequestsMessage.classList.remove('hidden');
                } else {
                    noRequestsMessage.classList.add('hidden');
                    querySnapshot.forEach((doc) => {
                        renderRequest(doc);
                    });
                }
            }, (error) => {
                console.error("Erro ao carregar dados em tempo real: ", error);
                loadingIndicator.classList.add('hidden');
                requestsListElement.innerHTML = `<p class="text-red-500 text-center mt-8">Erro ao carregar solicitações. Por favor, recarregue a página.</p>`;
            });
        }
    </script>
</body>
</html>
