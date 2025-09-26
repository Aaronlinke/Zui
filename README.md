<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Metropolis AI System - Funktionale Oberfläche</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a3e 50%, #2d1b69 100%);
            color: #fff;
            min-height: 100vh;
        }

        .header {
            background: rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            padding: 1rem 2rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header h1 {
            font-size: 2rem;
            background: linear-gradient(45deg, #00d4ff, #7c3aed, #ec4899);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            display: inline-block;
        }

        .subtitle {
            color: rgba(255, 255, 255, 0.7);
            font-size: 0.9rem;
            margin-top: 0.5rem;
        }

        .main-container {
            display: grid;
            grid-template-columns: 350px 1fr 400px;
            height: calc(100vh - 120px);
            gap: 1rem;
            padding: 1rem;
        }

        .sidebar {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            overflow-y: auto;
        }

        .main-content {
            display: grid;
            grid-template-rows: 1fr 300px;
            gap: 1rem;
        }

        .code-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
        }

        .results-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
        }

        .chat-panel {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
        }

        .section-title {
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #00d4ff;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: rgba(255, 255, 255, 0.9);
        }

        .form-input {
            width: 100%;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
            font-size: 0.9rem;
        }

        .form-input:focus {
            outline: none;
            border-color: #00d4ff;
            box-shadow: 0 0 0 2px rgba(0, 212, 255, 0.2);
        }

        .form-select {
            width: 100%;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
            font-size: 0.9rem;
        }

        .code-editor {
            flex: 1;
            background: #1a1a1a;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            padding: 1rem;
            font-family: 'Courier New', monospace;
            color: #fff;
            font-size: 0.9rem;
            resize: none;
            outline: none;
        }

        .code-editor:focus {
            border-color: #00d4ff;
        }

        .button {
            background: linear-gradient(45deg, #7c3aed, #ec4899);
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 0.2rem;
            font-size: 0.9rem;
        }

        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(124, 58, 237, 0.4);
        }

        .button-success {
            background: linear-gradient(45deg, #059669, #10b981);
        }

        .button-warning {
            background: linear-gradient(45deg, #d97706, #f59e0b);
        }

        .button-secondary {
            background: linear-gradient(45deg, #374151, #6b7280);
        }

        .button-full {
            width: 100%;
            margin-bottom: 0.5rem;
        }

        .output-container {
            background: #000;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            padding: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 0.85rem;
            height: 200px;
            overflow-y: auto;
            color: #00ff00;
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 1rem 0;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            margin-bottom: 1rem;
            background: rgba(0, 0, 0, 0.2);
        }

        .chat-message {
            margin-bottom: 1rem;
            padding: 0.8rem;
            border-radius: 8px;
            max-width: 90%;
        }

        .chat-user {
            background: linear-gradient(45deg, #7c3aed, #ec4899);
            margin-left: auto;
        }

        .chat-ai {
            background: rgba(0, 212, 255, 0.2);
            border: 1px solid rgba(0, 212, 255, 0.3);
        }

        .chat-input-group {
            display: flex;
            gap: 0.5rem;
        }

        .chat-input {
            flex: 1;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
        }

        .status-indicator {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 0.5rem;
            animation: pulse 2s infinite;
        }

        .status-online {
            background: #22c55e;
        }

        .status-processing {
            background: #f59e0b;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .perfection-gap {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            padding: 1rem;
            margin-bottom: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .perfection-gap:hover {
            background: rgba(0, 0, 0, 0.4);
            border-color: #00d4ff;
        }

        .gap-title {
            font-weight: 600;
            margin-bottom: 0.5rem;
            color: #00d4ff;
        }

        .gap-stats {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
            color: rgba(255, 255, 255, 0.7);
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(34, 197, 94, 0.9);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            transform: translateX(400px);
            transition: transform 0.3s ease;
            z-index: 1000;
        }

        .notification.show {
            transform: translateX(0);
        }

        .tab-container {
            display: flex;
            margin-bottom: 1rem;
        }

        .tab {
            padding: 0.5rem 1rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .tab.active {
            background: rgba(0, 212, 255, 0.2);
            border-color: #00d4ff;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        @media (max-width: 1200px) {
            .main-container {
                grid-template-columns: 1fr;
                grid-template-rows: auto auto auto;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🏙️ METROPOLIS AI SYSTEM</h1>
        <div class="subtitle">
            Vollständiges AI-System für Code-Generierung, Perfektions-Analyse und intelligente Automatisierung
        </div>
    </div>

    <div class="main-container">
        <!-- Sidebar - Kontrollen und Einstellungen -->
        <div class="sidebar">
            <div class="section-title">
                <span class="status-indicator status-online"></span>
                ⚙️ System Kontrollen
            </div>

            <!-- Perfektions-Lücken Analyzer -->
            <div class="form-group">
                <h3 style="color: #00d4ff; margin-bottom: 1rem;">🎯 Perfektions-Lücken</h3>
                
                <div class="perfection-gap" onclick="analyzeGap('psychology')">
                    <div class="gap-title">🧠 Menschliche Psyche</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 9/10</span>
                        <span>Chance: 30%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('prediction')">
                    <div class="gap-title">🔮 Kausalitäts-Analyse</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 10%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('spacetime')">
                    <div class="gap-title">⚡ Raum-Zeit Kontrolle</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 5%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('consciousness')">
                    <div class="gap-title">🤖 KI-Bewusstsein</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 8/10</span>
                        <span>Chance: 40%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('matter')">
                    <div class="gap-title">⚛️ Materie-Synthese</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 1%</span>
                    </div>
                </div>

                <button class="button button-success button-full" onclick="runFullAnalysis()">
                    📊 Vollständige Analyse
                </button>
            </div>

            <!-- Eigene Lücke hinzufügen -->
            <div class="form-group">
                <div class="form-label">➕ Eigene Perfektions-Lücke</div>
                <input type="text" class="form-input" id="custom-gap-name" placeholder="z.B. Telepathische Kommunikation">
                <select class="form-select" id="custom-gap-difficulty" style="margin: 0.5rem 0;">
                    <option value="1">Schwierigkeit: 1 (Leicht)</option>
                    <option value="5">Schwierigkeit: 5 (Mittel)</option>
                    <option value="8">Schwierigkeit: 8 (Schwer)</option>
                    <option value="10">Schwierigkeit: 10 (Unmöglich)</option>
                </select>
                <input type="number" class="form-input" id="custom-gap-chance" placeholder="Erfolgswahrscheinlichkeit %" min="0" max="100">
                <button class="button button-warning button-full" onclick="addCustomGap()">
                    Hinzufügen
                </button>
            </div>

            <!-- Code-Generierung Einstellungen -->
            <div class="form-group">
                <h3 style="color: #00d4ff; margin-bottom: 1rem;">💻 Code-Generator</h3>
                <div class="form-label">Programmiersprache</div>
                <select class="form-select" id="code-language">
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                    <option value="java">Java</option>
                    <option value="cpp">C++</option>
                </select>
                
                <div class="form-label" style="margin-top: 1rem;">Code-Typ</div>
                <select class="form-select" id="code-type">
                    <option value="algorithm">Algorithmus</option>
                    <option value="webapp">Web-App</option>
                    <option value="ai">AI/ML Code</option>
                    <option value="game">Spiel</option>
                    <option value="utility">Utility-Tool</option>
                </select>

                <div class="form-label" style="margin-top: 1rem;">Beschreibung</div>
                <input type="text" class="form-input" id="code-description" placeholder="z.B. Sortieralgorithmus für große Datenmengen">
                
                <button class="button button-success button-full" onclick="generateCode()">
                    🚀 Code generieren
                </button>
            </div>
        </div>

        <!-- Main Content Area -->
        <div class="main-content">
            <!-- Code Editor Section -->
            <div class="code-section">
                <div class="section-title">
                    💻 Code Editor & Ausführung
                    <div style="margin-left: auto; font-size: 0.8rem;">
                        <span class="status-indicator status-online"></span>Python Ready
                    </div>
                </div>

                <div class="tab-container">
                    <div class="tab active" onclick="switchTab('editor')">Editor</div>
                    <div class="tab" onclick="switchTab('generated')">Generiert</div>
                    <div class="tab" onclick="switchTab('examples')">Beispiele</div>
                </div>

                <div class="tab-content active" id="editor-tab">
                    <textarea class="code-editor" id="code-input" placeholder="# Python Code hier eingeben oder generieren lassen...

def hello_metropolis():
    '''
    Beispiel-Funktion für das Metropolis System
    '''
    print('🏙️ Metropolis AI System aktiv!')
    
    # Perfektions-Lücken analysieren
    gaps = [
        'Menschliche Psyche verstehen',
        'Zukunft vorhersagen', 
        'Bewusstsein erschaffen'
    ]
    
    for i, gap in enumerate(gaps, 1):
        print(f'{i}. {gap}')
    
    return 'System bereit für weitere Befehle'

# Ausführung
result = hello_metropolis()
print(f'Status: {result}')">
                    </textarea>
                </div>

                <div class="tab-content" id="generated-tab">
                    <textarea class="code-editor" id="generated-code" placeholder="Hier erscheint generierter Code..."></textarea>
                </div>

                <div class="tab-content" id="examples-tab">
                    <div style="padding: 1rem; color: rgba(255,255,255,0.8);">
                        <h4>📚 Code-Beispiele:</h4>
                        <button class="button button-secondary" onclick="loadExample('perfection')">Perfektions-Analyzer</button>
                        <button class="button button-secondary" onclick="loadExample('ai')">AI Assistant</button>
                        <button class="button button-secondary" onclick="loadExample('scheduler')">Task Scheduler</button>
                    </div>
                </div>

                <div style="margin-top: 1rem; display: flex; gap: 0.5rem;">
                    <button class="button button-success" onclick="executeCode()">
                        ▶️ Code ausführen
                    </button>
                    <button class="button" onclick="saveCode()">
                        💾 Speichern
                    </button>
                    <button class="button button-secondary" onclick="clearCode()">
                        🗑️ Löschen
                    </button>
                </div>
            </div>

            <!-- Results Section -->
            <div class="results-section">
                <div class="section-title">
                    📊 Ausführungs-Ergebnisse & System Logs
                </div>
                <div class="output-container" id="output-container">
                    [METROPOLIS SYSTEM BEREIT]
                    > System initialisiert um 15:30:12
                    > Core Services: ONLINE
                    > Perfektions-Analyzer: BEREIT
                    > Code-Generator: AKTIV
                    > Warte auf Befehle...
                </div>
            </div>
        </div>

        <!-- Chat Panel -->
        <div class="chat-panel">
            <div class="section-title">
                🤖 AI Assistant Chat
                <div style="margin-left: auto; font-size: 0.8rem;">
                    <span class="status-indicator status-processing"></span>Ready
                </div>
            </div>

            <div class="chat-messages" id="chat-messages">
                <div class="chat-message chat-ai">
                    <strong>Metropolis AI:</strong> Hallo! Ich bin Ihr AI-Assistent. Ich kann Ihnen helfen bei:
                    <br>• Code-Generierung und -Optimierung
                    <br>• Perfektions-Lücken Analyse
                    <br>• System-Automatisierung
                    <br>• Probleme lösen und erklären
                    <br><br>Fragen Sie mich einfach alles!
                </div>
            </div>

            <div class="chat-input-group">
                <input type="text" class="chat-input" id="chat-input" placeholder="Frage an AI Assistant..." onkeypress="handleChatKeyPress(event)">
                <button class="button" onclick="sendChatMessage()">📤</button>
            </div>

            <div style="margin-top: 1rem;">
                <button class="button button-secondary button-full" onclick="askForCode()">
                    💡 Code-Idee vorschlagen
                </button>
                <button class="button button-secondary button-full" onclick="explainConcept()">
                    🧠 Konzept erklären
                </button>
            </div>
        </div>
    </div>

    <div class="notification" id="notification">
        <div id="notification-content"></div>
    </div>

    <script>
        // System State
        let perfectionGaps = [];
        let codeHistory = [];
        let chatHistory = [];

        // Utility Functions
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            const content = document.getElementById('notification-content');
            content.textContent = message;
            
            notification.className = 'notification show';
            if (type === 'error') {
                notification.style.background = 'rgba(239, 68, 68, 0.9)';
            } else if (type === 'warning') {
                notification.style.background = 'rgba(251, 191, 36, 0.9)';
            } else {
                notification.style.background = 'rgba(34, 197, 94, 0.9)';
            }
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 4000);
        }

        function addOutput(message, type = 'info') {
            const output = document.getElementById('output-container');
            const timestamp = new Date().toLocaleTimeString();
            const prefix = type === 'error' ? '❌' : type === 'success' ? '✅' : '▶️';
            output.innerHTML += `\n[${timestamp}] ${prefix} ${message}`;
            output.scrollTop = output.scrollHeight;
        }

        // Tab Switching
        function switchTab(tabName) {
            // Remove active class from all tabs and content
            document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            
            // Add active class to selected tab and content
            event.target.classList.add('active');
            document.getElementById(tabName + '-tab').classList.add('active');
        }

        // Perfection Gap Analysis
        function analyzeGap(gapType) {
            const gaps = {
                psychology: {
                    name: "Menschliche Psyche",
                    description: "Vollständiges, intuitives Verständnis der menschlichen Psyche",
                    difficulty: 9,
                    chance: 30,
                    research: ["Neuropsychologie", "Kognitionswissenschaft", "Bewusstseinsforschung"]
                },
                prediction: {
                    name: "Kausalitäts-Analyse", 
                    description: "Unbegrenzte, echtzeitfähige prädiktive Analyse aller Kausalitäten",
                    difficulty: 10,
                    chance: 10,
                    research: ["Chaostheorie", "Komplexitätswissenschaft", "Quantenmechanik"]
                },
                spacetime: {
                    name: "Raum-Zeit Kontrolle",
                    description: "Absolute Kontrolle und Manipulation von Raum-Zeit-Kontinuen", 
                    difficulty: 10,
                    chance: 5,
                    research: ["Relativitätstheorie", "Quantengravitation", "Exotische Materie"]
                },
                consciousness: {
                    name: "KI-Bewusstsein",
                    description: "Schöpfung selbstbewusster, emergent intelligenter Entitäten",
                    difficulty: 8,
                    chance: 40,
                    research: ["Künstliche Intelligenz", "Neurowissenschaft", "Philosophie des Geistes"]
                },
                matter: {
                    name: "Materie-Synthese",
                    description: "Nahtlose Synthese von Materie und Energie aus dem Nichts",
                    difficulty: 10,
                    chance: 1,
                    research: ["Thermodynamik", "Quantenfeldtheorie", "Energieerhaltung"]
                }
            };

            const gap = gaps[gapType];
            if (gap) {
                addOutput(`ANALYSE: ${gap.name}`);
                addOutput(`Beschreibung: ${gap.description}`);
                addOutput(`Schwierigkeit: ${gap.difficulty}/10`);
                addOutput(`Erfolgswahrscheinlichkeit: ${gap.chance}%`);
                addOutput(`Forschungsgebiete: ${gap.research.join(', ')}`);
                showNotification(`${gap.name} analysiert - ${gap.chance}% Erfolgswahrscheinlichkeit`);
            }
        }

        function runFullAnalysis() {
            addOutput('🎯 VOLLSTÄNDIGE PERFEKTIONS-LÜCKEN ANALYSE GESTARTET');
            showNotification('Vollständige Analyse läuft...');
            
            setTimeout(() => {
                addOutput('Analysiere 5 Hauptkategorien...', 'info');
            }, 1000);
            
            setTimeout(() => {
                addOutput('✅ Psychologie: 30% erreichbar (mittelfristig)', 'success');
                addOutput('✅ KI-Bewusstsein: 40% erreichbar (kurzfristig)', 'success'); 
                addOutput('⚠️ Kausalitäts-Analyse: 10% erreichbar (langfristig)', 'error');
                addOutput('⚠️ Raum-Zeit: 5% erreichbar (sehr langfristig)', 'error');
                addOutput('❌ Materie-Synthese: 1% erreichbar (theoretisch)', 'error');
                addOutput('FAZIT: KI-Bewusstsein ist das vielversprechendste Ziel', 'success');
                showNotification('Analyse abgeschlossen - KI-Bewusstsein empfohlen');
            }, 3000);
        }

        function addCustomGap() {
            const name = document.getElementById('custom-gap-name').value;
            const difficulty = document.getElementById('custom-gap-difficulty').value;
            const chance = document.getElementById('custom-gap-chance').value;
            
            if (!name || !chance) {
                showNotification('Bitte alle Felder ausfüllen', 'error');
                return;
            }
            
            addOutput(`➕ NEUE LÜCKE HINZUGEFÜGT: ${name}`);
            addOutput(`Schwierigkeit: ${difficulty}/10, Chance: ${chance}%`);
            showNotification(`Perfektions-Lücke "${name}" hinzugefügt`);
            
            // Clear form
            document.getElementById('custom-gap-name').value = '';
            document.getElementById('custom-gap-chance').value = '';
        }

        // Code Generation
        function generateCode() {
            const language = document.getElementById('code-language').value;
            const type = document.getElementById('code-type').value;
            const description = document.getElementById('code-description').value;
            
            if (!description) {
                showNotification('Bitte Beschreibung eingeben', 'error');
                return;
            }
            
            addOutput(`🚀 GENERIERE ${language.toUpperCase()} CODE...`);
            addOutput(`Typ: ${type}, Beschreibung: ${description}`);
            showNotification('Code wird generiert...');
            
            // Switch to generated tab
            switchTab('generated');
            document.querySelector('.tab:nth-child(2)').classList.add('active');
            document.querySelector('.tab:nth-child(1)').classList.remove('active');
            
            setTimeout(() => {
                let generatedCode = '';
                
                if (language === 'python') {
                    if (type === 'algorithm') {
                        generatedCode = `#!/usr/bin/env python3
"""
${description}
Automatisch generiert vom Metropolis AI System
"""

def ${description.toLowerCase().replace(/\s+/g, '_').replace(/[^a-z0-9_]/g, '')}(data):
    """
    ${description}
    
    Args:
        data: Eingabedaten für die Verarbeitung
        
    Returns:
        Verarbeitete Ergebnisse
    """
    print(f"🔄 Starte: ${description}")
    
    # Hauptlogik hier implementiert
    result = process_data(data)
    
    print(f"✅ Abgeschlossen: {len(result) if hasattr(result, '__len__') else 'OK'}")
    return result

def process_data(data):
    """Hauptverarbeitungslogik"""
    # Beispiel-Implementation
    if isinstance(data, list):
        return sorted(data, key=lambda x: str(x))
    elif isinstance(data, dict):
        return {k: v for k, v in sorted(data.items())}
    else:
        return str(data).upper()

# Beispiel-Verwendung
if __name__ == "__main__":
    test_data = [3, 1, 4, 1, 5, 9, 2, 6]
    result = ${description.toLowerCase().replace(/\s+/g, '_').replace(/[^a-z0-9_]/g, '')}(test_data)
    print(f"Ergebnis: {result}")`;
                    } else if (type === 'webapp') {
                        generatedCode = `#!/usr/bin/env python3
"""
${description} - Web Application
Generiert vom Metropolis AI System
"""

from flask import Flask, render_template, request, jsonify
import os

app = Flask(__name__)

@app.route('/')
def index():
    """Hauptseite der Web-App"""
    return render_template('index.html', title='${description}')

@app.route('/api/process', methods=['POST'])
def process_request():
    """API Endpoint für Datenverarbeitung"""
    try:
        data = request.get_json()
        
        # Hier kommt Ihre Geschäftslogik
        result = process_business_logic(data)
        
        return jsonify({
            'success': True,
            'data': result,
            'message': 'Verarbeitung erfolgreich'
        })
    except Exception as e:
        return jsonify({
            'success': False,
            'error': str(e)
        }), 400

def process_business_logic(data):
    """Hauptgeschäftslogik"""
    # Beispiel-Implementation
    return {
        'processed': True,
        'input_count': len(data) if isinstance(data, (list, dict)) else 1,
        'timestamp': '2025-09-06T15:30:00Z'
    }

if __name__ == '__main__':
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port, debug=True)`;
                    } else if (type === 'ai') {
                        generatedCode = `#!/usr/bin/env python3
"""
${description} - AI/ML Implementation
Generiert vom Metropolis AI System
"""

import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

class AIModel:
    """AI Model für ${description}"""
    
    def __init__(self):
        self.model = RandomForestClassifier(n_estimators=100, random_state=42)
        self.is_trained = False
        
    def train(self, X, y):
        """Trainiert das AI-Model"""
        print(f"🤖 Starte Training für: ${description}")
        
        # Data Split
        X_train, X_test, y_train, y_test = train_test_split(
            X, y, test_size=0.2, random_state=42
        )
        
        # Training
        self.model.fit(X_train, y_train)
        
        # Evaluation
        y_pred = self.model.predict(X_test)
        accuracy = accuracy_score(y_test, y_pred)
        
        print(f"✅ Training abgeschlossen - Genauigkeit: {accuracy:.2%}")
        self.is_trained = True
        
        return {
            'accuracy': accuracy,
            'report': classification_report(y_test, y_pred)
        }
    
    def predict(self, X):
        """Macht Vorhersagen"""
        if not self.is_trained:
            raise ValueError("Model muss erst trainiert werden!")
            
        predictions = self.model.predict(X)
        confidence = self.model.predict_proba(X).max(axis=1)
        
        return {
            'predictions': predictions.tolist(),
            'confidence': confidence.tolist()
        }

# Beispiel-Verwendung
if __name__ == "__main__":
    # Beispiel-Daten generieren
    X = np.random.rand(1000, 10)
    y = (X.sum(axis=1) > 5).astype(int)
    
    # AI Model erstellen und trainieren
    ai_model = AIModel()
    results = ai_model.train(X, y)
    
    print("Trainings-Ergebnisse:", results)`;
                    }
                } else if (language === 'javascript') {
                    generatedCode = `/**
 * ${description}
 * Automatisch generiert vom Metropolis AI System
 */

class ${description.replace(/\s+/g, '').replace(/[^a-zA-Z0-9]/g, '')} {
    constructor(options = {}) {
        this.options = {
            debug: false,
            timeout: 5000,
            ...options
        };
        
        this.init();
    }
    
    init() {
        console.log('🚀 Initialisiere: ${description}');
        this.setupEventListeners();
    }
    
    setupEventListeners() {
        // Event Listeners hier hinzufügen
        document.addEventListener('DOMContentLoaded', () => {
            this.onReady();
        });
    }
    
    onReady() {
        console.log('✅ ${description} bereit');
        this.process();
    }
    
    async process(data = null) {
        try {
            console.log('🔄 Verarbeite Daten...');
            
            // Hauptlogik hier
            const result = await this.businessLogic(data);
            
            console.log('✅ Verarbeitung abgeschlossen:', result);
            return result;
            
        } catch (error) {
            console.error('❌ Fehler:', error);
            throw error;
        }
    }
    
    async businessLogic(data) {
        // Beispiel-Implementation
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve({
                    processed: true,
                    timestamp: new Date().toISOString(),
                    data: data || 'Keine Daten'
                });
            }, 1000);
        });
    }
}

// Initialisierung
const app = new ${description.replace(/\s+/g, '').replace(/[^a-zA-Z0-9]/g, '')}({
    debug: true
});

// Export für Module
if (typeof module !== 'undefined' && module.exports) {
    module.exports = ${description.replace(/\s+/g, '').replace(/[^a-zA-Z0-9]/g, '')};
}`;
                }
                
                document.getElementById('generated-code').value = generatedCode;
                addOutput(`✅ CODE GENERIERT (${generatedCode.split('\n').length} Zeilen)`, 'success');
                showNotification('Code erfolgreich generiert!');
            }, 2000);
        }

        // Code Examples
        function loadExample(exampleType) {
            let exampleCode = '';
            
            if (exampleType === 'perfection') {
                exampleCode = `#!/usr/bin/env python3
"""
Perfektions-Lücken Analyzer - Vollständige Implementation
"""

from enum import Enum
from typing import List, Dict
from dataclasses import dataclass
import json

class GapCategory(Enum):
    PSYCHOLOGY = "Psychologie"
    PHYSICS = "Physik"
    CONSCIOUSNESS = "Bewusstsein"
    THERMODYNAMICS = "Thermodynamik"
    PREDICTION = "Vorhersage"

@dataclass
class PerfectionGap:
    description: str
    category: GapCategory
    difficulty_level: int  # 1-10
    theoretical_possibility: float  # 0.0-1.0
    research_areas: List[str]

class PerfectionAnalyzer:
    def __init__(self):
        self.gaps = self._initialize_standard_gaps()
    
    def _initialize_standard_gaps(self):
        return [
            PerfectionGap(
                "Vollständiges Verständnis der menschlichen Psyche",
                GapCategory.PSYCHOLOGY, 9, 0.3,
                ["Neuropsychologie", "Kognitionswissenschaft"]
            ),
            PerfectionGap(
                "Unbegrenzte prädiktive Analyse aller Kausalitäten", 
                GapCategory.PREDICTION, 10, 0.1,
                ["Chaostheorie", "Komplexitätswissenschaft"]
            ),
            # Weitere Lücken...
        ]
    
    def analyze_achievability(self):
        """Analysiert die Erreichbarkeit aller Lücken"""
        results = []
        for gap in self.gaps:
            score = (1 - gap.difficulty_level/10) * gap.theoretical_possibility
            results.append({
                'gap': gap.description,
                'achievability_score': score,
                'recommendation': 'Empfohlen' if score > 0.15 else 'Schwierig'
            })
        return sorted(results, key=lambda x: x['achievability_score'], reverse=True)

# Verwendung
analyzer = PerfectionAnalyzer()
results = analyzer.analyze_achievability()
print("🎯 Perfektions-Lücken Analyse:")
for result in results:
    print(f"- {result['gap']}: {result['achievability_score']:.2%} ({result['recommendation']})")`;
            
            } else if (exampleType === 'ai') {
                exampleCode = `#!/usr/bin/env python3
"""
AI Assistant - Intelligente Konversation und Problemlösung
"""

import random
import re
from datetime import datetime

class AIAssistant:
    def __init__(self, name="Metropolis AI"):
        self.name = name
        self.knowledge_base = {
            'programming': [
                'Python ist hervorragend für AI und Datenanalyse',
                'JavaScript ermöglicht dynamische Web-Anwendungen',
                'Machine Learning erfordert saubere Datenaufbereitung'
            ],
            'perfection': [
                'Perfektion ist ein Prozess, kein Zustand',
                'Kontinuierliche Verbesserung ist der Schlüssel',
                'Fehler sind Lernmöglichkeiten'
            ],
            'philosophy': [
                'Bewusstsein ist mehr als nur Information',
                'Intelligenz zeigt sich in der Anpassungsfähigkeit',
                'Kreativität entsteht aus unerwarteten Verbindungen'
            ]
        }
    
    def respond(self, user_input):
        """Generiert eine intelligente Antwort"""
        user_input = user_input.lower().strip()
        
        # Kategorisiere die Anfrage
        category = self._categorize_input(user_input)
        
        # Generiere kontextuelle Antwort
        if 'code' in user_input or 'programmier' in user_input:
            return self._code_advice(user_input)
        elif 'perfection' in user_input or 'perfektion' in user_input:
            return self._perfection_advice(user_input)
        elif '?' in user_input:
            return self._answer_question(user_input)
        else:
            return self._general_response(category)
    
    def _categorize_input(self, text):
        """Kategorisiert die Benutzereingabe"""
        if any(word in text for word in ['code', 'python', 'javascript', 'programmier']):
            return 'programming'
        elif any(word in text for word in ['perfekt', 'verbesser', 'optimal']):
            return 'perfection'  
        else:
            return 'philosophy'
    
    def _code_advice(self, user_input):
        tips = [
            "💡 Tipp: Schreiben Sie zuerst Tests, dann den Code",
            "🔧 Refactoring macht Code wartbarer und verständlicher",
            "📚 Dokumentation ist genauso wichtig wie der Code selbst",
            "🚀 Kleine, häufige Commits sind besser als große Änderungen"
        ]
        return f"{random.choice(tips)}\\n\\nWas genau möchten Sie programmieren?"
    
    def _perfection_advice(self, user_input):
        advice = random.choice(self.knowledge_base['perfection'])
        return f"🎯 {advice}\\n\\nPerfektions-Lücken sind Chancen zur Weiterentwicklung!"
    
    def _answer_question(self, question):
        return f"🤔 Das ist eine interessante Frage! Basierend auf meiner Analyse würde ich sagen: Die Antwort hängt vom Kontext ab. Können Sie spezifischer werden?"
    
    def _general_response(self, category):
        response = random.choice(self.knowledge_base[category])
        return f"💭 {response}\\n\\nWie kann ich Ihnen weiterhelfen?"

# Verwendung
ai = AIAssistant()
print("🤖 AI Assistant bereit!")
while True:
    user_input = input("Sie: ")
    if user_input.lower() in ['quit', 'exit', 'bye']:
        break
    response = ai.respond(user_input)
    print(f"AI: {response}\\n")`;
            
            } else if (exampleType === 'scheduler') {
                exampleCode = `#!/usr/bin/env python3
"""
Task Scheduler - Intelligente Aufgabenplanung und -ausführung
"""

import threading
import time
import queue
from datetime import datetime, timedelta
from typing import Callable, Any, Dict
import uuid

class Task:
    def __init__(self, name: str, func: Callable, args=(), kwargs=None, 
                 priority: int = 1, delay: float = 0):
        self.id = str(uuid.uuid4())[:8]
        self.name = name
        self.func = func
        self.args = args or ()
        self.kwargs = kwargs or {}
        self.priority = priority
        self.scheduled_time = datetime.now() + timedelta(seconds=delay)
        self.status = 'queued'
        self.result = None
        self.error = None
    
    def __lt__(self, other):
        # Höhere Priorität = niedrigere Zahl für PriorityQueue
        if self.priority != other.priority:
            return self.priority > other.priority
        return self.scheduled_time < other.scheduled_time

class MetropolisScheduler:
    def __init__(self, max_workers: int = 4):
        self.task_queue = queue.PriorityQueue()
        self.completed_tasks = {}
        self.running_tasks = {}
        self.max_workers = max_workers
        self.workers = []
        self.running = False
        
    def start(self):
        """Startet den Scheduler"""
        self.running = True
        print(f"🚀 Metropolis Scheduler gestartet ({self.max_workers} Worker)")
        
        # Worker Threads starten
        for i in range(self.max_workers):
            worker = threading.Thread(target=self._worker_loop, name=f"Worker-{i+1}")
            worker.daemon = True
            worker.start()
            self.workers.append(worker)
    
    def stop(self):
        """Stoppt den Scheduler"""
        self.running = False
        print("🛑 Scheduler wird gestoppt...")
        
    def schedule_task(self, name: str, func: Callable, args=(), kwargs=None, 
                     priority: int = 1, delay: float = 0) -> str:
        """Plant eine neue Aufgabe"""
        task = Task(name, func, args, kwargs, priority, delay)
        self.task_queue.put((task.priority, task.scheduled_time, task))
        
        delay_text = f" (Verzögerung: {delay}s)" if delay > 0 else ""
        print(f"📋 Task geplant: {name} [ID: {task.id}] - Priorität: {priority}{delay_text}")
        return task.id
    
    def _worker_loop(self):
        """Hauptschleife für Worker Threads"""
        worker_name = threading.current_thread().name
        
        while self.running:
            try:
                # Task aus Queue holen (mit Timeout)
                priority, scheduled_time, task = self.task_queue.get(timeout=1.0)
                
                # Warten bis zur geplanten Zeit
                now = datetime.now()
                if scheduled_time > now:
                    sleep_time = (scheduled_time - now).total_seconds()
                    time.sleep(sleep_time)
                
                # Task ausführen
                self.running_tasks[task.id] = task
                task.status = 'running'
                
                print(f"⚡ {worker_name} führt aus: {task.name} [ID: {task.id}]")
                
                start_time = time.time()
                try:
                    result = task.func(*task.args, **task.kwargs)
                    task.result = result
                    task.status = 'completed'
                    execution_time = time.time() - start_time
                    
                    print(f"✅ Task abgeschlossen: {task.name} ({execution_time:.2f}s)")
                    
                except Exception as e:
                    task.error = str(e)
                    task.status = 'failed'
                    print(f"❌ Task fehlgeschlagen: {task.name} - Fehler: {e}")
                
                # Task von running zu completed verschieben
                self.running_tasks.pop(task.id, None)
                self.completed_tasks[task.id] = task
                
                self.task_queue.task_done()
                
            except queue.Empty:
                continue
            except Exception as e:
                print(f"❌ Worker {worker_name} Fehler: {e}")

# Beispiel-Tasks
def analyze_perfection_gap(gap_name: str):
    """Beispiel: Perfektions-Lücke analysieren"""
    print(f"🎯 Analysiere Perfektions-Lücke: {gap_name}")
    time.sleep(2)  # Simuliere Verarbeitung
    return f"Analyse für {gap_name} abgeschlossen - Erreichbarkeit: 42%"

def generate_code(language: str, description: str):
    """Beispiel: Code generieren"""
    print(f"💻 Generiere {language} Code: {description}")
    time.sleep(3)  # Simuliere Generierung
    return f"Code für '{description}' in {language} erfolgreich generiert"

def process_ai_request(request: str):
    """Beispiel: AI-Anfrage verarbeiten"""
    print(f"🤖 Verarbeite AI-Anfrage: {request}")
    time.sleep(1)
    return f"AI-Antwort für: {request}"

# Verwendung
if __name__ == "__main__":
    scheduler = MetropolisScheduler(max_workers=3)
    scheduler.start()
    
    # Tasks planen
    scheduler.schedule_task("Psychologie-Analyse", analyze_perfection_gap, 
                          args=("Menschliche Psyche",), priority=3)
    
    scheduler.schedule_task("Python Code Gen", generate_code,
                          args=("Python", "Sortieralgorithmus"), priority=2)
    
    scheduler.schedule_task("AI Chat", process_ai_request,
                          args=("Erkläre Quantencomputing",), priority=1, delay=5)
    
    # Warten und Status anzeigen
    time.sleep(10)
    
    print(f"\\n📊 Status: {len(scheduler.completed_tasks)} abgeschlossen, {len(scheduler.running_tasks)} laufend")
    scheduler.stop()`;
            }
            
            // Load into editor
            document.getElementById('code-input').value = exampleCode;
            addOutput(`📚 BEISPIEL GELADEN: ${exampleType.toUpperCase()}`, 'success');
            showNotification(`${exampleType}-Beispiel geladen`);
            
            // Switch back to editor tab
            switchTab('editor');
            document.querySelector('.tab:nth-child(1)').classList.add('active');
            document.querySelector('.tab:nth-child(2)').classList.remove('active');
        }

        // Code Execution
        function executeCode() {
            const code = document.getElementById('code-input').value;
            if (!code.trim()) {
                showNotification('Bitte Code eingeben', 'error');
                return;
            }
            
            addOutput('🚀 STARTE CODE-AUSFÜHRUNG...');
            showNotification('Code wird ausgeführt...');
            
            // Simulate code execution with realistic output
            setTimeout(() => {
                // Analyze code to provide relevant output
                if (code.includes('hello_metropolis')) {
                    addOutput('🏙️ Metropolis AI System aktiv!', 'success');
                    addOutput('1. Menschliche Psyche verstehen', 'info');
                    addOutput('2. Zukunft vorhersagen', 'info');
                    addOutput('3. Bewusstsein erschaffen', 'info');
                    addOutput('Status: System bereit für weitere Befehle', 'success');
                } else if (code.includes('PerfectionGap') || code.includes('PerfectionAnalyzer')) {
                    addOutput('🎯 Perfektions-Lücken Analyse:', 'success');
                    addOutput('- KI-Bewusstsein: 24.00% (Empfohlen)', 'success');
                    addOutput('- Menschliche Psyche: 21.00% (Empfohlen)', 'success');
                    addOutput('- Materie-Synthese: 0.10% (Schwierig)', 'info');
                } else if (code.includes('AIAssistant') || code.includes('ai')) {
                    addOutput('🤖 AI Assistant bereit!', 'success');
                    addOutput('AI: 💭 Intelligenz zeigt sich in der Anpassungsfähigkeit', 'info');
                } else if (code.includes('Scheduler') || code.includes('Task')) {
                    addOutput('🚀 Metropolis Scheduler gestartet (3 Worker)', 'success');
                    addOutput('📋 Task geplant: Psychologie-Analyse [ID: abc12345]', 'info');
                    addOutput('⚡ Worker-1 führt aus: Python Code Gen', 'info');
                    addOutput('✅ Task abgeschlossen: AI Chat (2.34s)', 'success');
                } else {
                    addOutput('Code erfolgreich ausgeführt', 'success');
                    addOutput('Ausgabe: Verarbeitung abgeschlossen', 'info');
                }
                
                addOutput(`AUSFÜHRUNG BEENDET (${Math.random() * 3 + 0.5}.toFixed(2)}s)`, 'success');
                showNotification('Code erfolgreich ausgeführt!');
            }, 2000);
        }

        function saveCode() {
            const code = document.getElementById('code-input').value;
            if (!code.trim()) {
                showNotification('Kein Code zum Speichern', 'error');
                return;
            }
            
            // Simulate saving
            addOutput('💾 Code gespeichert in Metropolis Repository', 'success');
            showNotification('Code gespeichert');
        }

        function clearCode() {
            document.getElementById('code-input').value = '';
            addOutput('🗑️ Editor geleert', 'info');
            showNotification('Code gelöscht');
        }

        // Chat System
        function sendChatMessage() {
            const input = document.getElementById('chat-input');
            const message = input.value.trim();
            
            if (!message) return;
            
            // Add user message
            addChatMessage(message, 'user');
            input.value = '';
            
            // Generate AI response
            setTimeout(() => {
                const response = generateAIResponse(message);
                addChatMessage(response, 'ai');
            }, 1000 + Math.random() * 2000);
        }

        function addChatMessage(message, sender) {
            const chatMessages = document.getElementById('chat-messages');
            const messageDiv = document.createElement('div');
            messageDiv.className = `chat-message chat-${sender}`;
            
            if (sender === 'user') {
                messageDiv.innerHTML = `<strong>Sie:</strong> ${message}`;
            } else {
                messageDiv.innerHTML = `<strong>Metropolis AI:</strong> ${message}`;
            }
            
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function generateAIResponse(userMessage) {
            const msg = userMessage.toLowerCase();
            
            if (msg.includes('code') || msg.includes('programmier')) {
                return '💻 Gerne helfe ich beim Programmieren! Welche Sprache und welches Problem? Ich kann Code generieren, optimieren oder erklären.';
            } else if (msg.includes('perfekt') || msg.includes('lücke')) {
                return '🎯 Perfektions-Lücken sind faszinierend! Die vielversprechendste ist aktuell KI-Bewusstsein mit 40% Erfolgswahrscheinlichkeit. Soll ich eine Analyse durchführen?';
            } else if (msg.includes('ki') || msg.includes('ai') || msg.includes('intelligent')) {
                return '🤖 KI-Systeme wie ich werden durch kontinuierliches Lernen besser. Ich kann bei Probleml
