<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NailPro - Sistema para Designer de Unhas</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #ff85a2;
            --secondary: #ffd6ff;
            --accent: #aaf683;
            --dark: #495057;
            --light: #f8f9fa;
        --login-bg: #fff1f2;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #fef6fb;
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, var(--secondary) 0%, var(--primary) 100%);
        }
        
        .nail-service-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }
        
        .calendar-day {
            transition: all 0.2s ease;
        }
        
        .calendar-day:hover:not(.disabled) {
            background-color: var(--secondary);
            cursor: pointer;
        }
        
        .calendar-day.selected {
            background-color: var(--primary);
            color: white;
        }
        
        #clientForm, #serviceForm, #appointmentForm {
            animation: fadeIn 0.3s ease-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .portfolio-item {
            transition: all 0.3s ease;
        }
        
        .portfolio-item:hover {
            transform: scale(1.03);
        }
        
        .modal {
            animation: modalOpen 0.4s ease-out;
        }
        
        @keyframes modalOpen {
            from { opacity: 0; transform: scale(0.8); }
            to { opacity: 1; transform: scale(1); }
        }
    </style>
</head>
<body class="min-h-screen">
    <!-- Login Page -->
    <div id="loginPage" class="fixed inset-0 flex items-center justify-center gradient-bg p-4">
        <div class="bg-white rounded-xl shadow-2xl p-8 w-full max-w-md">
            <div class="text-center mb-8">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/7e897537-f2f6-4f84-b29d-e0393418a47b.png" alt="Logo NailPro" class="mx-auto rounded-full border-2 border-pink-200">
                <h1 class="text-3xl font-bold text-gray-800 mt-4">NailPro</h1>
                <p class="text-gray-600 mt-2">Sistema de gestão para designers de unhas</p>
            </div>
            <form id="loginForm">
                <div class="mb-4">
                    <label for="email" class="block text-gray-700 mb-2">E-mail</label>
                    <input type="email" id="email" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                </div>
                <div class="mb-6">
                    <label for="password" class="block text-gray-700 mb-2">Senha</label>
                    <input type="password" id="password" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                </div>
                <button type="submit" class="w-full bg-pink-500 hover:bg-pink-600 text-white py-2 px-4 rounded-lg transition duration-300">
                    Entrar
                </button>
                <div class="mt-4 text-center space-y-2">
                    <div>
                        <a href="#forgot" class="text-pink-500 hover:text-pink-700 text-sm">Esqueceu sua senha?</a>
                    </div>
                    <div>
                        <span class="text-gray-600 text-sm">Não tem conta? </span>
                        <button id="registerBtn" class="text-pink-500 hover:text-pink-700 text-sm font-medium">Cadastre-se</button>
                    </div>
                </div>
            </form>
        </div>
    </div>

    <!-- Main Application Content (hidden initially) -->
    <div id="appContent" class="hidden">
        <!-- Header/Navigation -->
    <header class="gradient-bg text-white shadow-lg">
        <div class="container mx-auto px-4 py-6">
            <div class="flex justify-between items-center">
                <div class="flex items-center space-x-2">
                    <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/b3c76732-1bbe-45e3-96bd-514ab025b44f.png" alt="Ícone de logo de unha decorada com detalhes em rosa e branco" class="rounded-full border-2 border-white">
                    <h1 class="text-2xl font-bold">NailPro</h1>
                </div>
                <nav class="hidden md:block">
                    <ul class="flex space-x-6">
                        <li><a href="#dashboard" class="hover:text-pink-200 font-medium transition">Dashboard</a></li>
                        <li><a href="#agenda" class="hover:text-pink-200 font-medium transition">Agenda</a></li>
                        <li><a href="#clientes" class="hover:text-pink-200 font-medium transition">Clientes</a></li>
                        <li><a href="#portfolio" class="hover:text-pink-200 font-medium transition">Portfólio</a></li>
                        <li><a href="#servicos" class="hover:text-pink-200 font-medium transition">Serviços</a></li>
                    </ul>
                </nav>
                <button class="md:hidden text-xl mr-4">
                    <i class="fas fa-bars"></i>
                </button>
                <button id="logoutBtn" class="hidden md:block bg-white text-pink-500 px-4 py-1 rounded-full font-medium hover:bg-pink-100 transition">
                    <i class="fas fa-sign-out-alt mr-1"></i> Sair
                </button>
            </div>
        </div>
    </header>

    <!-- Mobile Menu -->
    <div id="mobileMenu" class="hidden fixed inset-0 bg-black bg-opacity-50 z-50">
        <div class="bg-white w-3/4 h-full p-6">
            <div class="flex justify-between items-center mb-8">
                <h2 class="text-xl font-bold">NailPro</h2>
                <button id="closeMobileMenu" class="text-2xl">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <ul class="space-y-4">
                <li><a href="#dashboard" class="block py-2 text-lg">Dashboard</a></li>
                <li><a href="#agenda" class="block py-2 text-lg">Agenda</a></li>
                <li><a href="#clientes" class="block py-2 text-lg">Clientes</a></li>
                <li><a href="#portfolio" class="block py-2 text-lg">Portfólio</a></li>
                <li><a href="#servicos" class="block py-2 text-lg">Serviços</a></li>
            </ul>
        </div>
    </div>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8">
        <!-- Dashboard Section -->
        <section id="dashboard" class="mb-16">
            <h2 class="text-3xl font-bold mb-6 text-gray-800">Dashboard</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                <div class="bg-white rounded-lg shadow-md p-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500">Agendamentos Hoje</p>
                            <p class="text-3xl font-bold text-pink-500">5</p>
                        </div>
                        <div class="bg-pink-100 p-3 rounded-full">
                            <i class="fas fa-calendar-day text-pink-500 text-xl"></i>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md p-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500">Novos Clientes</p>
                            <p class="text-3xl font-bold text-purple-500">3</p>
                        </div>
                        <div class="bg-purple-100 p-3 rounded-full">
                            <i class="fas fa-user-plus text-purple-500 text-xl"></i>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md p-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500">Faturamento Mês</p>
                            <p class="text-3xl font-bold text-green-500">R$2,850</p>
                        </div>
                        <div class="bg-green-100 p-3 rounded-full">
                            <i class="fas fa-coins text-green-500 text-xl"></i>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md p-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500">Serviços Populares</p>
                            <p class="text-3xl font-bold text-blue-500">Banho de Gel</p>
                        </div>
                        <div class="bg-blue-100 p-3 rounded-full">
                            <i class="fas fa-spa text-blue-500 text-xl"></i>
                        </div>
                    </div>
                </div>
            </div>

            <div class="bg-white rounded-lg shadow-md p-6 mb-8">
                <h3 class="text-xl font-semibold mb-4">Próximos Agendamentos</h3>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead>
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Cliente</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Serviço</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Data</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Hora</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap">Ana Silva</td>
                                <td class="px-6 py-4 whitespace-nowrap">Alongamento de Fibra</td>
                                <td class="px-6 py-4 whitespace-nowrap">15/06/2023</td>
                                <td class="px-6 py-4 whitespace-nowrap">14:00</td>
                                <td class="px-6 py-4 whitespace-nowrap"><span class="px-2 py-1 text-sm bg-green-100 text-green-800 rounded-full">Confirmado</span></td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap">Carla Fernandes</td>
                                <td class="px-6 py-4 whitespace-nowrap">Banho de Gel</td>
                                <td class="px-6 py-4 whitespace-nowrap">15/06/2023</td>
                                <td class="px-6 py-4 whitespace-nowrap">16:30</td>
                                <td class="px-6 py-4 whitespace-nowrap"><span class="px-2 py-1 text-sm bg-yellow-100 text-yellow-800 rounded-full">Pendente</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- Agenda Section -->
        <section id="agenda" class="mb-16">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold text-gray-800">Agenda</h2>
                <button id="openAppointmentModal" class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg transition flex items-center space-x-2">
                    <i class="fas fa-plus"></i>
                    <span>Novo Agendamento</span>
                </button>
            </div>

            <div class="bg-white rounded-lg shadow-md p-6 mb-8">
                <div class="flex justify-between items-center mb-6">
                    <div>
                        <h3 class="text-xl font-semibold">Junho 2023</h3>
                    </div>
                    <div class="flex space-x-2">
                        <button class="p-2 rounded-full hover:bg-gray-100">
                            <i class="fas fa-chevron-left"></i>
                        </button>
                        <button class="p-2 rounded-full hover:bg-gray-100">
                            <i class="fas fa-chevron-right"></i>
                        </button>
                    </div>
                </div>

                <div class="grid grid-cols-7 gap-2 mb-4">
                    <div class="text-center font-medium py-2">Dom</div>
                    <div class="text-center font-medium py-2">Seg</div>
                    <div class="text-center font-medium py-2">Ter</div>
                    <div class="text-center font-medium py-2">Qua</div>
                    <div class="text-center font-medium py-2">Qui</div>
                    <div class="text-center font-medium py-2">Sex</div>
                    <div class="text-center font-medium py-2">Sáb</div>
                </div>

                <div class="grid grid-cols-7 gap-2" id="calendar">
                    <!-- Calendar days will be populated by JavaScript -->
                </div>
            </div>

            <div class="bg-white rounded-lg shadow-md p-6">
                <h3 class="text-xl font-semibold mb-4">Agendamentos do Dia</h3>
                <div class="space-y-4" id="dailyAppointments">
                    <div class="border-l-4 border-pink-500 pl-4 py-2 bg-pink-50">
                        <div class="flex justify-between items-start">
                            <div>
                                <h4 class="font-medium">Maria Oliveira - Alongamento de Fibra</h4>
                                <p class="text-gray-600 text-sm">14:00 - 15:30</p>
                            </div>
                            <div class="flex space-x-2">
                                <button class="text-green-500 hover:text-green-700">
                                    <i class="fas fa-check"></i>
                                </button>
                                <button class="text-red-500 hover:text-red-700">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="border-l-4 border-purple-500 pl-4 py-2 bg-purple-50">
                        <div class="flex justify-between items-start">
                            <div>
                                <h4 class="font-medium">Juliana Santos - Decoração Francesinha</h4>
                                <p class="text-gray-600 text-sm">16:00 - 17:00</p>
                            </div>
                            <div class="flex space-x-2">
                                <button class="text-green-500 hover:text-green-700">
                                    <i class="fas fa-check"></i>
                                </button>
                                <button class="text-red-500 hover:text-red-700">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Clientes Section -->
        <section id="clientes" class="mb-16">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold text-gray-800">Clientes</h2>
                <button id="openClientModal" class="bg-purple-500 hover:bg-purple-600 text-white px-4 py-2 rounded-lg transition flex items-center space-x-2">
                    <i class="fas fa-plus"></i>
                    <span>Novo Cliente</span>
                </button>
            </div>

            <div class="bg-white rounded-lg shadow-md p-6">
                <div class="flex justify-between items-center mb-6">
                    <div class="flex-1 max-w-md">
                        <div class="relative">
                            <input type="text" placeholder="Buscar cliente..." class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500">
                            <div class="absolute left-3 top-2.5 text-gray-400">
                                <i class="fas fa-search"></i>
                            </div>
                        </div>
                    </div>
                    <div>
                        <button class="p-2 rounded-lg hover:bg-gray-100">
                            <i class="fas fa-filter"></i>
                        </button>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead>
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nome</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Telefone</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">E-mail</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Última Visita</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Ações</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200" id="clientsTable">
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap">Ana Silva</td>
                                <td class="px-6 py-4 whitespace-nowrap">(11) 98765-4321</td>
                                <td class="px-6 py-4 whitespace-nowrap">ana.silva@email.com</td>
                                <td class="px-6 py-4 whitespace-nowrap">10/06/2023</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <button class="text-blue-500 hover:text-blue-700 mr-3">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <button class="text-red-500 hover:text-red-700">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap">Carla Fernandes</td>
                                <td class="px-6 py-4 whitespace-nowrap">(11) 99876-5432</td>
                                <td class="px-6 py-4 whitespace-nowrap">carla.fernandes@email.com</td>
                                <td class="px-6 py-4 whitespace-nowrap">05/06/2023</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <button class="text-blue-500 hover:text-blue-700 mr-3">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <button class="text-red-500 hover:text-red-700">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <div class="flex justify-between items-center mt-4">
                    <div class="text-gray-500 text-sm">
                        Mostrando 1 a 2 de 2 clientes
                    </div>
                    <div class="flex space-x-2">
                        <button class="px-3 py-1 border border-gray-300 rounded hover:bg-gray-100 disabled:opacity-50" disabled>
                            Anterior
                        </button>
                        <button class="px-3 py-1 border border-gray-300 rounded hover:bg-gray-100 disabled:opacity-50" disabled>
                            Próximo
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Portfólio Section -->
        <section id="portfolio" class="mb-16">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold text-gray-800">Portfólio</h2>
                <button id="openPortfolioModal" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition flex items-center space-x-2">
                    <i class="fas fa-plus"></i>
                    <span>Adicionar Foto</span>
                </button>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
                <div class="portfolio-item bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="relative">
                        <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/899af92f-c08c-4b2f-bcb8-37a00db12811.png" alt="Unhas alongadas decoradas com flores roxas e detalhes em folha de ouro" class="w-full h-48 object-cover">
                        <div class="absolute top-2 right-2 bg-black bg-opacity-50 text-white px-2 py-1 rounded text-xs">
                            Francesinha
                        </div>
                    </div>
                    <div class="p-4">
                        <h3 class="font-medium mb-1">Design Floral</h3>
                        <p class="text-gray-600 text-sm mb-2">12/05/2023</p>
                        <div class="flex justify-between items-center">
                            <div class="flex space-x-1 text-yellow-400">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                            </div>
                            <div>
                                <button class="text-gray-400 hover:text-gray-600">
                                    <i class="fas fa-ellipsis-v"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="portfolio-item bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="relative">
                        <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/1562cff6-82c2-48eb-bb0f-41b312da5a92.png" alt="Unhas curtas em vermelho com detalhes geométricos em preto e branco" class="w-full h-48 object-cover">
                        <div class="absolute top-2 right-2 bg-black bg-opacity-50 text-white px-2 py-1 rounded text-xs">
                            Banho de Gel
                        </div>
                    </div>
                    <div class="p-4">
                        <h3 class="font-medium mb-1">Design Geométrico</h3>
                        <p class="text-gray-600 text-sm mb-2">28/05/2023</p>
                        <div class="flex justify-between items-center">
                            <div class="flex space-x-1 text-yellow-400">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star-half-alt"></i>
                            </div>
                            <div>
                                <button class="text-gray-400 hover:text-gray-600">
                                    <i class="fas fa-ellipsis-v"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Serviços Section -->
        <section id="servicos" class="mb-16">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold text-gray-800">Serviços</h2>
                <button id="openServiceModal" class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg transition flex items-center space-x-2">
                    <i class="fas fa-plus"></i>
                    <span>Novo Serviço</span>
                </button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div class="nail-service-card bg-white rounded-lg shadow-md p-6 transition duration-300">
                    <div class="flex justify-between items-start mb-4">
                        <h3 class="text-xl font-semibold">Banho de Gel</h3>
                        <span class="bg-pink-100 text-pink-800 text-xs px-2 py-1 rounded-full">Popular</span>
                    </div>
                    <p class="text-gray-600 mb-4">Aplicação de gel molde perfeito para unhas naturais, com duração de 3-4 semanas.</p>
                    <div class="flex justify-between items-center">
                        <span class="text-2xl font-bold text-gray-800">R$ 80,00</span>
                        <button class="text-gray-400 hover:text-gray-600">
                            <i class="fas fa-ellipsis-v"></i>
                        </button>
                    </div>
                </div>
                <div class="nail-service-card bg-white rounded-lg shadow-md p-6 transition duration-300">
                    <div class="flex justify-between items-start mb-4">
                        <h3 class="text-xl font-semibold">Alongamento de Fibra</h3>
                        <span class="bg-purple-100 text-purple-800 text-xs px-2 py-1 rounded-full">Destaque</span>
                    </div>
                    <p class="text-gray-600 mb-4">Alongamento com fibra de vidro para unhas resistentes e naturais, com duração de 4-5 semanas.</p>
                    <div class="flex justify-between items-center">
                        <span class="text-2xl font-bold text-gray-800">R$ 120,00</span>
                        <button class="text-gray-400 hover:text-gray-600">
                            <i class="fas fa-ellipsis-v"></i>
                        </button>
                    </div>
                </div>
                <div class="nail-service-card bg-white rounded-lg shadow-md p-6 transition duration-300">
                    <div class="flex justify-between items-start mb-4">
                        <h3 class="text-xl font-semibold">Francesinha</h3>
                        <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full">Clássico</span>
                    </div>
                    <p class="text-gray-600 mb-4">Design tradicional francês com ponta branca, perfeito para qualquer ocasião.</p>
                    <div class="flex justify-between items-center">
                        <span class="text-2xl font-bold text-gray-800">R$ 40,00</span>
                        <button class="text-gray-400 hover:text-gray-600">
                            <i class="fas fa-ellipsis-v"></i>
                        </button>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Client Modal -->
    <div id="clientModal" class="hidden fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center">
        <div class="bg-white rounded-lg p-6 w-full max-w-md modal">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold">Novo Cliente</h3>
                <button id="closeClientModal" class="text-gray-400 hover:text-gray-600">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <form id="clientForm">
                <div class="mb-4">
                    <label for="clientName" class="block text-gray-700 mb-2">Nome Completo*</label>
                    <input type="text" id="clientName" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500" required>
                </div>
                <div class="mb-4">
                    <label for="clientPhone" class="block text-gray-700 mb-2">Telefone*</label>
                    <input type="tel" id="clientPhone" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500" required>
                </div>
                <div class="mb-4">
                    <label for="clientEmail" class="block text-gray-700 mb-2">E-mail</label>
                    <input type="email" id="clientEmail" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500">
                </div>
                <div class="mb-4">
                    <label for="clientNotes" class="block text-gray-700 mb-2">Observações</label>
                    <textarea id="clientNotes" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"></textarea>
                </div>
                <div class="flex justify-end space-x-3">
                    <button type="button" id="cancelClient" class="px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-100 transition">
                        Cancelar
                    </button>
                    <button type="submit" class="px-4 py-2 bg-purple-500 text-white rounded-lg hover:bg-purple-600 transition">
                        Salvar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Service Modal -->
    <div id="serviceModal" class="hidden fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center">
        <div class="bg-white rounded-lg p-6 w-full max-w-md modal">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold">Novo Serviço</h3>
                <button id="closeServiceModal" class="text-gray-400 hover:text-gray-600">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <form id="serviceForm">
                <div class="mb-4">
                    <label for="serviceName" class="block text-gray-700 mb-2">Nome do Serviço*</label>
                    <input type="text" id="serviceName" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500" required>
                </div>
                <div class="mb-4">
                    <label for="serviceDescription" class="block text-gray-700 mb-2">Descrição</label>
                    <textarea id="serviceDescription" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"></textarea>
                </div>
                <div class="mb-4">
                    <label for="servicePrice" class="block text-gray-700 mb-2">Preço*</label>
                    <input type="number" id="servicePrice" step="0.01" min="0" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500" required>
                </div>
                <div class="mb-4">
                    <label for="serviceDuration" class="block text-gray-700 mb-2">Duração (minutos)*</label>
                    <input type="number" id="serviceDuration" min="15" step="15" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500" required>
                </div>
                <div class="flex justify-end space-x-3">
                    <button type="button" id="cancelService" class="px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-100 transition">
                        Cancelar
                    </button>
                    <button type="submit" class="px-4 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 transition">
                        Salvar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Appointment Modal -->
    <div id="appointmentModal" class="hidden fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center">
        <div class="bg-white rounded-lg p-6 w-full max-w-md modal">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold">Novo Agendamento</h3>
                <button id="closeAppointmentModal" class="text-gray-400 hover:text-gray-600">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <form id="appointmentForm">
                <div class="mb-4">
                    <label for="appointmentClient" class="block text-gray-700 mb-2">Cliente*</label>
                    <select id="appointmentClient" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                        <option value="">Selecione um cliente</option>
                        <option value="1">Ana Silva</option>
                        <option value="2">Carla Fernandes</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label for="appointmentService" class="block text-gray-700 mb-2">Serviço*</label>
                    <select id="appointmentService" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                        <option value="">Selecione um serviço</option>
                        <option value="1">Banho de Gel</option>
                        <option value="2">Alongamento de Fibra</option>
                        <option value="3">Francesinha</option>
                    </select>
                </div>
                <div class="grid grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="appointmentDate" class="block text-gray-700 mb-2">Data*</label>
                        <input type="date" id="appointmentDate" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                    </div>
                    <div>
                        <label for="appointmentTime" class="block text-gray-700 mb-2">Hora*</label>
                        <input type="time" id="appointmentTime" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                    </div>
                </div>
                <div class="mb-4">
                    <label for="appointmentNotes" class="block text-gray-700 mb-2">Observações</label>
                    <textarea id="appointmentNotes" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500"></textarea>
                </div>
                <div class="flex justify-end space-x-3">
                    <button type="button" id="cancelAppointment" class="px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-100 transition">
                        Cancelar
                    </button>
                    <button type="submit" class="px-4 py-2 bg-pink-500 text-white rounded-lg hover:bg-pink-600 transition">
                        Agendar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Portfolio Modal -->
    <div id="portfolioModal" class="hidden fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center">
        <div class="bg-white rounded-lg p-6 w-full max-w-md modal">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold">Adicionar ao Portfólio</h3>
                <button id="closePortfolioModal" class="text-gray-400 hover:text-gray-600">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <form id="portfolioForm">
                <div class="mb-4">
                    <label class="block text-gray-700 mb-2">Imagem*</label>
                    <div class="border-2 border-dashed border-gray-300 rounded-lg p-6 text-center">
                        <i class="fas fa-cloud-upload-alt text-4xl text-gray-400 mb-2"></i>
                        <p class="text-gray-500 mb-2">Arraste e solte a imagem aqui</p>
                        <p class="text-gray-400 text-sm mb-3">ou</p>
                        <button type="button" class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition">
                            Selecione do Computador
                        </button>
                    </div>
                </div>
                <div class="mb-4">
                    <label for="portfolioTitle" class="block text-gray-700 mb-2">Título*</label>
                    <input type="text" id="portfolioTitle" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <div class="mb-4">
                    <label for="portfolioDate" class="block text-gray-700 mb-2">Data*</label>
                    <input type="date" id="portfolioDate" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <div class="mb-4">
                    <label for="portfolioService" class="block text-gray-700 mb-2">Serviço Relacionado</label>
                    <select id="portfolioService" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="">Nenhum</option>
                        <option value="1">Banho de Gel</option>
                        <option value="2">Alongamento de Fibra</option>
                        <option value="3">Francesinha</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label for="portfolioDescription" class="block text-gray-700 mb-2">Descrição</label>
                    <textarea id="portfolioDescription" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"></textarea>
                </div>
                <div class="flex justify-end space-x-3">
                    <button type="button" id="cancelPortfolio" class="px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-100 transition">
                        Cancelar
                    </button>
                    <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition">
                        Adicionar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-8">
        <div class="container mx-auto px-4">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div>
                    <h3 class="text-lg font-semibold mb-4">NailPro</h3>
                    <p class="text-gray-400">O sistema completo para gerenciar seu negócio de design de unhas com eficiência e estilo.</p>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-4">Links Rápidos</h3>
                    <ul class="space-y-2">
                        <li><a href="#dashboard" class="text-gray-400 hover:text-white transition">Dashboard</a></li>
                        <li><a href="#agenda" class="text-gray-400 hover:text-white transition">Agenda</a></li>
                        <li><a href="#clientes" class="text-gray-400 hover:text-white transition">Clientes</a></li>
                        <li><a href="#portfolio" class="text-gray-400 hover:text-white transition">Portfólio</a></li>
                        <li><a href="#servicos" class="text-gray-400 hover:text-white transition">Serviços</a></li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-4">Contato</h3>
                    <ul class="space-y-2 text-gray-400">
                        <li class="flex items-center space-x-2">
                            <i class="fas fa-envelope"></i>
                            <span>contato@nailpro.com.br</span>
                        </li>
                        <li class="flex items-center space-x-2">
                            <i class="fas fa-phone"></i>
                            <span>(11) 98765-4321</span>
                        </li>
                    </ul>
                </div>
            </div>
            <div class="border-t border-gray-700 mt-8 pt-8 text-center text-gray-400">
                <p>&copy; 2023 NailPro. Todos os direitos reservados.</p>
            </div>
        </div>
    </footer>
    </div> <!-- closing appContent div -->

    <script>
        // Authentication functions
        function showRegisterModal() {
            const modal = `
                <div class="bg-white rounded-xl shadow-2xl p-8 w-full max-w-md">
                    <div class="text-center mb-8">
                        <h3 class="text-2xl font-bold text-gray-800">Criar Nova Conta</h3>
                    </div>
                    <form id="registerForm">
                        <div class="mb-4">
                            <label for="registerName" class="block text-gray-700 mb-2">Nome Completo*</label>
                            <input type="text" id="registerName" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                        </div>
                        <div class="mb-4">
                            <label for="registerEmail" class="block text-gray-700 mb-2">E-mail*</label>
                            <input type="email" id="registerEmail" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                        </div>
                        <div class="mb-4">
                            <label for="registerPhone" class="block text-gray-700 mb-2">Telefone</label>
                            <input type="tel" id="registerPhone" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500">
                        </div>
                        <div class="mb-6">
                            <label for="registerPassword" class="block text-gray-700 mb-2">Senha*</label>
                            <input type="password" id="registerPassword" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500" required>
                        </div>
                        <button type="submit" class="w-full bg-pink-500 hover:bg-pink-600 text-white py-2 px-4 rounded-lg transition duration-300">
                            Cadastrar
                        </button>
                    </form>
                </div>
            `;
            document.getElementById('loginPage').innerHTML = modal;
            
            // Handle form submission
            document.getElementById('registerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                alert('Conta criada com sucesso! Faça login para continuar.');
                window.location.reload(); // Reload to show login form again
            });
        }

        function login() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appContent').classList.remove('hidden');
        }

        function logout() {
            document.getElementById('loginPage').classList.remove('hidden');
            document.getElementById('appContent').classList.add('hidden');
            document.getElementById('email').value = '';
            document.getElementById('password').value = '';
        }

        // Authentication functions
        function login() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appContent').classList.remove('hidden');
        }

        function logout() {
            document.getElementById('loginPage').classList.remove('hidden');
            document.getElementById('appContent').classList.add('hidden');
            document.getElementById('email').value = '';
            document.getElementById('password').value = '';
        }

        // Login Form
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            // Simple validation
            if (email && password) {
                login();
            }
        });

        // Mobile menu toggle
        document.querySelector('header button').addEventListener('click', function() {
            document.getElementById('mobileMenu').classList.remove('hidden');
        });
        
        document.getElementById('closeMobileMenu').addEventListener('click', function() {
            document.getElementById('mobileMenu').classList.add('hidden');
        });

        // Modal toggles
        const toggleModal = (modalId, openBtnId, closeBtnId, cancelBtnId) => {
            const modal = document.getElementById(modalId);
            const openBtn = document.getElementById(openBtnId);
            const closeBtn = document.getElementById(closeBtnId);
            const cancelBtn = document.getElementById(cancelBtnId);
            
            openBtn.addEventListener('click', () => {
                modal.classList.remove('hidden');
            });
            
            closeBtn.addEventListener('click', () => {
                modal.classList.add('hidden');
            });
            
            if (cancelBtn) {
                cancelBtn.addEventListener('click', () => {
                    modal.classList.add('hidden');
                });
            }
        };

        toggleModal('clientModal', 'openClientModal', 'closeClientModal', 'cancelClient');
        toggleModal('serviceModal', 'openServiceModal', 'closeServiceModal', 'cancelService');
        toggleModal('appointmentModal', 'openAppointmentModal', 'closeAppointmentModal', 'cancelAppointment');
        toggleModal('portfolioModal', 'openPortfolioModal', 'closePortfolioModal', 'cancelPortfolio');

        // Generate calendar
        function generateCalendar() {
            const calendar = document.getElementById('calendar');
            const currentDate = new Date();
            const currentYear = currentDate.getFullYear();
            const currentMonth = currentDate.getMonth();
            
            // Get first day of month and total days in month
            const firstDay = new Date(currentYear, currentMonth, 1).getDay();
            const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
            
            // Get last month's days to show on calendar
            const prevMonthDays = new Date(currentYear, currentMonth, 0).getDate();
            
            // Clear calendar
            calendar.innerHTML = '';
            
            // Previous month days
            for (let i = firstDay - 1; i >= 0; i--) {
                const dayElement = document.createElement('div');
                dayElement.classList.add('p-2', 'text-center', 'text-gray-400', 'calendar-day', 'disabled');
                dayElement.textContent = prevMonthDays - i;
                calendar.appendChild(dayElement);
            }
            
            // Current month days
            for (let i = 1; i <= daysInMonth; i++) {
                const dayElement = document.createElement('div');
                dayElement.classList.add('p-2', 'text-center', 'calendar-day');
                dayElement.textContent = i;
                
                // Highlight current day
                if (i === currentDate.getDate() && currentMonth === new Date().getMonth() && currentYear === new Date().getFullYear()) {
                    dayElement.classList.add('selected');
                }
                
                calendar.appendChild(dayElement);
            }
            
            // Next month days (to fill the remaining spaces)
            const totalCells = firstDay + daysInMonth;
            const remainingCells = totalCells <= 35 ? 35 - totalCells : 42 - totalCells;
            
            for (let i = 1; i <= remainingCells; i++) {
                const dayElement = document.createElement('div');
                dayElement.classList.add('p-2', 'text-center', 'text-gray-400', 'calendar-day', 'disabled');
                dayElement.textContent = i;
                calendar.appendChild(dayElement);
            }
        }

        // Event listeners
        document.getElementById('logoutBtn').addEventListener('click', logout);
        document.getElementById('registerBtn').addEventListener('click', showRegisterModal);
        
        // Logout button
        document.getElementById('logoutBtn').addEventListener('click', logout);
        
        // Initialize calendar
        generateCalendar();

        // Form submissions
        document.getElementById('clientForm').addEventListener('submit', function(e) {
            e.preventDefault();
            alert('Cliente adicionado com sucesso!');
            this.reset();
            document.getElementById('clientModal').classList.add('hidden');
        });
        
        document.getElementById('serviceForm').addEventListener('submit', function(e) {
            e.preventDefault();
            alert('Serviço adicionado com sucesso!');
            this.reset();
            document.getElementById('serviceModal').classList.add('hidden');
        });
        
        document.getElementById('appointmentForm').addEventListener('submit', function(e) {
            e.preventDefault();
            alert('Agendamento realizado com sucesso!');
            this.reset();
            document.getElementById('appointmentModal').classList.add('hidden');
        });
        
        document.getElementById('portfolioForm').addEventListener('submit', function(e) {
            e.preventDefault();
            alert('Foto adicionada ao portfólio com sucesso!');
            this.reset();
            document.getElementById('portfolioModal').classList.add('hidden');
        });
    </script>
</body>
</html>
