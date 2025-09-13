<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal de Questões com IA</title>
    
    <!-- Carrega o Tailwind CSS para estilização -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Carrega as bibliotecas React e ReactDOM -->
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    
    <!-- Carrega o Babel para transpilar o JSX em JavaScript no navegador -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <!-- Bibliotecas para gerar PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <style>
        /* Estilo para a fonte e transições suaves */
        body {
            font-family: 'Inter', sans-serif;
        }
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

        /* Estilo para evitar quebra de página dentro de um item do relatório ao imprimir/gerar pdf */
        .report-item {
            break-inside: avoid;
        }
    </style>
</head>
<body class="bg-gray-100">
    
    <!-- Onde a aplicação React será renderizada -->
    <div id="root"></div>

    <!-- Script com o código da sua aplicação React -->
    <script type="text/babel">
        const { useState } = React;

        // =================================================================
        // IMPORTANTE: Insira sua chave de API do Google AI Studio aqui
        // Obtenha sua chave em: https://aistudio.google.com/app/apikey
        // =================================================================
        const YOUR_API_KEY = "AIzaSyCSFgX8V5F9Mg472kZoRsr1tRlZngqFHhU";
        // =================================================================

        // Função para chamar a API do Gemini e gerar questões
        async function generateQuestionsWithAI(topic, numQuestions, difficulty) {
          const systemPrompt = `Você é um assistente especialista em criar quizzes. Sua tarefa é gerar ${numQuestions} questões de múltipla escolha sobre um determinado tema, com um nível de dificuldade ${difficulty}.
            As questões devem ser claras, objetivas e ter 4 opções de resposta, com apenas uma correta.
            Para cada questão, inclua um campo 'explanation' com um breve resumo (1-2 frases) explicando por que a resposta correta está certa.
            Responda SEMPRE no formato JSON solicitado, sem nenhuma outra formatação ou texto adicional.`;
          
          const userQuery = `Gere ${numQuestions} questões sobre o tema: "${topic}", no nível de dificuldade ${difficulty}.`;

          const apiKey = YOUR_API_KEY; 
          const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

          const payload = {
            contents: [{ parts: [{ text: userQuery }] }],
            systemInstruction: {
              parts: [{ text: systemPrompt }]
            },
            generationConfig: {
              responseMimeType: "application/json",
              responseSchema: {
                type: "ARRAY",
                items: {
                  type: "OBJECT",
                  properties: {
                    question: { type: "STRING" },
                    options: {
                      type: "ARRAY",
                      items: { type: "STRING" }
                    },
                    answer: { type: "STRING" },
                    explanation: { type: "STRING" }
                  },
                  required: ["question", "options", "answer", "explanation"]
                }
              }
            }
          };

          let response;
          let retries = 3;
          let delay = 1000;
          while (retries > 0) {
            try {
              response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
              });
              if (response.ok) {
                const result = await response.json();
                const jsonText = result.candidates[0].content.parts[0].text;
                return JSON.parse(jsonText);
              } else {
                throw new Error(`API retornou o status: ${response.status}`);
              }
            } catch (error) {
              console.error("Erro na tentativa de chamada da API:", error);
            }
            retries--;
            if (retries > 0) {
              await new Promise(resolve => setTimeout(resolve, delay));
              delay *= 2;
            }
          }
          throw new Error("Falha ao gerar questões após várias tentativas.");
        }


        // --- Ícones ---
        const CheckIcon = ({ className = "h-6 w-6 text-green-500" }) => (
          <svg xmlns="http://www.w3.org/2000/svg" className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M5 13l4 4L19 7" />
          </svg>
        );

        const XIcon = ({ className = "h-6 w-6 text-red-500" }) => (
          <svg xmlns="http://www.w3.org/2000/svg" className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
          </svg>
        );

        const LoadingSpinner = () => (
            <div className="flex flex-col items-center justify-center space-y-4">
                <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"></div>
                <p className="text-gray-600 font-medium">Gerando questões com IA... Por favor, aguarde.</p>
            </div>
        );


        // --- Componente Principal ---
        function App() {
          const [gameState, setGameState] = useState('start'); // 'start', 'playing', 'finished', 'report'
          const [topic, setTopic] = useState('');
          const [difficulty, setDifficulty] = useState('Médio');
          const [numQuestions, setNumQuestions] = useState(5);
          const [questions, setQuestions] = useState([]);
          const [currentQuestionIndex, setCurrentQuestionIndex] = useState(0);
          const [userAnswers, setUserAnswers] = useState({});
          const [score, setScore] = useState(0);
          const [selectedAnswer, setSelectedAnswer] = useState(null);
          const [isAnswered, setIsAnswered] = useState(false);
          const [error, setError] = useState('');
          const [isLoading, setIsLoading] = useState(false);
          const [isDownloading, setIsDownloading] = useState(false);

          const startQuiz = async () => {
            if (YOUR_API_KEY === "COLE_SUA_CHAVE_DE_API_AQUI") {
                setError("Erro de configuração: Por favor, insira uma chave de API válida no código.");
                return;
            }
            if (!topic.trim()) {
                setError("Por favor, digite um tema.");
                return;
            }
            setIsLoading(true);
            setError('');
            try {
                const questionsFromAI = await generateQuestionsWithAI(topic, numQuestions, difficulty);
                if (questionsFromAI && questionsFromAI.length > 0) {
                    setQuestions(questionsFromAI);
                    setGameState('playing');
                    setCurrentQuestionIndex(0);
                    setUserAnswers({});
                    setScore(0);
                    setSelectedAnswer(null);
                    setIsAnswered(false);
                } else {
                    throw new Error("A IA não retornou questões válidas.");
                }
            } catch (err) {
                console.error(err);
                setError(`Desculpe, ocorreu um erro ao gerar as questões sobre "${topic}". Tente novamente.`);
                setGameState('start');
            } finally {
                setIsLoading(false);
            }
          };
          
          const handleAnswerSelect = (option) => {
            if (isAnswered) return;
            setSelectedAnswer(option);
            setIsAnswered(true);
            const currentQuestion = questions[currentQuestionIndex];
            const isCorrect = option === currentQuestion.answer;
            setUserAnswers({ ...userAnswers, [currentQuestionIndex]: { answer: option, isCorrect } });
            if (isCorrect) setScore(prevScore => prevScore + 1);
          };

          const handleNextQuestion = () => {
            if (currentQuestionIndex < questions.length - 1) {
              setCurrentQuestionIndex(prevIndex => prevIndex + 1);
              setSelectedAnswer(null);
              setIsAnswered(false);
            } else {
              setGameState('finished');
            }
          };

          const restartQuiz = () => {
            setGameState('start');
            setTopic('');
            setError('');
            setQuestions([]);
          };

          const handleDownloadPdf = async () => {
              const { jsPDF } = window.jspdf;
              const reportElement = document.getElementById("report-content");
              if (!reportElement) return;

              setIsDownloading(true);

              try {
                  const canvas = await html2canvas(reportElement, {
                      scale: 2, // Aumenta a resolução para melhor qualidade
                      useCORS: true,
                      backgroundColor: '#ffffff',
                  });

                  const imgData = canvas.toDataURL('image/png');
                  
                  const pdf = new jsPDF('p', 'mm', 'a4');
                  const pdfWidth = pdf.internal.pageSize.getWidth();
                  const pdfHeight = pdf.internal.pageSize.getHeight();
                  
                  const imgWidth = canvas.width;
                  const imgHeight = canvas.height;

                  const ratio = imgWidth / imgHeight;
                  let newImgWidth = pdfWidth - 20; // com margem de 10mm de cada lado
                  let newImgHeight = newImgWidth / ratio;
                  
                  // Se a imagem for muito alta, recalcula para caber na altura
                  if (newImgHeight > pdfHeight - 20) {
                      newImgHeight = pdfHeight - 20;
                      newImgWidth = newImgHeight * ratio;
                  }

                  const positionX = (pdfWidth - newImgWidth) / 2; // Centraliza
                  const positionY = 10;
                  
                  pdf.addImage(imgData, 'PNG', positionX, positionY, newImgWidth, newImgHeight);
                  pdf.save(`relatorio-${topic.replace(/\s+/g, '-')}.pdf`);

              } catch (error) {
                  console.error("Erro ao gerar PDF:", error);
                  alert("Desculpe, ocorreu um erro ao gerar o PDF.");
              } finally {
                  setIsDownloading(false);
              }
          };
          
          const getButtonClass = (option) => {
            if (!isAnswered) return "bg-white hover:bg-blue-100 text-blue-800";
            const currentQuestion = questions[currentQuestionIndex];
            if (option === currentQuestion.answer) return "bg-green-200 text-green-800 border-green-500";
            if (option === selectedAnswer) return "bg-red-200 text-red-800 border-red-500";
            return "bg-white text-blue-800 opacity-60";
          };

          return (
            <div className="min-h-screen font-sans flex items-center justify-center p-4">
              <div className="w-full max-w-2xl mx-auto bg-white rounded-2xl shadow-lg p-6 md:p-8 transition-all duration-500">
                
                {isLoading ? <LoadingSpinner /> : (
                  <>
                    {gameState === 'start' && (
                      <div className="text-center">
                        <h1 className="text-3xl md:text-4xl font-bold text-gray-800 mb-2">Portal de Questões IA</h1>
                        <p className="text-gray-600 mb-6">Digite um tema, escolha as opções e nossa IA criará um quiz para você.</p>
                        
                        <div className="mb-4">
                          <input type="text" value={topic} onChange={(e) => setTopic(e.target.value)} placeholder="Digite o tema aqui..." className="w-full p-3 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition" />
                        </div>

                        <div className="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6 text-left">
                            <div>
                                <label htmlFor="difficulty" className="block text-sm font-medium text-gray-700 mb-1">Nível de Dificuldade</label>
                                <select id="difficulty" value={difficulty} onChange={(e) => setDifficulty(e.target.value)} className="w-full p-3 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition">
                                    <option>Fácil</option>
                                    <option>Médio</option>
                                    <option>Difícil</option>
                                </select>
                            </div>
                            <div>
                                <label htmlFor="numQuestions" className="block text-sm font-medium text-gray-700 mb-1">Nº de Questões</label>
                                <input type="number" id="numQuestions" value={numQuestions} onChange={(e) => setNumQuestions(e.target.value)} min="1" max="10" className="w-full p-3 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition" />
                            </div>
                        </div>

                        <button onClick={startQuiz} disabled={!topic || isLoading} className="w-full px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 disabled:bg-gray-400 disabled:cursor-not-allowed transition-colors">Gerar Questões</button>
                        
                        <p className="text-xs text-gray-400 mt-4">As questões são geradas por inteligência artificial e podem conter imprecisões.</p>
                        {error && <p className="text-red-500 mt-4 font-medium">{error}</p>}
                      </div>
                    )}

                    {gameState === 'playing' && questions.length > 0 && (
                      <div>
                        <div className="flex justify-between items-center mb-4">
                          <p className="text-sm text-gray-500">Questão {currentQuestionIndex + 1} de {questions.length}</p>
                          <button onClick={restartQuiz} className="text-sm text-gray-500 hover:text-blue-600 font-semibold transition-colors">Voltar ao Início</button>
                        </div>
                        <h2 className="text-xl md:text-2xl font-semibold text-gray-800 mb-6">{questions[currentQuestionIndex].question}</h2>
                        <div className="space-y-3">
                          {questions[currentQuestionIndex].options.map((option, index) => (
                            <button key={index} onClick={() => handleAnswerSelect(option)} disabled={isAnswered} className={`w-full text-left p-4 font-medium rounded-lg border-2 transition-all duration-300 flex justify-between items-center ${getButtonClass(option)}`}>
                              <span>{option}</span>
                              {isAnswered && option === selectedAnswer && option === questions[currentQuestionIndex].answer && <CheckIcon />}
                              {isAnswered && option === selectedAnswer && option !== questions[currentQuestionIndex].answer && <XIcon />}
                            </button>
                          ))}
                        </div>
                        {isAnswered && (
                          <div className="mt-6 text-center">
                            <div className="bg-gray-100 p-4 rounded-lg text-left mb-6">
                                <h3 className="font-bold text-gray-700 mb-1">Resumo:</h3>
                                <p className="text-gray-600">{questions[currentQuestionIndex].explanation}</p>
                            </div>
                             <button onClick={handleNextQuestion} className="w-full sm:w-auto px-8 py-3 bg-green-600 text-white font-semibold rounded-lg shadow-md hover:bg-green-700 transition-colors">
                                {currentQuestionIndex < questions.length - 1 ? 'Próxima Questão' : 'Ver Resultado'}
                              </button>
                          </div>
                        )}
                      </div>
                    )}

                    {gameState === 'finished' && (
                      <div className="text-center">
                        <h2 className="text-3xl font-bold text-gray-800 mb-3">Quiz Finalizado!</h2>
                        <p className="text-xl text-gray-600 mb-6">Você acertou <span className="font-bold text-blue-600">{score}</span> de <span className="font-bold text-blue-600">{questions.length}</span> questões.</p>
                        <div className="mt-8 flex flex-col sm:flex-row justify-center gap-3">
                            <button onClick={startQuiz} className="px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition-colors">Gerar Novas Questões</button>
                            <button onClick={() => setGameState('report')} className="px-6 py-3 bg-gray-700 text-white font-semibold rounded-lg shadow-md hover:bg-gray-800 transition-colors">Ver Relatório</button>
                            <button onClick={restartQuiz} className="px-6 py-3 bg-gray-200 text-gray-800 font-semibold rounded-lg shadow-md hover:bg-gray-300 transition-colors">Voltar ao Início</button>
                        </div>
                      </div>
                    )}

                    {gameState === 'report' && (
                        <div>
                            <div id="report-content" className="text-left bg-white p-4">
                              <h2 className="text-2xl md:text-3xl font-bold text-gray-800 mb-2 text-center">Relatório de Desempenho</h2>
                              <p className="text-center text-gray-600 mb-1">Tema: <span className="font-semibold">{topic}</span></p>
                              <p className="text-center text-gray-600 mb-6">Pontuação Final: <span className="font-bold">{score} / {questions.length}</span> ({questions.length > 0 ? Math.round((score / questions.length) * 100) : 0}%)</p>
                              
                              <div className="space-y-4">
                                  {questions.map((q, index) => {
                                      const userAnswer = userAnswers[index];
                                      return (
                                          <div key={index} className="p-4 rounded-lg bg-gray-50 border report-item">
                                              <p className="font-semibold text-gray-800">{index + 1}. {q.question}</p>
                                              <div className={`mt-2 flex items-center gap-2 ${userAnswer.isCorrect ? 'text-green-700' : 'text-red-700'}`}>
                                                  {userAnswer.isCorrect ? <CheckIcon className="h-5 w-5"/> : <XIcon className="h-5 w-5"/>}
                                                  <span>Sua resposta: {userAnswer.answer}</span>
                                              </div>
                                              {!userAnswer.isCorrect && (
                                                  <p className="mt-1 text-gray-600 pl-7">Resposta correta: {q.answer}</p>
                                              )}
                                              <div className="mt-2 pt-2 border-t border-gray-200">
                                                  <p className="text-sm text-gray-700"><span className="font-semibold">Resumo:</span> {q.explanation}</p>
                                              </div>
                                          </div>
                                      );
                                  })}
                              </div>
                            </div>

                            <div className="mt-8 flex flex-col sm:flex-row justify-center gap-3">
                                <button onClick={handleDownloadPdf} disabled={isDownloading} className="px-6 py-3 bg-teal-600 text-white font-semibold rounded-lg shadow-md hover:bg-teal-700 transition-colors disabled:bg-gray-400">
                                  {isDownloading ? 'Baixando...' : 'Baixar Relatório (PDF)'}
                                </button>
                                <button onClick={startQuiz} className="px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition-colors">Gerar Novas Questões</button>
                                <button onClick={restartQuiz} className="px-6 py-3 bg-gray-200 text-gray-800 font-semibold rounded-lg shadow-md hover:bg-gray-300 transition-colors">Voltar ao Início</button>
                            </div>
                        </div>
                    )}
                  </>
                )}
              </div>
            </div>
          );
        }

        // Renderiza o componente App na div com id 'root'
        const container = document.getElementById('root');
        const root = ReactDOM.createRoot(container);
        root.render(<App />);
    </script>
</body>
</html>
