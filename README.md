# jgr.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Examen PSP Interactivo - Modo Entrenamiento</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Animaciones simples */
        .fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .shake {
            animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
        }
        @keyframes shake {
            10%, 90% { transform: translate3d(-1px, 0, 0); }
            20%, 80% { transform: translate3d(2px, 0, 0); }
            30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
            40%, 60% { transform: translate3d(4px, 0, 0); }
        }
    </style>
</head>
<body class="bg-slate-100 min-h-screen font-sans text-gray-800">

    <div id="app" class="flex flex-col items-center justify-center min-h-screen p-4">
        <!-- El contenido se renderizará aquí dinámicamente -->
    </div>

    <script>
        // --- DATOS DEL EXAMEN ---
        const questions = [
            { id: 1, question: "En la programación segura, ¿cómo deben tratarse los archivos de la aplicación para evitar modificaciones no autorizadas?", options: [{key:'A', text:"Deben ser siempre archivos ocultos."}, {key:'B', text:"Deben ser archivos de solo lectura."}, {key:'C', text:"Deben tener permisos de ejecución para todos los usuarios."}, {key:'D', text:"Deben almacenarse en una base de datos sin cifrar."}], correct: 'B' },
            { id: 2, question: "¿Cuál es la fase del proceso de control de acceso en la que el sistema comprueba que el usuario es quien dice ser (ej. mediante contraseña)?", options: [{key:'A', text:"Identificación."}, {key:'B', text:"Autorización."}, {key:'C', text:"Autenticación."}, {key:'D', text:"Certificación."}], correct: 'C' },
            { id: 3, question: "¿Cómo se denomina al último paso del control de acceso, donde se ofrece la información o el recurso si el paso anterior tuvo éxito?", options: [{key:'A', text:"Identificación."}, {key:'B', text:"Autorización."}, {key:'C', text:"Autenticación."}, {key:'D', text:"Verificación."}], correct: 'B' },
            { id: 4, question: "¿Qué técnica de autenticación incluye la lectura de retina, huella dactilar o reconocimiento de voz?", options: [{key:'A', text:"Criptografía simétrica."}, {key:'B', text:"Biometría."}, {key:'C', text:"Firma digital."}, {key:'D', text:"Certificación digital."}], correct: 'B' },
            { id: 5, question: "En Java, ¿qué método de la clase Cipher realiza finalmente la encriptación o desencriptación de los datos?", options: [{key:'A', text:"init()"}, {key:'B', text:"getInstance()"}, {key:'C', text:"doFinal()"}, {key:'D', text:"update()"}], correct: 'C' },
            { id: 6, question: "¿Cuál es el propósito principal de las tres condiciones de Bernstein?", options: [{key:'A', text:"Determinar si dos instrucciones pueden ejecutarse concurrentemente."}, {key:'B', text:"Establecer una comunicación segura mediante sockets."}, {key:'C', text:"Generar claves públicas y privadas para RSA."}, {key:'D', text:"Sincronizar hilos mediante el uso de semáforos."}], correct: 'A' },
            { id: 7, question: "¿Qué técnica de programación divide un problema grande en otros más pequeños para ejecutarlos simultáneamente en equipos multiprocesadores?", options: [{key:'A', text:"Programación distribuida."}, {key:'B', text:"Programación paralela."}, {key:'C', text:"Programación iterativa."}, {key:'D', text:"Programación monohilo."}], correct: 'B' },
            { id: 8, question: "¿Qué son los archivos ejecutables desde el punto de vista del sistema operativo?", options: [{key:'A', text:"Archivos de texto que contienen el código fuente."}, {key:'B', text:"Archivos binarios que contienen instrucciones traducidas a lenguaje máquina."}, {key:'C', text:"Servicios que se ejecutan siempre en segundo plano."}, {key:'D', text:"Directorios que almacenan las librerías de Java."}], correct: 'B' },
            { id: 9, question: "¿Qué es un servicio en el contexto de los procesos informáticos?", options: [{key:'A', text:"Un proceso que requiere interfaz gráfica obligatoriamente."}, {key:'B', text:"Un proceso que se ejecuta en segundo plano y no es controlado directamente por el usuario."}, {key:'C', text:"Un conjunto de hilos que comparten memoria principal."}, {key:'D', text:"Un algoritmo de cifrado asimétrico basado en RSA."}], correct: 'B' },
            { id: 10, question: "¿Qué función se utiliza específicamente en sistemas Windows para la creación de un nuevo proceso?", options: [{key:'A', text:"fork()"}, {key:'B', text:"start()"}, {key:'C', text:"createProcess()"}, {key:'D', text:"execute()"}], correct: 'C' },
            { id: 11, question: "¿Qué efecto tiene invocar el método sleep() sobre un proceso o hilo?", options: [{key:'A', text:"Detiene su ejecución permanentemente."}, {key:'B', text:"Suspende o detiene el hilo/proceso durante una cantidad específica de milisegundos."}, {key:'C', text:"Termina el método run() de forma ordenada."}, {key:'D', text:"Cambia su estado de 'Bloqueado' a 'Ejecutable'."}], correct: 'B' },
            { id: 12, question: "Comparado con la creación de un proceso, ¿qué ventaja principal ofrecen los hilos?", options: [{key:'A', text:"Los hilos no comparten memoria, lo que es más seguro."}, {key:'B', text:"La creación de un hilo tiene un coste menor que la de un proceso."}, {key:'C', text:"Los hilos son totalmente independientes entre sí."}, {key:'D', text:"Un hilo tarda más tiempo en realizar el cambio de contexto."}], correct: 'B' },
            { id: 13, question: "¿Qué método NO se invoca para parar o bloquear un hilo, sino que se usa para despertar a hilos bloqueados?", options: [{key:'A', text:"sleep()"}, {key:'B', text:"wait()"}, {key:'C', text:"notify()"}, {key:'D', text:"suspend()"}], correct: 'C' },
            { id: 14, question: "¿En qué momento se considera que un hilo ha pasado al estado 'Muerto'?", options: [{key:'A', text:"Cuando se invoca el método yield()."}, {key:'B', text:"Cuando finaliza la ejecución de su método run()."}, {key:'C', text:"Cuando se invoca el método start()."}, {key:'D', text:"Cuando entra en una sección crítica synchronized."}], correct: 'B' },
            { id: 15, question: "¿En qué capa del modelo TCP/IP se ubican los protocolos HTTP, FTP y DNS?", options: [{key:'A', text:"Capa de Internet."}, {key:'B', text:"Capa de Transporte."}, {key:'C', text:"Capa de Aplicación."}, {key:'D', text:"Capa de Red."}], correct: 'C' },
            { id: 16, question: "¿Qué mecanismo permite la comunicación entre aplicaciones a través de la red abstrayendo al usuario de las capas inferiores?", options: [{key:'A', text:"Sockets."}, {key:'B', text:"RMI."}, {key:'C', text:"DatagramPacket."}, {key:'D', text:"Balanceadores de carga."}], correct: 'A' },
            { id: 17, question: "¿Qué protocolo de transporte se caracteriza por ser orientado a conexión y garantizar la fiabilidad del envío?", options: [{key:'A', text:"UDP."}, {key:'B', text:"TCP."}, {key:'C', text:"IP."}, {key:'D', text:"DHCP."}], correct: 'B' },
            { id: 18, question: "En la programación de sockets, ¿qué método vincula el socket a una dirección IP y un puerto determinado?", options: [{key:'A', text:"connect()"}, {key:'B', text:"accept()"}, {key:'C', text:"bind()"}, {key:'D', text:"listen()"}], correct: 'C' },
            { id: 19, question: "¿Qué función de la clase ServerSocket se queda bloqueada esperando a que un cliente realice una petición de conexión?", options: [{key:'A', text:"bind()"}, {key:'B', text:"accept()"}, {key:'C', text:"read()"}, {key:'D', text:"close()"}], correct: 'B' },
            { id: 20, question: "¿Qué protocolo configurado en el servidor permite asignar de manera dinámica una IP a cada cliente que se conecta?", options: [{key:'A', text:"DNS."}, {key:'B', text:"DHCP."}, {key:'C', text:"SMTP."}, {key:'D', text:"NFS."}], correct: 'B' },
            { id: 21, question: "¿Cuándo es más aconsejable hacer uso de un servidor concurrente en lugar de uno iterativo?", options: [{key:'A', text:"Cuando el servidor solo atiende una petición cada vez."}, {key:'B', text:"Cuando se debe soportar una gran cantidad de peticiones simultáneas."}, {key:'C', text:"Cuando se utiliza el protocolo UDP para procesos cortos."}, {key:'D', text:"Cuando no se requiere el uso de hilos en el servidor."}], correct: 'B' },
            { id: 22, question: "¿Qué técnica de comunicación basada en programación estructurada es conceptualmente parecida al RMI de la programación orientada a objetos?", options: [{key:'A', text:"SOAP."}, {key:'B', text:"RPC (Remote Procedure Call)."}, {key:'C', text:"REST."}, {key:'D', text:"SFTP."}], correct: 'B' },
            { id: 23, question: "¿Qué herramienta se encarga de distribuir las peticiones masivas entre distintos servidores para evitar colapsos?", options: [{key:'A', text:"Servidor dedicado."}, {key:'B', text:"Balanceadores de carga."}, {key:'C', text:"Réplicas de servidores."}, {key:'D', text:"Monitorización de red."}], correct: 'B' },
            { id: 24, question: "¿Cómo se denomina al servidor que es único para una empresa y no se comparte con terceros?", options: [{key:'A', text:"Servidor compartido."}, {key:'B', text:"Servidor espejo."}, {key:'C', text:"Servidor dedicado."}, {key:'D', text:"Servidor concurrente."}], correct: 'C' },
            { id: 25, question: "¿Cuántos bits genera un resumen mediante el algoritmo de hash MD5?", options: [{key:'A', text:"160 bits."}, {key:'B', text:"128 bits."}, {key:'C', text:"256 bits."}, {key:'D', text:"64 bits."}], correct: 'B' },
            { id: 26, question: "El algoritmo RSA basa su seguridad en la dificultad de resolver el problema de:", options: [{key:'A', text:"La sustitución de caracteres."}, {key:'B', text:"La factorización de números enteros."}, {key:'C', text:"La exclusión mutua."}, {key:'D', text:"La condición de carrera."}], correct: 'B' },
            { id: 27, question: "¿Qué puerto utiliza por defecto el protocolo SSH para la comunicación segura?", options: [{key:'A', text:"80"}, {key:'B', text:"443"}, {key:'C', text:"22"}, {key:'D', text:"21"}], correct: 'C' },
            { id: 28, question: "¿Qué problema ocurre cuando dos procesos se bloquean mutuamente esperando recursos que tiene el otro?", options: [{key:'A', text:"Inanición."}, {key:'B', text:"Exclusión mutua."}, {key:'C', text:"Abrazo mortal (Deadlock)."}, {key:'D', text:"Inconsistencia de datos."}], correct: 'C' },
            { id: 29, question: "¿Qué comando se utiliza en Linux para mostrar los procesos activos en el sistema?", options: [{key:'A', text:"ls -f"}, {key:'B', text:"ps -f"}, {key:'C', text:"taskset"}, {key:'D', text:"nice"}], correct: 'B' },
            { id: 30, question: "¿Cuál es el rango de prioridad de un hilo en el lenguaje Java?", options: [{key:'A', text:"Entre 0 y 100."}, {key:'B', text:"Entre 1 y 10."}, {key:'C', text:"Entre -20 y 20."}, {key:'D', text:"Entre 1 y 5."}], correct: 'B' },
            { id: 31, question: "¿Cuál es el primer mensaje enviado por un cliente en el 'Three-way handshake' de TCP para establecer conexión?", options: [{key:'A', text:"ACK"}, {key:'B', text:"FIN"}, {key:'C', text:"SYN"}, {key:'D', text:"SYN-ACK"}], correct: 'C' },
            { id: 32, question: "¿Qué lenguaje utiliza obligatoriamente el protocolo SOAP para la comunicación entre objetos?", options: [{key:'A', text:"JSON"}, {key:'B', text:"HTML"}, {key:'C', text:"XML"}, {key:'D', text:"Texto plano"}], correct: 'C' },
            { id: 33, question: "¿Cuáles son las operaciones estándares permitidas en los servicios web tipo REST?", options: [{key:'A', text:"SYN, ACK, FIN."}, {key:'B', text:"POST, GET, PUT y DELETE."}, {key:'C', text:"START, RUN, SLEEP y STOP."}, {key:'D', text:"BIND, ACCEPT, SEND y RECEIVE."}], correct: 'B' },
            { id: 34, question: "¿Qué librería externa de Java se utiliza frecuentemente para programar clientes que suben archivos a servidores FTP?", options: [{key:'A', text:"Java Standard API"}, {key:'B', text:"Apache Commons Net"}, {key:'C', text:"Google Guava"}, {key:'D', text:"RMI Registry"}], correct: 'B' },
            { id: 35, question: "¿Cómo se denomina a un proceso que ha finalizado pero no ha liberado sus recursos en un sistema Unix?", options: [{key:'A', text:"Huérfano"}, {key:'B', text:"Zombie"}, {key:'C', text:"Activo"}, {key:'D', text:"Bloqueado"}], correct: 'B' },
            { id: 36, question: "¿Qué palabra clave de Java se utiliza para marcar un objeto como ocupado y evitar que varios hilos lo usen a la vez?", options: [{key:'A', text:"volatile"}, {key:'B', text:"synchronized"}, {key:'C', text:"static"}, {key:'D', text:"final"}], correct: 'B' },
            { id: 37, question: "¿Qué clase de Java representa una dirección IP?", options: [{key:'A', text:"URL"}, {key:'B', text:"InetAddress"}, {key:'C', text:"Socket"}, {key:'D', text:"ServerSocket"}], correct: 'B' },
            { id: 38, question: "La capacidad de que un servidor cuente con réplicas para que, en caso de fallo, otro servidor procese los servicios se conoce como:", options: [{key:'A', text:"Balanceo de carga"}, {key:'B', text:"Alta disponibilidad"}, {key:'C', text:"Programación paralela"}, {key:'D', text:"Encadenamiento de hilos"}], correct: 'B' },
            { id: 39, question: "¿Qué puerto utiliza el protocolo HTTPS por defecto?", options: [{key:'A', text:"80"}, {key:'B', text:"22"}, {key:'C', text:"443"}, {key:'D', text:"54321"}], correct: 'C' },
            { id: 40, question: "Para que un sistema sea seguro según la política de seguridad, ¿qué característica asegura que el emisor no niegue el envío?", options: [{key:'A', text:"Confidencialidad"}, {key:'B', text:"No repudio en origen"}, {key:'C', text:"Disponibilidad"}, {key:'D', text:"Integridad"}], correct: 'B' },
            { id: 41, question: "¿Qué método de la clase Thread se encarga de comenzar la ejecución de un hilo tras su inicialización?", options: [{key:'A', text:"start()"}, {key:'B', text:"run()"}, {key:'C', text:"init()"}, {key:'D', text:"execute()"}], correct: 'A' },
            { id: 42, question: "¿Qué técnica se utiliza para dividir un mensaje en paquetes en el nivel de transporte del modelo TCP/IP?", options: [{key:'A', text:"Encapsulación"}, {key:'B', text:"Fragmentación en paquetes"}, {key:'C', text:"Handshake"}, {key:'D', text:"Invocación remota"}], correct: 'B' }
        ];

        // --- ICONOS SVG (Inline) ---
        const Icons = {
            award: `<svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="7"/><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"/></svg>`,
            rotateCcw: `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/></svg>`,
            checkCircle: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>`,
            xCircle: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></svg>`,
            chevronLeft: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></svg>`,
            chevronRight: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"/></svg>`,
            alertCircle: `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>`
        };

        // --- ESTADO ---
        let state = {
            currentQuestion: 0,
            selectedAnswers: {},
            showResults: false
        };

        // --- FUNCIONES LÓGICAS ---
        function selectOption(key) {
            // Si ya hay respuesta para esta pregunta, no hacer nada (bloqueo)
            if (state.selectedAnswers[state.currentQuestion]) return;
            
            state.selectedAnswers[state.currentQuestion] = key;
            render();
        }

        function nextQuestion() {
            if (state.currentQuestion < questions.length - 1) {
                state.currentQuestion++;
            } else {
                state.showResults = true;
            }
            render();
        }

        function prevQuestion() {
            if (state.currentQuestion > 0) {
                state.currentQuestion--;
            }
            render();
        }

        function restartExam() {
            state.selectedAnswers = {};
            state.currentQuestion = 0;
            state.showResults = false;
            render();
        }

        function calculateScore() {
            let score = 0;
            questions.forEach((q, index) => {
                if (state.selectedAnswers[index] === q.correct) {
                    score++;
                }
            });
            return score;
        }

        // --- RENDERIZADO ---
        function render() {
            const app = document.getElementById('app');
            app.innerHTML = ''; // Limpiar

            if (state.showResults) {
                renderResults(app);
            } else {
                renderQuestion(app);
            }
        }

        function renderResults(container) {
            const score = calculateScore();
            const percentage = ((score / questions.length) * 100).toFixed(1);
            const isPass = Number(percentage) >= 50;

            let reviewHtml = '';
            questions.forEach((q, index) => {
                const userAnswer = state.selectedAnswers[index];
                const isCorrect = userAnswer === q.correct;
                const userOptionText = userAnswer ? q.options.find(o => o.key === userAnswer).text : 'Sin responder';
                const correctOptionText = q.options.find(o => o.key === q.correct).text;

                reviewHtml += `
                    <div class="p-4 rounded-lg border ${isCorrect ? 'bg-green-50 border-green-200' : 'bg-red-50 border-red-200'}">
                        <div class="flex items-start">
                            <div class="mt-1 mr-3 flex-shrink-0 ${isCorrect ? 'text-green-600' : 'text-red-600'}">
                                ${isCorrect ? Icons.checkCircle : Icons.xCircle}
                            </div>
                            <div class="w-full">
                                <p class="font-medium text-gray-900 mb-2">${q.id}. ${q.question}</p>
                                <div class="text-sm space-y-1">
                                    <p class="${isCorrect ? 'text-green-700' : 'text-red-700'}">
                                        <span class="font-semibold">Tu respuesta:</span> ${userAnswer ? `${userAnswer}) ${userOptionText}` : 'Sin responder'}
                                    </p>
                                    ${!isCorrect ? `
                                        <p class="text-green-700">
                                            <span class="font-semibold">Respuesta correcta:</span> ${q.correct}) ${correctOptionText}
                                        </p>
                                    ` : ''}
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            });

            container.innerHTML = `
                <div class="w-full max-w-3xl bg-white rounded-xl shadow-lg overflow-hidden fade-in">
                    <div class="bg-indigo-600 p-6 text-white text-center">
                        <div class="mx-auto mb-4 text-white w-16 h-16 flex justify-center items-center">${Icons.award}</div>
                        <h1 class="text-3xl font-bold mb-2">Resultados Finales</h1>
                        <p class="text-indigo-100">Programación de Servicios y Procesos</p>
                    </div>
                    
                    <div class="p-8 text-center border-b border-gray-200">
                        <div class="text-5xl font-bold text-gray-800 mb-2">${score} / ${questions.length}</div>
                        <div class="text-xl font-semibold ${isPass ? 'text-green-600' : 'text-red-500'}">
                            ${percentage}% - ${isPass ? 'Aprobado' : 'Suspenso'}
                        </div>
                        <button onclick="restartExam()" class="mt-6 inline-flex items-center justify-center px-6 py-2 bg-indigo-600 text-white rounded-full hover:bg-indigo-700 transition-colors">
                            <span class="mr-2">${Icons.rotateCcw}</span> Intentar de nuevo
                        </button>
                    </div>

                    <div class="p-6 bg-gray-50">
                        <h3 class="text-xl font-bold text-gray-800 mb-4">Revisión Completa</h3>
                        <div class="space-y-4">
                            ${reviewHtml}
                        </div>
                    </div>
                </div>
            `;
        }

        function renderQuestion(container) {
            const currentQ = questions[state.currentQuestion];
            const progress = ((state.currentQuestion + 1) / questions.length) * 100;
            const userAnswer = state.selectedAnswers[state.currentQuestion];
            const hasAnswered = userAnswer !== undefined;
            const isCorrect = hasAnswered && userAnswer === currentQ.correct;

            let optionsHtml = '';
            let feedbackHtml = '';

            currentQ.options.forEach(option => {
                const isSelected = userAnswer === option.key;
                const isCorrectOption = option.key === currentQ.correct;
                
                // Determinar estilos basados en si ya se respondió
                let containerClass = "border-gray-200 hover:border-indigo-300 hover:bg-gray-50 cursor-pointer";
                let circleClass = "bg-white text-gray-500 border-gray-300 group-hover:border-indigo-400";
                let iconHtml = '';

                if (hasAnswered) {
                    containerClass = "cursor-default"; // Quitar cursor pointer
                    
                    if (isCorrectOption) {
                        // Esta es la correcta: verde siempre
                        containerClass = "border-green-500 bg-green-50 text-green-900";
                        circleClass = "bg-green-500 text-white border-green-500";
                        iconHtml = `<div class="ml-auto text-green-600">${Icons.checkCircle}</div>`;
                    } else if (isSelected) {
                        // Esta es la que seleccionaste y es INCORRECTA: rojo
                        containerClass = "border-red-500 bg-red-50 text-red-900 shake";
                        circleClass = "bg-red-500 text-white border-red-500";
                        iconHtml = `<div class="ml-auto text-red-600">${Icons.xCircle}</div>`;
                    } else {
                        // Otras opciones irrelevantes: atenuadas
                        containerClass = "border-gray-100 bg-gray-50 text-gray-400 opacity-60";
                        circleClass = "bg-gray-100 text-gray-400 border-gray-200";
                    }
                } else if (isSelected) {
                    // Estado seleccionado (aunque ahora es inmediato, esto cubre transiciones)
                     containerClass = 'border-indigo-600 bg-indigo-50 text-indigo-900';
                     circleClass = 'bg-indigo-600 text-white border-indigo-600';
                }

                optionsHtml += `
                    <button onclick="selectOption('${option.key}')" 
                        class="w-full text-left p-4 rounded-xl border-2 transition-all duration-200 flex items-center group mb-3 ${containerClass}"
                        ${hasAnswered ? 'disabled' : ''}>
                        <span class="w-8 h-8 rounded-full flex items-center justify-center mr-4 border text-sm font-bold flex-shrink-0 ${circleClass}">
                            ${option.key}
                        </span>
                        <span class="text-base flex-grow">${option.text}</span>
                        ${iconHtml}
                    </button>
                `;
            });

            // Mensaje de feedback inmediato
            if (hasAnswered) {
                if (isCorrect) {
                    feedbackHtml = `
                        <div class="mt-4 p-4 bg-green-100 text-green-800 rounded-lg flex items-center fade-in border border-green-200">
                            <span class="mr-2">${Icons.checkCircle}</span> 
                            <strong>¡Correcto!</strong>
                        </div>`;
                } else {
                    feedbackHtml = `
                        <div class="mt-4 p-4 bg-red-100 text-red-800 rounded-lg flex items-center fade-in border border-red-200">
                            <span class="mr-2">${Icons.xCircle}</span>
                            <div>
                                <strong>Incorrecto.</strong> La respuesta correcta era la <strong>${currentQ.correct}</strong>.
                            </div>
                        </div>`;
                }
            }

            container.innerHTML = `
                <div class="w-full max-w-2xl bg-white rounded-2xl shadow-xl overflow-hidden fade-in">
                    <!-- Header -->
                    <div class="bg-white border-b border-gray-100 p-6 flex justify-between items-center">
                        <div>
                            <h2 class="text-xl font-bold text-gray-800">Test de PSP</h2>
                            <p class="text-sm text-gray-500">Modo Entrenamiento</p>
                        </div>
                        <div class="text-sm font-semibold text-indigo-600 bg-indigo-50 px-3 py-1 rounded-full">
                            ${state.currentQuestion + 1} / ${questions.length}
                        </div>
                    </div>

                    <!-- Progress Bar -->
                    <div class="w-full bg-gray-100 h-2">
                        <div class="bg-indigo-600 h-2 transition-all duration-300 ease-in-out" style="width: ${progress}%"></div>
                    </div>

                    <!-- Question Area -->
                    <div class="p-8 min-h-[400px]">
                        <h3 class="text-xl font-medium text-gray-900 mb-6 leading-relaxed">
                            ${currentQ.id}. ${currentQ.question}
                        </h3>

                        <div class="space-y-3">
                            ${optionsHtml}
                        </div>
                        
                        ${feedbackHtml}
                    </div>

                    <!-- Footer Navigation -->
                    <div class="bg-gray-50 p-6 flex justify-between items-center border-t border-gray-100">
                        <button onclick="prevQuestion()" ${state.currentQuestion === 0 ? 'disabled' : ''} 
                            class="flex items-center px-4 py-2 rounded-lg font-medium transition-colors ${state.currentQuestion === 0 ? 'text-gray-300 cursor-not-allowed' : 'text-gray-600 hover:bg-gray-200 hover:text-gray-900'}">
                            <span class="mr-1">${Icons.chevronLeft}</span> Anterior
                        </button>

                        <button onclick="nextQuestion()" ${!hasAnswered ? 'disabled' : ''}
                            class="flex items-center px-6 py-2 rounded-lg font-bold text-white transition-all transform active:scale-95 shadow-md
                            ${!hasAnswered ? 'bg-gray-300 cursor-not-allowed shadow-none' : 'bg-indigo-600 hover:bg-indigo-700 hover:shadow-lg'}">
                            ${state.currentQuestion === questions.length - 1 ? 'Ver Nota Final' : 'Siguiente'} <span class="ml-1">${Icons.chevronRight}</span>
                        </button>
                    </div>
                </div>
            `;
        }

        // Inicializar
        render();

    </script>
</body>
</html>
