<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TradingVision Pro - Análise de Gráficos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        
        .glass-effect {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .analysis-card {
            transition: all 0.3s ease;
        }
        
        .analysis-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
        }
        
        .pulse-animation {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        
        .upload-zone {
            border: 2px dashed #cbd5e0;
            transition: all 0.3s ease;
        }
        
        .upload-zone:hover {
            border-color: #667eea;
            background-color: rgba(102, 126, 234, 0.05);
        }
        
        .upload-zone.dragover {
            border-color: #667eea;
            background-color: rgba(102, 126, 234, 0.1);
        }
    </style>
</head>
<body class="gradient-bg min-h-screen">
    <div class="container mx-auto px-4 py-6">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-white mb-2">📈 TradingVision Pro</h1>
            <p class="text-white/80 text-lg">Análise Profissional de Gráficos de Velas</p>
        </div>

        <!-- Upload Section -->
        <div class="glass-effect rounded-2xl p-6 mb-8">
            <h2 class="text-2xl font-semibold text-white mb-4">📷 Enviar Gráfico para Análise</h2>
            
            <div id="uploadZone" class="upload-zone rounded-xl p-8 text-center cursor-pointer">
                <input type="file" id="imageInput" accept="image/*" class="hidden">
                <div id="uploadContent">
                    <div class="text-6xl mb-4">📊</div>
                    <h3 class="text-xl font-semibold text-gray-700 mb-2">Clique ou arraste uma imagem</h3>
                    <p class="text-gray-500">Suporte para JPG, PNG, WebP</p>
                </div>
                <div id="imagePreview" class="hidden">
                    <img id="previewImg" class="max-w-full max-h-64 mx-auto rounded-lg shadow-lg">
                    <button id="analyzeBtn" class="mt-4 bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-lg font-semibold transition-colors">
                        🔍 Analisar Gráfico
                    </button>
                </div>
            </div>
        </div>

        <!-- Analysis Results -->
        <div id="analysisSection" class="hidden">
            <!-- AI Confidence Score -->
            <div class="glass-effect rounded-2xl p-6 mb-6">
                <div class="flex items-center justify-between mb-4">
                    <h2 class="text-2xl font-semibold text-white">🤖 Análise IA - TradingVision Pro</h2>
                    <div class="flex items-center space-x-2">
                        <span class="text-white/80">Confiança:</span>
                        <div id="confidenceScore" class="bg-green-500 text-white px-3 py-1 rounded-full font-bold">--</div>
                    </div>
                </div>
                
                <div class="grid md:grid-cols-3 gap-4">
                    <div class="bg-white/10 rounded-lg p-4 text-center">
                        <div class="text-2xl mb-2">⚡</div>
                        <div class="text-white font-semibold">Análise Instantânea</div>
                        <div class="text-white/70 text-sm">< 3 segundos</div>
                    </div>
                    <div class="bg-white/10 rounded-lg p-4 text-center">
                        <div class="text-2xl mb-2">🎯</div>
                        <div class="text-white font-semibold">Precisão</div>
                        <div id="accuracyRate" class="text-white/70 text-sm">--</div>
                    </div>
                    <div class="bg-white/10 rounded-lg p-4 text-center">
                        <div class="text-2xl mb-2">📊</div>
                        <div class="text-white font-semibold">Padrões Detectados</div>
                        <div id="patternsCount" class="text-white/70 text-sm">--</div>
                    </div>
                </div>
            </div>

            <!-- Main Analysis Grid -->
            <div class="glass-effect rounded-2xl p-6 mb-6">
                <h2 class="text-2xl font-semibold text-white mb-4">📊 Análise Técnica Avançada</h2>
                
                <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Tendência Multi-Timeframe -->
                    <div class="analysis-card bg-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">📈</span>
                            <h3 class="text-lg font-semibold text-gray-800">Tendência Multi-TF</h3>
                        </div>
                        <div id="trendAnalysis" class="text-gray-600"></div>
                        <div id="timeframeAnalysis" class="mt-3 text-sm"></div>
                    </div>

                    <!-- Padrões Candlestick -->
                    <div class="analysis-card bg-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">🕯️</span>
                            <h3 class="text-lg font-semibold text-gray-800">Padrões Candlestick</h3>
                        </div>
                        <div id="patternAnalysis" class="text-gray-600"></div>
                        <div id="patternStrength" class="mt-3"></div>
                    </div>

                    <!-- Fibonacci & Ondas -->
                    <div class="analysis-card bg-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">🌊</span>
                            <h3 class="text-lg font-semibold text-gray-800">Fibonacci & Elliott</h3>
                        </div>
                        <div id="fibonacciAnalysis" class="text-gray-600"></div>
                    </div>

                    <!-- Suporte e Resistência Dinâmicos -->
                    <div class="analysis-card bg-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">⚖️</span>
                            <h3 class="text-lg font-semibold text-gray-800">S&R Dinâmicos</h3>
                        </div>
                        <div id="supportResistance" class="text-gray-600"></div>
                        <div id="dynamicLevels" class="mt-3 text-sm"></div>
                    </div>

                    <!-- Volume Profile -->
                    <div class="analysis-card bg-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">📊</span>
                            <h3 class="text-lg font-semibold text-gray-800">Volume Profile</h3>
                        </div>
                        <div id="volumeProfile" class="text-gray-600"></div>
                    </div>

                    <!-- Recomendação IA -->
                    <div class="analysis-card bg-gradient-to-br from-blue-500 to-purple-600 text-white rounded-xl p-6">
                        <div class="flex items-center mb-3">
                            <span class="text-2xl mr-3">🤖</span>
                            <h3 class="text-lg font-semibold">Recomendação IA</h3>
                        </div>
                        <div id="recommendation" class="text-white/90"></div>
                        <div id="aiInsight" class="mt-3 text-sm text-white/70"></div>
                    </div>
                </div>
            </div>

            <!-- Indicadores Técnicos Avançados -->
            <div class="glass-effect rounded-2xl p-6 mb-6">
                <h2 class="text-2xl font-semibold text-white mb-4">⚡ Suite de Indicadores Profissionais</h2>
                
                <!-- Osciladores -->
                <div class="mb-6">
                    <h3 class="text-lg font-semibold text-white mb-3">🎯 Osciladores</h3>
                    <div class="grid md:grid-cols-4 gap-4">
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">RSI (14)</div>
                            <div id="rsiValue" class="text-2xl font-bold text-blue-600">--</div>
                            <div id="rsiStatus" class="text-xs text-gray-400">--</div>
                            <div id="rsiBar" class="w-full bg-gray-200 rounded-full h-2 mt-2">
                                <div class="bg-blue-600 h-2 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">Stochastic</div>
                            <div id="stochValue" class="text-2xl font-bold text-purple-600">--</div>
                            <div id="stochStatus" class="text-xs text-gray-400">--</div>
                            <div id="stochBar" class="w-full bg-gray-200 rounded-full h-2 mt-2">
                                <div class="bg-purple-600 h-2 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">Williams %R</div>
                            <div id="williamsValue" class="text-2xl font-bold text-indigo-600">--</div>
                            <div id="williamsStatus" class="text-xs text-gray-400">--</div>
                            <div id="williamsBar" class="w-full bg-gray-200 rounded-full h-2 mt-2">
                                <div class="bg-indigo-600 h-2 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">CCI (20)</div>
                            <div id="cciValue" class="text-2xl font-bold text-pink-600">--</div>
                            <div id="cciStatus" class="text-xs text-gray-400">--</div>
                            <div id="cciBar" class="w-full bg-gray-200 rounded-full h-2 mt-2">
                                <div class="bg-pink-600 h-2 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Tendência e Momentum -->
                <div class="mb-6">
                    <h3 class="text-lg font-semibold text-white mb-3">📈 Tendência & Momentum</h3>
                    <div class="grid md:grid-cols-3 gap-4">
                        <div class="bg-white rounded-lg p-4">
                            <div class="text-sm text-gray-500 mb-2">MACD (12,26,9)</div>
                            <div class="flex justify-between items-center mb-2">
                                <span class="text-lg font-bold text-green-600" id="macdValue">--</span>
                                <span id="macdSignal" class="text-sm px-2 py-1 rounded-full">--</span>
                            </div>
                            <div id="macdStatus" class="text-xs text-gray-400">--</div>
                            <div class="mt-2">
                                <div class="flex justify-between text-xs text-gray-400">
                                    <span>Histograma:</span>
                                    <span id="macdHist">--</span>
                                </div>
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4">
                            <div class="text-sm text-gray-500 mb-2">ADX (14)</div>
                            <div class="flex justify-between items-center mb-2">
                                <span class="text-lg font-bold text-orange-600" id="adxValue">--</span>
                                <span id="adxStrength" class="text-sm px-2 py-1 rounded-full">--</span>
                            </div>
                            <div class="text-xs text-gray-400">
                                <div>+DI: <span id="plusDI">--</span></div>
                                <div>-DI: <span id="minusDI">--</span></div>
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4">
                            <div class="text-sm text-gray-500 mb-2">Parabolic SAR</div>
                            <div class="flex justify-between items-center mb-2">
                                <span class="text-lg font-bold text-teal-600" id="sarValue">--</span>
                                <span id="sarTrend" class="text-sm px-2 py-1 rounded-full">--</span>
                            </div>
                            <div id="sarStatus" class="text-xs text-gray-400">--</div>
                        </div>
                    </div>
                </div>

                <!-- Volume e Volatilidade -->
                <div>
                    <h3 class="text-lg font-semibold text-white mb-3">📊 Volume & Volatilidade</h3>
                    <div class="grid md:grid-cols-4 gap-4">
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">Volume</div>
                            <div id="volumeValue" class="text-2xl font-bold text-purple-600">--</div>
                            <div id="volumeStatus" class="text-xs text-gray-400">--</div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">OBV</div>
                            <div id="obvValue" class="text-2xl font-bold text-cyan-600">--</div>
                            <div id="obvStatus" class="text-xs text-gray-400">--</div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">Bollinger %B</div>
                            <div id="bbValue" class="text-2xl font-bold text-yellow-600">--</div>
                            <div id="bbStatus" class="text-xs text-gray-400">--</div>
                        </div>
                        
                        <div class="bg-white rounded-lg p-4 text-center">
                            <div class="text-sm text-gray-500 mb-1">ATR (14)</div>
                            <div id="atrValue" class="text-2xl font-bold text-red-600">--</div>
                            <div id="atrStatus" class="text-xs text-gray-400">--</div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Alertas e Sinais -->
            <div class="glass-effect rounded-2xl p-6 mb-6">
                <h2 class="text-2xl font-semibold text-white mb-4">🚨 Central de Alertas</h2>
                
                <div class="grid md:grid-cols-2 gap-6">
                    <!-- Alertas Ativos -->
                    <div class="bg-white rounded-xl p-6">
                        <h3 class="font-semibold text-gray-800 mb-4 flex items-center">
                            <span class="text-xl mr-2">🔔</span>
                            Alertas Detectados
                        </h3>
                        <div id="activeAlerts" class="space-y-3"></div>
                    </div>
                    
                    <!-- Sinais de Trading -->
                    <div class="bg-white rounded-xl p-6">
                        <h3 class="font-semibold text-gray-800 mb-4 flex items-center">
                            <span class="text-xl mr-2">⚡</span>
                            Sinais de Trading
                        </h3>
                        <div id="tradingSignals" class="space-y-3"></div>
                    </div>
                </div>
            </div>

            <!-- Risk Management Avançado -->
            <div class="glass-effect rounded-2xl p-6 mb-6">
                <h2 class="text-2xl font-semibold text-white mb-4">⚠️ Gestão de Risco Profissional</h2>
                
                <div class="grid md:grid-cols-3 gap-6">
                    <!-- Calculadora de Posição -->
                    <div class="bg-white rounded-xl p-6">
                        <h4 class="font-semibold text-gray-800 mb-3 flex items-center">
                            <span class="text-lg mr-2">🧮</span>
                            Calculadora de Posição
                        </h4>
                        <div id="positionCalc" class="text-gray-600 text-sm space-y-2"></div>
                    </div>
                    
                    <!-- Pontos de Entrada -->
                    <div class="bg-white rounded-xl p-6">
                        <h4 class="font-semibold text-gray-800 mb-3 flex items-center">
                            <span class="text-lg mr-2">📍</span>
                            Pontos de Entrada
                        </h4>
                        <div id="entryPoints" class="text-gray-600 text-sm"></div>
                    </div>
                    
                    <!-- Stop & Target -->
                    <div class="bg-white rounded-xl p-6">
                        <h4 class="font-semibold text-gray-800 mb-3 flex items-center">
                            <span class="text-lg mr-2">🎯</span>
                            Stop Loss & Take Profit
                        </h4>
                        <div id="stopLoss" class="text-gray-600 text-sm"></div>
                    </div>
                </div>
                
                <!-- Risk Metrics -->
                <div class="mt-6 bg-white rounded-xl p-6">
                    <h4 class="font-semibold text-gray-800 mb-4">📊 Métricas de Risco</h4>
                    <div class="grid md:grid-cols-4 gap-4">
                        <div class="text-center">
                            <div class="text-2xl font-bold text-blue-600" id="riskReward">--</div>
                            <div class="text-sm text-gray-500">Risk/Reward</div>
                        </div>
                        <div class="text-center">
                            <div class="text-2xl font-bold text-green-600" id="winRate">--</div>
                            <div class="text-sm text-gray-500">Win Rate</div>
                        </div>
                        <div class="text-center">
                            <div class="text-2xl font-bold text-purple-600" id="maxDrawdown">--</div>
                            <div class="text-sm text-gray-500">Max Drawdown</div>
                        </div>
                        <div class="text-center">
                            <div class="text-2xl font-bold text-orange-600" id="volatility">--</div>
                            <div class="text-sm text-gray-500">Volatilidade</div>
                        </div>
                    </div>
                </div>
                
                <div class="mt-4 p-4 bg-yellow-50 rounded-lg border-l-4 border-yellow-400">
                    <p class="text-sm text-yellow-800">
                        <strong>⚠️ Aviso Legal:</strong> Esta é uma análise simulada para fins educacionais. 
                        Sempre faça sua própria pesquisa e consulte um consultor financeiro antes de investir.
                    </p>
                </div>
            </div>

            <!-- Market Sentiment -->
            <div class="glass-effect rounded-2xl p-6">
                <h2 class="text-2xl font-semibold text-white mb-4">🌡️ Sentimento do Mercado</h2>
                
                <div class="grid md:grid-cols-2 gap-6">
                    <!-- Fear & Greed Index -->
                    <div class="bg-white rounded-xl p-6">
                        <h4 class="font-semibold text-gray-800 mb-4 flex items-center">
                            <span class="text-lg mr-2">😱</span>
                            Fear & Greed Index
                        </h4>
                        <div class="text-center">
                            <div id="fearGreedValue" class="text-4xl font-bold mb-2">--</div>
                            <div id="fearGreedStatus" class="text-lg font-semibold mb-2">--</div>
                            <div class="w-full bg-gray-200 rounded-full h-4">
                                <div id="fearGreedBar" class="h-4 rounded-full transition-all duration-1000" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Social Sentiment -->
                    <div class="bg-white rounded-xl p-6">
                        <h4 class="font-semibold text-gray-800 mb-4 flex items-center">
                            <span class="text-lg mr-2">💬</span>
                            Sentimento Social
                        </h4>
                        <div id="socialSentiment" class="space-y-3"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Loading Animation -->
        <div id="loadingSection" class="hidden text-center py-12">
            <div class="pulse-animation text-6xl mb-4">🔄</div>
            <h3 class="text-xl font-semibold text-white mb-2">Analisando Gráfico...</h3>
            <p class="text-white/70">Aplicando algoritmos de análise técnica</p>
        </div>
    </div>

    <script>
        const uploadZone = document.getElementById('uploadZone');
        const imageInput = document.getElementById('imageInput');
        const uploadContent = document.getElementById('uploadContent');
        const imagePreview = document.getElementById('imagePreview');
        const previewImg = document.getElementById('previewImg');
        const analyzeBtn = document.getElementById('analyzeBtn');
        const analysisSection = document.getElementById('analysisSection');
        const loadingSection = document.getElementById('loadingSection');

        // Dados simulados para análise avançada
        const analysisData = [
            {
                // Análise Principal
                trend: "Tendência de Alta Confirmada",
                trendDetail: "O gráfico mostra uma sequência de máximas e mínimas ascendentes, indicando força compradora. Volume crescente confirma o movimento.",
                timeframes: "1H: Bullish | 4H: Bullish | 1D: Bullish",
                pattern: "Martelo Invertido + Engolfo Bullish",
                patternDetail: "Combinação de padrões de reversão bullish. Confirmação forte com volume acima da média.",
                patternStrength: "Força: 85% | Confiabilidade: Alta",
                fibonacci: "Retração 61.8% respeitada",
                fibDetail: "Preço reagiu perfeitamente no nível de Fibonacci 61.8%, indicando continuação da tendência primária.",
                support: "Suporte Dinâmico: R$ 45.20",
                resistance: "Resistência: R$ 52.80",
                dynamicLevels: "MM20: R$ 46.50 | MM50: R$ 44.80 | Pivot: R$ 48.90",
                volumeProfile: "POC: R$ 47.80 | VAH: R$ 51.20 | VAL: R$ 44.60",
                recommendation: "COMPRA FORTE",
                recommendationDetail: "Recomendação de compra com múltiplas confirmações técnicas. Setup de alta probabilidade.",
                aiInsight: "IA detectou confluência de 7 sinais bullish. Probabilidade de sucesso: 78%",
                
                // Indicadores Avançados
                rsi: "68.5", rsiStatus: "Sobrecomprado Leve",
                stoch: "72.3", stochStatus: "Overbought",
                williams: "-25.8", williamsStatus: "Bullish",
                cci: "145.2", cciStatus: "Forte Alta",
                macd: "+0.85", macdStatus: "Bullish Forte", macdHist: "+0.23", macdSignal: "BUY",
                adx: "65.4", adxStrength: "Tendência Forte", plusDI: "28.5", minusDI: "15.2",
                sar: "R$ 44.80", sarTrend: "BULLISH", sarStatus: "Suporte dinâmico",
                volume: "Alto", volumeStatus: "158% da média",
                obv: "+2.5M", obvStatus: "Acumulação",
                bb: "0.78", bbStatus: "Próximo ao topo",
                atr: "2.45", atrStatus: "Volatilidade Normal",
                
                // Gestão de Risco
                entry: "Entrada: R$ 47.20 - R$ 48.00 | Confirmação: R$ 48.50",
                stopLoss: "Stop Loss: R$ 44.50 (-6.2%)\nTake Profit 1: R$ 51.00 (+7.8%)\nTake Profit 2: R$ 54.20 (+15.2%)",
                positionCalc: "Capital: R$ 10.000\nRisco: 2% (R$ 200)\nTamanho: 57 ações\nR/R: 1:2.8",
                riskReward: "1:2.8", winRate: "74%", maxDrawdown: "8.5%", volatility: "18.2%",
                
                // Alertas e Sinais
                alerts: [
                    { type: "success", message: "Rompimento de resistência em R$ 48.50", time: "2 min" },
                    { type: "warning", message: "RSI entrando em sobrecompra", time: "5 min" },
                    { type: "info", message: "Volume acima da média por 3 candles", time: "8 min" }
                ],
                signals: [
                    { type: "buy", strength: "Forte", message: "Sinal de compra confirmado", confidence: "85%" },
                    { type: "momentum", strength: "Médio", message: "Momentum positivo mantido", confidence: "72%" }
                ],
                
                // Sentimento
                fearGreed: 72, fearGreedStatus: "Ganância", fearGreedColor: "bg-orange-500",
                socialSentiment: [
                    { platform: "Twitter", sentiment: "Bullish 68%", color: "text-green-600" },
                    { platform: "Reddit", sentiment: "Positivo 74%", color: "text-green-600" },
                    { platform: "News", sentiment: "Neutro 52%", color: "text-yellow-600" }
                ],
                
                // Métricas IA
                confidence: "87%", accuracy: "78.5%", patterns: "12 detectados"
            },
            {
                // Cenário Lateral
                trend: "Consolidação Lateral",
                trendDetail: "Mercado em consolidação entre R$ 38.50 e R$ 42.00. Baixa volatilidade indica possível breakout iminente.",
                timeframes: "1H: Lateral | 4H: Lateral | 1D: Neutro",
                pattern: "Triângulo Simétrico + Bandeira",
                patternDetail: "Padrão de continuação em formação. Compressão de volatilidade sugere movimento explosivo próximo.",
                patternStrength: "Força: 65% | Confiabilidade: Média",
                fibonacci: "Consolidação entre 38.2% e 61.8%",
                fibDetail: "Preço oscilando dentro da zona de valor de Fibonacci, aguardando definição direcional.",
                support: "Suporte: R$ 38.50",
                resistance: "Resistência: R$ 42.00",
                dynamicLevels: "MM20: R$ 40.25 | MM50: R$ 39.80 | Pivot: R$ 40.25",
                volumeProfile: "POC: R$ 40.10 | VAH: R$ 41.50 | VAL: R$ 38.80",
                recommendation: "AGUARDAR BREAKOUT",
                recommendationDetail: "Aguardar rompimento com volume. Preparar para entrada na direção do breakout.",
                aiInsight: "IA sugere aguardar. Probabilidade de breakout nas próximas 24h: 68%",
                
                rsi: "52.3", rsiStatus: "Neutro",
                stoch: "48.7", stochStatus: "Neutro",
                williams: "-52.1", williamsStatus: "Neutro",
                cci: "-15.8", cciStatus: "Neutro",
                macd: "-0.12", macdStatus: "Bearish Fraco", macdHist: "-0.05", macdSignal: "HOLD",
                adx: "25.8", adxStrength: "Tendência Fraca", plusDI: "22.1", minusDI: "24.5",
                sar: "R$ 41.20", sarTrend: "NEUTRO", sarStatus: "Sem direção clara",
                volume: "Baixo", volumeStatus: "65% da média",
                obv: "-0.8M", obvStatus: "Distribuição Leve",
                bb: "0.45", bbStatus: "Meio das bandas",
                atr: "1.85", atrStatus: "Baixa Volatilidade",
                
                entry: "Aguardar: Compra > R$ 42.20 | Venda < R$ 38.30",
                stopLoss: "Stop Compra: R$ 40.50\nStop Venda: R$ 40.50\nTarget: Altura do padrão",
                positionCalc: "Aguardar confirmação\nRisco: 2%\nR/R esperado: 1:2.2",
                riskReward: "1:2.2", winRate: "62%", maxDrawdown: "12.1%", volatility: "14.8%",
                
                alerts: [
                    { type: "info", message: "Aproximando do ápice do triângulo", time: "1 min" },
                    { type: "warning", message: "Volume abaixo da média", time: "15 min" },
                    { type: "info", message: "Volatilidade em mínima de 5 dias", time: "1h" }
                ],
                signals: [
                    { type: "neutral", strength: "Fraco", message: "Aguardar definição", confidence: "45%" },
                    { type: "breakout", strength: "Médio", message: "Preparar para breakout", confidence: "68%" }
                ],
                
                fearGreed: 45, fearGreedStatus: "Neutro", fearGreedColor: "bg-gray-500",
                socialSentiment: [
                    { platform: "Twitter", sentiment: "Neutro 51%", color: "text-gray-600" },
                    { platform: "Reddit", sentiment: "Misto 48%", color: "text-gray-600" },
                    { platform: "News", sentiment: "Cautela 45%", color: "text-yellow-600" }
                ],
                
                confidence: "65%", accuracy: "62.3%", patterns: "8 detectados"
            },
            {
                // Cenário Bearish
                trend: "Tendência de Baixa Forte",
                trendDetail: "Sequência de máximas e mínimas descendentes. Pressão vendedora dominante com volume confirmando distribuição.",
                timeframes: "1H: Bearish | 4H: Bearish | 1D: Bearish",
                pattern: "Engolfo Bearish + Estrela Cadente",
                patternDetail: "Combinação de padrões de reversão bearish. Rejeição forte nos níveis de resistência.",
                patternStrength: "Força: 92% | Confiabilidade: Muito Alta",
                fibonacci: "Rejeição no 50% de retração",
                fibDetail: "Forte rejeição no nível de 50% de Fibonacci, confirmando continuação da tendência de baixa.",
                support: "Suporte: R$ 28.90",
                resistance: "Resistência: R$ 35.20",
                dynamicLevels: "MM20: R$ 32.10 | MM50: R$ 34.20 | Pivot: R$ 31.50",
                volumeProfile: "POC: R$ 31.80 | VAH: R$ 34.50 | VAL: R$ 29.20",
                recommendation: "VENDA FORTE",
                recommendationDetail: "Recomendação de venda com múltiplas confirmações bearish. Setup de alta probabilidade.",
                aiInsight: "IA detectou padrão de distribuição. Probabilidade de queda: 82%",
                
                rsi: "28.7", rsiStatus: "Sobrevendido",
                stoch: "15.2", stochStatus: "Oversold",
                williams: "-85.4", williamsStatus: "Bearish Forte",
                cci: "-185.6", cciStatus: "Forte Baixa",
                macd: "-1.25", macdStatus: "Bearish Forte", macdHist: "-0.45", macdSignal: "SELL",
                adx: "78.2", adxStrength: "Tendência Muito Forte", plusDI: "12.8", minusDI: "35.6",
                sar: "R$ 35.80", sarTrend: "BEARISH", sarStatus: "Resistência dinâmica",
                volume: "Alto", volumeStatus: "185% da média",
                obv: "-4.2M", obvStatus: "Distribuição Forte",
                bb: "0.15", bbStatus: "Próximo à banda inferior",
                atr: "3.25", atrStatus: "Alta Volatilidade",
                
                entry: "Entrada: R$ 33.80 - R$ 34.50 | Confirmação: R$ 33.20",
                stopLoss: "Stop Loss: R$ 36.00 (+5.8%)\nTake Profit 1: R$ 30.50 (-9.8%)\nTake Profit 2: R$ 27.80 (-18.5%)",
                positionCalc: "Capital: R$ 10.000\nRisco: 2% (R$ 200)\nTamanho: 91 ações\nR/R: 1:3.2",
                riskReward: "1:3.2", winRate: "81%", maxDrawdown: "15.2%", volatility: "24.8%",
                
                alerts: [
                    { type: "danger", message: "Rompimento de suporte em R$ 32.50", time: "1 min" },
                    { type: "danger", message: "Volume de pânico detectado", time: "3 min" },
                    { type: "warning", message: "RSI em território de sobrevenda", time: "12 min" }
                ],
                signals: [
                    { type: "sell", strength: "Muito Forte", message: "Sinal de venda confirmado", confidence: "92%" },
                    { type: "breakdown", strength: "Forte", message: "Breakdown confirmado", confidence: "88%" }
                ],
                
                fearGreed: 18, fearGreedStatus: "Medo Extremo", fearGreedColor: "bg-red-600",
                socialSentiment: [
                    { platform: "Twitter", sentiment: "Bearish 78%", color: "text-red-600" },
                    { platform: "Reddit", sentiment: "Pânico 82%", color: "text-red-600" },
                    { platform: "News", sentiment: "Negativo 71%", color: "text-red-600" }
                ],
                
                confidence: "92%", accuracy: "85.7%", patterns: "15 detectados"
            }
        ];

        // Upload zone interactions
        uploadZone.addEventListener('click', () => imageInput.click());
        
        uploadZone.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadZone.classList.add('dragover');
        });
        
        uploadZone.addEventListener('dragleave', () => {
            uploadZone.classList.remove('dragover');
        });
        
        uploadZone.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadZone.classList.remove('dragover');
            const files = e.dataTransfer.files;
            if (files.length > 0) {
                handleImageUpload(files[0]);
            }
        });

        imageInput.addEventListener('change', (e) => {
            if (e.target.files.length > 0) {
                handleImageUpload(e.target.files[0]);
            }
        });

        function handleImageUpload(file) {
            if (!file.type.startsWith('image/')) {
                alert('Por favor, selecione apenas arquivos de imagem.');
                return;
            }

            const reader = new FileReader();
            reader.onload = (e) => {
                previewImg.src = e.target.result;
                uploadContent.classList.add('hidden');
                imagePreview.classList.remove('hidden');
            };
            reader.readAsDataURL(file);
        }

        analyzeBtn.addEventListener('click', () => {
            // Show loading
            analysisSection.classList.add('hidden');
            loadingSection.classList.remove('hidden');
            
            // Simulate analysis delay
            setTimeout(() => {
                performAnalysis();
                loadingSection.classList.add('hidden');
                analysisSection.classList.remove('hidden');
                
                // Scroll to results
                analysisSection.scrollIntoView({ behavior: 'smooth' });
            }, 3000);
        });

        function performAnalysis() {
            // Get random analysis data
            const analysis = analysisData[Math.floor(Math.random() * analysisData.length)];
            
            // AI Confidence Metrics
            document.getElementById('confidenceScore').textContent = analysis.confidence;
            document.getElementById('accuracyRate').textContent = analysis.accuracy;
            document.getElementById('patternsCount').textContent = analysis.patterns;
            
            // Main Analysis
            document.getElementById('trendAnalysis').innerHTML = `
                <div class="font-semibold text-lg mb-2 ${analysis.trend.includes('Alta') ? 'text-green-600' : analysis.trend.includes('Baixa') ? 'text-red-600' : 'text-yellow-600'}">${analysis.trend}</div>
                <p class="text-sm mb-2">${analysis.trendDetail}</p>
            `;
            
            document.getElementById('timeframeAnalysis').innerHTML = `
                <div class="text-xs bg-gray-100 p-2 rounded">
                    <strong>Multi-Timeframe:</strong><br>${analysis.timeframes}
                </div>
            `;
            
            document.getElementById('patternAnalysis').innerHTML = `
                <div class="font-semibold text-lg mb-2 text-blue-600">${analysis.pattern}</div>
                <p class="text-sm">${analysis.patternDetail}</p>
            `;
            
            document.getElementById('patternStrength').innerHTML = `
                <div class="text-xs bg-blue-50 p-2 rounded text-blue-800">
                    ${analysis.patternStrength}
                </div>
            `;
            
            document.getElementById('fibonacciAnalysis').innerHTML = `
                <div class="font-semibold text-lg mb-2 text-purple-600">${analysis.fibonacci}</div>
                <p class="text-sm">${analysis.fibDetail}</p>
            `;
            
            document.getElementById('supportResistance').innerHTML = `
                <div class="mb-2">
                    <span class="font-semibold text-red-600">${analysis.support}</span>
                </div>
                <div class="mb-2">
                    <span class="font-semibold text-green-600">${analysis.resistance}</span>
                </div>
            `;
            
            document.getElementById('dynamicLevels').innerHTML = `
                <div class="text-xs bg-gray-100 p-2 rounded">
                    ${analysis.dynamicLevels}
                </div>
            `;
            
            document.getElementById('volumeProfile').innerHTML = `
                <div class="font-semibold text-lg mb-2 text-indigo-600">Volume Profile</div>
                <div class="text-sm">${analysis.volumeProfile}</div>
            `;
            
            document.getElementById('recommendation').innerHTML = `
                <div class="font-semibold text-lg mb-2">${analysis.recommendation}</div>
                <p class="text-sm mb-2">${analysis.recommendationDetail}</p>
            `;
            
            document.getElementById('aiInsight').innerHTML = analysis.aiInsight;
            
            // Advanced Technical Indicators
            updateIndicator('rsi', analysis.rsi, analysis.rsiStatus);
            updateIndicator('stoch', analysis.stoch, analysis.stochStatus);
            updateIndicator('williams', analysis.williams, analysis.williamsStatus);
            updateIndicator('cci', analysis.cci, analysis.cciStatus);
            
            // MACD
            document.getElementById('macdValue').textContent = analysis.macd;
            document.getElementById('macdStatus').textContent = analysis.macdStatus;
            document.getElementById('macdHist').textContent = analysis.macdHist;
            document.getElementById('macdSignal').textContent = analysis.macdSignal;
            document.getElementById('macdSignal').className = `text-sm px-2 py-1 rounded-full ${
                analysis.macdSignal === 'BUY' ? 'bg-green-100 text-green-800' : 
                analysis.macdSignal === 'SELL' ? 'bg-red-100 text-red-800' : 
                'bg-gray-100 text-gray-800'
            }`;
            
            // ADX
            document.getElementById('adxValue').textContent = analysis.adx;
            document.getElementById('adxStrength').textContent = analysis.adxStrength;
            document.getElementById('adxStrength').className = `text-sm px-2 py-1 rounded-full ${
                parseFloat(analysis.adx) > 50 ? 'bg-green-100 text-green-800' : 
                parseFloat(analysis.adx) > 25 ? 'bg-yellow-100 text-yellow-800' : 
                'bg-gray-100 text-gray-800'
            }`;
            document.getElementById('plusDI').textContent = analysis.plusDI;
            document.getElementById('minusDI').textContent = analysis.minusDI;
            
            // Parabolic SAR
            document.getElementById('sarValue').textContent = analysis.sar;
            document.getElementById('sarTrend').textContent = analysis.sarTrend;
            document.getElementById('sarTrend').className = `text-sm px-2 py-1 rounded-full ${
                analysis.sarTrend === 'BULLISH' ? 'bg-green-100 text-green-800' : 
                analysis.sarTrend === 'BEARISH' ? 'bg-red-100 text-red-800' : 
                'bg-gray-100 text-gray-800'
            }`;
            document.getElementById('sarStatus').textContent = analysis.sarStatus;
            
            // Volume & Volatility
            document.getElementById('volumeValue').textContent = analysis.volume;
            document.getElementById('volumeStatus').textContent = analysis.volumeStatus;
            document.getElementById('obvValue').textContent = analysis.obv;
            document.getElementById('obvStatus').textContent = analysis.obvStatus;
            document.getElementById('bbValue').textContent = analysis.bb;
            document.getElementById('bbStatus').textContent = analysis.bbStatus;
            document.getElementById('atrValue').textContent = analysis.atr;
            document.getElementById('atrStatus').textContent = analysis.atrStatus;
            
            // Alerts
            const alertsContainer = document.getElementById('activeAlerts');
            alertsContainer.innerHTML = '';
            analysis.alerts.forEach(alert => {
                const alertDiv = document.createElement('div');
                alertDiv.className = `p-3 rounded-lg border-l-4 ${
                    alert.type === 'success' ? 'bg-green-50 border-green-400 text-green-800' :
                    alert.type === 'warning' ? 'bg-yellow-50 border-yellow-400 text-yellow-800' :
                    alert.type === 'danger' ? 'bg-red-50 border-red-400 text-red-800' :
                    'bg-blue-50 border-blue-400 text-blue-800'
                }`;
                alertDiv.innerHTML = `
                    <div class="flex justify-between items-start">
                        <div class="text-sm font-medium">${alert.message}</div>
                        <div class="text-xs opacity-75">${alert.time}</div>
                    </div>
                `;
                alertsContainer.appendChild(alertDiv);
            });
            
            // Trading Signals
            const signalsContainer = document.getElementById('tradingSignals');
            signalsContainer.innerHTML = '';
            analysis.signals.forEach(signal => {
                const signalDiv = document.createElement('div');
                signalDiv.className = `p-3 rounded-lg border ${
                    signal.type === 'buy' ? 'bg-green-50 border-green-200' :
                    signal.type === 'sell' ? 'bg-red-50 border-red-200' :
                    'bg-gray-50 border-gray-200'
                }`;
                signalDiv.innerHTML = `
                    <div class="flex justify-between items-center mb-1">
                        <div class="font-medium text-sm">${signal.message}</div>
                        <div class="text-xs bg-white px-2 py-1 rounded">${signal.confidence}</div>
                    </div>
                    <div class="text-xs opacity-75">Força: ${signal.strength}</div>
                `;
                signalsContainer.appendChild(signalDiv);
            });
            
            // Risk Management
            document.getElementById('positionCalc').innerHTML = analysis.positionCalc.replace(/\n/g, '<br>');
            document.getElementById('entryPoints').innerHTML = analysis.entry.replace(/\n/g, '<br>');
            document.getElementById('stopLoss').innerHTML = analysis.stopLoss.replace(/\n/g, '<br>');
            
            // Risk Metrics
            document.getElementById('riskReward').textContent = analysis.riskReward;
            document.getElementById('winRate').textContent = analysis.winRate;
            document.getElementById('maxDrawdown').textContent = analysis.maxDrawdown;
            document.getElementById('volatility').textContent = analysis.volatility;
            
            // Fear & Greed Index
            document.getElementById('fearGreedValue').textContent = analysis.fearGreed;
            document.getElementById('fearGreedStatus').textContent = analysis.fearGreedStatus;
            const fearGreedBar = document.getElementById('fearGreedBar');
            fearGreedBar.style.width = analysis.fearGreed + '%';
            fearGreedBar.className = `h-4 rounded-full transition-all duration-1000 ${analysis.fearGreedColor}`;
            
            // Social Sentiment
            const socialContainer = document.getElementById('socialSentiment');
            socialContainer.innerHTML = '';
            analysis.socialSentiment.forEach(social => {
                const socialDiv = document.createElement('div');
                socialDiv.className = 'flex justify-between items-center p-2 bg-gray-50 rounded';
                socialDiv.innerHTML = `
                    <span class="font-medium">${social.platform}</span>
                    <span class="${social.color} font-semibold">${social.sentiment}</span>
                `;
                socialContainer.appendChild(socialDiv);
            });
        }
        
        function updateIndicator(id, value, status) {
            document.getElementById(id + 'Value').textContent = value;
            document.getElementById(id + 'Status').textContent = status;
            
            // Update progress bar for oscillators
            const bar = document.getElementById(id + 'Bar');
            if (bar) {
                let percentage = 0;
                if (id === 'rsi' || id === 'stoch') {
                    percentage = parseFloat(value);
                } else if (id === 'williams') {
                    percentage = 100 + parseFloat(value); // Williams %R is negative
                } else if (id === 'cci') {
                    percentage = Math.min(100, Math.max(0, (parseFloat(value) + 200) / 4)); // Normalize CCI
                }
                bar.firstElementChild.style.width = percentage + '%';
            }
        }

        // Demo analysis button for testing
        setTimeout(() => {
            const demoBtn = document.createElement('button');
            demoBtn.textContent = '🎯 Ver Análise Demo';
            demoBtn.className = 'fixed bottom-6 right-6 bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-full shadow-lg transition-colors text-sm font-semibold';
            demoBtn.addEventListener('click', () => {
                loadingSection.classList.remove('hidden');
                setTimeout(() => {
                    performAnalysis();
                    loadingSection.classList.add('hidden');
                    analysisSection.classList.remove('hidden');
                    analysisSection.scrollIntoView({ behavior: 'smooth' });
                }, 2000);
            });
            document.body.appendChild(demoBtn);
        }, 1000);
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'96d08fcfc5b40ed6',t:'MTc1NDg0MDExMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
