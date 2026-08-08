Smart Laboratory Asset & Inventory Management System for higher education institution. Designed for Infrastructure and Equipment Accreditation compliance with real-time stock tracking, QR/Barcode scanning, and maintenance logs


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EquipX - Smart Laboratory Asset & Inventory Management </title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Chart.js for Analytics -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- QRCode Generator Lib -->
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.1/build/qrcode.min.js"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eef2ff',
                            100: '#e0e7ff',
                            500: '#6366f1',
                            600: '#4f46e5',
                            700: '#4338ca',
                            900: '#312e81'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .print-only { display: none; }
        @media print {
            body * { visibility: hidden; }
            #printable-area, #printable-area * { visibility: visible; }
            #printable-area { position: absolute; left: 0; top: 0; width: 100%; }
            .no-print { display: none !important; }
            .print-only { display: block; }
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #c7d2fe; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #818cf8; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 antialiased min-h-screen flex flex-col">

    <!-- ========================================== -->
    <!-- LOGIN / AUTH SCREEN                        -->
    <!-- ========================================== -->
    <div id="auth-screen" class="fixed inset-0 z-50 flex items-center justify-center bg-gradient-to-br from-indigo-900 via-slate-900 to-black p-4">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md overflow-hidden border border-gray-100 animate-fade-in">
            <div class="bg-indigo-600 p-6 text-white text-center relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-32 h-32 bg-indigo-500 rounded-full opacity-50 pointer-events-none"></div>
                <div class="inline-flex items-center justify-center w-16 h-16 bg-white/10 rounded-2xl mb-3 backdrop-blur-sm border border-white/20">
                    <i class="fa-solid fa-microscope text-3xl text-white"></i>
                </div>
                <h1 class="text-2xl font-bold tracking-tight">EquipX</h1>
                <p class="text-indigo-200 text-sm mt-1">Smart Lab Infrastructure & Accreditation System</p>
            </div>
            <div class="p-8">
                <div class="mb-6">
                    <label class="block text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2">Select Demo Role to Quick Login</label>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="quickLogin('admin')" class="p-3 text-left border rounded-xl hover:border-indigo-600 hover:bg-indigo-50/50 transition flex items-center gap-3 group">
                            <div class="w-8 h-8 rounded-lg bg-indigo-100 text-indigo-600 flex items-center justify-center font-bold text-xs group-hover:bg-indigo-600 group-hover:text-white transition">AD</div>
                            <div>
                                <div class="text-sm font-semibold text-gray-900">Admin</div>
                                <div class="text-xs text-gray-500">Full Access</div>
                            </div>
                        </button>
                        <button onclick="quickLogin('staff')" class="p-3 text-left border rounded-xl hover:border-indigo-600 hover:bg-indigo-50/50 transition flex items-center gap-3 group">
                            <div class="w-8 h-8 rounded-lg bg-blue-100 text-blue-600 flex items-center justify-center font-bold text-xs group-hover:bg-blue-600 group-hover:text-white transition">ST</div>
                            <div>
                                <div class="text-sm font-semibold text-gray-900">Lab Staff</div>
                                <div class="text-xs text-gray-500">Scan & Issue</div>
                            </div>
                        </button>
                        <button onclick="quickLogin('student')" class="p-3 text-left border rounded-xl hover:border-indigo-600 hover:bg-indigo-50/50 transition flex items-center gap-3 group">
                            <div class="w-8 h-8 rounded-lg bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold text-xs group-hover:bg-emerald-600 group-hover:text-white transition">SD</div>
                            <div>
                                <div class="text-sm font-semibold text-gray-900">Student</div>
                                <div class="text-xs text-gray-500">Borrow & Return</div>
                            </div>
                        </button>
                        <button onclick="quickLogin('evaluator')" class="p-3 text-left border rounded-xl hover:border-indigo-600 hover:bg-indigo-50/50 transition flex items-center gap-3 group">
                            <div class="w-8 h-8 rounded-lg bg-amber-100 text-amber-600 flex items-center justify-center font-bold text-xs group-hover:bg-amber-600 group-hover:text-white transition">EV</div>
                            <div>
                                <div class="text-sm font-semibold text-gray-900">Evaluator</div>
                                <div class="text-xs text-gray-500"></div>
                            </div>
                        </button>
                    </div>
                </div>

                <div class="relative flex py-2 items-center">
                    <div class="flex-grow border-t border-gray-200"></div>
                    <span class="flex-shrink mx-4 text-gray-400 text-xs uppercase tracking-wider">Or sign in manually</span>
                    <div class="flex-grow border-t border-gray-200"></div>
                </div>

                <form id="login-form" onsubmit="handleLogin(event)" class="mt-4 space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Email / Username</label>
                        <input type="text" id="login-email" required class="w-full px-4 py-2 border rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 text-sm" placeholder="admin@university.edu">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                        <input type="password" id="login-password" required class="w-full px-4 py-2 border rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 text-sm" placeholder="••••••••">
                    </div>
                    <button type="submit" class="w-full py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-xl shadow-lg shadow-indigo-200 transition">
                        Sign In
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- ========================================== -->
    <!-- MAIN APP WRAPPER                           -->
    <!-- ========================================== -->
    <div id="app-wrapper" class="flex h-screen overflow-hidden hidden">
        
        <!-- SIDEBAR -->
        <aside id="sidebar" class="w-64 bg-slate-900 text-slate-300 flex flex-col flex-shrink-0 transition-all duration-300 z-30">
            <div class="p-5 border-b border-slate-800 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="w-9 h-9 bg-indigo-600 rounded-xl flex items-center justify-center text-white font-bold shadow-md shadow-indigo-900/50">
                        <i class="fa-solid fa-microscope text-lg"></i>
                    </div>
                    <div>
                        <span class="font-bold text-white text-lg tracking-wide">EquipX</span>
                        <span class="block text-[10px] text-indigo-400 font-semibold uppercase tracking-wider"></span>
                    </div>
                </div>
            </div>

            <!-- Navigation Links -->
            <nav id="sidebar-nav" class="flex-1 overflow-y-auto px-3 py-4 space-y-1">
                <!-- Dynamically populated based on role -->
            </nav>

            <!-- User Footer Profile & Logout -->
            <div class="p-4 border-t border-slate-800 flex items-center justify-between">
                <div class="flex items-center gap-3 truncate">
                    <div id="user-avatar" class="w-9 h-9 rounded-full bg-indigo-600 text-white flex items-center justify-center font-bold text-sm flex-shrink-0">AD</div>
                    <div class="truncate">
                        <div id="user-name-display" class="text-sm font-medium text-white truncate">Administrator</div>
                        <div id="user-role-display" class="text-xs text-indigo-400 truncate uppercase">Admin</div>
                    </div>
                </div>
                <button onclick="logout()" title="Logout" class="p-2 text-slate-400 hover:text-red-400 hover:bg-slate-800 rounded-lg transition">
                    <i class="fa-solid fa-right-from-bracket"></i>
                </button>
            </div>
        </aside>

        <!-- CONTENT AREA -->
        <div class="flex-1 flex flex-col overflow-hidden">
            <!-- TOP HEADER -->
            <header class="h-16 bg-white border-b border-gray-200 flex items-center justify-between px-6 z-20">
                <div class="flex items-center gap-4">
                    <button onclick="toggleSidebar()" class="text-gray-500 hover:text-gray-700 md:hidden">
                        <i class="fa-solid fa-bars text-lg"></i>
                    </button>
                    <!-- Global Search Bar -->
                    <div class="relative w-72 sm:w-96">
                        <span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-gray-400">
                            <i class="fa-solid fa-magnifying-glass text-sm"></i>
                        </span>
                        <input type="text" id="global-search" oninput="handleGlobalSearch(this.value)" placeholder="Search equipment, asset ID, lab, serial..." class="w-full pl-9 pr-4 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:bg-white transition">
                        <!-- Search Results Dropdown -->
                        <div id="global-search-results" class="absolute left-0 right-0 mt-2 bg-white rounded-xl shadow-2xl border border-gray-100 max-h-96 overflow-y-auto hidden z-50"></div>
                    </div>
                </div>

                <div class="flex items-center gap-3">
                    <!-- Notifications Button -->
                    <div class="relative">
                        <button onclick="toggleNotifications()" class="relative p-2 text-gray-600 hover:text-indigo-600 hover:bg-gray-100 rounded-xl transition">
                            <i class="fa-regular fa-bell text-lg"></i>
                            <span id="notification-badge" class="absolute top-1 right-1 w-5 h-5 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">3</span>
                        </button>
                        <!-- Notifications Dropdown -->
                        <div id="notification-dropdown" class="absolute right-0 mt-2 w-80 bg-white rounded-2xl shadow-2xl border border-gray-100 hidden z-50 overflow-hidden">
                            <div class="p-4 border-b border-gray-100 flex items-center justify-between bg-gray-50">
                                <span class="font-bold text-sm text-gray-800">Alert Center & Notifications</span>
                                <span class="text-xs bg-red-100 text-red-600 font-bold px-2 py-0.5 rounded-full" id="notification-count-badge">3 alerts</span>
                            </div>
                            <div id="notification-list" class="max-h-80 overflow-y-auto divide-y divide-gray-100">
                                <!-- Populated dynamically -->
                            </div>
                        </div>
                    </div>

                    <!-- Quick  Shortcut -->
                    <button onclick="switchView('accreditation')" class="hidden sm:flex items-center gap-2 px-3 py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 text-sm font-semibold rounded-xl border border-indigo-200 transition">
                        <i class="fa-solid fa-award text-indigo-600"></i>
                        <span>V11 Dashboard</span>
                    </button>
                </div>
            </header>

            <!-- MAIN VIEW CONTAINER -->
            <main id="main-content" class="flex-1 overflow-y-auto p-6 bg-gray-50/50">
                <!-- Dynamic View Rendered Here -->
            </main>
        </div>
    </div>

    <!-- ========================================== -->
    <!-- MODAL CONTAINER                            -->
    <!-- ========================================== -->
    <div id="modal-container" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm hidden">
        <div id="modal-content" class="bg-white rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto m-4 border border-gray-100">
            <!-- Modal Body -->
        </div>
    </div>

    <!-- ========================================== -->
    <!-- APPLICATION JAVASCRIPT                     -->
    <!-- ========================================== -->
    <script>
        // --- 1. STATE & DATABASE SCHEMA ---
        const DEMO_DATA = {
            currentUser: null, // { name, role, email, id }
            departments: [
                {
                    id: 'dept-aiml',
                    name: 'Artificial Intelligence & Machine Learning (AI/ML)',
                    code: 'AIML',
                    hod: 'Dr. Alan Turing',
                    description: 'Advanced computing research and neural network laboratories.',
                    laboratories: [
                        { id: 'lab-aiml-1', name: 'Machine Learning Lab', room: '302', floor: '3rd', building: 'Tech Block A', capacity: 60, staff: 'Prof. Sarah Connor', status: 'Active' },
                        { id: 'lab-aiml-2', name: 'Computer Lab', room: '304', floor: '3rd', building: 'Tech Block A', capacity: 50, staff: 'Mr. John Doe', status: 'Active' }
                    ]
                },
                {
                    id: 'dept-cse',
                    name: 'Computer Science & Engineering (CSE)',
                    code: 'CSE',
                    hod: 'Dr. Ada Lovelace',
                    description: 'Core computing, systems architecture, and IoT research labs.',
                    laboratories: [
                        { id: 'lab-cse-1', name: 'Computer Lab', room: '201', floor: '2nd', building: 'Main Engineering', capacity: 75, staff: 'Dr. Grace Hopper', status: 'Active' }
                    ]
                },
                {
                    id: 'dept-ise',
                    name: 'Information Science & Engineering (ISE)',
                    code: 'ISE',
                    hod: 'Dr. Tim Berners-Lee',
                    description: 'Information systems, cloud computing, and database labs.',
                    laboratories: [
                        { id: 'lab-ise-1', name: 'Computer Lab', room: '401', floor: '4th', building: 'Tech Block B', capacity: 60, staff: 'Mrs. Margaret Hamilton', status: 'Active' }
                    ]
                },
                {
                    id: 'dept-ece',
                    name: 'Electronics & Communication Engineering (ECE)',
                    code: 'ECE',
                    hod: 'Dr. Nikola Tesla',
                    description: 'Embedded systems, analog circuits, and communications laboratories.',
                    laboratories: [
                        { id: 'lab-ece-1', name: 'Digital Electronics Lab', room: '101', floor: '1st', building: 'Science Wing', capacity: 50, staff: 'Mr. Thomas Edison', status: 'Active' },
                        { id: 'lab-ece-2', name: 'Analog Electronics Lab', room: '102', floor: '1st', building: 'Science Wing', capacity: 50, staff: 'Dr. James Clerk Maxwell', status: 'Active' }
                    ]
                }
            ],
            equipment: [
                // AI/ML - ML Lab
                { id: 'AIML-ML-001', name: 'GPU Workstation', category: 'Computing', deptId: 'dept-aiml', labId: 'lab-aiml-1', type: 'Hardware', mfg: 'NVIDIA', model: 'DGX Station A100', serial: 'NV-DGX-9921', mfgDate: '2024-01-10', purchaseDate: '2024-03-15', installDate: '2024-03-20', price: 450000, vendor: 'NVIDIA Corp', invoice: 'INV-2024-089', warrantyStart: '2024-03-15', warrantyExpiry: '2027-03-15', totalQty: 10, availQty: 8, issuedQty: 1, maintQty: 1, damagedQty: 0, lostQty: 0, minStock: 2, condition: 'Good', status: 'Available', location: 'Rack 1, ML Lab', desc: 'High performance AI training rig with 8x A100 GPUs.' },
                { id: 'AIML-ML-002', name: 'Machine Learning Server', category: 'Server', deptId: 'dept-aiml', labId: 'lab-aiml-1', type: 'Hardware', mfg: 'Dell', model: 'PowerEdge R750xa', serial: 'DL-R750-441', mfgDate: '2023-11-05', purchaseDate: '2023-12-01', installDate: '2023-12-10', price: 280000, vendor: 'Dell Technologies', invoice: 'INV-2023-441', warrantyStart: '2023-12-01', warrantyExpiry: '2026-12-01', totalQty: 2, availQty: 2, issuedQty: 0, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 1, condition: 'Good', status: 'Available', location: 'Server Room, ML Lab', desc: 'Dual Xeon rack server for distributed deep learning.' },
                { id: 'AIML-CL-001', name: 'Desktop Computer', category: 'Computing', deptId: 'dept-aiml', labId: 'lab-aiml-2', type: 'Hardware', mfg: 'HP', model: 'Z2 G9 Tower', serial: 'HP-Z2G9-101', mfgDate: '2024-02-10', purchaseDate: '2024-04-01', installDate: '2024-04-05', price: 85000, vendor: 'HP India', invoice: 'INV-HP-992', warrantyStart: '2024-04-01', warrantyExpiry: '2027-04-01', totalQty: 40, availQty: 38, issuedQty: 2, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 5, condition: 'Good', status: 'Available', location: 'Computer Lab B', desc: 'Workstation for student AI labs.' },
                { id: 'AIML-CL-002', name: 'Laptop', category: 'Computing', deptId: 'dept-aiml', labId: 'lab-aiml-2', type: 'Hardware', mfg: 'Apple', model: 'MacBook Pro M3 Max', serial: 'AP-MBP-331', mfgDate: '2024-01-15', purchaseDate: '2024-02-10', installDate: '2024-02-12', price: 240000, vendor: 'Apple Authorized', invoice: 'INV-APL-112', warrantyStart: '2024-02-10', warrantyExpiry: '2026-02-10', totalQty: 10, availQty: 7, issuedQty: 3, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 2, condition: 'Good', status: 'Available', location: 'Lab Cabinet 2', desc: 'Mobile ML development laptop.' },

                // CSE - Computer Lab
                { id: 'CSE-CL-001', name: 'Desktop Computer', category: 'Computing', deptId: 'dept-cse', labId: 'lab-cse-1', type: 'Hardware', mfg: 'Lenovo', model: 'ThinkCentre M90q', serial: 'LN-M90-551', mfgDate: '2023-08-12', purchaseDate: '2023-09-10', installDate: '2023-09-15', price: 65000, vendor: 'Lenovo India', invoice: 'INV-LN-332', warrantyStart: '2023-09-10', warrantyExpiry: '2026-09-10', totalQty: 40, availQty: 40, issuedQty: 0, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 5, condition: 'Good', status: 'Available', location: 'CSE Lab Row 1-4', desc: 'Standard student terminal.' },
                { id: 'CSE-CL-002', name: 'Raspberry Pi Kit', category: 'IoT', deptId: 'dept-cse', labId: 'lab-cse-1', type: 'Hardware', mfg: 'Raspberry Pi Foundation', model: 'Pi 4 Model B (8GB)', serial: 'RPI-4B-882', mfgDate: '2023-06-01', purchaseDate: '2023-07-01', installDate: '2023-07-05', price: 8500, vendor: 'Element14', invoice: 'INV-EL-091', warrantyStart: '2023-07-01', warrantyExpiry: '2025-07-01', totalQty: 20, availQty: 15, issuedQty: 4, maintQty: 1, damagedQty: 0, lostQty: 0, minStock: 3, condition: 'Good', status: 'Available', location: 'Shelf A3', desc: 'IoT prototyping kit.' },
                { id: 'CSE-CL-003', name: 'Networking Kit', category: 'Networking', deptId: 'dept-cse', labId: 'lab-cse-1', type: 'Hardware', mfg: 'Cisco', model: 'CCNA Lab Bundle Catalyst', serial: 'CSC-CCNA-11', mfgDate: '2023-05-10', purchaseDate: '2023-06-01', installDate: '2023-06-10', price: 150000, vendor: 'Cisco Systems', invoice: 'INV-CSC-55', warrantyStart: '2023-06-01', warrantyExpiry: '2026-06-01', totalQty: 10, availQty: 8, issuedQty: 2, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 2, condition: 'Good', status: 'Available', location: 'Rack C', desc: 'Routers and switches for networking labs.' },

                // ISE - Computer Lab
                { id: 'ISE-CL-001', name: 'Desktop Computer', category: 'Computing', deptId: 'dept-ise', labId: 'lab-ise-1', type: 'Hardware', mfg: 'Dell', model: 'OptiPlex 7010', serial: 'DL-OPT-701', mfgDate: '2023-10-10', purchaseDate: '2023-11-01', installDate: '2023-11-05', price: 60000, vendor: 'Dell India', invoice: 'INV-DL-881', warrantyStart: '2023-11-01', warrantyExpiry: '2026-11-01', totalQty: 40, availQty: 39, issuedQty: 1, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 5, condition: 'Good', status: 'Available', location: 'ISE Lab Main', desc: 'Information systems student terminal.' },
                { id: 'ISE-CL-002', name: 'Development Board', category: 'Embedded', deptId: 'dept-ise', labId: 'lab-ise-1', type: 'Hardware', mfg: 'Arduino', model: 'GIGA R1 WiFi', serial: 'ARD-GIGA-22', mfgDate: '2024-01-20', purchaseDate: '2024-02-15', installDate: '2024-02-20', price: 7500, vendor: 'Arduino CC', invoice: 'INV-ARD-44', warrantyStart: '2024-02-15', warrantyExpiry: '2026-02-15', totalQty: 20, availQty: 18, issuedQty: 2, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 3, condition: 'Good', status: 'Available', location: 'Cabinet B2', desc: 'Dual-core microcontroller development board.' },

                // ECE - Digital & Analog Labs
                { id: 'ECE-DIG-001', name: 'Digital Trainer Kit', category: 'Electronics', deptId: 'dept-ece', labId: 'lab-ece-1', type: 'Hardware', mfg: 'Scientech', model: 'DTK-200', serial: 'SC-DTK-01', mfgDate: '2024-03-15', purchaseDate: '2024-05-20', installDate: '2024-05-25', price: 12000, vendor: 'Scientech Tech', invoice: 'INV-SC-101', warrantyStart: '2024-05-20', warrantyExpiry: '2027-05-20', totalQty: 20, availQty: 15, issuedQty: 4, maintQty: 1, damagedQty: 0, lostQty: 0, minStock: 4, condition: 'Good', status: 'Available', location: 'Digital Lab Bench 1-10', desc: 'Microprocessor & Logic gate trainer kit.' },
                { id: 'ECE-DIG-002', name: 'Digital Multimeter', category: 'Measurement', deptId: 'dept-ece', labId: 'lab-ece-1', type: 'Hardware', mfg: 'Fluke', model: '117 True RMS', serial: 'FLK-117-99', mfgDate: '2023-09-01', purchaseDate: '2023-10-01', installDate: '2023-10-05', price: 18000, vendor: 'Fluke Corp', invoice: 'INV-FLK-22', warrantyStart: '2023-10-01', warrantyExpiry: '2026-10-01', totalQty: 10, availQty: 7, issuedQty: 2, maintQty: 1, damagedQty: 0, lostQty: 0, minStock: 2, condition: 'Good', status: 'Available', location: 'Tool Crib A', desc: 'High precision digital multimeter.' },
                { id: 'ECE-DIG-003', name: 'Digital Oscilloscope', category: 'Measurement', deptId: 'dept-ece', labId: 'lab-ece-1', type: 'Hardware', mfg: 'Keysight', model: 'EDUX1002G', serial: 'KS-OSC-55', mfgDate: '2023-07-15', purchaseDate: '2023-08-10', installDate: '2023-08-15', price: 75000, vendor: 'Keysight Technologies', invoice: 'INV-KS-77', warrantyStart: '2023-08-10', warrantyExpiry: '2026-08-10', totalQty: 5, availQty: 4, issuedQty: 1, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 1, condition: 'Good', status: 'Available', location: 'Bench 5', desc: '100MHz 2-channel digital storage oscilloscope.' },
                
                // Consumables (ECE)
                { id: 'ECE-CON-001', name: '1KΩ Resistor', category: 'Consumable', deptId: 'dept-ece', labId: 'lab-ece-1', type: 'Consumable', mfg: 'Yageo', model: 'Carbon Film 1/4W', serial: 'BULK-RES-1K', mfgDate: '2024-01-01', purchaseDate: '2024-02-01', installDate: '2024-02-05', price: 1, vendor: 'Electronic Spares', invoice: 'INV-RES-01', warrantyStart: '2024-02-01', warrantyExpiry: '2029-02-01', totalQty: 100, availQty: 95, issuedQty: 5, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 15, condition: 'New', status: 'Available', location: 'Bin R-1', desc: 'Standard 1K ohm resistors for lab experiments.' },
                { id: 'ECE-CON-002', name: 'LED (Red/Green/Blue)', category: 'Consumable', deptId: 'dept-ece', labId: 'lab-ece-1', type: 'Consumable', mfg: 'Lite-On', model: '5mm Diffused', serial: 'BULK-LED-5M', mfgDate: '2024-01-01', purchaseDate: '2024-02-01', installDate: '2024-02-05', price: 2, vendor: 'Electronic Spares', invoice: 'INV-LED-02', warrantyStart: '2024-02-01', warrantyExpiry: '2029-02-01', totalQty: 200, availQty: 180, issuedQty: 20, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 30, condition: 'New', status: 'Available', location: 'Bin L-2', desc: 'Assorted 5mm indicator LEDs.' },

                // Analog Lab ECE
                { id: 'ECE-ANA-001', name: 'Analog Trainer Kit', category: 'Electronics', deptId: 'dept-ece', labId: 'lab-ece-2', type: 'Hardware', mfg: 'Scientech', model: 'ATK-100', serial: 'SC-ATK-01', mfgDate: '2024-02-10', purchaseDate: '2024-04-01', installDate: '2024-04-05', price: 10000, vendor: 'Scientech Tech', invoice: 'INV-SC-102', warrantyStart: '2024-04-01', warrantyExpiry: '2027-04-01', totalQty: 20, availQty: 18, issuedQty: 2, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 3, condition: 'Good', status: 'Available', location: 'Analog Lab Bench', desc: 'Op-amp & BJT analog circuit trainer.' },
                { id: 'ECE-ANA-002', name: 'Function Generator', category: 'Measurement', deptId: 'dept-ece', labId: 'lab-ece-2', type: 'Hardware', mfg: 'Tektronix', model: 'AFG1022', serial: 'TK-AFG-33', mfgDate: '2023-08-01', purchaseDate: '2023-09-01', installDate: '2023-09-05', price: 45000, vendor: 'Tektronix India', invoice: 'INV-TK-88', warrantyStart: '2023-09-01', warrantyExpiry: '2026-09-01', totalQty: 5, availQty: 5, issuedQty: 0, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 1, condition: 'Good', status: 'Available', location: 'Analog Shelf 1', desc: '25MHz arbitrary waveform generator.' },
                { id: 'ECE-ANA-003', name: 'Capacitor Set', category: 'Consumable', deptId: 'dept-ece', labId: 'lab-ece-2', type: 'Consumable', mfg: 'Murata', model: 'Ceramic Disc Assorted', serial: 'BULK-CAP-SET', mfgDate: '2024-01-01', purchaseDate: '2024-02-01', installDate: '2024-02-05', price: 150, vendor: 'Electronic Spares', invoice: 'INV-CAP-03', warrantyStart: '2024-02-01', warrantyExpiry: '2029-02-01', totalQty: 100, availQty: 85, issuedQty: 15, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 20, condition: 'New', status: 'Available', location: 'Bin C-1', desc: 'Assorted ceramic and electrolytic capacitors.' },
                { id: 'ECE-ANA-004', name: 'Transistor Set', category: 'Consumable', deptId: 'dept-ece', labId: 'lab-ece-2', type: 'Consumable', mfg: 'ON Semi', model: 'BC547 / BC557 Pack', serial: 'BULK-TR-PACK', mfgDate: '2024-01-01', purchaseDate: '2024-02-01', installDate: '2024-02-05', price: 200, vendor: 'Electronic Spares', invoice: 'INV-TR-04', warrantyStart: '2024-02-01', warrantyExpiry: '2029-02-01', totalQty: 100, availQty: 90, issuedQty: 10, maintQty: 0, damagedQty: 0, lostQty: 0, minStock: 20, condition: 'New', status: 'Available', location: 'Bin T-1', desc: 'NPN and PNP general purpose transistors.' }
            ],
            transactions: [
                { id: 'TXN-101', equipmentId: 'ECE-DIG-001', name: 'Rahul Kumar', email: 'rahul@university.edu', studentId: 'ECE123', dept: 'ECE', purpose: 'Digital Lab Experiment 4', qty: 1, borrowedDate: '2026-08-08 10:30', expectedReturn: '2026-08-08 16:00', status: 'Issued' },
                { id: 'TXN-100', equipmentId: 'ECE-CON-001', name: 'Aisha Sharma', email: 'aisha@university.edu', studentId: 'ECE145', dept: 'ECE', purpose: 'Resistor divider circuit', qty: 1, borrowedDate: '2026-08-07 11:15', expectedReturn: '2026-08-07 14:00', status: 'Returned', returnDate: '2026-08-07 13:30', condition: 'Good' },
                { id: 'TXN-099', equipmentId: 'AIML-ML-001', name: 'Arun Patel', email: 'arun@university.edu', studentId: 'AIML012', dept: 'AI/ML', purpose: 'PyTorch Model Training', qty: 1, borrowedDate: '2026-08-06 09:00', expectedReturn: '2026-08-08 18:00', status: 'Issued' }
            ],
            maintenance: [
                { id: 'MAINT-01', equipmentId: 'ECE-DIG-002', date: '2026-08-08', type: 'Battery Replacement', cost: 450, vendor: 'Internal Lab Staff', status: 'Under Maintenance', desc: 'Replacing 9V internal battery and calibration check.' },
                { id: 'MAINT-02', equipmentId: 'AIML-ML-001', date: '2026-06-12', type: 'Thermal Paste Replacement', cost: 1200, vendor: 'NVIDIA Authorized', status: 'Completed', desc: 'Routine thermal maintenance for 8x GPU nodes.' },
                { id: 'MAINT-03', equipmentId: 'CSE-CL-002', date: '2026-05-20', type: 'Power Supply Repair', cost: 600, vendor: 'Element14 Support', status: 'Completed', desc: 'Replaced faulty USB-C power regulators.' }
            ],
            auditLogs: [
                { date: '08 Aug 2026 10:30', action: 'Rahul borrowed 1 Digital Trainer Kit (ECE-DIG-001).' },
                { date: '08 Aug 2026 09:15', action: 'Lab Assistant sent Digital Multimeter (ECE-DIG-002) for maintenance.' },
                { date: '07 Aug 2026 13:30', action: 'Aisha returned 1 1KΩ Resistor in Good condition.' },
                { date: '01 Aug 2026 11:00', action: 'Admin added GPU Workstation (AIML-ML-001) to Machine Learning Lab.' }
            ],
            notifications: [
                { id: 1, type: 'warning', title: 'Low Stock Alert', msg: '1KΩ Resistor stock is at 95 (Minimum: 15)', time: '10m ago' },
                { id: 2, type: 'danger', title: 'Overdue Equipment', msg: 'Digital Multimeter (ECE-DIG-002) is under maintenance past schedule.', time: '1h ago' },
                { id: 3, type: 'info', title: ' Audit', msg: 'Infrastructure readiness score is 94%. Ready for review.', time: '2h ago' }
            ]
        };

        // Current active view state
        let currentView = 'dashboard';
        let currentDeptFilter = 'all';

        // --- 2. INITIALIZATION & AUTH ---
        window.addEventListener('DOMContentLoaded', () => {
            // Check if user session exists in localStorage
            const savedUser = localStorage.getItem('EquipX_user');
            if (savedUser) {
                DEMO_DATA.currentUser = JSON.parse(savedUser);
                initApp();
            } else {
                // Show login screen
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('app-wrapper').classList.add('hidden');
            }
        });

        function quickLogin(role) {
            let user = {};
            if (role === 'admin') {
                user = { name: 'Dr. Robert Administrator', email: 'admin@university.edu', role: 'admin', id: 'ADM001' };
            } else if (role === 'staff') {
                user = { name: 'Prof. Thomas Edison', email: 'staff@university.edu', role: 'staff', id: 'STF001' };
            } else if (role === 'student') {
                user = { name: 'Rahul Kumar', email: 'rahul@university.edu', role: 'student', id: 'ECE123' };
            } else if (role === 'evaluator') {
                user = { name: 'Prof. NAAC Evaluator', email: 'evaluator@naac.gov.in', role: 'evaluator', id: 'EVA001' };
            }
            DEMO_DATA.currentUser = user;
            localStorage.setItem('EquipX_user', JSON.stringify(user));
            initApp();
        }

        function handleLogin(e) {
            e.preventDefault();
            const email = document.getElementById('login-email').value;
            let role = 'admin';
            if (email.includes('staff')) role = 'staff';
            else if (email.includes('student')) role = 'student';
            else if (email.includes('eval') || email.includes('naac')) role = 'evaluator';

            DEMO_DATA.currentUser = { name: email.split('@')[0], email: email, role: role, id: 'USR999' };
            localStorage.setItem('EquipX_user', JSON.stringify(DEMO_DATA.currentUser));
            initApp();
        }

        function logout() {
            localStorage.removeItem('EquipX_user');
            location.reload();
        }

        function initApp() {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app-wrapper').classList.remove('hidden');
            
            // Update User Profile UI
            const u = DEMO_DATA.currentUser;
            document.getElementById('user-name-display').innerText = u.name;
            document.getElementById('user-role-display').innerText = u.role.toUpperCase();
            document.getElementById('user-avatar').innerText = u.name.split(' ').map(n=>n[0]).join('').substring(0,2).toUpperCase();

            renderSidebar();
            renderNotifications();
            switchView('dashboard');
        }

        // --- 3. NAVIGATION & SIDEBAR ---
        function renderSidebar() {
            const role = DEMO_DATA.currentUser.role;
            const nav = document.getElementById('sidebar-nav');
            
            const menuItems = [
                { id: 'dashboard', label: 'Dashboard', icon: 'fa-chart-pie', roles: ['admin', 'staff', 'student', 'evaluator'] },
                { id: 'departments', label: 'Departments', icon: 'fa-building-columns', roles: ['admin', 'staff', 'evaluator'] },
                { id: 'laboratories', label: 'Laboratories', icon: 'fa-flask', roles: ['admin', 'staff', 'evaluator'] },
                { id: 'inventory', label: 'Inventory & Assets', icon: 'fa-boxes-stacked', roles: ['admin', 'staff', 'student', 'evaluator'] },
                { id: 'scan', label: '📷 Scan Equipment', icon: 'fa-qrcode', roles: ['admin', 'staff', 'student'] },
                { id: 'borrow', label: 'Borrow / Return', icon: 'fa-right-left', roles: ['admin', 'staff', 'student'] },
                { id: 'maintenance', label: 'Maintenance & Service', icon: 'fa-screwdriver-wrench', roles: ['admin', 'staff', 'evaluator'] },
                { id: 'users', label: 'User Management', icon: 'fa-users', roles: ['admin'] },
                { id: 'reports', label: 'Reports & Audit', icon: 'fa-file-lines', roles: ['admin', 'evaluator'] },
                { id: 'accreditation', label: '', icon: 'fa-award', roles: ['admin', 'evaluator'] }
            ];

            let html = '';
            menuItems.forEach(item => {
                if (item.roles.includes(role)) {
                    const active = currentView === item.id;
                    html += `
                        <button onclick="switchView('${item.id}')" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-sm font-medium transition ${active ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-600/30' : 'text-slate-300 hover:bg-slate-800 hover:text-white'}">
                            <i class="fa-solid ${item.icon} w-5 text-center"></i>
                            <span>${item.label}</span>
                        </button>
                    `;
                }
            });
            nav.innerHTML = html;
        }

        function switchView(viewId) {
            currentView = viewId;
            renderSidebar();
            const container = document.getElementById('main-content');
            
            // Scroll to top
            container.scrollTop = 0;

            switch(viewId) {
                case 'dashboard': renderDashboard(container); break;
                case 'departments': renderDepartments(container); break;
                case 'laboratories': renderLaboratories(container); break;
                case 'inventory': renderInventory(container); break;
                case 'scan': renderScanView(container); break;
                case 'borrow': renderBorrowReturnView(container); break;
                case 'maintenance': renderMaintenanceView(container); break;
                case 'users': renderUsersView(container); break;
                case 'reports': renderReportsView(container); break;
                case 'accreditation': renderAccreditationView(container); break;
                default: container.innerHTML = `<h2 class="text-xl font-bold">View not found</h2>`;
            }
        }

        function toggleSidebar() {
            const sidebar = document.getElementById('sidebar');
            sidebar.classList.toggle('-translate-x-full');
        }

        // --- 4. VIEW RENDERERS ---

        // --- Dashboard View ---
        function renderDashboard(container) {
            // Calculate stats
            let totalDepts = DEMO_DATA.departments.length;
            let totalLabs = DEMO_DATA.departments.reduce((acc, d) => acc + d.laboratories.length, 0);
            let totalEqCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.totalQty, 0);
            let availCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.availQty, 0);
            let issuedCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.issuedQty, 0);
            let maintCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.maintQty, 0);
            let damagedCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.damagedQty, 0);
            let lostCount = DEMO_DATA.equipment.reduce((acc, e) => acc + e.lostQty, 0);
            let lowStockCount = DEMO_DATA.equipment.filter(e => e.availQty <= e.minStock && e.availQty > 0).length;
            let outStockCount = DEMO_DATA.equipment.filter(e => e.availQty === 0).length;

            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Dashboard & Infrastructure Overview</h1>
                            <p class="text-sm text-gray-500">Real-time tracking for  compliance and asset management.</p>
                        </div>
                        <div class="flex items-center gap-3">
                            <button onclick="switchView('scan')" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                                <i class="fa-solid fa-qrcode"></i> Scan Asset QR
                            </button>
                            <button onclick="switchView('accreditation')" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                                <i class="fa-solid fa-award"></i> V11 Report
                            </button>
                        </div>
                    </div>

                    <!-- Metrics Grid -->
                    <div class="grid grid-cols-2 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm flex items-center gap-4">
                            <div class="w-12 h-12 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-xl font-bold"><i class="fa-solid fa-building-columns"></i></div>
                            <div>
                                <div class="text-xs font-semibold text-gray-400 uppercase">Departments / Labs</div>
                                <div class="text-2xl font-bold text-gray-900">${totalDepts} / ${totalLabs}</div>
                            </div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm flex items-center gap-4">
                            <div class="w-12 h-12 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center text-xl font-bold"><i class="fa-solid fa-boxes-stacked"></i></div>
                            <div>
                                <div class="text-xs font-semibold text-gray-400 uppercase">Total Equipment</div>
                                <div class="text-2xl font-bold text-gray-900">${totalEqCount}</div>
                            </div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm flex items-center gap-4">
                            <div class="w-12 h-12 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl font-bold"><i class="fa-solid fa-check-circle"></i></div>
                            <div>
                                <div class="text-xs font-semibold text-gray-400 uppercase">Available Units</div>
                                <div class="text-2xl font-bold text-emerald-600">${availCount}</div>
                            </div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm flex items-center gap-4">
                            <div class="w-12 h-12 rounded-xl bg-amber-50 text-amber-600 flex items-center justify-center text-xl font-bold"><i class="fa-solid fa-hand-holding"></i></div>
                            <div>
                                <div class="text-xs font-semibold text-gray-400 uppercase">Issued Items</div>
                                <div class="text-2xl font-bold text-amber-600">${issuedCount}</div>
                            </div>
                        </div>
                    </div>

                    <!-- Secondary Metrics -->
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                        <div class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Under Maintenance</div>
                            <div class="text-xl font-bold text-indigo-600">${maintCount} items</div>
                        </div>
                        <div class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Damaged / Lost</div>
                            <div class="text-xl font-bold text-red-600">${damagedCount} / ${lostCount}</div>
                        </div>
                        <div class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Low Stock Consumables</div>
                            <div class="text-xl font-bold text-amber-600">${lowStockCount} items</div>
                        </div>
                        <div class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Out of Stock</div>
                            <div class="text-xl font-bold text-red-600">${outStockCount} items</div>
                        </div>
                    </div>

                    <!-- Departments Overview Cards -->
                    <div>
                        <h2 class="text-lg font-bold text-gray-900 mb-4">Department Infrastructure Breakdown</h2>
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                            ${DEMO_DATA.departments.map(dept => {
                                const deptEq = DEMO_DATA.equipment.filter(e => e.deptId === dept.id);
                                const total = deptEq.reduce((a,b)=>a+b.totalQty,0);
                                const avail = deptEq.reduce((a,b)=>a+b.availQty,0);
                                return `
                                    <div onclick="openDepartmentDetail('${dept.id}')" class="bg-white p-6 rounded-2xl border border-gray-100 shadow-sm hover:shadow-md transition cursor-pointer flex flex-col justify-between group">
                                        <div>
                                            <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 font-bold flex items-center justify-center mb-3 group-hover:bg-indigo-600 group-hover:text-white transition">${dept.code}</div>
                                            <h3 class="font-bold text-gray-900 text-base mb-1">${dept.name}</h3>
                                            <p class="text-xs text-gray-500 mb-4">${dept.laboratories.length} Laboratories • HOD: ${dept.hod}</p>
                                        </div>
                                        <div class="border-t border-gray-100 pt-3 flex items-center justify-between text-xs font-semibold">
                                            <span class="text-gray-600">${total} Assets</span>
                                            <span class="text-emerald-600">${avail} Available</span>
                                        </div>
                                    </div>
                                `;
                            }).join('')}
                        </div>
                    </div>

                    <!-- Recent Audit Activity -->
                    <div class="bg-white p-6 rounded-2xl border border-gray-100 shadow-sm">
                        <h3 class="text-base font-bold text-gray-900 mb-4">Recent Audit Log & Transactions</h3>
                        <div class="divide-y divide-gray-100">
                            ${DEMO_DATA.auditLogs.slice(0, 5).map(log => `
                                <div class="py-3 flex items-center justify-between text-sm">
                                    <div class="flex items-center gap-3">
                                        <div class="w-2 h-2 rounded-full bg-indigo-600"></div>
                                        <span class="text-gray-800">${log.action}</span>
                                    </div>
                                    <span class="text-xs text-gray-400 font-medium">${log.date}</span>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </div>
            `;
        }

        // --- Departments View ---
        function renderDepartments(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Departments Management</h1>
                            <p class="text-sm text-gray-500">Institution academic departments and associated research laboratories.</p>
                        </div>
                        ${DEMO_DATA.currentUser.role === 'admin' ? `
                            <button onclick="openAddDepartmentModal()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                                <i class="fa-solid fa-plus"></i> Add Department
                            </button>
                        ` : ''}
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        ${DEMO_DATA.departments.map(dept => {
                            const deptEq = DEMO_DATA.equipment.filter(e => e.deptId === dept.id);
                            const totalQty = deptEq.reduce((a,b)=>a+b.totalQty,0);
                            const availQty = deptEq.reduce((a,b)=>a+b.availQty,0);
                            const maintQty = deptEq.reduce((a,b)=>a+b.maintQty,0);
                            return `
                                <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden flex flex-col justify-between">
                                    <div class="p-6">
                                        <div class="flex items-start justify-between mb-4">
                                            <div>
                                                <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-lg">${dept.code}</span>
                                                <h3 class="text-lg font-bold text-gray-900 mt-2">${dept.name}</h3>
                                            </div>
                                            <div class="text-right">
                                                <span class="text-xs text-gray-400 uppercase block font-semibold">HOD</span>
                                                <span class="text-sm font-bold text-gray-700">${dept.hod}</span>
                                            </div>
                                        </div>
                                        <p class="text-sm text-gray-600 mb-4">${dept.description}</p>
                                        
                                        <div class="mb-4">
                                            <h4 class="text-xs font-bold text-gray-400 uppercase mb-2">Laboratories (${dept.laboratories.length})</h4>
                                            <div class="space-y-2">
                                                ${dept.laboratories.map(lab => `
                                                    <div onclick="openLabDetail('${lab.id}')" class="p-3 bg-gray-50 hover:bg-indigo-50/50 rounded-xl border border-gray-100 flex items-center justify-between cursor-pointer transition">
                                                        <div>
                                                            <div class="font-semibold text-sm text-gray-900">${lab.name}</div>
                                                            <div class="text-xs text-gray-500">Room ${lab.room}, ${lab.building} • Staff: ${lab.staff}</div>
                                                        </div>
                                                        <i class="fa-solid fa-chevron-right text-xs text-gray-400"></i>
                                                    </div>
                                                `).join('')}
                                            </div>
                                        </div>
                                    </div>
                                    <div class="bg-gray-50 px-6 py-4 border-t border-gray-100 flex items-center justify-between text-xs font-semibold text-gray-600">
                                        <span>Total Equipment: <strong class="text-gray-900">${totalQty}</strong></span>
                                        <span>Available: <strong class="text-emerald-600">${availQty}</strong></span>
                                        <span>Under Service: <strong class="text-indigo-600">${maintQty}</strong></span>
                                    </div>
                                </div>
                            `;
                        }).join('')}
                    </div>
                </div>
            `;
        }

        // --- Laboratories View ---
        function renderLaboratories(container) {
            let allLabs = [];
            DEMO_DATA.departments.forEach(d => {
                d.laboratories.forEach(l => {
                    allLabs.push({ ...l, deptName: d.name, deptCode: d.code, deptId: d.id });
                });
            });

            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Laboratory Facilities</h1>
                            <p class="text-sm text-gray-500">All registered institutional labs and infrastructure capacity.</p>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                        ${allLabs.map(lab => {
                            const labEq = DEMO_DATA.equipment.filter(e => e.labId === lab.id);
                            const total = labEq.reduce((a,b)=>a+b.totalQty,0);
                            const avail = labEq.reduce((a,b)=>a+b.availQty,0);
                            const maint = labEq.reduce((a,b)=>a+b.maintQty,0);
                            return `
                                <div onclick="openLabDetail('${lab.id}')" class="bg-white rounded-2xl border border-gray-100 shadow-sm p-6 hover:shadow-md transition cursor-pointer flex flex-col justify-between">
                                    <div>
                                        <div class="flex items-center justify-between mb-3">
                                            <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-lg">${lab.deptCode}</span>
                                            <span class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-emerald-100 text-emerald-700">${lab.status}</span>
                                        </div>
                                        <h3 class="font-bold text-lg text-gray-900 mb-1">${lab.name}</h3>
                                        <p class="text-xs text-gray-500 mb-4">${lab.deptName}</p>
                                        
                                        <div class="space-y-2 text-xs text-gray-600 mb-4">
                                            <div class="flex justify-between"><span>Location:</span> <strong class="text-gray-800">Room ${lab.room}, ${lab.building}</strong></div>
                                            <div class="flex justify-between"><span>Capacity:</span> <strong class="text-gray-800">${lab.capacity} Students</strong></div>
                                            <div class="flex justify-between"><span>Lab In-Charge:</span> <strong class="text-gray-800">${lab.staff}</strong></div>
                                        </div>
                                    </div>
                                    <div class="border-t border-gray-100 pt-3 flex items-center justify-between text-xs font-semibold">
                                        <span class="text-gray-600">${total} Assets</span>
                                        <span class="text-emerald-600">${avail} Available</span>
                                        <span class="text-indigo-600">${maint} Maint.</span>
                                    </div>
                                </div>
                            `;
                        }).join('')}
                    </div>
                </div>
            `;
        }

        // --- Inventory & Equipment View ---
        function renderInventory(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Equipment & Consumables Inventory</h1>
                            <p class="text-sm text-gray-500">Real-time stock tracking, QR codes, and condition monitoring.</p>
                        </div>
                        <div class="flex items-center gap-3">
                            ${DEMO_DATA.currentUser.role === 'admin' || DEMO_DATA.currentUser.role === 'staff' ? `
                                <button onclick="openAddEquipmentModal()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                                    <i class="fa-solid fa-plus"></i> Add Equipment
                                </button>
                            ` : ''}
                        </div>
                    </div>

                    <!-- Filters Bar -->
                    <div class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm flex flex-wrap gap-4 items-center justify-between">
                        <div class="flex flex-wrap gap-3 items-center">
                            <select id="filter-dept" onchange="filterInventory()" class="px-3 py-2 border rounded-xl text-sm bg-gray-50">
                                <option value="all">All Departments</option>
                                ${DEMO_DATA.departments.map(d => `<option value="${d.id}">${d.code}</option>`).join('')}
                            </select>
                            <select id="filter-type" onchange="filterInventory()" class="px-3 py-2 border rounded-xl text-sm bg-gray-50">
                                <option value="all">All Types</option>
                                <option value="Hardware">Hardware / Equipment</option>
                                <option value="Consumable">Consumables</option>
                            </select>
                            <select id="filter-status" onchange="filterInventory()" class="px-3 py-2 border rounded-xl text-sm bg-gray-50">
                                <option value="all">All Status</option>
                                <option value="Available">Available</option>
                                <option value="Low Stock">Low Stock</option>
                                <option value="Under Maintenance">Under Maintenance</option>
                            </select>
                        </div>
                        <div class="text-xs text-gray-500 font-semibold" id="inventory-count-label">
                            Showing ${DEMO_DATA.equipment.length} items
                        </div>
                    </div>

                    <!-- Inventory Table -->
                    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead>
                                    <tr class="bg-gray-50 border-b border-gray-100 text-[11px] font-bold text-gray-400 uppercase tracking-wider">
                                        <th class="p-4">Item & Asset ID</th>
                                        <th class="p-4">Department / Lab</th>
                                        <th class="p-4">Manufacturer & Model</th>
                                        <th class="p-4 text-center">Quantities (Avail/Total)</th>
                                        <th class="p-4">Status / Condition</th>
                                        <th class="p-4 text-right">Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="inventory-table-body" class="divide-y divide-gray-100 text-sm">
                                    ${renderInventoryRows(DEMO_DATA.equipment)}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderInventoryRows(items) {
            if (items.length === 0) {
                return `<tr><td colspan="6" class="p-8 text-center text-gray-400">No equipment found matching filters.</td></tr>`;
            }
            return items.map(item => {
                const dept = DEMO_DATA.departments.find(d => d.id === item.deptId);
                let labName = 'Lab';
                if (dept) {
                    const lab = dept.laboratories.find(l => l.id === item.labId);
                    if (lab) labName = lab.name;
                }

                let statusBadge = '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-700">🟢 Available</span>';
                if (item.availQty === 0) {
                    statusBadge = '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-red-100 text-red-700">🔴 Out of Stock</span>';
                } else if (item.availQty <= item.minStock) {
                    statusBadge = '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-700">🟡 Low Stock</span>';
                }
                if (item.maintQty > 0) {
                    statusBadge = '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-indigo-100 text-indigo-700">🔵 Maintenance</span>';
                }

                return `
                    <tr class="hover:bg-gray-50/50 transition">
                        <td class="p-4">
                            <div class="font-bold text-gray-900">${item.name}</div>
                            <div class="text-xs font-mono text-indigo-600">${item.id}</div>
                        </td>
                        <td class="p-4">
                            <div class="font-medium text-gray-800">${dept ? dept.code : 'N/A'}</div>
                            <div class="text-xs text-gray-500">${labName}</div>
                        </td>
                        <td class="p-4">
                            <div class="font-medium text-gray-800">${item.mfg}</div>
                            <div class="text-xs text-gray-500">${item.model}</div>
                        </td>
                        <td class="p-4 text-center">
                            <span class="font-bold text-emerald-600">${item.availQty}</span> / <span class="text-gray-600">${item.totalQty}</span>
                        </td>
                        <td class="p-4">
                            ${statusBadge}
                        </td>
                        <td class="p-4 text-right space-x-2">
                            <button onclick="openEquipmentDetail('${item.id}')" class="px-3 py-1.5 bg-gray-100 hover:bg-gray-200 text-gray-700 text-xs font-semibold rounded-lg transition" title="View Details">
                                <i class="fa-solid fa-eye"></i>
                            </button>
                            <button onclick="openQrModal('${item.id}')" class="px-3 py-1.5 bg-indigo-50 hover:bg-indigo-100 text-indigo-600 text-xs font-semibold rounded-lg transition" title="QR Code">
                                <i class="fa-solid fa-qrcode"></i>
                            </button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function filterInventory() {
            const deptId = document.getElementById('filter-dept').value;
            const type = document.getElementById('filter-type').value;
            const status = document.getElementById('filter-status').value;

            let filtered = DEMO_DATA.equipment.filter(item => {
                if (deptId !== 'all' && item.deptId !== deptId) return false;
                if (type !== 'all' && item.type !== type) return false;
                if (status === 'Available' && item.availQty === 0) return false;
                if (status === 'Low Stock' && (item.availQty > item.minStock || item.availQty === 0)) return false;
                if (status === 'Under Maintenance' && item.maintQty === 0) return false;
                return true;
            });

            document.getElementById('inventory-table-body').innerHTML = renderInventoryRows(filtered);
            document.getElementById('inventory-count-label').innerText = `Showing ${filtered.length} items`;
        }

        // --- Scan Equipment View (Mobile Camera Scanner Simulation) ---
        function renderScanView(container) {
            container.innerHTML = `
                <div class="max-w-xl mx-auto space-y-6">
                    <div class="text-center">
                        <div class="inline-flex items-center justify-center w-16 h-16 bg-indigo-50 text-indigo-600 rounded-2xl mb-3 shadow-sm">
                            <i class="fa-solid fa-qrcode text-3xl"></i>
                        </div>
                        <h1 class="text-2xl font-bold text-gray-900">Mobile QR & Barcode Scanner</h1>
                        <p class="text-sm text-gray-500">Scan equipment barcode or enter Asset ID to instantly borrow, return, or service.</p>
                    </div>

                    <!-- Scan Box Simulated Camera View -->
                    <div class="bg-slate-900 rounded-3xl p-6 shadow-xl text-white relative overflow-hidden text-center">
                        <div class="absolute inset-0 opacity-20 bg-[radial-gradient(#6366f1_1px,transparent_1px)] [background-size:16px_16px]"></div>
                        <div class="relative z-10 py-10">
                            <div class="w-64 h-40 mx-auto border-2 border-dashed border-indigo-400 rounded-2xl flex flex-col items-center justify-center mb-6 relative bg-slate-800/50 backdrop-blur-sm">
                                <div class="absolute inset-0 flex items-center justify-center">
                                    <div class="w-full h-0.5 bg-red-500 animate-pulse"></div>
                                </div>
                                <i class="fa-solid fa-camera text-3xl text-indigo-400 mb-2"></i>
                                <span class="text-xs text-slate-300 font-medium">Align QR / Barcode within frame</span>
                            </div>

                            <label class="block text-xs font-semibold text-slate-400 uppercase mb-2">Or select a demo asset to simulate scan:</label>
                            <div class="flex gap-2 max-w-sm mx-auto">
                                <select id="simulated-scan-id" class="flex-1 px-4 py-2.5 bg-slate-800 border border-slate-700 text-white rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                                    ${DEMO_DATA.equipment.map(e => `<option value="${e.id}">${e.name} (${e.id})</option>`).join('')}
                                </select>
                                <button onclick="executeScan()" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold text-sm rounded-xl shadow-lg transition">
                                    Scan
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            `;
        }

        function executeScan() {
            const assetId = document.getElementById('simulated-scan-id').value;
            openEquipmentDetail(assetId);
        }

        // --- Borrow / Return View ---
        function renderBorrowReturnView(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div>
                        <h1 class="text-2xl font-bold text-gray-900">Borrow & Return Transactions</h1>
                        <p class="text-sm text-gray-500">Active student borrowings, movement history, and return logs.</p>
                    </div>

                    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
                        <div class="p-4 border-b border-gray-100 bg-gray-50 font-bold text-sm text-gray-700">
                            Active & Past Transactions
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead>
                                    <tr class="bg-gray-50/50 border-b border-gray-100 text-[11px] font-bold text-gray-400 uppercase tracking-wider">
                                        <th class="p-4">Transaction ID</th>
                                        <th class="p-4">Equipment / Asset ID</th>
                                        <th class="p-4">Borrower Name (ID)</th>
                                        <th class="p-4">Purpose</th>
                                        <th class="p-4">Borrowed Date</th>
                                        <th class="p-4">Status</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-gray-100 text-sm">
                                    ${DEMO_DATA.transactions.map(t => {
                                        const eq = DEMO_DATA.equipment.find(e => e.id === t.equipmentId);
                                        return `
                                            <tr class="hover:bg-gray-50/50 transition">
                                                <td class="p-4 font-mono text-xs text-indigo-600 font-bold">${t.id}</td>
                                                <td class="p-4">
                                                    <div class="font-bold text-gray-900">${eq ? eq.name : 'Item'}</div>
                                                    <div class="text-xs text-gray-500">${t.equipmentId}</div>
                                                </td>
                                                <td class="p-4">
                                                    <div class="font-medium text-gray-800">${t.name}</div>
                                                    <div class="text-xs text-gray-500">ID: ${t.studentId} • ${t.email}</div>
                                                </td>
                                                <td class="p-4 text-gray-600">${t.purpose}</td>
                                                <td class="p-4 text-xs text-gray-500">${t.borrowedDate}</td>
                                                <td class="p-4">
                                                    ${t.status === 'Issued' ? '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-700">🔴 Issued</span>' : '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-700">🟢 Returned</span>'}
                                                </td>
                                            </tr>
                                        `;
                                    }).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            `;
        }

        // --- Maintenance & Service View ---
        function renderMaintenanceView(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Maintenance & Service History</h1>
                            <p class="text-sm text-gray-500">Equipment service logs, vendor repair records, and accreditation compliance.</p>
                        </div>
                    </div>

                    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead>
                                    <tr class="bg-gray-50 border-b border-gray-100 text-[11px] font-bold text-gray-400 uppercase tracking-wider">
                                        <th class="p-4">Service ID</th>
                                        <th class="p-4">Equipment ID</th>
                                        <th class="p-4">Service Type & Description</th>
                                        <th class="p-4">Vendor / Center</th>
                                        <th class="p-4">Cost (₹)</th>
                                        <th class="p-4">Status</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-gray-100 text-sm">
                                    ${DEMO_DATA.maintenance.map(m => {
                                        const eq = DEMO_DATA.equipment.find(e => e.id === m.equipmentId);
                                        return `
                                            <tr class="hover:bg-gray-50/50 transition">
                                                <td class="p-4 font-mono text-xs font-bold text-indigo-600">${m.id}</td>
                                                <td class="p-4 font-mono font-medium text-gray-900">${m.equipmentId} <span class="block text-xs text-gray-500">${eq ? eq.name : ''}</span></td>
                                                <td class="p-4">
                                                    <div class="font-bold text-gray-800">${m.type}</div>
                                                    <div class="text-xs text-gray-500">${m.desc}</div>
                                                </td>
                                                <td class="p-4 text-gray-600">${m.vendor}</td>
                                                <td class="p-4 font-semibold text-gray-900">₹${m.cost}</td>
                                                <td class="p-4">
                                                    ${m.status === 'Under Maintenance' ? '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-700">🔵 In Service</span>' : '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-700">🟢 Completed</span>'}
                                                </td>
                                            </tr>
                                        `;
                                    }).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            `;
        }

        // --- User Management View ---
        function renderUsersView(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">User Management & Role Permissions</h1>
                            <p class="text-sm text-gray-500">Manage administrator, lab staff, student, and evaluator access.</p>
                        </div>
                        <button onclick="alert('User invitation link sent successfully.')" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                            <i class="fa-solid fa-user-plus"></i> Add User
                        </button>
                    </div>

                    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-gray-50 border-b border-gray-100 text-[11px] font-bold text-gray-400 uppercase tracking-wider">
                                    <th class="p-4">Name</th>
                                    <th class="p-4">Email</th>
                                    <th class="p-4">Role</th>
                                    <th class="p-4">ID / Roll No</th>
                                    <th class="p-4 text-right">Actions</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-100 text-sm">
                                <tr class="hover:bg-gray-50/50">
                                    <td class="p-4 font-bold text-gray-900">Dr. Robert Administrator</td>
                                    <td class="p-4 text-gray-600">admin@university.edu</td>
                                    <td class="p-4"><span class="px-2.5 py-1 rounded-full text-xs font-bold bg-purple-100 text-purple-700">Admin</span></td>
                                    <td class="p-4 font-mono text-xs">ADM001</td>
                                    <td class="p-4 text-right"><button class="text-indigo-600 hover:underline text-xs font-semibold">Edit</button></td>
                                </tr>
                                <tr class="hover:bg-gray-50/50">
                                    <td class="p-4 font-bold text-gray-900">Prof. Thomas Edison</td>
                                    <td class="p-4 text-gray-600">staff@university.edu</td>
                                    <td class="p-4"><span class="px-2.5 py-1 rounded-full text-xs font-bold bg-blue-100 text-blue-700">Lab Staff</span></td>
                                    <td class="p-4 font-mono text-xs">STF001</td>
                                    <td class="p-4 text-right"><button class="text-indigo-600 hover:underline text-xs font-semibold">Edit</button></td>
                                </tr>
                                <tr class="hover:bg-gray-50/50">
                                    <td class="p-4 font-bold text-gray-900">Rahul Kumar</td>
                                    <td class="p-4 text-gray-600">rahul@university.edu</td>
                                    <td class="p-4"><span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-700">Student</span></td>
                                    <td class="p-4 font-mono text-xs">ECE123</td>
                                    <td class="p-4 text-right"><button class="text-indigo-600 hover:underline text-xs font-semibold">Edit</button></td>
                                </tr>
                                <tr class="hover:bg-gray-50/50">
                                    <td class="p-4 font-bold text-gray-900">Prof. NAAC Evaluator</td>
                                    <td class="p-4 text-gray-600">evaluator@naac.gov.in</td>
                                    <td class="p-4"><span class="px-2.5 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-700">Evaluator (V11)</span></td>
                                    <td class="p-4 font-mono text-xs">EVA001</td>
                                    <td class="p-4 text-right"><button class="text-indigo-600 hover:underline text-xs font-semibold">Edit</button></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            `;
        }

        // --- Reports View ---
        function renderReportsView(container) {
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h1 class="text-2xl font-bold text-gray-900">Reports & Analytics Center</h1>
                            <p class="text-sm text-gray-500">Comprehensive department, inventory, and movement reports.</p>
                        </div>
                        <button onclick="window.print()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                            <i class="fa-solid fa-print"></i> Print Reports
                        </button>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="bg-white p-6 rounded-2xl border border-gray-100 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center font-bold mb-3"><i class="fa-solid fa-file-invoice"></i></div>
                                <h3 class="font-bold text-gray-900 text-base mb-1">Equipment Inventory Report</h3>
                                <p class="text-xs text-gray-500 mb-4">Complete asset register across all 4 departments and 6 laboratories.</p>
                            </div>
                            <button onclick="switchView('accreditation')" class="w-full py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 font-semibold text-xs rounded-xl transition">Generate Report</button>
                        </div>
                        <div class="bg-white p-6 rounded-2xl border border-gray-100 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="w-10 h-10 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center font-bold mb-3"><i class="fa-solid fa-right-left"></i></div>
                                <h3 class="font-bold text-gray-900 text-base mb-1">Borrow & Return Movement</h3>
                                <p class="text-xs text-gray-500 mb-4">Detailed student borrowing logs and consumable stock burn-down.</p>
                            </div>
                            <button onclick="switchView('borrow')" class="w-full py-2 bg-blue-50 hover:bg-blue-100 text-blue-700 font-semibold text-xs rounded-xl transition">View Transactions</button>
                        </div>
                        <div class="bg-white p-6 rounded-2xl border border-gray-100 shadow-sm flex flex-col justify-between">
                            <div>
                                <div class="w-10 h-10 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center font-bold mb-3"><i class="fa-solid fa-screwdriver-wrench"></i></div>
                                <h3 class="font-bold text-gray-900 text-base mb-1">Maintenance & Service Log</h3>
                                <p class="text-xs text-gray-500 mb-4">Vendor repair history, calibration records, and maintenance costs.</p>
                            </div>
                            <button onclick="switchView('maintenance')" class="w-full py-2 bg-emerald-50 hover:bg-emerald-100 text-emerald-700 font-semibold text-xs rounded-xl transition">View Maintenance</button>
                        </div>
                    </div>
                </div>
            `;
        }

        // ---  Readiness Dashboard ---
        function renderAccreditationView(container) {
            let totalEq = DEMO_DATA.equipment.reduce((a,b)=>a+b.totalQty,0);
            let activeMaint = DEMO_DATA.equipment.reduce((a,b)=>a+b.maintQty,0);

            container.innerHTML = `
                <div class="space-y-6" id="printable-area">
                    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                        <div>
                            <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-lg uppercase tracking-wider">V11 Compliance</span>
                            <h1 class="text-2xl font-bold text-gray-900 mt-2">V11 Infrastructure & Equipment Accreditation Dashboard</h1>
                            <p class="text-sm text-gray-500">Official documentation register for institutional laboratories and facilities review.</p>
                        </div>
                        <div class="flex items-center gap-3 no-print">
                            <button onclick="window.print()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center gap-2">
                                <i class="fa-solid fa-file-pdf"></i> Export PDF / Print
                            </button>
                        </div>
                    </div>

                    <!-- Readiness Summary Cards -->
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Documentation Score</div>
                            <div class="text-2xl font-bold text-emerald-600">96.4%</div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Total Equipment Records</div>
                            <div class="text-2xl font-bold text-gray-900">${totalEq} Units</div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Maintenance Completeness</div>
                            <div class="text-2xl font-bold text-indigo-600">100%</div>
                        </div>
                        <div class="bg-white p-5 rounded-2xl border border-gray-100 shadow-sm">
                            <div class="text-xs font-semibold text-gray-400 uppercase mb-1">Under Service</div>
                            <div class="text-2xl font-bold text-amber-600">${activeMaint} Items</div>
                        </div>
                    </div>

                    <!-- Detailed Accreditation Table by Department & Lab -->
                    <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden">
                        <div class="p-6 border-b border-gray-100 bg-gray-50 flex items-center justify-between">
                            <div>
                                <h3 class="font-bold text-gray-900">Institutional Infrastructure & Asset Register</h3>
                                <p class="text-xs text-gray-500">Hierarchy: Institution → Department → Laboratory → Equipment</p>
                            </div>
                            <span class="text-xs font-bold bg-emerald-100 text-emerald-700 px-3 py-1 rounded-full">Accreditation Ready</span>
                        </div>
                        <div class="p-6 space-y-8">
                            ${DEMO_DATA.departments.map(dept => `
                                <div class="border-b border-gray-100 pb-6 last:border-0 last:pb-0">
                                    <div class="flex items-center gap-3 mb-4">
                                        <div class="w-8 h-8 rounded-lg bg-indigo-600 text-white font-bold text-xs flex items-center justify-center">${dept.code}</div>
                                        <div>
                                            <h4 class="font-bold text-base text-gray-900">${dept.name}</h4>
                                            <p class="text-xs text-gray-500">HOD: ${dept.hod}</p>
                                        </div>
                                    </div>

                                    <div class="space-y-4 pl-4 border-l-2 border-indigo-100 ml-4">
                                        ${dept.laboratories.map(lab => {
                                            const labEq = DEMO_DATA.equipment.filter(e => e.labId === lab.id);
                                            return `
                                                <div>
                                                    <div class="font-semibold text-sm text-gray-800 mb-2 flex items-center gap-2">
                                                        <i class="fa-solid fa-flask text-indigo-600 text-xs"></i>
                                                        <span>${lab.name}</span>
                                                        <span class="text-xs text-gray-400 font-normal">(Room ${lab.room}, Capacity: ${lab.capacity}, Staff: ${lab.staff})</span>
                                                    </div>

                                                    <div class="overflow-x-auto">
                                                        <table class="w-full text-left border-collapse text-xs">
                                                            <thead>
                                                                <tr class="bg-gray-50 text-gray-400 uppercase font-semibold">
                                                                    <th class="p-3">Asset ID & Name</th>
                                                                    <th class="p-3">Manufacturer / Model</th>
                                                                    <th class="p-3">Purchase Date / Warranty</th>
                                                                    <th class="p-3 text-center">Qty (Avail/Total)</th>
                                                                    <th class="p-3">Condition & Status</th>
                                                                </tr>
                                                            </thead>
                                                            <tbody class="divide-y divide-gray-100">
                                                                ${labEq.map(eq => `
                                                                    <tr>
                                                                        <td class="p-3">
                                                                            <span class="font-bold text-gray-900 block">${eq.name}</span>
                                                                            <span class="font-mono text-[10px] text-indigo-600">${eq.id}</span>
                                                                        </td>
                                                                        <td class="p-3">
                                                                            <span class="font-medium text-gray-800 block">${eq.mfg}</span>
                                                                            <span class="text-[10px] text-gray-500">${eq.model}</span>
                                                                        </td>
                                                                        <td class="p-3 text-gray-600">
                                                                            ${eq.purchaseDate}
                                                                            <span class="block text-[10px] text-emerald-600 font-semibold">Warranty Active</span>
                                                                        </td>
                                                                        <td class="p-3 text-center font-bold">
                                                                            <span class="text-emerald-600">${eq.availQty}</span> / ${eq.totalQty}
                                                                        </td>
                                                                        <td class="p-3">
                                                                            <span class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-emerald-100 text-emerald-700">${eq.condition}</span>
                                                                        </td>
                                                                    </tr>
                                                                `).join('')}
                                                            </tbody>
                                                        </table>
                                                    </div>
                                                </div>
                                            `;
                                        }).join('')}
                                    </div>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </div>
            `;
        }

        // --- 5. MODALS & INTERACTIVE ACTIONS ---

        function openEquipmentDetail(eqId) {
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);
            if (!item) return;

            const dept = DEMO_DATA.departments.find(d => d.id === item.deptId);
            let labName = 'Lab';
            if (dept) {
                const lab = dept.laboratories.find(l => l.id === item.labId);
                if (lab) labName = lab.name;
            }

            const modal = document.getElementById('modal-container');
            const content = document.getElementById('modal-content');
            modal.classList.remove('hidden');

            content.innerHTML = `
                <div class="p-6 space-y-6">
                    <div class="flex items-center justify-between border-b border-gray-100 pb-4">
                        <div>
                            <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-lg">${item.id}</span>
                            <h2 class="text-xl font-bold text-gray-900 mt-2">${item.name}</h2>
                            <p class="text-xs text-gray-500">${dept ? dept.name : ''} • ${labName}</p>
                        </div>
                        <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-gray-100 text-gray-500 hover:bg-gray-200 flex items-center justify-center transition">
                            <i class="fa-solid fa-xmark"></i>
                        </button>
                    </div>

                    <div class="grid grid-cols-2 gap-4 text-xs">
                        <div class="bg-gray-50 p-3 rounded-xl">
                            <span class="text-gray-400 block font-semibold">Manufacturer</span>
                            <strong class="text-gray-800 text-sm">${item.mfg} (${item.model})</strong>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-xl">
                            <span class="text-gray-400 block font-semibold">Serial Number</span>
                            <strong class="text-gray-800 text-sm font-mono">${item.serial}</strong>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-xl">
                            <span class="text-gray-400 block font-semibold">Available / Total Qty</span>
                            <strong class="text-emerald-600 text-sm">${item.availQty} / ${item.totalQty}</strong>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-xl">
                            <span class="text-gray-400 block font-semibold">Current Condition</span>
                            <strong class="text-gray-800 text-sm">${item.condition}</strong>
                        </div>
                    </div>

                    <div>
                        <h4 class="text-xs font-bold text-gray-400 uppercase mb-2">Description & Location</h4>
                        <p class="text-sm text-gray-600 bg-gray-50 p-3 rounded-xl">${item.desc}</p>
                        <p class="text-xs text-gray-500 mt-2">Location: <strong>${item.location}</strong></p>
                    </div>

                    <!-- Action Buttons -->
                    <div class="border-t border-gray-100 pt-4 flex gap-3">
                        <button onclick="openBorrowModal('${item.id}')" class="flex-1 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold rounded-xl text-sm transition flex items-center justify-center gap-2 ${item.availQty === 0 ? 'opacity-55 cursor-not-allowed' : ''}" ${item.availQty === 0 ? 'disabled' : ''}>
                            <i class="fa-solid fa-hand-holding"></i> Borrow Item (${item.availQty} Available)
                        </button>
                        <button onclick="openServiceModal('${item.id}')" class="px-4 py-3 bg-gray-100 hover:bg-gray-200 text-gray-700 font-semibold rounded-xl text-sm transition" title="Send for Service">
                            <i class="fa-solid fa-screwdriver-wrench"></i>
                        </button>
                        <button onclick="openQrModal('${item.id}')" class="px-4 py-3 bg-indigo-50 hover:bg-indigo-100 text-indigo-600 font-semibold rounded-xl text-sm transition" title="QR Code">
                            <i class="fa-solid fa-qrcode"></i>
                        </button>
                    </div>
                </div>
            `;
        }

        function openBorrowModal(eqId) {
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);
            const user = DEMO_DATA.currentUser;
            const content = document.getElementById('modal-content');

            content.innerHTML = `
                <div class="p-6 space-y-6">
                    <div class="flex items-center justify-between border-b border-gray-100 pb-4">
                        <div>
                            <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-lg">Borrow Request</span>
                            <h2 class="text-xl font-bold text-gray-900 mt-2">${item.name}</h2>
                            <p class="text-xs text-gray-500">Asset ID: ${item.id} • Available Stock: ${item.availQty}</p>
                        </div>
                        <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-gray-100 text-gray-500 hover:bg-gray-200 flex items-center justify-center transition">
                            <i class="fa-solid fa-xmark"></i>
                        </button>
                    </div>

                    <form onsubmit="submitBorrow(event, '${item.id}')" class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium text-gray-700 mb-1">Full Name</label>
                            <input type="text" id="borrow-name" required value="${user.name}" class="w-full px-4 py-2 border rounded-xl text-sm bg-gray-50">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium text-gray-700 mb-1">Email ID</label>
                                <input type="email" id="borrow-email" required value="${user.email}" class="w-full px-4 py-2 border rounded-xl text-sm bg-gray-50">
                            </div>
                            <div>
                                <label class="block text-xs font-medium text-gray-700 mb-1">Student / Staff ID</label>
                                <input type="text" id="borrow-id" required value="${user.id}" class="w-full px-4 py-2 border rounded-xl text-sm bg-gray-50">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-gray-700 mb-1">Purpose of Borrowing</label>
                            <input type="text" id="borrow-purpose" required placeholder="e.g., Lab Experiment 3, Final Project" class="w-full px-4 py-2 border rounded-xl text-sm">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-gray-700 mb-1">Quantity (Available: ${item.availQty})</label>
                            <input type="number" id="borrow-qty" min="1" max="${item.availQty}" value="1" required class="w-full px-4 py-2 border rounded-xl text-sm">
                        </div>
                        <button type="submit" class="w-full py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold rounded-xl shadow-lg transition text-sm">
                            Confirm Borrow & Update Inventory
                        </button>
                    </form>
                </div>
            `;
        }

        function submitBorrow(e, eqId) {
            e.preventDefault();
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);
            const qty = parseInt(document.getElementById('borrow-qty').value);

            if (qty > item.availQty) {
                alert('Requested quantity exceeds available stock.');
                return;
            }

            // Update item counts
            item.availQty -= qty;
            item.issuedQty += qty;

            // Add transaction
            const newTxn = {
                id: 'TXN-' + Math.floor(100 + Math.random() * 900),
                equipmentId: item.id,
                name: document.getElementById('borrow-name').value,
                email: document.getElementById('borrow-email').value,
                studentId: document.getElementById('borrow-id').value,
                purpose: document.getElementById('borrow-purpose').value,
                qty: qty,
                borrowedDate: new Date().toISOString().replace('T', ' ').substring(0, 16),
                status: 'Issued'
            };
            DEMO_DATA.transactions.unshift(newTxn);

            // Add audit log
            DEMO_DATA.auditLogs.unshift({
                date: new Date().toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' }) + ' ' + new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                action: `${newTxn.name} borrowed ${qty} ${item.name} (${item.id}).`
            });

            closeModal();
            alert(`Success! Borrow recorded. Remaining stock: ${item.availQty}`);
            switchView(currentView);
        }

        function openServiceModal(eqId) {
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);
            const content = document.getElementById('modal-content');

            content.innerHTML = `
                <div class="p-6 space-y-6">
                    <div class="flex items-center justify-between border-b border-gray-100 pb-4">
                        <div>
                            <span class="text-xs font-bold text-amber-600 bg-amber-50 px-2.5 py-1 rounded-lg">Send for Maintenance</span>
                            <h2 class="text-xl font-bold text-gray-900 mt-2">${item.name}</h2>
                            <p class="text-xs text-gray-500">Asset ID: ${item.id}</p>
                        </div>
                        <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-gray-100 text-gray-500 hover:bg-gray-200 flex items-center justify-center transition">
                            <i class="fa-solid fa-xmark"></i>
                        </button>
                    </div>

                    <form onsubmit="submitService(event, '${item.id}')" class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium text-gray-700 mb-1">Service Reason / Problem Description</label>
                            <input type="text" id="service-desc" required placeholder="e.g., Calibration error, screen replacement" class="w-full px-4 py-2 border rounded-xl text-sm">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium text-gray-700 mb-1">Service Vendor / Center</label>
                                <input type="text" id="service-vendor" required placeholder="e.g., Authorized Service Center" class="w-full px-4 py-2 border rounded-xl text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-medium text-gray-700 mb-1">Estimated Cost (₹)</label>
                                <input type="number" id="service-cost" required value="500" class="w-full px-4 py-2 border rounded-xl text-sm">
                            </div>
                        </div>
                        <button type="submit" class="w-full py-3 bg-amber-600 hover:bg-amber-700 text-white font-semibold rounded-xl shadow-lg transition text-sm">
                            Send for Service & Update Status
                        </button>
                    </form>
                </div>
            `;
        }

        function submitService(e, eqId) {
            e.preventDefault();
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);

            if (item.availQty <= 0) {
                alert('No available units to send for maintenance.');
                return;
            }

            item.availQty -= 1;
            item.maintQty += 1;

            DEMO_DATA.maintenance.unshift({
                id: 'MAINT-' + Math.floor(10 + Math.random() * 90),
                equipmentId: item.id,
                date: new Date().toISOString().substring(0, 10),
                type: document.getElementById('service-desc').value,
                cost: parseInt(document.getElementById('service-cost').value),
                vendor: document.getElementById('service-vendor').value,
                status: 'Under Maintenance',
                desc: document.getElementById('service-desc').value
            });

            DEMO_DATA.auditLogs.unshift({
                date: new Date().toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' }) + ' ' + new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                action: `Equipment ${item.name} (${item.id}) sent for service.`
            });

            closeModal();
            alert('Equipment sent for service. Status updated to Under Maintenance.');
            switchView(currentView);
        }

        function openQrModal(eqId) {
            const item = DEMO_DATA.equipment.find(e => e.id === eqId);
            const modal = document.getElementById('modal-container');
            const content = document.getElementById('modal-content');
            modal.classList.remove('hidden');

            content.innerHTML = `
                <div class="p-6 text-center space-y-4">
                    <div class="flex justify-between items-center border-b border-gray-100 pb-3">
                        <span class="font-bold text-gray-900 text-sm">Asset QR & Barcode</span>
                        <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-gray-100 text-gray-500 hover:bg-gray-200 flex items-center justify-center transition">
                            <i class="fa-solid fa-xmark"></i>
                        </button>
                    </div>
                    <div>
                        <div class="font-bold text-lg text-gray-900">${item.name}</div>
                        <div class="text-xs font-mono text-indigo-600">${item.id}</div>
                    </div>
                    <div class="p-4 bg-gray-50 rounded-2xl inline-block border border-gray-100">
                        <canvas id="qrcode-canvas" class="mx-auto"></canvas>
                    </div>
                    <div class="text-xs text-gray-500">Scan using mobile camera to borrow, return, or inspect asset.</div>
                    <button onclick="window.print()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold rounded-xl text-sm transition">
                        Print QR Code Tag
                    </button>
                </div>
            `;

            // Generate QR Code on Canvas
            setTimeout(() => {
                const canvas = document.getElementById('qrcode-canvas');
                if (canvas) {
                    QRCode.toCanvas(canvas, item.id, { width: 180, margin: 1 }, function (error) {
                        if (error) console.error(error);
                    });
                }
            }, 100);
        }

        function closeModal() {
            document.getElementById('modal-container').classList.add('hidden');
        }

        function toggleNotifications() {
            const dropdown = document.getElementById('notification-dropdown');
            dropdown.classList.toggle('hidden');
            renderNotifications();
        }

        function renderNotifications() {
            const list = document.getElementById('notification-list');
            document.getElementById('notification-badge').innerText = DEMO_DATA.notifications.length;
            document.getElementById('notification-count-badge').innerText = DEMO_DATA.notifications.length + ' alerts';

            list.innerHTML = DEMO_DATA.notifications.map(n => `
                <div class="p-4 hover:bg-gray-50 transition flex items-start gap-3">
                    <div class="w-8 h-8 rounded-lg bg-amber-50 text-amber-600 flex items-center justify-center flex-shrink-0 text-xs font-bold"><i class="fa-solid fa-triangle-exclamation"></i></div>
                    <div>
                        <div class="text-xs font-bold text-gray-900">${n.title}</div>
                        <div class="text-xs text-gray-600 mt-0.5">${n.msg}</div>
                        <div class="text-[10px] text-gray-400 mt-1">${n.time}</div>
                    </div>
                </div>
            `).join('');
        }

        function handleGlobalSearch(query) {
            const results = document.getElementById('global-search-results');
            if (!query || query.trim().length === 0) {
                results.classList.add('hidden');
                return;
            }

            const q = query.toLowerCase();
            const matched = DEMO_DATA.equipment.filter(e => 
                e.name.toLowerCase().includes(q) || 
                e.id.toLowerCase().includes(q) || 
                e.serial.toLowerCase().includes(q) || 
                e.mfg.toLowerCase().includes(q)
            );

            if (matched.length === 0) {
                results.innerHTML = `<div class="p-4 text-xs text-gray-400 text-center">No matching assets found.</div>`;
                results.classList.remove('hidden');
                return;
            }

            results.innerHTML = matched.map(item => `
                <div onclick="openEquipmentDetail('${item.id}'); document.getElementById('global-search-results').classList.add('hidden');" class="p-3 hover:bg-indigo-50/50 cursor-pointer border-b border-gray-100 last:border-0 flex items-center justify-between">
                    <div>
                        <div class="font-bold text-xs text-gray-900">${item.name}</div>
                        <div class="text-[10px] font-mono text-indigo-600">${item.id}</div>
                    </div>
                    <span class="text-[10px] font-bold px-2 py-0.5 bg-emerald-100 text-emerald-700 rounded-full">Available: ${item.availQty}</span>
                </div>
            `).join('');
            results.classList.remove('hidden');
        }

        function openDepartmentDetail(deptId) {
            switchView('departments');
        }

        function openLabDetail(labId) {
            switchView('laboratories');
        }

        function openAddEquipmentModal() {
            alert('Equipment registration modal opened. (Demo mode: Use quick pre-loaded items or click Scan).');
        }

        function openAddDepartmentModal() {
            alert('Department registration modal opened.');
        }
    </script>
</body>
</html>
