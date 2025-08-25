<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário de Permuta de Serviço Oficial</title>
    <!-- Inclui o Tailwind CSS para estilização moderna e responsiva -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Estilos para a impressão e geração de PDF */
        @media print {
            body {
                background-color: white;
            }
            .no-print {
                display: none;
            }
            .printable-form {
                border: none !important;
                box-shadow: none !important;
                padding: 0 !important;
                margin: 0 !important;
                max-width: none !important;
            }
            input[type="text"], input[type="date"] {
                border: none !important;
                border-bottom: 1px solid #000 !important;
                padding: 0 !important;
            }
        }
        .input-error {
            border-color: #ef4444;
        }
        #custom-alert {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 9999;
            background-color: rgba(0, 0, 0, 0.75);
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
    <div id="custom-alert" class="hidden fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-75 z-[9999]">
        <div class="bg-white p-6 rounded-xl shadow-xl max-w-sm w-full text-center">
            <h3 class="text-xl font-bold mb-4" id="alert-title"></h3>
            <p class="text-gray-700 mb-6" id="alert-message"></p>
            <button onclick="closeCustomAlert()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg">OK</button>
        </div>
    </div>
    <div class="printable-form bg-white rounded-3xl shadow-3d p-6 md:p-10 max-w-4xl mx-auto border-4 border-gray-800">
        
        <!-- Cabeçalho tipo folha timbrada - Brasão removido da visualização no-print -->
        <div class="flex flex-col items-center justify-center text-center p-4 bg-gray-800 text-white rounded-t-2xl no-print">
            <h1 class="text-xl font-bold tracking-wide">ESTADO DO MARANHÃO</h1>
            <p class="text-sm font-light">SECRETARIA DE SEGURANÇA PÚBLICA</p>
            <p class="text-sm font-light">COMANDO DE POLICIAMENTO DE ÁREA DO INTERIOR-5 (CPA/I-5)</p>
            <p class="text-xs mt-1 font-extralight">10º BATALHÃO DA POLÍCIA MILITAR DO MARANHÃO - 2ª CIA</p>
        </div>
        
        <h2 class="text-2xl font-extrabold text-center text-gray-800 my-8">FORMULÁRIO DE PERMUTA DE SERVIÇO</h2>
        
        <!-- Campos para a localização -->
        <div class="mb-6">
            <h5 class="text-base font-bold text-gray-800 mb-4 uppercase tracking-wider">Localização:</h5>
            <div class="flex flex-wrap gap-6">
                <label class="flex items-center text-base text-gray-700 cursor-pointer">
                    <input type="checkbox" name="localizacao" value="SEDE" class="form-checkbox h-5 w-5 rounded-md text-blue-600 border-gray-400 focus:ring-blue-500 transition duration-150 ease-in-out">
                    <span class="ml-2 font-medium">SEDE</span>
                </label>
                <label class="flex items-center text-base text-gray-700 cursor-pointer">
                    <input type="checkbox" name="localizacao" value="DPM TURILÂNDIA" class="form-checkbox h-5 w-5 rounded-md text-blue-600 border-gray-400 focus:ring-blue-500 transition duration-150 ease-in-out">
                    <span class="ml-2 font-medium">DPM TURILÂNDIA</span>
                </label>
                <label class="flex items-center text-base text-gray-700 cursor-pointer">
                    <input type="checkbox" name="localizacao" value="DPM TURIAÇU" class="form-checkbox h-5 w-5 rounded-md text-blue-600 border-gray-400 focus:ring-blue-500 transition duration-150 ease-in-out">
                    <span class="ml-2 font-medium">DPM TURIAÇU</span>
                </label>
            </div>
            <p id="localizacaoWarning" class="text-sm text-red-600 mt-2 hidden font-semibold">Selecione uma localização.</p>
        </div>
        
        <!-- Campos para o PM Substituído -->
        <div class="mb-6 border-t-2 pt-6">
            <h5 class="text-base font-bold text-gray-800 mb-4 uppercase tracking-wider">PM SUBSTITUÍDO:</h5>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                    <label for="pmSubstituidoNome" class="text-sm font-medium text-gray-600 block">Nome:</label>
                    <input type="text" id="pmSubstituidoNome" placeholder="SD PM 001/22 FULANO" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md" list="pmList">
                    <p id="pmSubstituidoNomeWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                </div>
                <div>
                    <label for="pmSubstituidoIdentificacao" class="text-sm font-medium text-gray-600 block">ID:</label>
                    <input type="tel" id="pmSubstituidoIdentificacao" placeholder="012345" pattern="\d*" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                    <p id="pmSubstituidoIdentificacaoWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                </div>
            </div>
        </div>

        <!-- Campos para o PM Substituto -->
        <div class="mb-6">
            <h5 class="text-base font-bold text-gray-800 mb-4 uppercase tracking-wider">PM SUBSTITUTO:</h5>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div>
                    <label for="pmSubstitutoNome" class="text-sm font-medium text-gray-600 block">Nome:</label>
                    <input type="text" id="pmSubstitutoNome" placeholder="SD PM 001/22 FULANO" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md" list="pmList">
                    <p id="pmSubstitutoNomeWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                </div>
                <div>
                    <label for="pmSubstitutoIdentificacao" class="text-sm font-medium text-gray-600 block">ID:</label>
                    <input type="tel" id="pmSubstitutoIdentificacao" placeholder="012345" pattern="\d*" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                    <p id="pmSubstitutoIdentificacaoWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                </div>
            </div>
        </div>

        <!-- Datalist para a lista de PMs -->
        <datalist id="pmList"></datalist>

        <!-- Campos para as datas e tipo de serviço -->
        <div class="mb-6 border-t-2 pt-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="flex flex-col">
                    <label class="text-sm font-bold text-gray-800 uppercase tracking-wide mb-2">DATA(S) DO SERVIÇO PERMUTADO:</label>
                    <p class="text-xs text-gray-500 mb-2">Escolha entre 1 a 3 dias consecutivos</p>
                    <div class="flex flex-col md:flex-row gap-4">
                        <div class="w-full">
                            <label for="dataServicoPermutadoInicio" class="text-xs text-gray-500 block">Início:</label>
                            <input type="date" id="dataServicoPermutadoInicio" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                            <p id="dataServicoPermutadoInicioWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                        </div>
                        <div class="w-full">
                            <label for="dataServicoPermutadoFim" class="text-xs text-gray-500 block">Término:</label>
                            <input type="date" id="dataServicoPermutadoFim" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                            <p id="dataServicoPermutadoFimWarning" class="text-xs text-red-600 mt-1 hidden">Campo obrigatório.</p>
                        </div>
                    </div>
                </div>
                <div class="flex flex-col">
                    <div class="flex items-center justify-between mb-2">
                        <label class="text-sm font-bold text-gray-800 uppercase tracking-wide">DATA(S) DO PAGAMENTO DO SERVIÇO:</label>
                        <div class="flex items-center">
                            <input type="checkbox" id="noPagamentoCheckbox" class="form-checkbox h-4 w-4 rounded text-blue-600 border-gray-400 focus:ring-blue-500">
                            <label for="noPagamentoCheckbox" class="text-xs text-gray-600 ml-2">Não há pagamento</label>
                        </div>
                    </div>
                    <div id="pagamentoServicoFields" class="flex flex-col md:flex-row gap-4">
                        <div class="w-full">
                            <label for="dataPagamentoServicoInicio" class="text-xs text-gray-500 block">Início:</label>
                            <input type="date" id="dataPagamentoServicoInicio" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                        </div>
                        <div class="w-full">
                            <label for="dataPagamentoServicoFim" class="text-xs text-gray-500 block">Término:</label>
                            <input type="date" id="dataPagamentoServicoFim" class="w-full mt-1 p-3 border-b-2 border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 transition-colors duration-200 rounded-md">
                        </div>
                    </div>
                </div>
            </div>
            <p id="dateWarning" class="text-sm text-red-600 mt-4 text-center hidden">A diferença entre as datas não pode ser superior a 3 dias.</p>
        </div>
        
        <!-- Botões de ação do formulário -->
        <div class="no-print flex flex-col md:flex-row justify-center items-center gap-4 mt-8">
            <button onclick="printForm()" class="bg-blue-700 hover:bg-blue-800 text-white font-bold py-3 px-8 rounded-xl shadow-lg transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-600 focus:ring-offset-2">
                Concluir Permuta
            </button>
            <button onclick="signWithGovBr()" class="bg-gray-500 hover:bg-gray-600 text-white font-bold py-3 px-8 rounded-xl shadow-lg transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-offset-2">
                Ir para o GOV.BR
            </button>
            <button onclick="sendToCommander()" class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-8 rounded-xl shadow-lg transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-green-400 focus:ring-offset-2">
                Enviar para o Comandante
            </button>
        </div>

        <!-- Seção para acompanhar o status -->
        <div class="mt-12 pt-6 border-t-2 border-gray-200">
            <h3 class="text-2xl font-bold text-gray-800 text-center mb-6">Minhas Solicitações de Permuta</h3>
            <div id="my-requests-list" class="flex flex-col gap-4">
                <!-- As solicitações do usuário serão renderizadas aqui -->
            </div>
            <div id="my-requests-loading" class="flex items-center justify-center py-10 hidden">
                <div class="spinner"></div>
                <span class="ml-4 text-gray-600">Carregando suas solicitações...</span>
            </div>
            <div id="my-requests-empty" class="hidden text-center text-gray-500 py-10">
                <p>Você ainda não enviou nenhuma solicitação de permuta.</p>
            </div>
        </div>
    </div>

    <script type="module">
        // Firebase Imports
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
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

        // HTML elements for status tracker
        const myRequestsListElement = document.getElementById('my-requests-list');
        const myRequestsLoading = document.getElementById('my-requests-loading');
        const myRequestsEmpty = document.getElementById('my-requests-empty');

        // Authenticate the user and then fetch requests
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                userId = user.uid;
                isAuthReady = true;
                console.log("Usuário autenticado. UID:", userId);
                await fetchMyRequests();
            } else {
                console.log("Tentando autenticação...");
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }
            }
        });

        window.showCustomAlert = function(title, message) {
            document.getElementById('alert-title').textContent = title;
            document.getElementById('alert-message').textContent = message;
            document.getElementById('custom-alert').classList.remove('hidden');
        };

        window.closeCustomAlert = function() {
            document.getElementById('custom-alert').classList.add('hidden');
        };

        const brasaoUrl = 'https://i.ibb.co/Xr6X43nG/BRAS-O.png';
        
        // Atribui a URL do brasão para a imagem no cabeçalho e para a impressão
        const headerBrasaoElement = document.getElementById('header-brasao');
        if (headerBrasaoElement) {
            headerBrasaoElement.src = brasaoUrl;
        }

        const noPagamentoCheckbox = document.getElementById('noPagamentoCheckbox');
        const pagamentoServicoInicio = document.getElementById('dataPagamentoServicoInicio');
        const pagamentoServicoFim = document.getElementById('dataPagamentoServicoFim');
        const fields = document.getElementById('pagamentoServicoFields');

        function checkPagamentoStatus() {
            if (pagamentoServicoInicio.value.trim() === '' && pagamentoServicoFim.value.trim() === '') {
                noPagamentoCheckbox.checked = true;
                fields.classList.add('hidden');
            } else {
                noPagamentoCheckbox.checked = false;
                fields.classList.remove('hidden');
            }
        }
        
        pagamentoServicoInicio.addEventListener('input', checkPagamentoStatus);
        pagamentoServicoFim.addEventListener('input', checkPagamentoStatus);

        noPagamentoCheckbox.addEventListener('change', function() {
            if (this.checked) {
                fields.classList.add('hidden');
                pagamentoServicoInicio.value = '';
                pagamentoServicoFim.value = '';
            } else {
                fields.classList.remove('hidden');
            }
        });
        
        const localizacaoCheckboxes = document.querySelectorAll('input[name="localizacao"]');
        localizacaoCheckboxes.forEach(checkbox => {
            checkbox.addEventListener('change', function() {
                if (this.checked) {
                    localizacaoCheckboxes.forEach(otherCheckbox => {
                        if (otherCheckbox !== this) {
                            otherCheckbox.checked = false;
                        }
                    });
                }
                updatePmList();
                validateLocalizacao();
            });
        });

        const pmData = {
            "Cb Pm 626/14 Coriolano": null,
            "Sd Pm 473/16 André": "849351",
            "Sd Pm 408/18 Ribeiro": null,
            "Sd Pm 424/18 Rodrigues": null,
            "Sd Pm 885/18 Albert": null,
            "Sd Pm 502/22 Sales": "869293",
            "SD PM 572/22 Theodosio": "871896",
            "Sd Pm 457/24 Eduardo Silva": "869987"
        };
        
        const pmSubstituidoNomeInput = document.getElementById('pmSubstituidoNome');
        const pmSubstituidoIdentificacaoInput = document.getElementById('pmSubstituidoIdentificacao');
        const pmSubstitutoNomeInput = document.getElementById('pmSubstitutoNome');
        const pmSubstitutoIdentificacaoInput = document.getElementById('pmSubstitutoIdentificacao');

        pmSubstituidoNomeInput.addEventListener('input', (event) => {
            const selectedName = event.target.value;
            if (pmData[selectedName]) {
                pmSubstituidoIdentificacaoInput.value = pmData[selectedName];
            } else {
                pmSubstituidoIdentificacaoInput.value = '';
            }
        });
        
        pmSubstitutoNomeInput.addEventListener('input', (event) => {
            const selectedName = event.target.value;
            if (pmData[selectedName]) {
                pmSubstitutoIdentificacaoInput.value = pmData[selectedName];
            } else {
                pmSubstitutoIdentificacaoInput.value = '';
            }
        });

        function updatePmList() {
            const pmList = document.getElementById('pmList');
            const turiacuCheckbox = document.querySelector('input[name="localizacao"][value="DPM TURIAÇU"]');

            // Limpa a lista existente
            pmList.innerHTML = '';

            if (turiacuCheckbox.checked) {
                for (const name in pmData) {
                    const option = document.createElement('option');
                    option.value = name;
                    pmList.appendChild(option);
                }
            }
        }

        function validateLocalizacao() {
            const warning = document.getElementById('localizacaoWarning');
            const anyChecked = Array.from(localizacaoCheckboxes).some(cb => cb.checked);
            if (anyChecked) {
                warning.classList.add('hidden');
                return true;
            } else {
                warning.classList.remove('hidden');
                return false;
            }
        }

        function validateBrasao() {
            // Não há mais upload, então sempre retorna true
            return true;
        }

        function validateField(fieldId, warningId) {
            const field = document.getElementById(fieldId);
            const warning = document.getElementById(warningId);
            if (!field.value.trim()) {
                field.classList.add('input-error');
                warning.classList.remove('hidden');
                return false;
            } else {
                field.classList.remove('input-error');
                warning.classList.add('hidden');
                return true;
            }
        }

        function validateDateRange(startDateId, endDateId, warningId) {
            const startDateInput = document.getElementById(startDateId);
            const endDateInput = document.getElementById(endDateId);
            const warningElement = document.getElementById(warningId);
            let isValid = true;
            
            if (!startDateInput.value.trim()) {
                startDateInput.classList.add('input-error');
                document.getElementById('dataServicoPermutadoInicioWarning').classList.remove('hidden');
                isValid = false;
            } else {
                startDateInput.classList.remove('input-error');
                document.getElementById('dataServicoPermutadoInicioWarning').classList.add('hidden');
            }

            if (!endDateInput.value.trim()) {
                endDateInput.classList.add('input-error');
                document.getElementById('dataServicoPermutadoFimWarning').classList.remove('hidden');
                isValid = false;
            } else {
                endDateInput.classList.remove('input-error');
                document.getElementById('dataServicoPermutadoFimWarning').classList.add('hidden');
            }

            if (isValid && startDateInput.value && endDateInput.value) {
                const startDate = new Date(startDateInput.value + 'T00:00:00');
                const endDate = new Date(endDateInput.value + 'T00:00:00');
                
                const diffTime = Math.abs(endDate - startDate);
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));

                if (diffDays > 2) {
                    warningElement.classList.remove('hidden');
                    return false;
                } else {
                    warningElement.classList.add('hidden');
                    return true;
                }
            }
            return isValid;
        }

        const dateFields = [
            { start: 'dataServicoPermutadoInicio', end: 'dataServicoPermutadoFim', warn: 'dateWarning' },
            { start: 'dataPagamentoServicoInicio', end: 'dataPagamentoServicoFim', warn: 'dateWarning' }
        ];
        
        dateFields.forEach(field => {
            const startDateInput = document.getElementById(field.start);
            const endDateInput = document.getElementById(field.end);
            
            startDateInput.addEventListener('change', () => validateDateRange(field.start, field.end, field.warn));
            endDateInput.addEventListener('change', () => validateDateRange(field.start, field.end, field.warn));
        });

        const requiredFields = [
            { id: 'pmSubstituidoNome', warningId: 'pmSubstituidoNomeWarning' },
            { id: 'pmSubstituidoIdentificacao', warningId: 'pmSubstituidoIdentificacaoWarning' },
            { id: 'pmSubstitutoNome', warningId: 'pmSubstitutoNomeWarning' },
            { id: 'pmSubstitutoIdentificacao', warningId: 'pmSubstitutoIdentificacaoWarning' },
        ];
        requiredFields.forEach(field => {
            document.getElementById(field.id).addEventListener('input', () => validateField(field.id, field.warningId));
        });
        
        function validateAllFields() {
            let isValid = true;
            isValid = validateBrasao() && isValid;
            isValid = validateLocalizacao() && isValid;
            isValid = validateField('pmSubstituidoNome', 'pmSubstituidoNomeWarning') && isValid;
            isValid = validateField('pmSubstituidoIdentificacao', 'pmSubstituidoIdentificacaoWarning') && isValid;
            isValid = validateField('pmSubstitutoNome', 'pmSubstitutoNomeWarning') && isValid;
            isValid = validateField('pmSubstitutoIdentificacao', 'pmSubstitutoIdentificacaoWarning') && isValid;
            isValid = validateDateRange('dataServicoPermutadoInicio', 'dataServicoPermutadoFim', 'dateWarning') && isValid;
            return isValid;
        }
        
        function formatarDataRange(startDateStr, endDateStr) {
            const dates = [];
            let currentDate = new Date(startDateStr + 'T00:00:00');
            const finalDate = new Date(endDateStr + 'T00:00:00');

            if (startDateStr && endDateStr) {
                while (currentDate <= finalDate) {
                    dates.push(new Date(currentDate));
                    currentDate.setDate(currentDate.getDate() + 1);
                }
            }
            
            if (dates.length === 0) {
                return '';
            }

            if (dates.length === 1) {
                const d = dates[0];
                return `${String(d.getDate()).padStart(2, '0')}/${String(d.getMonth() + 1).padStart(2, '0')}/${d.getFullYear()}`;
            }

            let result = '';
            let currentMonth = dates[0].getMonth();
            let currentYear = dates[0].getFullYear();
            let daysInGroup = [];
            
            for (let i = 0; i < dates.length; i++) {
                const d = dates[i];
                if (d.getMonth() === currentMonth && d.getFullYear() === currentYear) {
                    daysInGroup.push(String(d.getDate()).padStart(2, '0'));
                } else {
                    if (result !== '') {
                        result += ' e ';
                    }
                    result += `${daysInGroup.join(', ')}/${String(currentMonth + 1).padStart(2, '0')}`;
                    currentMonth = d.getMonth();
                    currentYear = d.getFullYear();
                    daysInGroup = [String(d.getDate()).padStart(2, '0')];
                }
            }
            if (daysInGroup.length > 0) {
                if (result !== '') {
                    result += ' e ';
                }
                result += `${daysInGroup.join(' e ')}/${String(currentMonth + 1).padStart(2, '0')}/${currentYear}`;
            }
            
            return result;
        }

        window.sendToCommander = async function() {
            if (!isAuthReady) {
                showCustomAlert("Aguarde...", "Aguarde a autenticação para enviar a solicitação.");
                return;
            }
            if (!validateAllFields()) {
                showCustomAlert("Erro de Validação", "Por favor, preencha todos os campos obrigatórios corretamente.");
                return;
            }

            const localizacao = document.querySelector('input[name="localizacao"]:checked').value;
            const pmSubstituidoNome = document.getElementById('pmSubstituidoNome').value.toUpperCase();
            const pmSubstituidoId = document.getElementById('pmSubstituidoIdentificacao').value;
            const pmSubstitutoNome = document.getElementById('pmSubstitutoNome').value.toUpperCase();
            const pmSubstitutoId = document.getElementById('pmSubstitutoIdentificacao').value;
            const servicoPermutadoInicio = document.getElementById('dataServicoPermutadoInicio').value;
            const servicoPermutadoFim = document.getElementById('dataServicoPermutadoFim').value;
            const pagamentoServicoInicio = document.getElementById('dataPagamentoServicoInicio').value;
            const pagamentoServicoFim = document.getElementById('dataPagamentoServicoFim').value;
            const noPagamento = document.getElementById('noPagamentoCheckbox').checked;

            const requestData = {
                localizacao: localizacao,
                pmSubstituido: { nome: pmSubstituidoNome, id: pmSubstituidoId },
                pmSubstituto: { nome: pmSubstitutoNome, id: pmSubstitutoId },
                servicoPermutado: { inicio: servicoPermutadoInicio, fim: servicoPermutadoFim },
                pagamentoServico: { inicio: pagamentoServicoInicio, fim: pagamentoServicoFim, naoHaPagamento: noPagamento },
                status: 'Pendente',
                userId: userId, // Adiciona o ID do usuário para rastreamento
                timestamp: new Date()
            };

            try {
                // Adiciona o documento no Firestore
                const docRef = await addDoc(collection(db, `/artifacts/${appId}/public/data/permuta_requests`), requestData);
                showCustomAlert("Sucesso!", "Sua solicitação de permuta foi enviada para o Comandante.");
                console.log("Documento escrito com ID: ", docRef.id);
            } catch (e) {
                console.error("Erro ao adicionar documento: ", e);
                showCustomAlert("Erro", "Não foi possível enviar a solicitação. Tente novamente.");
            }
        };

        window.printForm = function() {
            if (!validateAllFields()) {
                return;
            }
            
            // Coleta os dados do formulário
            const pmSubstituidoNome = document.getElementById('pmSubstituidoNome').value.toUpperCase();
            const pmSubstituidoId = document.getElementById('pmSubstituidoIdentificacao').value;
            const pmSubstitutoNome = document.getElementById('pmSubstitutoNome').value.toUpperCase();
            const pmSubstitutoId = document.getElementById('pmSubstitutoIdentificacao').value;
            const servicoPermutadoInicio = document.getElementById('dataServicoPermutadoInicio').value;
            const servicoPermutadoFim = document.getElementById('dataServicoPermutadoFim').value;
            const pagamentoServicoInicio = document.getElementById('dataPagamentoServicoInicio').value;
            const pagamentoServicoFim = document.getElementById('dataPagamentoServicoFim').value;
            const noPagamentoCheckbox = document.getElementById('noPagamentoCheckbox').checked;

            // Coleta a localização selecionada
            const localizacaoChecked = document.querySelector('input[name="localizacao"]:checked')?.value || ' ';
            let localizacaoSede = localizacaoChecked === 'SEDE' ? 'X' : '&nbsp;';
            let localizacaoTurilandia = localizacaoChecked === 'DPM TURILÂNDIA' ? 'X' : '&nbsp;';
            let localizacaoTuriacu = localizacaoChecked === 'DPM TURIAÇU' ? 'X' : '&nbsp;';
            
            // Coleta o tipo de serviço com base na quantidade de dias
            const diasServico = servicoPermutadoInicio && servicoPermutadoFim ? Math.ceil((new Date(servicoPermutadoFim) - new Date(servicoPermutadoInicio)) / (1000 * 60 * 60 * 24)) + 1 : 0;
            let tipo24Horas = '&nbsp;';
            let tipo48Horas = '&nbsp;';
            let tipo72Horas = '&nbsp;';

            if (diasServico === 1) {
                tipo24Horas = 'X';
            } else if (diasServico === 2) {
                tipo48Horas = 'X';
            } else if (diasServico >= 3) {
                tipo72Horas = 'X';
            }

            const servicoPermutadoTexto = formatarDataRange(servicoPermutadoInicio, servicoPermutadoFim);
            const pagamentoServicoTexto = formatarDataRange(pagamentoServicoInicio, pagamentoServicoFim);
            
            let pagamentoHtml = '';
            if (!noPagamentoCheckbox) {
                pagamentoHtml = `<p class="text-base mt-2">Data do pagamento do serviço: ${pagamentoServicoTexto}</p>`;
            }
            
            const brasaoUrlPrint = 'https://i.ibb.co/Xr6X43nG/BRAS-O.png';

            const printContent = `
                <!DOCTYPE html>
                <html lang="pt-BR">
                <head>
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <title>Formulário de Permuta - Impressão</title>
                    <style>
                        body {
                            font-family: 'Times New Roman', Times, serif;
                            margin: 1.5cm; /* Margens reduzidas para caber em uma página */
                            padding: 0;
                            line-height: 1.15;
                        }
                        .container {
                            width: 100%;
                            max-width: 800px;
                            margin: 0 auto;
                        }
                        .text-center { text-align: center; }
                        .text-sm { font-size: 0.875rem; }
                        .text-xs { font-size: 0.75rem; }
                        .text-base { font-size: 1rem; }
                        .mt-2 { margin-top: 0.5rem; }
                        .mt-4 { margin-top: 1rem; }
                        .mt-8 { margin-top: 2rem; }
                        .mb-6 { margin-bottom: 1.5rem; }
                        .font-bold { font-weight: bold; }
                    </style>
                </head>
                <body>
                    <div class="container">
                        <div class="text-center" style="line-height: 1.15; margin: 0;">
                            <img src="${brasaoUrlPrint}" alt="Brasão" 
                                style="width: 100px; height: 120px; display: block; margin: 0 auto 5px;">

                            <p class="font-bold" style="margin: 0;">ESTADO DO MARANHÃO</p>
                            <p class="text-sm" style="margin: 0;">SECRETARIA DE SEGURANÇA DE SEGURANÇA</p>
                            <p class="text-sm" style="margin: 0;">CPA/I-5 – 10º BPM</p>
                            <p class="text-sm" style="margin: 0;">10º BATALHÃO DA POLÍCIA MILITAR DO MARANHÃO</p>
                            <p class="text-sm" style="margin: 0;">2ª COMPANHIA DO 10° BATALHÃO DE POLÍCIA MILITAR</p>
                            <p class="text-xs" style="margin: 0;">Rua Dr. Paulo Ramos, s/nº, Centro, Santa Helena - MA, Telefax: (98) 99243-6850 - Email: 2cia10bpm@gmail.com</p>

                            <p class="font-bold" style="margin-top: 2rem; margin-bottom: 0;">FORMULÁRIO DE AUTORIZAÇÃO PARA PERMUTA DE SERVIÇO</p>
                        </div>
                    </div>
                    
                    <div class="mt-4 mb-4">
                        <p class="text-base">SEDE: ( ${localizacaoSede} )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DPM TURILÂNDIA: ( ${localizacaoTurilandia} )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DPM TURIAÇU: ( ${localizacaoTuriacu} )</p>
                    </div>
                    
                    <div style="margin-bottom: 0,25rem;">
                        <p class="text-base">PM SUBSTITUÍDO: ${pmSubstituidoNome} - ${pmSubstituidoId}</p>
                        <p style="margin-top: 4rem;">Assinatura:________________________________________</p>
                    </div>
                    
                    <div style="margin-bottom: 0,25rem;">
                        <p class="text-base">PM SUBSTITUTO: ${pmSubstitutoNome} - ${pmSubstitutoId}</p>
                        <p style="margin-top: 4rem;">Assinatura:________________________________________</p>
                    </div>
                    
                    <div class="mb-2">
                        <p class="text-base">Data do serviço permutado: ${servicoPermutadoTexto}</p>
                        ${pagamentoHtml}
                    </div>
                    
                    <div class="mb-4" style="text-align: left; margin-left: 20px;">
                        <p class="text-base">( ${tipo24Horas} ) 24 Horas - ( ${tipo48Horas} ) 48 Horas - ( ${tipo72Horas} ) 72 Horas</p>
                    </div>

                    <div class="text-center" style="margin-top: 2rem;">
                        <p class="text-base" style="margin: 0;">AUTORIZO A PERMUTA ENTRE OS POLICIAIS MILITARES ACIMA RELACIONADOS: (  ) Sim  (  ) Não</p>

                        <p class="text-base" style="margin-top: 3rem; margin-bottom: 0;">_____________________________________________________</p>
                        <p class="text-base" style="margin-top: 0.2rem; margin-bottom: 0;">JOSE RIBAMAR BRAGA JUNIOR - 1º TEN QOPM</p>
                        <p class="text-sm" style="margin-top: 0;">COMANDANTE DA 2°CP/10°BPM</p>
                    </div>
                </body>
                </html>
            `;

            // Abre uma nova janela para a impressão
            const printWindow = window.open('', '_blank');
            if (printWindow) {
                printWindow.document.open();
                printWindow.document.write(printContent);
                printWindow.document.close();
                printWindow.focus();
                printWindow.print();
            } else {
                // substitua o alerta por uma mensagem na tela
                const alertBox = document.createElement('div');
                alertBox.textContent = 'Por favor, habilite pop-ups para imprimir o formulário.';
                alertBox.style.cssText = 'position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: #ffcccc; padding: 20px; border: 1px solid #ff0000; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); text-align: center; z-index: 1000;';
                document.body.appendChild(alertBox);
                setTimeout(() => document.body.removeChild(alertBox), 5000);
            }
        }
        
        // Função de simulação para a assinatura digital do GOV.BR
        function signWithGovBr() {
            window.open('https://sso.acesso.gov.br/login?client_id=assinador.iti.br&authorization_id=198daa71163', '_blank');
        }

        // Função para renderizar uma solicitação
        function renderMyRequest(doc) {
            const request = doc.data();
            const card = document.createElement('div');
            card.className = `bg-gray-100 p-4 rounded-xl shadow-md border-2 border-gray-200`;
            card.innerHTML = `
                <div class="flex items-center justify-between mb-2">
                    <h4 class="text-base font-bold text-gray-800">Solicitação em ${new Date(request.timestamp.seconds * 1000).toLocaleString('pt-BR')}</h4>
                    <span class="px-3 py-1 rounded-full font-semibold text-xs status-${request.status}">
                        ${request.status}
                    </span>
                </div>
                <div class="text-sm text-gray-600">
                    <p><strong>Permuta de:</strong> ${request.pmSubstituido.nome} por ${request.pmSubstituto.nome}</p>
                    <p><strong>Serviço:</strong> ${formatarDataRange(request.servicoPermutado.inicio, request.servicoPermutado.fim)}</p>
                </div>
            `;
            myRequestsListElement.appendChild(card);
        }

        // Função para buscar e exibir as solicitações do usuário
        async function fetchMyRequests() {
            if (!isAuthReady) {
                console.log("Autenticação não finalizada, aguardando...");
                return;
            }

            myRequestsLoading.classList.remove('hidden');
            myRequestsListElement.innerHTML = '';
            myRequestsEmpty.classList.add('hidden');
            
            const requestsCollection = collection(db, `/artifacts/${appId}/public/data/permuta_requests`);
            const q = query(requestsCollection, where("userId", "==", userId));

            onSnapshot(q, (querySnapshot) => {
                myRequestsLoading.classList.add('hidden');
                myRequestsListElement.innerHTML = '';

                if (querySnapshot.empty) {
                    myRequestsEmpty.classList.remove('hidden');
                } else {
                    querySnapshot.forEach((doc) => {
                        renderMyRequest(doc);
                    });
                }
            }, (error) => {
                console.error("Erro ao carregar solicitações em tempo real: ", error);
                myRequestsLoading.classList.add('hidden');
                myRequestsListElement.innerHTML = `<p class="text-red-500 text-center mt-4">Erro ao carregar suas solicitações.</p>`;
            });
        }
    </script>
</body>
</html>
