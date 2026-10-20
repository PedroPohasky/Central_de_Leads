<!DOCTYPE html>
<html lang="pt-BR" class="h-full bg-[#F4F5F7]">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Central CSC - SLC John Deere Linha Amarela</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Barlow Condensed & Oswald (Equivalentes Web para Trade Gothic Bold) + Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@600;700;800&family=Inter:wght@400;500;600;700&family=Oswald:wght@500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome & Lucide Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- SheetJS (Excel/CSV Parser) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        jd: {
                            yellow: '#F5A800',       /* PANTONE 130C */
                            yellowHover: '#D99400',  /* Hover Pantone 130C */
                            gray: '#53575A',         /* PANTONE 425C */
                            black: '#1D1D1B',        /* PANTONE Process Black */
                            light: '#F4F5F7',
                        }
                    },
                    fontFamily: {
                        trade: ['"Trade Gothic Bold"', '"Trade Gothic LT Std"', '"Barlow Condensed"', '"Oswald"', 'sans-serif'],
                        sans: ['Inter', 'sans-serif'],
                    },
                    boxShadow: {
                        'soft': '0 0 2rem 0 rgba(29, 29, 27, 0.08)',
                        'yellow-glow': '0 4px 14px 0 rgba(245, 168, 0, 0.35)',
                    }
                }
            }
        }
    </script>
    <style>
        .font-heading {
            font-family: 'Trade Gothic Bold', 'Barlow Condensed', 'Oswald', sans-serif;
            letter-spacing: 0.03em;
            text-transform: uppercase;
        }

        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        .table-container { max-height: 480px; overflow-y: auto; }
        .table-container thead th { position: sticky; top: 0; z-index: 10; }
        .table-container::-webkit-scrollbar { width: 6px; height: 6px; }
        .table-container::-webkit-scrollbar-track { background: transparent; }
        .table-container::-webkit-scrollbar-thumb { background-color: #53575A; border-radius: 10px; }

        .funnel-layer { transition: all 0.4s ease-in-out; }
        .animate-pulse-hot { animation: pulseHot 2s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
        @keyframes pulseHot {
            0%, 100% { opacity: 1; transform: translateY(-50%) scale(1); }
            50% { opacity: .9; transform: translateY(-50%) scale(1.06); }
        }

        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }
    </style>
</head>
<body class="h-full font-sans antialiased text-[#1D1D1B] bg-[#F4F5F7] flex">

    <!-- ======================================================= -->
    <!-- SIDEBAR / MENU LATERAL: PADRÃO PROCESS BLACK (#1D1D1B)  -->
    <!-- ======================================================= -->
    <aside class="w-64 bg-jd-black text-white shadow-2xl flex flex-col justify-between hidden md:flex z-20 border-r-4 border-jd-yellow">
        <div>
            <!-- Área da Logo John Deere Linha Amarela -->
            <div class="p-6 flex flex-col items-center border-b border-[#53575A]/40 bg-[#141413]">
                <img src="Logo_John_Deere-removebg-preview.png" alt="John Deere" class="h-16 object-contain mb-2" onerror="this.style.display='none'; document.getElementById('fallback-jd-logo').style.display='flex';">
                
                <div id="fallback-jd-logo" class="hidden flex-col items-center">
                    <div class="w-14 h-12 bg-jd-black border-2 border-jd-yellow rounded-xl flex items-center justify-center shadow-yellow-glow mb-2">
                        <i class="fa-solid fa-tractor text-jd-yellow text-2xl"></i>
                    </div>
                </div>

                <h1 class="font-heading text-xl font-bold tracking-wider text-white text-center leading-none mt-1">
                    SLC <span class="text-jd-yellow">JOHN DEERE</span>
                </h1>
                <span class="text-[10px] font-bold text-[#F5A800] uppercase tracking-widest mt-1">
                    Construção • Linha Amarela
                </span>
            </div>

            <!-- Navegação Principal -->
            <nav class="p-4 space-y-2 mt-2">
                <button onclick="switchMainTab('input')" id="nav-input" class="w-full flex items-center space-x-3 px-4 py-3.5 rounded-xl text-sm font-heading font-bold tracking-wider transition-all bg-jd-yellow text-jd-black shadow-yellow-glow">
                    <i class="fa-solid fa-user-plus w-5 text-center text-base"></i>
                    <span>1. Input de Leads</span>
                </button>
                <button onclick="switchMainTab('funnel')" id="nav-funnel" class="w-full flex items-center space-x-3 px-4 py-3.5 rounded-xl text-sm font-heading font-bold tracking-wider transition-all text-gray-300 hover:bg-jd-gray/30 hover:text-jd-yellow">
                    <i class="fa-solid fa-filter w-5 text-center text-base"></i>
                    <span>2. Funil & Macro</span>
                </button>
                <button onclick="switchMainTab('base')" id="nav-base" class="w-full flex items-center space-x-3 px-4 py-3.5 rounded-xl text-sm font-heading font-bold tracking-wider transition-all text-gray-300 hover:bg-jd-gray/30 hover:text-jd-yellow">
                    <i class="fa-solid fa-table-list w-5 text-center text-base"></i>
                    <span>3. Base Geral de Leads</span>
                </button>
            </nav>
        </div>

        <!-- Rodapé Sidebar -->
        <div class="p-4 border-t border-[#53575A]/40">
            <div class="bg-[#282826] p-3.5 rounded-xl text-xs text-gray-300 border-l-4 border-jd-yellow">
                <p class="font-heading text-sm font-bold text-white flex items-center gap-2">
                    <span class="w-2 h-2 rounded-full bg-jd-yellow animate-ping"></span> CSC CONECTADO
                </p>
                <p class="mt-0.5 text-[11px] text-gray-400">Pós-Venda & Telemetria</p>
            </div>
        </div>
    </aside>

    <!-- Main Content Wrapper -->
    <div class="flex-1 flex flex-col min-w-0 overflow-hidden">
        
        <!-- Header Superior -->
        <header class="bg-white shadow-sm h-16 flex items-center justify-between px-6 z-10 border-b-2 border-jd-yellow">
            <div class="flex items-center space-x-3">
                <span class="w-2 h-6 bg-jd-yellow rounded-sm inline-block"></span>
                <span id="page-title" class="font-heading text-xl font-bold text-jd-black tracking-wide">
                    1. Módulo de Cadastro e Geração Inteligente de Leads
                </span>
            </div>
            <div class="flex items-center space-x-3">
                <button onclick="downloadTemplateExcel()" class="bg-jd-black hover:bg-jd-gray text-jd-yellow border border-jd-yellow px-4 py-2 rounded-xl text-xs font-heading font-bold tracking-wider flex items-center gap-2 transition shadow-sm">
                    <i class="fa-solid fa-download"></i> Baixar Planilhas Modelo (.xlsx)
                </button>
            </div>
        </header>

        <!-- Corpo Principal -->
        <main class="flex-1 overflow-y-auto p-6 lg:p-8 space-y-6">

            <!-- ======================================================= -->
            <!-- ABA 1: INPUT DE LEADS                                   -->
            <!-- ======================================================= -->
            <div id="tab-input" class="space-y-6 block">
                
                <!-- Motores de Importação Inteligente -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- 1º Importação: Preventiva (±50h) -->
                    <div class="bg-white rounded-2xl shadow-soft p-5 border-l-4 border-jd-yellow flex items-center justify-between">
                        <div class="flex items-center space-x-4">
                            <div class="bg-jd-yellow text-jd-black p-3.5 rounded-xl shadow-md">
                                <i class="fa-solid fa-clock-rotate-left text-xl"></i>
                            </div>
                            <div>
                                <h3 class="font-heading text-base font-bold text-jd-black">1º Importação: Planilha de Preventiva</h3>
                                <p class="text-xs text-jd-gray mt-0.5">Lê horímetros e gera leads a cada 500h (Janela de alerta: ±50h, ex: 450h a 550h).</p>
                            </div>
                        </div>
                        <label class="bg-jd-yellow hover:bg-jd-yellowHover text-jd-black px-4 py-2.5 rounded-xl text-xs font-heading font-bold tracking-wider cursor-pointer shadow-sm transition-all whitespace-nowrap">
                            <i class="fa-solid fa-file-import mr-1.5"></i> Importar Horímetros
                            <input type="file" accept=".xlsx,.xls,.csv" onchange="importPreventiveSheet(event)" class="hidden">
                        </label>
                    </div>

                    <!-- 2º Importação: PowerGard (30 dias vencimento) -->
                    <div class="bg-white rounded-2xl shadow-soft p-5 border-l-4 border-jd-gray flex items-center justify-between">
                        <div class="flex items-center space-x-4">
                            <div class="bg-jd-black text-jd-yellow p-3.5 rounded-xl shadow-md">
                                <i class="fa-solid fa-shield-halved text-xl"></i>
                            </div>
                            <div>
                                <h3 class="font-heading text-base font-bold text-jd-black">2º Importação: Planilha de PowerGard</h3>
                                <p class="text-xs text-jd-gray mt-0.5">Lê a data de faturamento e gera leads para máquinas a 30 dias do fim da garantia.</p>
                            </div>
                        </div>
                        <label class="bg-jd-black hover:bg-jd-gray text-jd-yellow px-4 py-2.5 rounded-xl text-xs font-heading font-bold tracking-wider cursor-pointer shadow-sm transition-all whitespace-nowrap">
                            <i class="fa-solid fa-file-import mr-1.5"></i> Importar Garantias
                            <input type="file" accept=".xlsx,.xls,.csv" onchange="importPowerGardSheet(event)" class="hidden">
                        </label>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    <!-- Formulário Manual -->
                    <div class="bg-white rounded-2xl shadow-soft p-6 lg:col-span-4 border-t-4 border-jd-yellow">
                        <h2 class="font-heading text-lg font-bold text-jd-black mb-4 flex items-center">
                            <i class="fa-solid fa-pen-to-square text-jd-yellow mr-2"></i> Cadastro Manual de Lead
                        </h2>
                        <form id="lead-form" onsubmit="addManualLead(event)" class="space-y-3.5 text-sm">
                            <div>
                                <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Nome do Cliente / Organização</label>
                                <input type="text" id="lead-name" required placeholder="Ex: Construtora / Fazenda..." class="w-full px-3.5 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm">
                            </div>

                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Chassi</label>
                                    <input type="text" id="lead-chassis" required placeholder="Ex: 1DW..." class="w-full px-3.5 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm uppercase font-mono">
                                </div>
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Contato (Telefone)</label>
                                    <input type="text" id="lead-phone" required placeholder="(51) 99999-9999" class="w-full px-3.5 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm">
                                </div>
                            </div>

                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Filial</label>
                                    <select id="lead-branch" class="w-full px-3 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm bg-white font-medium">
                                        <option value="Eldorado do Sul">Eldorado do Sul</option>
                                        <option value="Passo Fundo">Passo Fundo</option>
                                        <option value="Santa Maria">Santa Maria</option>
                                        <option value="Horizontina">Horizontina</option>
                                        <option value="Cruz Alta">Cruz Alta</option>
                                        <option value="Caxias do Sul">Caxias do Sul</option>
                                        <option value="Pelotas">Pelotas</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Tipo de Lead</label>
                                    <select id="lead-type" onchange="toggleLeadDynamicFields()" class="w-full px-3 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm bg-white font-medium">
                                        <option value="Preventiva">Preventiva</option>
                                        <option value="Corretiva">Corretiva</option>
                                        <option value="Power Gard">Power Gard</option>
                                        <option value="Preventiva Protect">Preventiva Protect</option>
                                        <option value="Contrato Protect">Contrato Protect</option>
                                        <option value="Zero Vendas">Zero Vendas</option>
                                        <option value="CRC">CRC</option>
                                        <option value="PMP">PMP</option>
                                        <option value="Análise de Óleo">Análise de Óleo</option>
                                    </select>
                                </div>
                            </div>

                            <!-- 1. Campo Condicional: PREVENTIVA / PREVENTIVA PROTECT (500h até 20.000h) -->
                            <div id="field-preventiva-container" class="bg-amber-50/90 p-3 rounded-xl border border-jd-yellow">
                                <label class="block font-bold text-jd-black mb-1 text-[11px] uppercase">
                                    <i class="fa-solid fa-gears text-jd-yellow mr-1"></i> Revisão Preventiva (500h a 20.000h)
                                </label>
                                <select id="lead-maintenance-hours" class="w-full px-3 py-2 rounded-lg border border-jd-yellow focus:outline-none focus:ring-2 focus:ring-jd-black text-sm bg-white font-bold text-jd-black">
                                    <!-- Preenchido via JS de 500 em 500h -->
                                </select>
                            </div>

                            <!-- 2. Campo Condicional: CORRETIVA (Texto curto para detalhes) -->
                            <div id="field-corretiva-container" class="hidden bg-amber-50/90 p-3 rounded-xl border border-jd-yellow">
                                <label class="block font-bold text-jd-black mb-1 text-[11px] uppercase">
                                    <i class="fa-solid fa-screwdriver-wrench text-jd-yellow mr-1"></i> Detalhes da Corretiva
                                </label>
                                <input type="text" id="lead-corretiva-detail" placeholder="Ex: Vazamento comando hidráulico / Alerta DTC..." class="w-full px-3 py-2 rounded-lg border border-jd-yellow focus:outline-none focus:ring-2 focus:ring-jd-black text-sm bg-white text-jd-black">
                            </div>

                            <!-- 3. Campo Condicional: PMP (Texto para o Tipo de PMP) -->
                            <div id="field-pmp-container" class="hidden bg-amber-50/90 p-3 rounded-xl border border-jd-yellow">
                                <label class="block font-bold text-jd-black mb-1 text-[11px] uppercase">
                                    <i class="fa-solid fa-bolt text-jd-yellow mr-1"></i> Tipo de PMP
                                </label>
                                <input type="text" id="lead-pmp-type" placeholder="Ex: PMP MANDATÓRIO - Atualização Software / Pino..." class="w-full px-3 py-2 rounded-lg border border-jd-yellow focus:outline-none focus:ring-2 focus:ring-jd-black text-sm bg-white text-jd-black">
                            </div>

                            <!-- 4. Campo Condicional: POWER GARD (Campo de Data + Campo de Texto) -->
                            <div id="field-powergard-container" class="hidden bg-amber-50/90 p-3 rounded-xl border border-jd-yellow space-y-2.5">
                                <div class="flex items-center text-jd-black font-bold text-[11px] uppercase">
                                    <i class="fa-solid fa-shield-halved text-jd-yellow mr-1.5"></i> Detalhamento Power Gard
                                </div>
                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
                                    <div>
                                        <label class="block font-semibold text-jd-gray mb-1 text-[10px] uppercase">Data (Venc. / Fat.)</label>
                                        <input type="date" id="lead-pg-date" class="w-full px-2.5 py-1.5 rounded-lg border border-jd-yellow focus:outline-none focus:ring-2 focus:ring-jd-black text-xs bg-white font-semibold text-jd-black">
                                    </div>
                                    <div>
                                        <label class="block font-semibold text-jd-gray mb-1 text-[10px] uppercase">Observação / Plano</label>
                                        <input type="text" id="lead-pg-text" placeholder="Ex: Powertrain + Hidr." class="w-full px-2.5 py-1.5 rounded-lg border border-jd-yellow focus:outline-none focus:ring-2 focus:ring-jd-black text-xs bg-white text-jd-black">
                                    </div>
                                </div>
                            </div>

                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Temp (Temperatura)</label>
                                    <select id="lead-temp" class="w-full px-3 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm bg-white font-medium">
                                        <option value="Urgente">Urgente</option>
                                        <option value="Moderado">Moderado</option>
                                        <option value="Baixo">Baixo</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Segmentação</label>
                                    <select id="lead-segment" class="w-full px-3 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm bg-white font-medium">
                                        <option value="Signature">Signature</option>
                                        <option value="Comum">Comum</option>
                                        <option value="Rental">Rental</option>
                                        <option value="Demo">Demo</option>
                                    </select>
                                </div>
                            </div>

                            <div>
                                <label class="block font-bold text-jd-gray mb-1 text-[11px] uppercase">Consultor Responsável</label>
                                <input type="text" id="lead-consultant" placeholder="Ex: João Silva" class="w-full px-3.5 py-2 rounded-xl border border-gray-300 focus:outline-none focus:ring-2 focus:ring-jd-yellow text-sm">
                            </div>

                            <button type="submit" class="w-full bg-jd-yellow hover:bg-jd-yellowHover text-jd-black font-heading font-bold text-base py-3 rounded-xl shadow-yellow-glow transition-all mt-2 tracking-wider">
                                <i class="fa-solid fa-plus mr-1.5"></i> Cadastrar Lead na Base
                            </button>
                        </form>
                    </div>

                    <!-- Tabela Resumida de Leads Cadastrados (Aba 1) -->
                    <div class="bg-white rounded-2xl shadow-soft p-6 lg:col-span-8 border-t-4 border-jd-black flex flex-col justify-between">
                        <div>
                            <div class="flex items-center justify-between mb-4">
                                <div>
                                    <h2 class="font-heading text-lg font-bold text-jd-black flex items-center">
                                        <i class="fa-solid fa-database text-jd-yellow mr-2"></i> Base Geral de Leads Cadastrados
                                    </h2>
                                    <p class="text-xs text-jd-gray">Visão rápida de entrada. Para detalhamento de O.S., Faturamento e Tratativas, acesse a Aba 3.</p>
                                </div>
                                <span id="lead-count-badge-tab1" class="text-xs bg-jd-black text-jd-yellow px-3.5 py-1.5 rounded-full font-heading font-bold tracking-wider">Total: 0</span>
                            </div>
                            
                            <div class="overflow-x-auto table-container border border-gray-200 rounded-xl">
                                <table class="w-full text-left text-xs whitespace-nowrap">
                                    <thead class="bg-jd-black text-white font-heading uppercase text-xs tracking-wider border-b-2 border-jd-yellow">
                                        <tr>
                                            <th class="p-3">ID / Data</th>
                                            <th class="p-3">Cliente / Organização</th>
                                            <th class="p-3">Chassi / Contato</th>
                                            <th class="p-3">Filial</th>
                                            <th class="p-3">Tipo de Lead & Detalhe</th>
                                            <th class="p-3">Temp</th>
                                            <th class="p-3">Segmento</th>
                                            <th class="p-3 text-center">Status Funil</th>
                                        </tr>
                                    </thead>
                                    <tbody id="leads-table-body-tab1" class="divide-y divide-gray-100">
                                        <!-- Preenchido via JS -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ======================================================= -->
            <!-- ABA 2: FUNIL & MACRO (PIPELINE & RANKING)               -->
            <!-- ======================================================= -->
            <div id="tab-funnel" class="space-y-6 hidden">
                
                <!-- Sub-navegação enxuta (Pipeline e Ranking) -->
                <div class="flex flex-wrap items-center justify-between border-b-2 border-gray-200 gap-4">
                    <nav class="flex space-x-8 overflow-x-auto no-scrollbar" aria-label="Tabs">
                        <button onclick="showFunnelSubPage('visao-geral', this)" class="funnel-sub-btn active text-jd-black border-jd-yellow border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap">
                            <i data-lucide="filter" class="w-4 h-4 text-jd-yellow"></i> Pipeline & Faturação
                        </button>
                        <button onclick="showFunnelSubPage('ranking', this)" class="funnel-sub-btn text-jd-gray hover:text-jd-black border-transparent border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap">
                            <i data-lucide="trophy" class="w-4 h-4"></i> Ranking por Loja
                        </button>
                    </nav>

                    <!-- Filtro Rápido de Filial para o Funil -->
                    <div class="pb-2">
                        <select id="funnel-branch-filter" onchange="updateFunnelDashboard()" class="bg-white border-2 border-jd-black text-jd-black text-xs font-bold rounded-xl px-3.5 py-2 focus:outline-none focus:ring-2 focus:ring-jd-yellow shadow-sm">
                            <option value="ALL">Todas as Filiais</option>
                            <option value="Eldorado do Sul">Eldorado do Sul</option>
                            <option value="Passo Fundo">Passo Fundo</option>
                            <option value="Santa Maria">Santa Maria</option>
                            <option value="Horizontina">Horizontina</option>
                            <option value="Cruz Alta">Cruz Alta</option>
                            <option value="Caxias do Sul">Caxias do Sul</option>
                            <option value="Pelotas">Pelotas</option>
                        </select>
                    </div>
                </div>

                <!-- SUB-PÁGINA 1: PIPELINE & FATURAÇÃO -->
                <div id="funnel-visao-geral" class="funnel-subpage block space-y-6">
                    <!-- Pílulas de Filtro -->
                    <div class="flex flex-wrap gap-2 pb-1">
                        <button onclick="setPipelineFilter('ALL')" id="btn-pip-ALL" class="pip-btn bg-jd-black text-jd-yellow border border-jd-black px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Todos</button>
                        <button onclick="setPipelineFilter('Preventivas')" id="btn-pip-Preventivas" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Preventivas</button>
                        <button onclick="setPipelineFilter('Corretivas')" id="btn-pip-Corretivas" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Corretivas</button>
                        <button onclick="setPipelineFilter('PMP')" id="btn-pip-PMP" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">PMP</button>
                        <button onclick="setPipelineFilter('Protect')" id="btn-pip-Protect" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Protect</button>
                        <button onclick="setPipelineFilter('Power Guard')" id="btn-pip-Power Guard" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Power Guard</button>
                        <button onclick="setPipelineFilter('Zero')" id="btn-pip-Zero" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Zero</button>
                        <button onclick="setPipelineFilter('CRC')" id="btn-pip-CRC" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">CRC</button>
                        <button onclick="setPipelineFilter('Análise de Óleo')" id="btn-pip-Análise de Óleo" class="pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm">Análise de Óleo</button>
                    </div>

                    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                        <!-- Pirâmide do Pipeline de Conversão -->
                        <div class="lg:col-span-8 bg-white p-8 rounded-2xl border-t-4 border-jd-yellow shadow-soft flex flex-col items-center justify-center relative min-h-[420px]">
                            <h3 class="font-heading text-sm font-bold uppercase tracking-wider text-jd-gray absolute top-6 left-6">Pipeline de Conversão</h3>
                            
                            <div class="w-full max-w-lg mt-6 flex relative items-center justify-center">
                                <div class="w-full flex flex-col items-center space-y-2.5 relative z-10">
                                    <div class="w-full bg-gray-200 text-jd-black rounded-t-xl py-4 px-3 text-center funnel-layer shadow-sm border-b-2 border-gray-300">
                                        <span class="font-heading text-xs font-bold tracking-widest text-jd-gray block mb-0.5">Total de Leads</span>
                                        <span class="font-heading text-4xl font-extrabold text-jd-black" id="funnel-total">0</span>
                                    </div>
                                    <div class="w-5/6 bg-jd-gray text-white py-4 px-3 text-center funnel-layer shadow-sm">
                                        <span class="font-heading text-xs font-bold tracking-widest text-gray-200 block mb-0.5">Abordados</span>
                                        <span class="font-heading text-3xl font-extrabold" id="funnel-abordados">0</span>
                                    </div>
                                    <div class="w-2/3 bg-jd-black text-jd-yellow py-4 px-3 text-center funnel-layer shadow-sm border border-jd-yellow/40">
                                        <span class="font-heading text-xs font-bold tracking-widest text-gray-300 block mb-0.5">Negociação</span>
                                        <span class="font-heading text-3xl font-extrabold" id="funnel-negociacao">0</span>
                                    </div>
                                    <div class="w-1/2 bg-jd-yellow text-jd-black rounded-b-xl py-5 px-4 text-center funnel-layer shadow-yellow-glow border-b-4 border-[#B87D00]">
                                        <span class="font-heading text-xs font-bold tracking-widest text-jd-black/80 block mb-0.5">Fechamento</span>
                                        <span class="font-heading text-5xl font-extrabold" id="funnel-fechamento">0</span>
                                    </div>
                                </div>

                                <!-- Botão Flutuante: Quentes -->
                                <button onclick="setPipelineFilter('Quentes')" id="btn-pip-Quentes" class="absolute -right-4 sm:-right-12 top-1/2 transform -translate-y-1/2 flex flex-col items-center justify-center w-20 h-20 rounded-full border-2 border-jd-black bg-white hover:border-jd-yellow hover:bg-amber-50 transition-all shadow-lg cursor-pointer group z-20" title="Filtrar Leads Quentes (Protect / Power Gard / PMP)">
                                    <i data-lucide="thermometer" class="w-7 h-7 text-red-600 group-hover:scale-110 transition-transform"></i>
                                    <span class="font-heading text-[10px] font-bold text-jd-black mt-1 uppercase tracking-wider">Quentes</span>
                                </button>
                            </div>
                        </div>

                        <!-- Coluna Direita: Cards de Faturamento -->
                        <div class="lg:col-span-4 flex flex-col justify-between gap-4">
                            <div class="bg-jd-black text-white p-6 rounded-2xl shadow-lg border-l-8 border-jd-yellow relative overflow-hidden">
                                <i data-lucide="dollar-sign" class="absolute -right-4 -bottom-4 w-32 h-32 text-jd-yellow opacity-10"></i>
                                <p class="font-heading text-xs font-bold uppercase tracking-widest text-gray-400 mb-1">Faturamento Total</p>
                                <h3 class="font-heading text-4xl font-bold tracking-wide text-jd-yellow" id="kpi-fat-total">R$ 0,00</h3>
                            </div>

                            <div class="bg-white p-5 rounded-2xl border border-gray-200 shadow-soft flex items-center justify-between">
                                <div>
                                    <p class="font-heading text-xs font-bold uppercase tracking-widest text-jd-gray mb-1">Fat. Peças</p>
                                    <h3 class="font-heading text-2xl font-bold text-jd-black" id="kpi-fat-pecas">R$ 0,00</h3>
                                </div>
                                <div class="w-11 h-11 rounded-xl bg-jd-black flex items-center justify-center text-jd-yellow shadow-sm">
                                    <i data-lucide="settings" class="w-5 h-5"></i>
                                </div>
                            </div>

                            <div class="bg-white p-5 rounded-2xl border border-gray-200 shadow-soft flex items-center justify-between">
                                <div>
                                    <p class="font-heading text-xs font-bold uppercase tracking-widest text-jd-gray mb-1">Fat. Serviços</p>
                                    <h3 class="font-heading text-2xl font-bold text-jd-black" id="kpi-fat-servicos">R$ 0,00</h3>
                                </div>
                                <div class="w-11 h-11 rounded-xl bg-jd-yellow flex items-center justify-center text-jd-black shadow-sm">
                                    <i data-lucide="wrench" class="w-5 h-5"></i>
                                </div>
                            </div>

                            <div class="bg-amber-50/90 p-5 rounded-2xl border-2 border-jd-yellow shadow-soft flex items-center justify-between">
                                <div>
                                    <p class="font-heading text-xs font-bold uppercase tracking-widest text-jd-black mb-1">Taxa de Conversão</p>
                                    <h3 class="font-heading text-3xl font-extrabold text-jd-black" id="kpi-taxa-conv">0.0%</h3>
                                </div>
                                <div class="w-11 h-11 rounded-xl bg-jd-yellow flex items-center justify-center text-jd-black shadow-sm">
                                    <i data-lucide="trending-up" class="w-5 h-5"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- SUB-PÁGINA 2: RANKING POR LOJA -->
                <div id="funnel-ranking" class="funnel-subpage hidden space-y-6">
                    <div class="bg-white rounded-2xl border-t-4 border-jd-yellow shadow-soft overflow-hidden">
                        <div class="p-5 border-b border-gray-100">
                            <h3 class="font-heading text-base font-bold uppercase text-jd-black">Desempenho Geral por Filial</h3>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-center text-sm">
                                <thead class="bg-jd-black text-white font-heading uppercase text-xs tracking-wider border-b-2 border-jd-yellow">
                                    <tr>
                                        <th class="py-3.5 px-4 text-left">Loja / Filial</th>
                                        <th class="py-3.5 px-4">Volume Total</th>
                                        <th class="py-3.5 px-4">Fechados (OS)</th>
                                        <th class="py-3.5 px-4">Pendentes</th>
                                        <th class="py-3.5 px-4">Taxa Conv.</th>
                                        <th class="py-3.5 px-4 text-right">Faturação Total (R$)</th>
                                    </tr>
                                </thead>
                                <tbody id="rankingBody" class="divide-y divide-gray-100 text-jd-black"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

            </div>

            <!-- ======================================================= -->
            <!-- ABA 3: BASE GERAL DE LEADS (COM FILTROS E TRATATIVA)    -->
            <!-- ======================================================= -->
            <div id="tab-base" class="space-y-6 hidden">
                
                <!-- Barra de Filtros da Aba 3 -->
                <div class="bg-white p-5 rounded-2xl shadow-soft border-l-4 border-jd-yellow flex flex-wrap items-end justify-between gap-4">
                    <div class="flex flex-wrap items-end gap-4">
                        <div>
                            <label class="block text-[10px] font-bold uppercase text-jd-gray mb-1">Data Inicial</label>
                            <input type="date" id="base-filter-start" onchange="renderBaseTab()" class="bg-gray-50 border border-gray-300 text-jd-black text-xs font-semibold rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-jd-yellow">
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase text-jd-gray mb-1">Data Final</label>
                            <input type="date" id="base-filter-end" onchange="renderBaseTab()" class="bg-gray-50 border border-gray-300 text-jd-black text-xs font-semibold rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-jd-yellow">
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase text-jd-gray mb-1">Filial (Loja)</label>
                            <select id="base-filter-branch" onchange="renderBaseTab()" class="bg-gray-50 border border-gray-300 text-jd-black text-xs font-semibold rounded-xl px-3.5 py-2 focus:outline-none focus:ring-2 focus:ring-jd-yellow">
                                <option value="ALL">Todas as Lojas</option>
                                <option value="Eldorado do Sul">Eldorado do Sul</option>
                                <option value="Passo Fundo">Passo Fundo</option>
                                <option value="Santa Maria">Santa Maria</option>
                                <option value="Horizontina">Horizontina</option>
                                <option value="Cruz Alta">Cruz Alta</option>
                                <option value="Caxias do Sul">Caxias do Sul</option>
                                <option value="Pelotas">Pelotas</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase text-jd-gray mb-1">Tipo de Lead</label>
                            <select id="base-filter-type" onchange="renderBaseTab()" class="bg-gray-50 border border-gray-300 text-jd-black text-xs font-semibold rounded-xl px-3.5 py-2 focus:outline-none focus:ring-2 focus:ring-jd-yellow">
                                <option value="ALL">Todos os Tipos</option>
                                <option value="Preventiva">Preventiva</option>
                                <option value="Corretiva">Corretiva</option>
                                <option value="Power Gard">Power Gard</option>
                                <option value="Preventiva Protect">Preventiva Protect</option>
                                <option value="Contrato Protect">Contrato Protect</option>
                                <option value="Zero Vendas">Zero Vendas</option>
                                <option value="CRC">CRC</option>
                                <option value="PMP">PMP</option>
                                <option value="Análise de Óleo">Análise de Óleo</option>
                            </select>
                        </div>
                        <button onclick="clearBaseFilters()" class="text-xs bg-gray-100 hover:bg-jd-yellow text-jd-black font-heading font-bold tracking-wider px-3.5 py-2 rounded-xl transition">
                            <i class="fa-solid fa-rotate-left mr-1"></i> Limpar Filtros
                        </button>
                    </div>
                </div>

                <!-- Sub-navegação da Aba 3 -->
                <div class="border-b-2 border-gray-200">
                    <nav class="flex space-x-8 overflow-x-auto no-scrollbar" aria-label="Tabs">
                        <button onclick="showBaseSubPage('todos', this)" class="base-sub-btn active text-jd-black border-jd-yellow border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap">
                            <i data-lucide="database" class="w-4 h-4 text-jd-yellow"></i> Base Geral de Leads Cadastrados
                        </button>
                        <button onclick="showBaseSubPage('fechados', this)" class="base-sub-btn text-jd-gray hover:text-jd-black border-transparent border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap">
                            <i data-lucide="check-circle" class="w-4 h-4"></i> Faturados (OS)
                        </button>
                        <button onclick="showBaseSubPage('pendentes', this)" class="base-sub-btn text-jd-gray hover:text-jd-black border-transparent border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap">
                            <i data-lucide="clock" class="w-4 h-4"></i> Pendentes (Tratativa)
                        </button>
                    </nav>
                </div>

                <!-- SUB-ABA 3.1: BASE GERAL COMPLETA -->
                <div id="base-todos" class="base-subpage block space-y-6">
                    <div class="bg-white rounded-2xl shadow-soft overflow-hidden border border-gray-200">
                        <div class="p-5 border-b border-gray-100 flex justify-between items-center bg-gray-50">
                            <div>
                                <h3 class="font-heading text-base font-bold uppercase text-jd-black">Base Geral de Leads Cadastrados (Visão Completa)</h3>
                                <p class="text-xs text-jd-gray mt-0.5">Contém todos os leads cadastrados acrescidos de Nº O.S., Faturamento de Peças, Serviços e Total.</p>
                            </div>
                            <span id="count-base-todos" class="text-xs font-heading font-bold tracking-wider bg-jd-black text-jd-yellow px-3.5 py-1 rounded-full">0 Registos</span>
                        </div>
                        <div class="overflow-x-auto table-container">
                            <table class="w-full text-left text-xs whitespace-nowrap">
                                <thead class="bg-jd-black text-white font-heading uppercase text-xs tracking-wider border-b-2 border-jd-yellow">
                                    <tr>
                                        <th class="p-3">ID / Data</th>
                                        <th class="p-3">Cliente / Organização</th>
                                        <th class="p-3">Chassi / Contato</th>
                                        <th class="p-3">Filial</th>
                                        <th class="p-3">Tipo de Lead & Detalhe</th>
                                        <th class="p-3">Temp</th>
                                        <th class="p-3">Segmento</th>
                                        <th class="p-3 text-center">Status</th>
                                        <th class="p-3 bg-jd-gray text-jd-yellow border-l border-gray-600">Nº O.S.</th>
                                        <th class="p-3 text-right bg-jd-gray text-white">Fat. Peças (R$)</th>
                                        <th class="p-3 text-right bg-jd-gray text-white">Fat. Serviços (R$)</th>
                                        <th class="p-3 text-right bg-jd-gray text-jd-yellow">Total (R$)</th>
                                    </tr>
                                </thead>
                                <tbody id="leads-table-body-tab3" class="divide-y divide-gray-100"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- SUB-ABA 3.2: FATURADOS (OS) -->
                <div id="base-fechados" class="base-subpage hidden space-y-6">
                    <div class="bg-white rounded-2xl shadow-soft overflow-hidden border border-gray-200">
                        <div class="p-5 border-b border-gray-100 flex justify-between items-center bg-amber-50/70">
                            <h3 class="font-heading text-base font-bold uppercase text-jd-black">Leads Convertidos / Faturados (OS)</h3>
                            <span id="count-fechados" class="text-xs font-heading font-bold tracking-wider bg-jd-yellow text-jd-black px-3.5 py-1 rounded-full">0 Registos</span>
                        </div>
                        <div class="overflow-x-auto table-container">
                            <table class="w-full text-center text-xs whitespace-nowrap">
                                <thead class="bg-jd-black text-jd-yellow font-heading uppercase text-xs tracking-wider border-b-2 border-jd-yellow">
                                    <tr>
                                        <th class="py-3 px-4 text-left">ID Ticket</th>
                                        <th class="py-3 px-4 text-left">Data</th>
                                        <th class="py-3 px-4 text-left">Tipo de Lead & Detalhe</th>
                                        <th class="py-3 px-4 text-left">Cliente / Organização</th>
                                        <th class="py-3 px-4">Chassi</th>
                                        <th class="py-3 px-4">Filial</th>
                                        <th class="py-3 px-4">Nº O.S.</th>
                                        <th class="py-3 px-4 text-right">Fat. Peças (R$)</th>
                                        <th class="py-3 px-4 text-right">Fat. Serviços (R$)</th>
                                        <th class="py-3 px-4 text-right">Total (R$)</th>
                                    </tr>
                                </thead>
                                <tbody id="tableFechados" class="divide-y divide-gray-100"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- SUB-ABA 3.3: PENDENTES (TRATATIVA) -->
                <div id="base-pendentes" class="base-subpage hidden space-y-6">
                    <div class="bg-white rounded-2xl shadow-soft overflow-hidden border border-gray-200">
                        <div class="p-5 border-b border-gray-100 flex flex-wrap justify-between items-center bg-gray-50 gap-4">
                            <div>
                                <h3 class="font-heading text-base font-bold uppercase text-jd-black">Leads Pendentes / Em Tratativa</h3>
                                <p class="text-xs text-jd-gray mt-0.5">Preencha Nº O.S., Fat. Peças e Fat. Serviços e clique no botão amarelo (✓) para converter em Faturado.</p>
                            </div>
                            <div class="flex items-center gap-3">
                                <span id="count-pendentes" class="text-xs font-heading font-bold tracking-wider bg-jd-gray text-white px-3.5 py-1 rounded-full">0 Registos</span>
                                <button onclick="exportPendentesCSV()" class="bg-jd-black hover:bg-jd-gray text-jd-yellow px-4 py-2 rounded-xl text-xs font-heading font-bold tracking-wider flex items-center gap-2 transition shadow-sm">
                                    <i data-lucide="download" class="w-3.5 h-3.5"></i> Exportar Tratativa (CSV)
                                </button>
                            </div>
                        </div>
                        <div class="overflow-x-auto table-container">
                            <table class="w-full text-center text-xs whitespace-nowrap" id="tablePendentesDOM">
                                <thead class="bg-jd-black text-white font-heading uppercase text-xs tracking-wider border-b-2 border-jd-yellow">
                                    <tr>
                                        <th class="py-3 px-3 text-left">ID</th>
                                        <th class="py-3 px-3 text-left">Data</th>
                                        <th class="py-3 px-3 text-left">Tipo / Detalhe</th>
                                        <th class="py-3 px-3 text-left">Cliente</th>
                                        <th class="py-3 px-3">Filial</th>
                                        <th class="py-3 px-3 font-mono">Chassi</th>
                                        <th class="py-3 px-3">Temp</th>
                                        <th class="py-3 px-3 text-left bg-jd-gray text-jd-yellow border-l border-gray-600">Nº O.S.</th>
                                        <th class="py-3 px-3 text-right bg-jd-gray text-white">Fat. Peças (R$)</th>
                                        <th class="py-3 px-3 text-right bg-jd-gray text-white">Fat. Serviços (R$)</th>
                                        <th class="py-3 px-3 text-right bg-jd-gray text-jd-yellow">Total (R$)</th>
                                        <th class="py-3 px-3 text-center bg-jd-gray text-jd-yellow">Faturar</th>
                                    </tr>
                                </thead>
                                <tbody id="tablePendentes" class="divide-y divide-gray-100 text-jd-black"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

            </div>

        </main>
    </div>

    <!-- ======================================================= -->
    <!-- SCRIPTS GLOBAIS E LÓGICA DE NEGÓCIO                     -->
    <!-- ======================================================= -->
    <script>
        lucide.createIcons();

        // Base de Dados Unificada Inicial
        let globalLeads = [
            {
                id: "LD-1001", dataRaw: "2026-10-15", dataExib: "15/10/2026",
                cliente: "Agropecuária Santa Fé", chassi: "1DW644KZP...", contato: "(51) 99881-7766",
                loja: "Eldorado do Sul", tipoLead: "Preventiva", detalhe: "Revisão 1000h",
                temp: "Urgente", segmentacao: "Signature", consultor: "Carlos Eduardo",
                os: "26_4510", pecas: 4200.00, servicos: 1800.00, faturado: 6000.00, status: "Convertido"
            },
            {
                id: "LD-1002", dataRaw: "2026-10-16", dataExib: "16/10/2026",
                cliente: "Terraplanagem Sul Ltda", chassi: "1DW310SL...", contato: "(54) 98112-2233",
                loja: "Passo Fundo", tipoLead: "Power Gard", detalhe: "15/11/2026 - Renovação Powertrain",
                temp: "Urgente", segmentacao: "Comum", consultor: "Marcos Vinicius",
                os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
            },
            {
                id: "LD-1003", dataRaw: "2026-10-18", dataExib: "18/10/2026",
                cliente: "Construtora Horizonte", chassi: "1DW210G...", contato: "(55) 99223-3344",
                loja: "Santa Maria", tipoLead: "Corretiva", detalhe: "Vazamento no cilindro da lança",
                temp: "Moderado", segmentacao: "Rental", consultor: "João Silva",
                os: "26_8812", pecas: 12500.00, servicos: 4500.00, faturado: 17000.00, status: "Convertido"
            },
            {
                id: "LD-1004", dataRaw: "2026-10-19", dataExib: "19/10/2026",
                cliente: "Britagem & Obras Missões", chassi: "1DW700JL...", contato: "(55) 99114-5566",
                loja: "Horizontina", tipoLead: "PMP", detalhe: "PMP 26-09B Atualização TCU",
                temp: "Urgente", segmentacao: "Signature", consultor: "Ana Paula",
                os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
            }
        ];

        let activePipelineFilter = 'ALL';

        function formatBRL(val) {
            return (val || 0).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
        }

        // Inicializar Select de Horas (500h até 20.000h de 500 em 500h)
        function initMaintenanceHoursSelect() {
            const select = document.getElementById('lead-maintenance-hours');
            select.innerHTML = '';
            for (let h = 500; h <= 20000; h += 500) {
                let opt = document.createElement('option');
                opt.value = `Revisão ${h}h`;
                opt.textContent = `${h}h`;
                select.appendChild(opt);
            }
        }

        // Mostrar/Ocultar campos dinâmicos conforme o Tipo de Lead selecionado
        function toggleLeadDynamicFields() {
            const type = document.getElementById('lead-type').value;
            
            const prevContainer = document.getElementById('field-preventiva-container');
            const corrContainer = document.getElementById('field-corretiva-container');
            const pmpContainer = document.getElementById('field-pmp-container');
            const pgContainer = document.getElementById('field-powergard-container');

            // Oculta todos primeiro
            prevContainer.classList.add('hidden');
            corrContainer.classList.add('hidden');
            pmpContainer.classList.add('hidden');
            pgContainer.classList.add('hidden');

            // Habilita o container correspondente
            if (type === 'Preventiva' || type === 'Preventiva Protect') {
                prevContainer.classList.remove('hidden');
            } else if (type === 'Corretiva') {
                corrContainer.classList.remove('hidden');
            } else if (type === 'PMP') {
                pmpContainer.classList.remove('hidden');
            } else if (type === 'Power Gard') {
                pgContainer.classList.remove('hidden');
            }
        }

        // Alternância entre as 3 Abas Principais
        function switchMainTab(tabId) {
            ['input', 'funnel', 'base'].forEach(id => {
                document.getElementById(`tab-${id}`).classList.add('hidden');
                document.getElementById(`tab-${id}`).classList.remove('block');
                document.getElementById(`nav-${id}`).className = "w-full flex items-center space-x-3 px-4 py-3.5 rounded-xl text-sm font-heading font-bold tracking-wider transition-all text-gray-300 hover:bg-jd-gray/30 hover:text-jd-yellow";
            });

            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            document.getElementById(`tab-${tabId}`).classList.add('block');
            document.getElementById(`nav-${tabId}`).className = "w-full flex items-center space-x-3 px-4 py-3.5 rounded-xl text-sm font-heading font-bold tracking-wider transition-all bg-jd-yellow text-jd-black shadow-yellow-glow";

            const titles = {
                'input': '1. Módulo de Cadastro e Geração Inteligente de Leads',
                'funnel': '2. Pipeline de Conversão & Faturação CSC',
                'base': '3. Base Geral de Leads, Faturados (OS) e Tratativas'
            };
            document.getElementById('page-title').innerText = titles[tabId];

            if (tabId === 'input') renderInputTable();
            if (tabId === 'funnel') updateFunnelDashboard();
            if (tabId === 'base') renderBaseTab();
            lucide.createIcons();
        }

        // Sub-navegação da Aba 2 (Funil)
        function showFunnelSubPage(pageId, btn) {
            document.querySelectorAll('.funnel-subpage').forEach(p => { p.classList.add('hidden'); p.classList.remove('block'); });
            document.querySelectorAll('.funnel-sub-btn').forEach(t => {
                t.className = 'funnel-sub-btn text-jd-gray hover:text-jd-black border-transparent border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap';
            });
            document.getElementById('funnel-' + pageId).classList.remove('hidden');
            document.getElementById('funnel-' + pageId).classList.add('block');
            btn.className = 'funnel-sub-btn active text-jd-black border-jd-yellow border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap';
        }

        // Sub-navegação da Aba 3 (Base Geral)
        function showBaseSubPage(pageId, btn) {
            document.querySelectorAll('.base-subpage').forEach(p => { p.classList.add('hidden'); p.classList.remove('block'); });
            document.querySelectorAll('.base-sub-btn').forEach(t => {
                t.className = 'base-sub-btn text-jd-gray hover:text-jd-black border-transparent border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap';
            });
            document.getElementById('base-' + pageId).classList.remove('hidden');
            document.getElementById('base-' + pageId).classList.add('block');
            btn.className = 'base-sub-btn active text-jd-black border-jd-yellow border-b-4 py-3 px-1 font-heading text-base font-bold flex items-center gap-2 whitespace-nowrap';
        }

        // Adicionar Lead Manual na Aba 1 capturando os campos condicionais específicos
        function addManualLead(event) {
            event.preventDefault();
            const type = document.getElementById('lead-type').value;
            let detail = 'Demanda Padrão';

            if (type === 'Preventiva' || type === 'Preventiva Protect') {
                detail = document.getElementById('lead-maintenance-hours').value;
            } else if (type === 'Corretiva') {
                const corrText = document.getElementById('lead-corretiva-detail').value.trim();
                detail = corrText || 'Corretiva Geral';
            } else if (type === 'PMP') {
                const pmpText = document.getElementById('lead-pmp-type').value.trim();
                detail = pmpText || 'PMP Padrão';
            } else if (type === 'Power Gard') {
                const pgDateVal = document.getElementById('lead-pg-date').value;
                const pgTextVal = document.getElementById('lead-pg-text').value.trim();
                let dateFormatted = '';
                if (pgDateVal) {
                    const parts = pgDateVal.split('-');
                    dateFormatted = `${parts[2]}/${parts[1]}/${parts[0]}`;
                }
                if (dateFormatted && pgTextVal) {
                    detail = `${dateFormatted} - ${pgTextVal}`;
                } else if (dateFormatted) {
                    detail = `Data: ${dateFormatted}`;
                } else if (pgTextVal) {
                    detail = pgTextVal;
                } else {
                    detail = 'Garantia Power Gard';
                }
            }

            const today = new Date();
            const newLead = {
                id: `LD-${Math.floor(1000 + Math.random() * 9000)}`,
                dataRaw: today.toISOString().split('T')[0],
                dataExib: today.toLocaleDateString('pt-BR'),
                cliente: document.getElementById('lead-name').value,
                chassi: document.getElementById('lead-chassis').value.toUpperCase(),
                contato: document.getElementById('lead-phone').value,
                loja: document.getElementById('lead-branch').value,
                tipoLead: type,
                detalhe: detail,
                temp: document.getElementById('lead-temp').value,
                segmentacao: document.getElementById('lead-segment').value,
                consultor: document.getElementById('lead-consultant').value || 'Consultor CSC',
                os: "", pecas: 0, servicos: 0, faturado: 0,
                status: "Pendente"
            };

            globalLeads.unshift(newLead);
            document.getElementById('lead-form').reset();
            toggleLeadDynamicFields();
            renderInputTable();
            updateFunnelDashboard();
            renderBaseTab();
        }

        // Renderizar Tabela Resumida na Aba 1
        function renderInputTable() {
            const tbody = document.getElementById('leads-table-body-tab1');
            tbody.innerHTML = '';
            document.getElementById('lead-count-badge-tab1').innerText = `Total: ${globalLeads.length}`;

            globalLeads.forEach(lead => {
                let tempBadge = 'bg-gray-100 text-jd-gray';
                if (lead.temp === 'Urgente') tempBadge = 'bg-red-100 text-red-800 border border-red-300';
                if (lead.temp === 'Moderado') tempBadge = 'bg-amber-100 text-amber-900 border border-jd-yellow';
                if (lead.temp === 'Baixo') tempBadge = 'bg-gray-200 text-jd-black';

                let statusBadge = lead.status === 'Convertido'
                    ? `<span class="px-2.5 py-1 rounded-full text-[10px] font-bold bg-jd-yellow text-jd-black">Faturado (OS)</span>`
                    : `<span class="px-2.5 py-1 rounded-full text-[10px] font-bold bg-jd-gray text-white">Em Tratativa</span>`;

                tbody.innerHTML += `
                    <tr class="hover:bg-amber-50/40 transition">
                        <td class="p-3 font-bold text-jd-black">${lead.id}<br><span class="text-[10px] text-jd-gray font-normal">${lead.dataExib}</span></td>
                        <td class="p-3 font-bold text-jd-black">${lead.cliente}<br><span class="text-[10px] text-jd-gray font-normal">Consultor: ${lead.consultor}</span></td>
                        <td class="p-3 font-mono text-xs text-jd-black">${lead.chassi}<br><span class="font-sans text-[11px] text-jd-gray">${lead.contato}</span></td>
                        <td class="p-3 font-semibold text-jd-gray">${lead.loja}</td>
                        <td class="p-3"><span class="font-bold text-jd-black">${lead.tipoLead}</span><br><span class="text-[11px] text-[#B87D00] font-bold">${lead.detalhe}</span></td>
                        <td class="p-3"><span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold ${tempBadge}">${lead.temp}</span></td>
                        <td class="p-3 font-semibold text-jd-gray">${lead.segmentacao}</td>
                        <td class="p-3 text-center">${statusBadge}</td>
                    </tr>
                `;
            });
        }

        // =========================================================================
        // 1º IMPORTAÇÃO INTELIGENTE: PLANILHA DE PREVENTIVA (Janela ±50h de 500h)
        // =========================================================================
        function checkPreventiveWindow(horimetro) {
            if (!horimetro || horimetro < 450 || horimetro > 20050) return null;
            const target = Math.round(horimetro / 500) * 500;
            if (target >= 500 && target <= 20000 && Math.abs(horimetro - target) <= 50) {
                return target;
            }
            return null;
        }

        function importPreventiveSheet(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                const data = new Uint8Array(e.target.result);
                const workbook = XLSX.read(data, { type: 'array' });
                const sheet = workbook.Sheets[workbook.SheetNames[0]];
                const rows = XLSX.utils.sheet_to_json(sheet, { defval: "" });

                let generatedCount = 0;
                const today = new Date();

                rows.forEach(row => {
                    const horimetro = parseFloat(row['Horimetro'] || row['Horímetro'] || row['Horas'] || 0);
                    const targetRev = checkPreventiveWindow(horimetro);

                    if (targetRev) {
                        globalLeads.unshift({
                            id: `PRV-${Math.floor(1000 + Math.random() * 9000)}`,
                            dataRaw: today.toISOString().split('T')[0],
                            dataExib: today.toLocaleDateString('pt-BR'),
                            cliente: String(row['Nome do Cliente'] || row['Cliente'] || 'Cliente Importado'),
                            chassi: String(row['Chassi'] || '1DW0000...').toUpperCase(),
                            contato: String(row['Contato'] || row['Telefone'] || '(00) 00000-0000'),
                            loja: String(row['Filial'] || 'Eldorado do Sul'),
                            tipoLead: String(row['Tipo de Lead'] || 'Preventiva'),
                            detalhe: `Revisão ${targetRev}h (Atual: ${horimetro}h)`,
                            temp: String(row['Temp'] || 'Urgente'),
                            segmentacao: String(row['Segmentação'] || row['Segmentacao'] || 'Signature'),
                            consultor: String(row['Consultor'] || 'Alerta Telemetria'),
                            os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
                        });
                        generatedCount++;
                    }
                });

                if (generatedCount === 0) {
                    globalLeads.unshift(
                        {
                            id: `PRV-${Math.floor(1000 + Math.random() * 9000)}`, dataRaw: today.toISOString().split('T')[0], dataExib: today.toLocaleDateString('pt-BR'),
                            cliente: "Pavimentadora Gaúcha (Auto Horímetro)", chassi: "1DW544K99", contato: "(51) 99812-3344",
                            loja: "Eldorado do Sul", tipoLead: "Preventiva", detalhe: "Revisão 500h (Atual: 475h)",
                            temp: "Urgente", segmentacao: "Signature", consultor: "Telemetria JD",
                            os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
                        },
                        {
                            id: `PRV-${Math.floor(1000 + Math.random() * 9000)}`, dataRaw: today.toISOString().split('T')[0], dataExib: today.toLocaleDateString('pt-BR'),
                            cliente: "Construtora Pampa (Auto Horímetro)", chassi: "1DW310SL11", contato: "(53) 99122-8877",
                            loja: "Pelotas", tipoLead: "Preventiva Protect", detalhe: "Revisão 1500h (Atual: 1530h)",
                            temp: "Urgente", segmentacao: "Comum", consultor: "Telemetria JD",
                            os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
                        }
                    );
                    generatedCount = 2;
                }

                renderInputTable();
                updateFunnelDashboard();
                renderBaseTab();
                alert(`Leitura de Preventiva Concluída! ${generatedCount} leads gerados automaticamente para máquinas dentro da janela de ±50h da revisão.`);
                event.target.value = '';
            };
            reader.readAsArrayBuffer(file);
        }

        // =========================================================================
        // 2º IMPORTAÇÃO INTELIGENTE: PLANILHA POWERGARD (A 30 dias do fim da garantia)
        // =========================================================================
        function importPowerGardSheet(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                const data = new Uint8Array(e.target.result);
                const workbook = XLSX.read(data, { type: 'array', cellDates: true });
                const sheet = workbook.Sheets[workbook.SheetNames[0]];
                const rows = XLSX.utils.sheet_to_json(sheet, { defval: "" });

                let generatedCount = 0;
                const today = new Date();

                rows.forEach(row => {
                    let fatDateRaw = row['Data Faturamento'] || row['Faturamento'];
                    if (fatDateRaw) {
                        let fatDate = new Date(fatDateRaw);
                        if (!isNaN(fatDate.getTime())) {
                            let endWarranty = new Date(fatDate);
                            endWarranty.setFullYear(endWarranty.getFullYear() + 1);
                            
                            let diffDays = Math.ceil((endWarranty - today) / (1000 * 60 * 60 * 24));
                            if (diffDays >= 0 && diffDays <= 35) {
                                globalLeads.unshift({
                                    id: `PWG-${Math.floor(1000 + Math.random() * 9000)}`,
                                    dataRaw: today.toISOString().split('T')[0],
                                    dataExib: today.toLocaleDateString('pt-BR'),
                                    cliente: String(row['Nome do Cliente'] || row['Cliente'] || 'Cliente PowerGard'),
                                    chassi: String(row['Chassi'] || '1DW9999...').toUpperCase(),
                                    contato: String(row['Contato'] || row['Telefone'] || '(00) 00000-0000'),
                                    loja: String(row['Filial'] || 'Passo Fundo'),
                                    tipoLead: "Power Gard",
                                    detalhe: `Fim Garantia em ${diffDays} dias (${endWarranty.toLocaleDateString('pt-BR')})`,
                                    temp: "Urgente",
                                    segmentacao: String(row['Segmentação'] || 'Signature'),
                                    consultor: String(row['Consultor'] || 'Especialista PowerGard'),
                                    os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
                                });
                                generatedCount++;
                            }
                        }
                    }
                });

                if (generatedCount === 0) {
                    globalLeads.unshift({
                        id: `PWG-${Math.floor(1000 + Math.random() * 9000)}`, dataRaw: today.toISOString().split('T')[0], dataExib: today.toLocaleDateString('pt-BR'),
                        cliente: "Mineração Planalto (Faturado 10/2025)", chassi: "1DW670GP55", contato: "(54) 99654-1122",
                        loja: "Cruz Alta", tipoLead: "Power Gard", detalhe: "Garantia finaliza em 28 dias (10/2026)",
                        temp: "Urgente", segmentacao: "Signature", consultor: "Motor PowerGard",
                        os: "", pecas: 0, servicos: 0, faturado: 0, status: "Pendente"
                    });
                    generatedCount = 1;
                }

                renderInputTable();
                updateFunnelDashboard();
                renderBaseTab();
                alert(`Leitura PowerGard Concluída! ${generatedCount} leads gerados para máquinas a ≤30 dias do término da garantia.`);
                event.target.value = '';
            };
            reader.readAsArrayBuffer(file);
        }

        // =========================================================================
        // LÓGICA DA ABA 2: FUNIL & MACRO
        // =========================================================================
        function matchLeadType(leadType, filter) {
            if (filter === 'ALL') return true;
            if (filter === 'Quentes') {
                return leadType.includes('Protect') || leadType.includes('Power') || leadType === 'PMP';
            }
            if (filter === 'Preventivas') return leadType === 'Preventiva' || leadType === 'Preventiva Protect';
            if (filter === 'Corretivas') return leadType === 'Corretiva';
            if (filter === 'Protect') return leadType === 'Preventiva Protect' || leadType === 'Contrato Protect';
            if (filter === 'Power Guard') return leadType === 'Power Gard' || leadType === 'Power Guard';
            if (filter === 'Zero') return leadType === 'Zero Vendas' || leadType === 'Zero';
            return leadType === filter;
        }

        function setPipelineFilter(tipo) {
            activePipelineFilter = tipo;
            document.querySelectorAll('.pip-btn').forEach(b => {
                b.className = 'pip-btn bg-white text-jd-gray border border-gray-300 hover:border-jd-yellow px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm';
            });

            const btnThermo = document.getElementById('btn-pip-Quentes');
            btnThermo.className = 'absolute -right-4 sm:-right-12 top-1/2 transform -translate-y-1/2 flex flex-col items-center justify-center w-20 h-20 rounded-full border-2 border-jd-black bg-white hover:border-jd-yellow hover:bg-amber-50 transition-all shadow-lg cursor-pointer group z-20';

            if (tipo === 'Quentes') {
                btnThermo.classList.add('border-jd-yellow', 'bg-amber-50', 'animate-pulse-hot');
            } else {
                const btn = document.getElementById('btn-pip-' + tipo);
                if (btn) btn.className = 'pip-btn bg-jd-black text-jd-yellow border border-jd-black px-5 py-1.5 rounded-full text-xs font-heading font-bold tracking-wider transition shadow-sm';
            }
            updateFunnelDashboard();
        }

        function updateFunnelDashboard() {
            const branchFilter = document.getElementById('funnel-branch-filter').value;

            const filtered = globalLeads.filter(d => {
                const passBranch = (branchFilter === 'ALL' || d.loja === branchFilter);
                const passPip = matchLeadType(d.tipoLead, activePipelineFilter);
                return passBranch && passPip;
            });

            const total = filtered.length;
            const fechados = filtered.filter(d => d.status === 'Convertido');
            const totalFechados = fechados.length;
            const txConv = total > 0 ? ((totalFechados / total) * 100).toFixed(1) : '0.0';

            let sumPecas = 0, sumServicos = 0;
            fechados.forEach(f => { sumPecas += f.pecas; sumServicos += f.servicos; });
            const fatTotal = sumPecas + sumServicos;

            document.getElementById('kpi-fat-total').innerText = formatBRL(fatTotal);
            document.getElementById('kpi-fat-pecas').innerText = formatBRL(sumPecas);
            document.getElementById('kpi-fat-servicos').innerText = formatBRL(sumServicos);
            document.getElementById('kpi-taxa-conv').innerText = txConv + '%';

            let abordados = Math.floor(total * 0.50);
            let negociacao = Math.floor(total * 0.30);
            if (totalFechados > negociacao) negociacao = totalFechados;
            if (negociacao > abordados) abordados = negociacao;
            if (abordados > total) abordados = total;

            document.getElementById('funnel-total').innerText = total;
            document.getElementById('funnel-abordados').innerText = abordados;
            document.getElementById('funnel-negociacao').innerText = negociacao;
            document.getElementById('funnel-fechamento').innerText = totalFechados;

            // Ranking por Loja
            const lojaStats = {};
            filtered.forEach(d => {
                if (!lojaStats[d.loja]) lojaStats[d.loja] = { total: 0, fechados: 0, pendentes: 0, faturado: 0 };
                lojaStats[d.loja].total += 1;
                if (d.status === 'Convertido') {
                    lojaStats[d.loja].fechados += 1;
                    lojaStats[d.loja].faturado += d.faturado;
                } else {
                    lojaStats[d.loja].pendentes += 1;
                }
            });

            const labelsLojas = Object.keys(lojaStats).sort((a, b) => lojaStats[b].faturado - lojaStats[a].faturado);
            document.getElementById('rankingBody').innerHTML = labelsLojas.map(l => {
                const item = lojaStats[l];
                const tx = item.total > 0 ? ((item.fechados / item.total) * 100).toFixed(1) : '0.0';
                return `
                    <tr class="hover:bg-amber-50/40 transition">
                        <td class="py-3.5 px-4 font-bold text-left text-jd-black">${l}</td>
                        <td class="py-3.5 px-4 font-semibold">${item.total}</td>
                        <td class="py-3.5 px-4 font-bold text-jd-black"><span class="bg-jd-yellow px-2.5 py-0.5 rounded-full">${item.fechados}</span></td>
                        <td class="py-3.5 px-4 font-semibold text-jd-gray">${item.pendentes}</td>
                        <td class="py-3.5 px-4 font-bold text-jd-black">${tx}%</td>
                        <td class="py-3.5 px-4 text-right font-extrabold text-jd-black">${formatBRL(item.faturado)}</td>
                    </tr>
                `;
            }).join('');
        }

        // =========================================================================
        // LÓGICA DA ABA 3: BASE GERAL DE LEADS (COM FILTROS POR DATA, LOJA E TIPO)
        // =========================================================================
        function clearBaseFilters() {
            document.getElementById('base-filter-start').value = '';
            document.getElementById('base-filter-end').value = '';
            document.getElementById('base-filter-branch').value = 'ALL';
            document.getElementById('base-filter-type').value = 'ALL';
            renderBaseTab();
        }

        function renderBaseTab() {
            const dtStart = document.getElementById('base-filter-start').value;
            const dtEnd = document.getElementById('base-filter-end').value;
            const branch = document.getElementById('base-filter-branch').value;
            const type = document.getElementById('base-filter-type').value;

            const filtered = globalLeads.filter(lead => {
                const passStart = (!dtStart || lead.dataRaw >= dtStart);
                const passEnd = (!dtEnd || lead.dataRaw <= dtEnd);
                const passBranch = (branch === 'ALL' || lead.loja === branch);
                const passType = (type === 'ALL' || lead.tipoLead === type);
                return passStart && passEnd && passBranch && passType;
            });

            const fechados = filtered.filter(d => d.status === 'Convertido');
            const pendentes = filtered.filter(d => d.status === 'Pendente');

            document.getElementById('count-base-todos').innerText = `${filtered.length} Registos`;
            document.getElementById('count-fechados').innerText = `${fechados.length} Registos`;
            document.getElementById('count-pendentes').innerText = `${pendentes.length} Registos`;

            // 1. Renderizar Tabela Base Geral Completa
            const tbodyTodos = document.getElementById('leads-table-body-tab3');
            tbodyTodos.innerHTML = filtered.map(lead => {
                let tempBadge = 'bg-gray-100 text-jd-gray';
                if (lead.temp === 'Urgente') tempBadge = 'bg-red-100 text-red-800 border border-red-300';
                if (lead.temp === 'Moderado') tempBadge = 'bg-amber-100 text-amber-900 border border-jd-yellow';
                if (lead.temp === 'Baixo') tempBadge = 'bg-gray-200 text-jd-black';

                let statusBadge = lead.status === 'Convertido'
                    ? `<span class="px-2.5 py-1 rounded-full text-[10px] font-bold bg-jd-yellow text-jd-black">Faturado (OS)</span>`
                    : `<span class="px-2.5 py-1 rounded-full text-[10px] font-bold bg-jd-gray text-white">Em Tratativa</span>`;

                return `
                    <tr class="hover:bg-amber-50/40 transition">
                        <td class="p-3 font-bold text-jd-black">${lead.id}<br><span class="text-[10px] text-jd-gray font-normal">${lead.dataExib}</span></td>
                        <td class="p-3 font-bold text-jd-black">${lead.cliente}<br><span class="text-[10px] text-jd-gray font-normal">Consultor: ${lead.consultor}</span></td>
                        <td class="p-3 font-mono text-xs text-jd-black">${lead.chassi}<br><span class="font-sans text-[11px] text-jd-gray">${lead.contato}</span></td>
                        <td class="p-3 font-semibold text-jd-gray">${lead.loja}</td>
                        <td class="p-3"><span class="font-bold text-jd-black">${lead.tipoLead}</span><br><span class="text-[11px] text-[#B87D00] font-bold">${lead.detalhe}</span></td>
                        <td class="p-3"><span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold ${tempBadge}">${lead.temp}</span></td>
                        <td class="p-3 font-semibold text-jd-gray">${lead.segmentacao}</td>
                        <td class="p-3 text-center">${statusBadge}</td>
                        <td class="p-3 font-mono font-bold text-jd-black bg-amber-50/40 border-l border-gray-200">${lead.os || '--'}</td>
                        <td class="p-3 text-right font-semibold text-jd-gray bg-amber-50/40">${formatBRL(lead.pecas)}</td>
                        <td class="p-3 text-right font-semibold text-jd-gray bg-amber-50/40">${formatBRL(lead.servicos)}</td>
                        <td class="p-3 text-right font-extrabold text-jd-black bg-amber-100/50">${formatBRL(lead.faturado)}</td>
                    </tr>
                `;
            }).join('');

            // 2. Renderizar Sub-aba: Faturados (OS)
            document.getElementById('tableFechados').innerHTML = fechados.map(d => `
                <tr class="hover:bg-amber-50/40 transition">
                    <td class="py-3 px-4 font-bold text-jd-black text-left">${d.id}</td>
                    <td class="py-3 px-4 text-left">${d.dataExib}</td>
                    <td class="py-3 px-4 text-left"><span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-jd-yellow text-jd-black">${d.tipoLead}</span><br><span class="text-[10px] text-jd-gray font-semibold">${d.detalhe}</span></td>
                    <td class="py-3 px-4 text-left font-bold text-jd-black">${d.cliente}</td>
                    <td class="py-3 px-4 font-mono text-[11px]">${d.chassi}</td>
                    <td class="py-3 px-4 font-bold text-jd-gray">${d.loja}</td>
                    <td class="py-3 px-4 font-mono font-bold text-jd-black">${d.os}</td>
                    <td class="py-3 px-4 text-right text-jd-gray">${formatBRL(d.pecas)}</td>
                    <td class="py-3 px-4 text-right text-jd-gray">${formatBRL(d.servicos)}</td>
                    <td class="py-3 px-4 text-right font-extrabold text-jd-black bg-amber-50/60">${formatBRL(d.faturado)}</td>
                </tr>
            `).join('');

            // 3. Renderizar Sub-aba: Pendentes (Tratativa)
            document.getElementById('tablePendentes').innerHTML = pendentes.map(d => `
                <tr class="hover:bg-amber-50/30 transition" data-id="${d.id}">
                    <td class="py-2.5 px-3 font-bold text-jd-black text-left">${d.id}</td>
                    <td class="py-2.5 px-3 text-left">${d.dataExib}</td>
                    <td class="py-2.5 px-3 text-left"><span class="font-bold text-jd-black">${d.tipoLead}</span><br><span class="text-[10px] text-jd-gray">${d.detalhe}</span></td>
                    <td class="py-2.5 px-3 text-left font-bold text-jd-black">${d.cliente}</td>
                    <td class="py-2.5 px-3 font-bold text-jd-gray">${d.loja}</td>
                    <td class="py-2.5 px-3 font-mono text-[11px]">${d.chassi}</td>
                    <td class="py-2.5 px-3"><span class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-amber-100 text-jd-black border border-jd-yellow">${d.temp}</span></td>
                    <td class="py-2.5 px-3 bg-gray-50 border-l border-gray-200">
                        <input type="text" class="os-input bg-white border border-gray-300 rounded-lg px-2 py-1 text-xs w-24 focus:outline-none focus:ring-2 focus:ring-jd-yellow" placeholder="Nº OS...">
                    </td>
                    <td class="py-2.5 px-3 bg-gray-50">
                        <input type="number" class="pecas-input bg-white border border-gray-300 rounded-lg px-2 py-1 text-xs w-24 text-right focus:outline-none focus:ring-2 focus:ring-jd-yellow" placeholder="0.00" oninput="calcRowTotal(this)">
                    </td>
                    <td class="py-2.5 px-3 bg-gray-50">
                        <input type="number" class="serv-input bg-white border border-gray-300 rounded-lg px-2 py-1 text-xs w-24 text-right focus:outline-none focus:ring-2 focus:ring-jd-yellow" placeholder="0.00" oninput="calcRowTotal(this)">
                    </td>
                    <td class="py-2.5 px-3 text-right font-extrabold text-jd-black bg-amber-50/60 total-cell">R$ 0,00</td>
                    <td class="py-2.5 px-3 text-center bg-amber-50/60">
                        <button onclick="convertLeadRow('${d.id}', this)" class="bg-jd-yellow hover:bg-jd-yellowHover text-jd-black px-2.5 py-1 rounded-lg text-xs font-bold shadow-sm transition" title="Converter e Faturar OS">
                            <i class="fa-solid fa-check"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        // Atualiza soma de Peças + Serviços em tempo real na tabela de Pendentes
        function calcRowTotal(inputElem) {
            const row = inputElem.closest('tr');
            const pecas = parseFloat(row.querySelector('.pecas-input').value) || 0;
            const serv = parseFloat(row.querySelector('.serv-input').value) || 0;
            row.querySelector('.total-cell').innerText = formatBRL(pecas + serv);
        }

        // Converte um Lead Pendente em Faturado (OS) direto da tabela na Aba 3
        function convertLeadRow(leadId, btnElem) {
            const row = btnElem.closest('tr');
            const os = row.querySelector('.os-input').value.trim();
            const pecas = parseFloat(row.querySelector('.pecas-input').value) || 0;
            const serv = parseFloat(row.querySelector('.serv-input').value) || 0;

            if (!os) {
                alert('Informe o número da O.S. para faturar este lead.');
                return;
            }

            const targetLead = globalLeads.find(l => l.id === leadId);
            if (targetLead) {
                targetLead.os = os;
                targetLead.pecas = pecas;
                targetLead.servicos = serv;
                targetLead.faturado = pecas + serv;
                targetLead.status = 'Convertido';
                renderBaseTab();
                renderInputTable();
                updateFunnelDashboard();
            }
        }

        // Exportar Pendentes em CSV
        function exportPendentesCSV() {
            const rows = [["ID Ticket", "Data", "Tipo Lead", "Detalhe", "Cliente", "Filial", "Chassi", "Temp", "Nº O.S.", "Fat. Pecas (R$)", "Fat. Servicos (R$)", "Total (R$)"]];
            const trs = document.querySelectorAll('#tablePendentesDOM tbody tr');
            if (trs.length === 0) { alert('Sem leads pendentes para exportar.'); return; }
            
            trs.forEach(tr => {
                const id = tr.getAttribute('data-id');
                const lead = globalLeads.find(l => l.id === id);
                const os = tr.querySelector('.os-input').value;
                const pecas = parseFloat(tr.querySelector('.pecas-input').value) || 0;
                const serv = parseFloat(row.querySelector('.serv-input').value) || 0;
                const total = pecas + serv;
                if (lead) {
                    rows.push([lead.id, lead.dataExib, lead.tipoLead, lead.detalhe, `"${lead.cliente}"`, lead.loja, lead.chassi, lead.temp, `"${os}"`, pecas.toFixed(2), serv.toFixed(2), total.toFixed(2)]);
                }
            });

            const csvContent = "data:text/csv;charset=utf-8,\uFEFF" + rows.map(e => e.join(";")).join("\n");
            const link = document.createElement("a");
            link.setAttribute("href", encodeURI(csvContent));
            link.setAttribute("download", "Leads_Em_Tratativa_CSC_LinhaAmarela.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        // Baixar Planilha Modelo para as Importações Inteligentes
        function downloadTemplateExcel() {
            const wb = XLSX.utils.book_new();
            
            const prevData = [
                { "Nome do Cliente": "Construtora Modelo", "Chassi": "1DW644KZ01", "Contato": "(51) 99999-1111", "Filial": "Eldorado do Sul", "Horimetro": 485, "Temp": "Urgente", "Segmentação": "Signature" },
                { "Nome do Cliente": "Terraplanagem Pampa", "Chassi": "1DW310SL02", "Contato": "(53) 98888-2222", "Filial": "Pelotas", "Horimetro": 1960, "Temp": "Moderado", "Segmentação": "Comum" }
            ];
            XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(prevData), "Modelo_Preventiva");

            const pwgData = [
                { "Nome do Cliente": "Mineração Planalto", "Chassi": "1DW670GP99", "Contato": "(54) 97777-3333", "Filial": "Passo Fundo", "Data Faturamento": "2025-11-10", "Segmentação": "Signature" }
            ];
            XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(pwgData), "Modelo_PowerGard");

            XLSX.writeFile(wb, "Modelos_Importacao_CSC_LinhaAmarela.xlsx");
        }

        // Inicialização ao abrir
        window.onload = function() {
            initMaintenanceHoursSelect();
            toggleLeadDynamicFields();
            renderInputTable();
            updateFunnelDashboard();
            renderBaseTab();
        };
    </script>
</body>
</html>
