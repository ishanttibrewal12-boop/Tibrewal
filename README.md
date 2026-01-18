<!DOCTYPE html>
<html lang="en">
<head>
    <base target="_self">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tibrewal & Tibrewal Private Limited - Staff Management</title>
    <meta name="description" content="Staff management, attendance tracking, and payroll system for Tibrewal & Tibrewal Private Limited">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: "#1e40af",
                        secondary: "#3b82f6",
                        accent: "#60a5fa",
                        dark: "#1e3a8a",
                        light: "#dbeafe"
                    },
                    fontFamily: {
                        'poppins': ['Poppins', 'sans-serif']
                    },
                    spacing: {
                        '128': '32rem',
                        '144': '36rem'
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
        }
        .sidebar {
            transition: all 0.3s ease;
        }
        .table-container {
            max-height: 500px;
            overflow-y: auto;
        }
        .attendance-badge {
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 500;
        }
        .present { background-color: #d1fae5; color: #065f46; }
        .absent { background-color: #fee2e2; color: #991b1b; }
        .halfday { background-color: #fef3c7; color: #92400e; }
        .holiday { background-color: #e0e7ff; color: #3730a3; }
        .sunday { background-color: #f3e8ff; color: #6b21a8; }
        
        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                left: -100%;
                z-index: 50;
            }
            .sidebar.active {
                left: 0;
            }
            .overlay {
                display: none;
                position: fixed;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                background: rgba(0,0,0,0.5);
                z-index: 40;
            }
            .overlay.active {
                display: block;
            }
        }
    </style>
</head>
<body class="min-h-screen bg-gray-50 font-poppins">
    <!-- Login Screen -->
    <div id="loginScreen" class="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-50 to-white p-4">
        <div class="bg-white rounded-2xl shadow-xl p-8 w-full max-w-md">
            <div class="text-center mb-8">
                <div class="flex justify-center mb-4">
                    <div class="w-16 h-16 bg-blue-600 rounded-full flex items-center justify-center">
                        <i class="fas fa-building text-white text-2xl"></i>
                    </div>
                </div>
                <h1 class="text-2xl font-bold text-gray-800">Tibrewal & Tibrewal</h1>
                <p class="text-gray-600 mt-2">Private Limited</p>
                <p class="text-sm text-gray-500 mt-1">Mining & Construction Company</p>
            </div>
            
            <form id="loginForm" class="space-y-6">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">Username</label>
                    <input type="text" id="username" 
                           class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition"
                           placeholder="Enter your username" required>
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">Password</label>
                    <input type="password" id="password" 
                           class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition"
                           placeholder="Enter your password" required>
                </div>
                
                <button type="submit" 
                        class="w-full bg-blue-600 text-white py-3 px-4 rounded-lg font-medium hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition">
                    Login as Admin
                </button>
                
                <div class="text-center text-sm text-gray-500">
                    <p>Only authorized administrators can access this system</p>
                    <p class="mt-2 text-xs">Contact: 9386469006 | ishanttibrewal12@gmail.com</p>
                </div>
            </form>
        </div>
    </div>

    <!-- Main Dashboard (Hidden by default) -->
    <div id="dashboard" class="hidden">
        <!-- Mobile Menu Button -->
        <button id="menuToggle" class="lg:hidden fixed top-4 left-4 z-50 bg-blue-600 text-white p-3 rounded-lg shadow-lg">
            <i class="fas fa-bars"></i>
        </button>

        <!-- Overlay for mobile -->
        <div id="overlay" class="overlay"></div>

        <!-- Sidebar -->
        <div class="sidebar fixed lg:static inset-y-0 left-0 w-64 bg-white shadow-lg z-40">
            <div class="p-6 border-b">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center">
                        <i class="fas fa-building text-white"></i>
                    </div>
                    <div>
                        <h2 class="font-bold text-gray-800">Tibrewal & Tibrewal</h2>
                        <p class="text-xs text-gray-500">Admin Panel</p>
                    </div>
                </div>
            </div>
            
            <nav class="p-4 space-y-2">
                <a href="#" data-section="dashboard" class="nav-link active flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-blue-600">
                    <i class="fas fa-tachometer-alt w-5"></i>
                    <span>Dashboard</span>
                </a>
                <a href="#" data-section="staff" class="nav-link flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-gray-700">
                    <i class="fas fa-users w-5"></i>
                    <span>Staff Management</span>
                </a>
                <a href="#" data-section="attendance" class="nav-link flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-gray-700">
                    <i class="fas fa-calendar-check w-5"></i>
                    <span>Attendance</span>
                </a>
                <a href="#" data-section="payroll" class="nav-link flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-gray-700">
                    <i class="fas fa-rupee-sign w-5"></i>
                    <span>Payroll</span>
                </a>
                <a href="#" data-section="reports" class="nav-link flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-gray-700">
                    <i class="fas fa-chart-bar w-5"></i>
                    <span>Reports</span>
                </a>
                <a href="#" data-section="settings" class="nav-link flex items-center space-x-3 p-3 rounded-lg hover:bg-blue-50 text-gray-700">
                    <i class="fas fa-cog w-5"></i>
                    <span>Settings</span>
                </a>
            </nav>
            
            <div class="absolute bottom-0 left-0 right-0 p-4 border-t">
                <div class="flex items-center space-x-3 p-3">
                    <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                        <i class="fas fa-user text-blue-600"></i>
                    </div>
                    <div class="flex-1">
                        <p class="text-sm font-medium" id="currentAdmin"></p>
                        <p class="text-xs text-gray-500">Administrator</p>
                    </div>
                </div>
                <button id="logoutBtn" 
                        class="w-full mt-2 flex items-center justify-center space-x-2 p-2 text-red-600 hover:bg-red-50 rounded-lg">
                    <i class="fas fa-sign-out-alt"></i>
                    <span>Logout</span>
                </button>
            </div>
        </div>

        <!-- Main Content -->
        <div class="lg:ml-64 min-h-screen">
            <!-- Top Bar -->
            <header class="bg-white shadow-sm border-b">
                <div class="px-6 py-4">
                    <div class="flex justify-between items-center">
                        <div>
                            <h1 class="text-xl font-bold text-gray-800" id="pageTitle">Dashboard</h1>
                            <p class="text-sm text-gray-600">Manage staff, attendance, and payroll</p>
                        </div>
                        <div class="flex items-center space-x-4">
                            <div class="text-right">
                                <p class="text-sm font-medium" id="currentDate"></p>
                                <p class="text-xs text-gray-500" id="currentTime"></p>
                            </div>
                            <div class="w-10 h-10 bg-blue-100 rounded-full flex items-center justify-center">
                                <i class="fas fa-bell text-blue-600"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </header>

            <!-- Content Sections -->
            <main class="p-6">
                <!-- Dashboard Section -->
                <section id="dashboardSection" class="content-section">
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                        <div class="bg-white p-6 rounded-xl shadow-sm border">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm text-gray-500">Total Staff</p>
                                    <p class="text-3xl font-bold text-gray-800" id="totalStaff">0</p>
                                </div>
                                <div class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-users text-blue-600 text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <span class="text-sm text-green-600">
                                    <i class="fas fa-arrow-up"></i> Manage all workers
                                </span>
                            </div>
                        </div>
                        
                        <div class="bg-white p-6 rounded-xl shadow-sm border">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm text-gray-500">Present Today</p>
                                    <p class="text-3xl font-bold text-gray-800" id="presentToday">0</p>
                                </div>
                                <div class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-check-circle text-green-600 text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <span class="text-sm text-gray-600">Attendance tracking</span>
                            </div>
                        </div>
                        
                        <div class="bg-white p-6 rounded-xl shadow-sm border">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm text-gray-500">Monthly Salary</p>
                                    <p class="text-3xl font-bold text-gray-800">₹<span id="monthlySalary">0</span></p>
                                </div>
                                <div class="w-12 h-12 bg-purple-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-rupee-sign text-purple-600 text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <span class="text-sm text-gray-600">Total payroll amount</span>
                            </div>
                        </div>
                        
                        <div class="bg-white p-6 rounded-xl shadow-sm border">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm text-gray-500">Pending Tasks</p>
                                    <p class="text-3xl font-bold text-gray-800" id="pendingTasks">0</p>
                                </div>
                                <div class="w-12 h-12 bg-yellow-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-tasks text-yellow-600 text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <span class="text-sm text-gray-600">Require attention</span>
                            </div>
                        </div>
                    </div>

                    <!-- Quick Actions -->
                    <div class="bg-white rounded-xl shadow-sm border p-6 mb-8">
                        <h2 class="text-lg font-bold text-gray-800 mb-4">Quick Actions</h2>
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                            <button onclick="showSection('staff')" class="flex items-center space-x-3 p-4 border rounded-lg hover:bg-blue-50 hover:border-blue-300 transition">
                                <div class="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-user-plus text-blue-600"></i>
                                </div>
                                <div class="text-left">
                                    <p class="font-medium">Add New Staff</p>
                                    <p class="text-sm text-gray-500">Register worker</p>
                                </div>
                            </button>
                            
                            <button onclick="showSection('attendance')" class="flex items-center space-x-3 p-4 border rounded-lg hover:bg-green-50 hover:border-green-300 transition">
                                <div class="w-10 h-10 bg-green-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-calendar-alt text-green-600"></i>
                                </div>
                                <div class="text-left">
                                    <p class="font-medium">Mark Attendance</p>
                                    <p class="text-sm text-gray-500">Today's record</p>
                                </div>
                            </button>
                            
                            <button onclick="showSection('payroll')" class="flex items-center space-x-3 p-4 border rounded-lg hover:bg-purple-50 hover:border-purple-300 transition">
                                <div class="w-10 h-10 bg-purple-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-calculator text-purple-600"></i>
                                </div>
                                <div class="text-left">
                                    <p class="font-medium">Calculate Salary</p>
                                    <p class="text-sm text-gray-500">Generate payroll</p>
                                </div>
                            </button>
                            
                            <button onclick="showSection('reports')" class="flex items-center space-x-3 p-4 border rounded-lg hover:bg-orange-50 hover:border-orange-300 transition">
                                <div class="w-10 h-10 bg-orange-100 rounded-lg flex items-center justify-center">
                                    <i class="fas fa-file-export text-orange-600"></i>
                                </div>
                                <div class="text-left">
                                    <p class="font-medium">Generate Report</p>
                                    <p class="text-sm text-gray-500">Monthly summary</p>
                                </div>
                            </button>
                        </div>
                    </div>

                    <!-- Recent Activity -->
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                        <div class="bg-white rounded-xl shadow-sm border p-6">
                            <h2 class="text-lg font-bold text-gray-800 mb-4">Recent Staff Additions</h2>
                            <div class="space-y-4" id="recentStaff">
                                <!-- Will be populated by JavaScript -->
                            </div>
                        </div>
                        
                        <div class="bg-white rounded-xl shadow-sm border p-6">
                            <h2 class="text-lg font-bold text-gray-800 mb-4">Attendance Overview</h2>
                            <div class="space-y-4">
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-600">Present</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-32 h-2 bg-gray-200 rounded-full overflow-hidden">
                                            <div class="h-full bg-green-500" style="width: 75%"></div>
                                        </div>
                                        <span class="font-medium">75%</span>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-600">Absent</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-32 h-2 bg-gray-200 rounded-full overflow-hidden">
                                            <div class="h-full bg-red-500" style="width: 15%"></div>
                                        </div>
                                        <span class="font-medium">15%</span>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-600">Half Day</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-32 h-2 bg-gray-200 rounded-full overflow-hidden">
                                            <div class="h-full bg-yellow-500" style="width: 8%"></div>
                                        </div>
                                        <span class="font-medium">8%</span>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <span class="text-gray-600">Holiday</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-32 h-2 bg-gray-200 rounded-full overflow-hidden">
                                            <div class="h-full bg-blue-500" style="width: 2%"></div>
                                        </div>
                                        <span class="font-medium">2%</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Staff Management Section -->
                <section id="staffSection" class="content-section hidden">
                    <div class="bg-white rounded-xl shadow-sm border p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-xl font-bold text-gray-800">Staff Management</h2>
                            <button onclick="openAddStaffModal()" 
                                    class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 flex items-center space-x-2">
                                <i class="fas fa-plus"></i>
                                <span>Add New Staff</span>
                            </button>
                        </div>
                        
                        <!-- Search and Filter -->
                        <div class="mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div class="relative">
                                <input type="text" id="staffSearch" 
                                       class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                       placeholder="Search by name or ID">
                                <i class="fas fa-search absolute right-3 top-3 text-gray-400"></i>
                            </div>
                            <select id="departmentFilter" 
                                    class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="">All Departments</option>
                                <option value="mining">Mining</option>
                                <option value="construction">Construction</option>
                                <option value="administration">Administration</option>
                                <option value="transport">Transport</option>
                                <option value="security">Security</option>
                            </select>
                            <select id="statusFilter" 
                                    class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="">All Status</option>
                                <option value="active">Active</option>
                                <option value="inactive">Inactive</option>
                                <option value="on-leave">On Leave</option>
                            </select>
                        </div>
                        
                        <!-- Staff Table -->
                        <div class="table-container border rounded-lg">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Photo</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Name</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Department</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Position</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Salary (₹)</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200" id="staffTableBody">
                                    <!-- Will be populated by JavaScript -->
                                </tbody>
                            </table>
                        </div>
                        
                        <!-- Pagination -->
                        <div class="mt-6 flex justify-between items-center">
                            <div class="text-sm text-gray-500">
                                Showing <span id="staffStart">1</span> to <span id="staffEnd">10</span> of <span id="totalStaffCount">0</span> staff members
                            </div>
                            <div class="flex space-x-2">
                                <button onclick="prevStaffPage()" 
                                        class="px-3 py-1 border rounded-lg hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed">
                                    Previous
                                </button>
                                <button onclick="nextStaffPage()" 
                                        class="px-3 py-1 border rounded-lg hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed">
                                    Next
                                </button>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Attendance Section -->
                <section id="attendanceSection" class="content-section hidden">
                    <div class="bg-white rounded-xl shadow-sm border p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-xl font-bold text-gray-800">Attendance Management</h2>
                            <div class="flex items-center space-x-4">
                                <input type="date" id="attendanceDate" 
                                       class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <button onclick="markAllPresent()" 
                                        class="bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700">
                                    Mark All Present
                                </button>
                            </div>
                        </div>
                        
                        <!-- Attendance Legend -->
                        <div class="mb-6 flex flex-wrap gap-3">
                            <div class="flex items-center space-x-2">
                                <div class="w-4 h-4 rounded-full bg-green-500"></div>
                                <span class="text-sm">Present</span>
                            </div>
                            <div class="flex items-center space-x-2">
                                <div class="w-4 h-4 rounded-full bg-red-500"></div>
                                <span class="text-sm">Absent</span>
                            </div>
                            <div class="flex items-center space-x-2">
                                <div class="w-4 h-4 rounded-full bg-yellow-500"></div>
                                <span class="text-sm">Half Day</span>
                            </div>
                            <div class="flex items-center space-x-2">
                                <div class="w-4 h-4 rounded-full bg-blue-500"></div>
                                <span class="text-sm">Holiday</span>
                            </div>
                            <div class="flex items-center space-x-2">
                                <div class="w-4 h-4 rounded-full bg-purple-500"></div>
                                <span class="text-sm">Sunday</span>
                            </div>
                        </div>
                        
                        <!-- Attendance Table -->
                        <div class="table-container border rounded-lg">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Staff ID</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Name</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Department</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Check-in Time</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Check-out Time</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200" id="attendanceTableBody">
                                    <!-- Will be populated by JavaScript -->
                                </tbody>
                            </table>
                        </div>
                        
                        <!-- Quick Actions -->
                        <div class="mt-6 grid grid-cols-2 md:grid-cols-4 gap-4">
                            <button onclick="bulkMarkAttendance('present')" 
                                    class="p-4 bg-green-50 border border-green-200 rounded-lg hover:bg-green-100 transition">
                                <div class="flex items-center justify-center space-x-2">
                                    <i class="fas fa-check-circle text-green-600"></i>
                                    <span class="font-medium">Mark Present</span>
                                </div>
                            </button>
                            <button onclick="bulkMarkAttendance('absent')" 
                                    class="p-4 bg-red-50 border border-red-200 rounded-lg hover:bg-red-100 transition">
                                <div class="flex items-center justify-center space-x-2">
                                    <i class="fas fa-times-circle text-red-600"></i>
                                    <span class="font-medium">Mark Absent</span>
                                </div>
                            </button>
                            <button onclick="bulkMarkAttendance('halfday')" 
                                    class="p-4 bg-yellow-50 border border-yellow-200 rounded-lg hover:bg-yellow-100 transition">
                                <div class="flex items-center justify-center space-x-2">
                                    <i class="fas fa-clock text-yellow-600"></i>
                                    <span class="font-medium">Half Day</span>
                                </div>
                            </button>
                            <button onclick="bulkMarkAttendance('holiday')" 
                                    class="p-4 bg-blue-50 border border-blue-200 rounded-lg hover:bg-blue-100 transition">
                                <div class="flex items-center justify-center space-x-2">
                                    <i class="fas fa-umbrella-beach text-blue-600"></i>
                                    <span class="font-medium">Mark Holiday</span>
                                </div>
                            </button>
                        </div>
                    </div>
                </section>

                <!-- Payroll Section -->
                <section id="payrollSection" class="content-section hidden">
                    <div class="bg-white rounded-xl shadow-sm border p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-xl font-bold text-gray-800">Payroll Management</h2>
                            <div class="flex items-center space-x-4">
                                <select id="payrollMonth" 
                                        class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                    <option value="1">January</option>
                                    <option value="2">February</option>
                                    <option value="3">March</option>
                                    <option value="4">April</option>
                                    <option value="5">May</option>
                                    <option value="6">June</option>
                                    <option value="7">July</option>
                                    <option value="8">August</option>
                                    <option value="9">September</option>
                                    <option value="10">October</option>
                                    <option value="11">November</option>
                                    <option value="12">December</option>
                                </select>
                                <select id="payrollYear" 
                                        class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                    <!-- Will be populated by JavaScript -->
                                </select>
                                <button onclick="calculatePayroll()" 
                                        class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
                                    Calculate Payroll
                                </button>
                            </div>
                        </div>
                        
                        <!-- Payroll Summary -->
                        <div class="mb-6 grid grid-cols-1 md:grid-cols-4 gap-4">
                            <div class="bg-blue-50 p-4 rounded-lg border border-blue-100">
                                <p class="text-sm text-blue-600">Total Salary</p>
                                <p class="text-2xl font-bold text-gray-800">₹<span id="totalSalaryAmount">0</span></p>
                            </div>
                            <div class="bg-green-50 p-4 rounded-lg border border-green-100">
                                <p class="text-sm text-green-600">Total Present Days</p>
                                <p class="text-2xl font-bold text-gray-800" id="totalPresentDays">0</p>
                            </div>
                            <div class="bg-yellow-50 p-4 rounded-lg border border-yellow-100">
                                <p class="text-sm text-yellow-600">Total Absent Days</p>
                                <p class="text-2xl font-bold text-gray-800" id="totalAbsentDays">0</p>
                            </div>
                            <div class="bg-purple-50 p-4 rounded-lg border border-purple-100">
                                <p class="text-sm text-purple-600">Staff Count</p>
                                <p class="text-2xl font-bold text-gray-800" id="payrollStaffCount">0</p>
                            </div>
                        </div>
                        
                        <!-- Payroll Table -->
                        <div class="table-container border rounded-lg">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Staff ID</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Name</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Basic Salary</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Present Days</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Absent Days</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Deductions</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Bonus</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Net Salary</th>
                                        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-gray-200" id="payrollTableBody">
                                    <!-- Will be populated by JavaScript -->
                                </tbody>
                            </table>
                        </div>
                        
                        <!-- Payroll Actions -->
                        <div class="mt-6 flex justify-between items-center">
                            <div class="text-sm text-gray-500">
                                Total Payable Amount: <span class="font-bold text-lg">₹<span id="totalPayable">0</span></span>
                            </div>
                            <div class="flex space-x-4">
                                <button onclick="exportPayroll()" 
                                        class="px-4 py-2 border border-green-600 text-green-600 rounded-lg hover:bg-green-50">
                                    <i class="fas fa-file-export mr-2"></i>Export Excel
                                </button>
                                <button onclick="printPayroll()" 
                                        class="px-4 py-2 border border-blue-600 text-blue-600 rounded-lg hover:bg-blue-50">
                                    <i class="fas fa-print mr-2"></i>Print Payroll
                                </button>
                                <button onclick="processPayment()" 
                                        class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700">
                                    <i class="fas fa-rupee-sign mr-2"></i>Process Payment
                                </button>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Reports Section -->
                <section id="reportsSection" class="content-section hidden">
                    <div class="bg-white rounded-xl shadow-sm border p-6">
                        <h2 class="text-xl font-bold text-gray-800 mb-6">Reports & Analytics</h2>
                        
                        <!-- Report Filters -->
                        <div class="mb-6 grid grid-cols-1 md:grid-cols-4 gap-4">
                            <select id="reportType" 
                                    class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="attendance">Attendance Report</option>
                                <option value="payroll">Payroll Report</option>
                                <option value="staff">Staff Report</option>
                                <option value="department">Department Report</option>
                            </select>
                            <input type="month" id="reportMonth" 
                                   class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                            <select id="reportDepartment" 
                                    class="px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="">All Departments</option>
                                <option value="mining">Mining</option>
                                <option value="construction">Construction</option>
                                <option value="administration">Administration</option>
                            </select>
                            <button onclick="generateReport()" 
                                    class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
                                Generate Report
                            </button>
                        </div>
                        
                        <!-- Report Display -->
                        <div class="border rounded-lg p-6">
                            <div id="reportContent">
                                <div class="text-center py-12 text-gray-500">
                                    <i class="fas fa-chart-bar text-4xl mb-4"></i>
                                    <p>Select report type and click "Generate Report" to view analytics</p>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Export Options -->
                        <div class="mt-6 flex justify-end space-x-4">
                            <button onclick="exportPDF()" 
                                    class="px-4 py-2 border border-red-600 text-red-600 rounded-lg hover:bg-red-50">
                                <i class="fas fa-file-pdf mr-2"></i>Export PDF
                            </button>
                            <button onclick="exportExcel()" 
                                    class="px-4 py-2 border border-green-600 text-green-600 rounded-lg hover:bg-green-50">
                                <i class="fas fa-file-excel mr-2"></i>Export Excel
                            </button>
                            <button onclick="printReport()" 
                                    class="px-4 py-2 border border-blue-600 text-blue-600 rounded-lg hover:bg-blue-50">
                                <i class="fas fa-print mr-2"></i>Print Report
                            </button>
                        </div>
                    </div>
                </section>

                <!-- Settings Section -->
                <section id="settingsSection" class="content-section hidden">
                    <div class="bg-white rounded-xl shadow-sm border p-6">
                        <h2 class="text-xl font-bold text-gray-800 mb-6">System Settings</h2>
                        
                        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                            <!-- Company Information -->
                            <div class="lg:col-span-2">
                                <div class="mb-6">
                                    <h3 class="text-lg font-medium text-gray-800 mb-4">Company Information</h3>
                                    <div class="space-y-4">
                                        <div>
                                            <label class="block text-sm font-medium text-gray-700 mb-1">Company Name</label>
                                            <input type="text" 
                                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                                   value="Tibrewal & Tibrewal Private Limited">
                                        </div>
                                        <div>
                                            <label class="block text-sm font-medium text-gray-700 mb-1">Address</label>
                                            <textarea class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none" rows="2">Tibrewal Tyres, Gunia Mahagama, Jharkhand 814154</textarea>
                                        </div>
                                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                            <div>
                                                <label class="block text-sm font-medium text-gray-700 mb-1">Mobile Number</label>
                                                <input type="text" 
                                                       class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                                       value="9386469006">
                                            </div>
                                            <div>
                                                <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                                                <input type="email" 
                                                       class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                                       value="ishanttibrewal12@gmail.com">
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                
                                <!-- Admin Management -->
                                <div>
                                    <h3 class="text-lg font-medium text-gray-800 mb-4">Admin Management</h3>
                                    <div class="space-y-3">
                                        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                                            <div>
                                                <p class="font-medium">Ishant Tibrewal</p>
                                                <p class="text-sm text-gray-500">Username: ishant_admin</p>
                                            </div>
                                            <span class="px-3 py-1 bg-green-100 text-green-800 text-sm rounded-full">Active</span>
                                        </div>
                                        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                                            <div>
                                                <p class="font-medium">Trishav Tibrewal</p>
                                                <p class="text-sm text-gray-500">Username: trishav_admin</p>
                                            </div>
                                            <span class="px-3 py-1 bg-green-100 text-green-800 text-sm rounded-full">Active</span>
                                        </div>
                                        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                                            <div>
                                                <p class="font-medium">Abhay Jalan</p>
                                                <p class="text-sm text-gray-500">Username: abhay_admin</p>
                                            </div>
                                            <span class="px-3 py-1 bg-green-100 text-green-800 text-sm rounded-full">Active</span>
                                        </div>
                                        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                                            <div>
                                                <p class="font-medium">Sunil Tibrewal</p>
                                                <p class="text-sm text-gray-500">Username: sunil_admin</p>
                                            </div>
                                            <span class="px-3 py-1 bg-green-100 text-green-800 text-sm rounded-full">Active</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <!-- System Settings -->
                            <div>
                                <h3 class="text-lg font-medium text-gray-800 mb-4">System Configuration</h3>
                                <div class="space-y-4">
                                    <div>
                                        <label class="flex items-center space-x-3">
                                            <input type="checkbox" class="rounded text-blue-600" checked>
                                            <span class="text-sm text-gray-700">Enable Auto Attendance</span>
                                        </label>
                                    </div>
                                    <div>
                                        <label class="flex items-center space-x-3">
                                            <input type="checkbox" class="rounded text-blue-600" checked>
                                            <span class="text-sm text-gray-700">Send Salary Slips via Email</span>
                                        </label>
                                    </div>
                                    <div>
                                        <label class="flex items-center space-x-3">
                                            <input type="checkbox" class="rounded text-blue-600">
                                            <span class="text-sm text-gray-700">Enable Biometric Integration</span>
                                        </label>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Currency</label>
                                        <select class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                            <option selected>Indian Rupee (₹)</option>
                                            <option>US Dollar ($)</option>
                                            <option>Euro (€)</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Working Hours per Day</label>
                                        <input type="number" 
                                               class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                               value="8">
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Days in Month</label>
                                        <input type="number" 
                                               class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                               value="26">
                                    </div>
                                </div>
                                
                                <div class="mt-8">
                                    <button onclick="saveSettings()" 
                                            class="w-full bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700">
                                        Save Settings
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>
            </main>
        </div>
    </div>

    <!-- Add Staff Modal -->
    <div id="addStaffModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">Add New Staff Member</h3>
                    <button onclick="closeAddStaffModal()" 
                            class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times text-2xl"></i>
                    </button>
                </div>
                
                <form id="addStaffForm" class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Full Name *</label>
                            <input type="text" name="name" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Employee ID *</label>
                            <input type="text" name="employeeId" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Department *</label>
                            <select name="department" required
                                    class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="">Select Department</option>
                                <option value="mining">Mining</option>
                                <option value="construction">Construction</option>
                                <option value="administration">Administration</option>
                                <option value="transport">Transport</option>
                                <option value="security">Security</option>
                                <option value="maintenance">Maintenance</option>
                                <option value="quality">Quality Control</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Position *</label>
                            <input type="text" name="position" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Monthly Salary (₹) *</label>
                            <input type="number" name="salary" required min="0"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Joining Date *</label>
                            <input type="date" name="joiningDate" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Contact Number</label>
                            <input type="tel" name="contact"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Email Address</label>
                            <input type="email" name="email"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div class="md:col-span-2">
                            <label class="block text-sm font-medium text-gray-700 mb-2">Address</label>
                            <textarea name="address" rows="2"
                                      class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"></textarea>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Photo URL</label>
                            <input type="text" name="photoUrl"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                                   placeholder="https://picsum.photos/200?random=1">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Status</label>
                            <select name="status"
                                    class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="active">Active</option>
                                <option value="inactive">Inactive</option>
                                <option value="on-leave">On Leave</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="flex justify-end space-x-4 pt-6 border-t">
                        <button type="button" onclick="closeAddStaffModal()"
                                class="px-6 py-2 border border-gray-300 text-gray-700 rounded-lg hover:bg-gray-50">
                            Cancel
                        </button>
                        <button type="submit"
                                class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
                            Add Staff Member
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Edit Staff Modal -->
    <div id="editStaffModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">Edit Staff Member</h3>
                    <button onclick="closeEditStaffModal()" 
                            class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times text-2xl"></i>
                    </button>
                </div>
                
                <form id="editStaffForm" class="space-y-6">
                    <input type="hidden" id="editStaffId">
                    
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Full Name *</label>
                            <input type="text" id="editName" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Employee ID *</label>
                            <input type="text" id="editEmployeeId" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Department *</label>
                            <select id="editDepartment" required
                                    class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="">Select Department</option>
                                <option value="mining">Mining</option>
                                <option value="construction">Construction</option>
                                <option value="administration">Administration</option>
                                <option value="transport">Transport</option>
                                <option value="security">Security</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Position *</label>
                            <input type="text" id="editPosition" required
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Monthly Salary (₹) *</label>
                            <input type="number" id="editSalary" required min="0"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Contact Number</label>
                            <input type="tel" id="editContact"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Email Address</label>
                            <input type="email" id="editEmail"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div class="md:col-span-2">
                            <label class="block text-sm font-medium text-gray-700 mb-2">Address</label>
                            <textarea id="editAddress" rows="2"
                                      class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"></textarea>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Photo URL</label>
                            <input type="text" id="editPhotoUrl"
                                   class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Status</label>
                            <select id="editStatus"
                                    class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none">
                                <option value="active">Active</option>
                                <option value="inactive">Inactive</option>
                                <option value="on-leave">On Leave</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="flex justify-end space-x-4 pt-6 border-t">
                        <button type="button" onclick="closeEditStaffModal()"
                                class="px-6 py-2 border border-gray-300 text-gray-700 rounded-lg hover:bg-gray-50">
                            Cancel
                        </button>
                        <button type="submit"
                                class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
                            Update Staff
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Success Toast -->
    <div id="successToast" class="fixed top-4 right-4 bg-green-500 text-white px-6 py-3 rounded-lg shadow-lg hidden z-50">
        <div class="flex items-center space-x-3">
            <i class="fas fa-check-circle"></i>
            <span id="toastMessage">Operation completed successfully!</span>
        </div>
    </div>

    <!-- Confirmation Modal -->
    <div id="confirmModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-md">
            <div class="p-6">
                <div class="flex items-center space-x-3 mb-4">
                    <div class="w-10 h-10 bg-red-100 rounded-full flex items-center justify-center">
                        <i class="fas fa-exclamation-triangle text-red-600"></i>
                    </div>
                    <h3 class="text-lg font-bold text-gray-800" id="confirmTitle">Confirm Action</h3>
                </div>
                
                <p class="text-gray-600 mb-6" id="confirmMessage">Are you sure you want to perform this action?</p>
                
                <div class="flex justify-end space-x-4">
                    <button onclick="closeConfirmModal()"
                            class="px-6 py-2 border border-gray-300 text-gray-700 rounded-lg hover:bg-gray-50">
                        Cancel
                    </button>
                    <button onclick="confirmAction()"
                            class="px-6 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700">
                        Confirm
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Define all data at the beginning
        const adminCredentials = {
            "ishant_admin": { "password": "Ishant@2024", "name": "Ishant Tibrewal" },
            "trishav_admin": { "password": "Trishav@2024", "name": "Trishav Tibrewal" },
            "abhay_admin": { "password": "Abhay@2024", "name": "Abhay Jalan" },
            "sunil_admin": { "password": "Sunil@2024", "name": "Sunil Tibrewal" }
        };

        // Sample staff data (can handle 200+ workers)
        let staffData = [
            {
                "id": 1,
                "employeeId": "TT001",
                "name": "Ramesh Kumar",
                "department": "mining",
                "position": "Mining Supervisor",
                "salary": 35000,
                "contact": "9876543210",
                "email": "ramesh@example.com",
                "address": "Gunia Mahagama, Jharkhand",
                "photoUrl": "https://picsum.photos/200?random=1",
                "status": "active",
                "joiningDate": "2023-01-15"
            },
            {
                "id": 2,
                "employeeId": "TT002",
                "name": "Suresh Singh",
                "department": "construction",
                "position": "Construction Foreman",
                "salary": 32000,
                "contact": "9876543211",
                "email": "suresh@example.com",
                "address": "Gunia Mahagama, Jharkhand",
                "photoUrl": "https://picsum.photos/200?random=2",
                "status": "active",
                "joiningDate": "2023-02-20"
            },
            {
                "id": 3,
                "employeeId": "TT003",
                "name": "Amit Sharma",
                "department": "transport",
                "position": "Truck Driver",
                "salary": 28000,
                "contact": "9876543212",
                "email": "amit@example.com",
                "address": "Gunia Mahagama, Jharkhand",
                "photoUrl": "https://picsum.photos/200?random=3",
                "status": "active",
                "joiningDate": "2023-03-10"
            },
            {
                "id": 4,
                "employeeId": "TT004",
                "name": "Rajesh Patel",
                "department": "security",
                "position": "Security Guard",
                "salary": 22000,
                "contact": "9876543213",
                "email": "rajesh@example.com",
                "address": "Gunia Mahagama, Jharkhand",
                "photoUrl": "https://picsum.photos/200?random=4",
                "status": "active",
                "joiningDate": "2023-04-05"
            },
            {
                "id": 5,
                "employeeId": "TT005",
                "name": "Vikram Singh",
                "department": "mining",
                "position": "Miner",
                "salary": 25000,
                "contact": "9876543214",
                "email": "vikram@example.com",
                "address": "Gunia Mahagama, Jharkhand",
                "photoUrl": "https://picsum.photos/200?random=5",
                "status": "active",
                "joiningDate": "2023-05-12"
            }
        ];

        // Attendance data
        let attendanceData = {};
        
        // Payroll data
        let payrollData = {};
        
        // Current user
        let currentUser = null;
        
        // Staff pagination
        let currentStaffPage = 1;
        const staffPerPage = 10;

        // DOM Elements
        const loginScreen = document.getElementById("loginScreen");
        const dashboard = document.getElementById("dashboard");
        const loginForm = document.getElementById("loginForm");
        const currentAdmin = document.getElementById("currentAdmin");
        const logoutBtn = document.getElementById("logoutBtn");
        const menuToggle = document.getElementById("menuToggle");
        const overlay = document.getElementById("overlay");
        const sidebar = document.querySelector(".sidebar");
        const navLinks = document.querySelectorAll(".nav-link");
        const contentSections = document.querySelectorAll(".content-section");
        const pageTitle = document.getElementById("pageTitle");
        const currentDate = document.getElementById("currentDate");
        const currentTime = document.getElementById("currentTime");
        const totalStaff = document.getElementById("totalStaff");
        const presentToday = document.getElementById("presentToday");
        const monthlySalary = document.getElementById("monthlySalary");
        const pendingTasks = document.getElementById("pendingTasks");
        const recentStaff = document.getElementById("recentStaff");
        const staffTableBody = document.getElementById("staffTableBody");
        const attendanceTableBody = document.getElementById("attendanceTableBody");
        const payrollTableBody = document.getElementById("payrollTableBody");

        // Initialize the application
        function init() {
            // Set current date and time
            updateDateTime();
            setInterval(updateDateTime, 60000);
            
            // Load sample data
            loadSampleData();
            
            // Update dashboard stats
            updateDashboardStats();
            
            // Populate recent staff
            updateRecentStaff();
            
            // Populate year dropdown
            populateYearDropdown();
            
            // Event listeners
            setupEventListeners();
        }

        function setupEventListeners() {
            // Login form
            loginForm.addEventListener("submit", handleLogin);
            
            // Logout button
            logoutBtn.addEventListener("click", handleLogout);
            
            // Mobile menu toggle
            menuToggle.addEventListener("click", toggleMobileMenu);
            overlay.addEventListener("click", toggleMobileMenu);
            
            // Navigation links
            navLinks.forEach(link => {
                link.addEventListener("click", function(e) {
                    e.preventDefault();
                    const section = this.getAttribute("data-section");
                    showSection(section);
                    if (window.innerWidth < 1024) {
                        toggleMobileMenu();
                    }
                });
            });
            
            // Add staff form
            document.getElementById("addStaffForm")?.addEventListener("submit", handleAddStaff);
            
            // Edit staff form
            document.getElementById("editStaffForm")?.addEventListener("submit", handleEditStaff);
            
            // Search and filter
            document.getElementById("staffSearch")?.addEventListener("input", filterStaff);
            document.getElementById("departmentFilter")?.addEventListener("change", filterStaff);
            document.getElementById("statusFilter")?.addEventListener("change", filterStaff);
            
            // Attendance date change
            document.getElementById("attendanceDate")?.addEventListener("change", loadAttendance);
            
            // Initialize attendance for today
            const today = new Date().toISOString().split("T")[0];
            document.getElementById("attendanceDate").value = today;
        }

        function handleLogin(e) {
            e.preventDefault();
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;
            
            if (adminCredentials[username] && adminCredentials[username].password === password) {
                currentUser = adminCredentials[username];
                showToast("Login successful! Welcome " + currentUser.name);
                
                // Switch to dashboard
                loginScreen.classList.add("hidden");
                dashboard.classList.remove("hidden");
                
                // Set current admin name
                currentAdmin.textContent = currentUser.name;
                
                // Load initial data
                loadStaffTable();
                loadAttendance();
            } else {
                showToast("Invalid username or password", "error");
            }
        }

        function handleLogout() {
            currentUser = null;
            dashboard.classList.add("hidden");
            loginScreen.classList.remove("hidden");
            document.getElementById("username").value = "";
            document.getElementById("password").value = "";
            showToast("Logged out successfully");
        }

        function toggleMobileMenu() {
            sidebar.classList.toggle("active");
            overlay.classList.toggle("active");
        }

        function showSection(section) {
            // Hide all sections
            contentSections.forEach(sec => sec.classList.add("hidden"));
            
            // Remove active class from all nav links
            navLinks.forEach(link => link.classList.remove("active", "text-blue-600"));
            
            // Show selected section
            document.getElementById(section + "Section").classList.remove("hidden");
            
            // Update active nav link
            document.querySelector(`[data-section="${section}"]`).classList.add("active", "text-blue-600");
            
            // Update page title
            const titles = {
                "dashboard": "Dashboard",
                "staff": "Staff Management",
                "attendance": "Attendance Management",
                "payroll": "Payroll Management",
                "reports": "Reports & Analytics",
                "settings": "System Settings"
            };
            pageTitle.textContent = titles[section];
            
            // Load section data if needed
            if (section === "staff") {
                loadStaffTable();
            } else if (section === "attendance") {
                loadAttendance();
            } else if (section === "payroll") {
                loadPayroll();
            }
        }

        function updateDateTime() {
            const now = new Date();
            currentDate.textContent = now.toLocaleDateString("en-IN", {
                weekday: "long",
                year: "numeric",
                month: "long",
                day: "numeric"
            });
            currentTime.textContent = now.toLocaleTimeString("en-IN", {
                hour: "2-digit",
                minute: "2-digit"
            });
        }

        function updateDashboardStats() {
            totalStaff.textContent = staffData.length;
            
            // Calculate present today (sample data)
            const presentCount = Math.floor(staffData.length * 0.75);
            presentToday.textContent = presentCount;
            
            // Calculate total monthly salary
            const totalSalary = staffData.reduce((sum, staff) => sum + staff.salary, 0);
            monthlySalary.textContent = totalSalary.toLocaleString("en-IN");
            
            // Calculate pending tasks
            pendingTasks.textContent = Math.floor(staffData.length * 0.1);
        }

        function updateRecentStaff() {
            recentStaff.innerHTML = "";
            const recent = staffData.slice(0, 5);
            
            recent.forEach(staff => {
                const div = document.createElement("div");
                div.className = "flex items-center space-x-3 p-3 hover:bg-gray-50 rounded-lg";
                div.innerHTML = `
                    <img src="${staff.photoUrl}" alt="${staff.name}" class="w-10 h-10 rounded-full">
                    <div class="flex-1">
                        <p class="font-medium">${staff.name}</p>
                        <p class="text-sm text-gray-500">${staff.position} • ${getDepartmentName(staff.department)}</p>
                    </div>
                    <span class="text-sm text-gray-500">${formatDate(staff.joiningDate)}</span>
                `;
                recentStaff.appendChild(div);
            });
        }

        function loadStaffTable() {
            staffTableBody.innerHTML = "";
            const start = (currentStaffPage - 1) * staffPerPage;
            const end = start + staffPerPage;
            const pageData = staffData.slice(start, end);
            
            pageData.forEach(staff => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${staff.employeeId}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <img src="${staff.photoUrl}" alt="${staff.name}" class="w-10 h-10 rounded-full">
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm font-medium text-gray-900">${staff.name}</div>
                        <div class="text-sm text-gray-500">${staff.contact || "N/A"}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-2 py-1 text-xs rounded-full ${getDepartmentColor(staff.department)}">
                            ${getDepartmentName(staff.department)}
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${staff.position}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">₹${staff.salary.toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-2 py-1 text-xs rounded-full ${getStatusColor(staff.status)}">
                            ${staff.status.charAt(0).toUpperCase() + staff.status.slice(1)}
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                        <button onclick="editStaff(${staff.id})" class="text-blue-600 hover:text-blue-900 mr-3">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="deleteStaff(${staff.id})" class="text-red-600 hover:text-red-900">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                staffTableBody.appendChild(row);
            });
            
            // Update pagination info
            document.getElementById("staffStart").textContent = start + 1;
            document.getElementById("staffEnd").textContent = Math.min(end, staffData.length);
            document.getElementById("totalStaffCount").textContent = staffData.length;
        }

        function filterStaff() {
            const searchTerm = document.getElementById("staffSearch").value.toLowerCase();
            const department = document.getElementById("departmentFilter").value;
            const status = document.getElementById("statusFilter").value;
            
            let filtered = staffData;
            
            if (searchTerm) {
                filtered = filtered.filter(staff => 
                    staff.name.toLowerCase().includes(searchTerm) ||
                    staff.employeeId.toLowerCase().includes(searchTerm) ||
                    staff.position.toLowerCase().includes(searchTerm)
                );
            }
            
            if (department) {
                filtered = filtered.filter(staff => staff.department === department);
            }
            
            if (status) {
                filtered = filtered.filter(staff => staff.status === status);
            }
            
            // Update table with filtered data
            staffTableBody.innerHTML = "";
            filtered.forEach(staff => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${staff.employeeId}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <img src="${staff.photoUrl}" alt="${staff.name}" class="w-10 h-10 rounded-full">
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm font-medium text-gray-900">${staff.name}</div>
                        <div class="text-sm text-gray-500">${staff.contact || "N/A"}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-2 py-1 text-xs rounded-full ${getDepartmentColor(staff.department)}">
                            ${getDepartmentName(staff.department)}
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${staff.position}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">₹${staff.salary.toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-2 py-1 text-xs rounded-full ${getStatusColor(staff.status)}">
                            ${staff.status.charAt(0).toUpperCase() + staff.status.slice(1)}
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                        <button onclick="editStaff(${staff.id})" class="text-blue-600 hover:text-blue-900 mr-3">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="deleteStaff(${staff.id})" class="text-red-600 hover:text-red-900">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                staffTableBody.appendChild(row);
            });
        }

        function openAddStaffModal() {
            document.getElementById("addStaffModal").classList.remove("hidden");
        }

        function closeAddStaffModal() {
            document.getElementById("addStaffModal").classList.add("hidden");
            document.getElementById("addStaffForm").reset();
        }

        function handleAddStaff(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            
            const newStaff = {
                id: staffData.length + 1,
                employeeId: formData.get("employeeId"),
                name: formData.get("name"),
                department: formData.get("department"),
                position: formData.get("position"),
                salary: parseInt(formData.get("salary")),
                contact: formData.get("contact"),
                email: formData.get("email"),
                address: formData.get("address"),
                photoUrl: formData.get("photoUrl") || `https://picsum.photos/200?random=${staffData.length + 100}`,
                status: formData.get("status"),
                joiningDate: formData.get("joiningDate")
            };
            
            staffData.push(newStaff);
            closeAddStaffModal();
            loadStaffTable();
            updateDashboardStats();
            updateRecentStaff();
            showToast("Staff member added successfully!");
        }

        function editStaff(id) {
            const staff = staffData.find(s => s.id === id);
            if (!staff) return;
            
            document.getElementById("editStaffId").value = staff.id;
            document.getElementById("editName").value = staff.name;
            document.getElementById("editEmployeeId").value = staff.employeeId;
            document.getElementById("editDepartment").value = staff.department;
            document.getElementById("editPosition").value = staff.position;
            document.getElementById("editSalary").value = staff.salary;
            document.getElementById("editContact").value = staff.contact || "";
            document.getElementById("editEmail").value = staff.email || "";
            document.getElementById("editAddress").value = staff.address || "";
            document.getElementById("editPhotoUrl").value = staff.photoUrl;
            document.getElementById("editStatus").value = staff.status;
            
            document.getElementById("editStaffModal").classList.remove("hidden");
        }

        function closeEditStaffModal() {
            document.getElementById("editStaffModal").classList.add("hidden");
        }

        function handleEditStaff(e) {
            e.preventDefault();
            const id = parseInt(document.getElementById("editStaffId").value);
            const index = staffData.findIndex(s => s.id === id);
            
            if (index !== -1) {
                staffData[index] = {
                    ...staffData[index],
                    name: document.getElementById("editName").value,
                    employeeId: document.getElementById("editEmployeeId").value,
                    department: document.getElementById("editDepartment").value,
                    position: document.getElementById("editPosition").value,
                    salary: parseInt(document.getElementById("editSalary").value),
                    contact: document.getElementById("editContact").value,
                    email: document.getElementById("editEmail").value,
                    address: document.getElementById("editAddress").value,
                    photoUrl: document.getElementById("editPhotoUrl").value,
                    status: document.getElementById("editStatus").value
                };
                
                closeEditStaffModal();
                loadStaffTable();
                updateRecentStaff();
                showToast("Staff member updated successfully!");
            }
        }

        function deleteStaff(id) {
            showConfirmModal(
                "Delete Staff Member",
                "Are you sure you want to delete this staff member? This action cannot be undone.",
                () => {
                    staffData = staffData.filter(s => s.id !== id);
                    loadStaffTable();
                    updateDashboardStats();
                    updateRecentStaff();
                    showToast("Staff member deleted successfully!");
                }
            );
        }

        function loadAttendance() {
            const date = document.getElementById("attendanceDate").value;
            attendanceTableBody.innerHTML = "";
            
            staffData.forEach(staff => {
                // Generate random attendance for demo
                const statuses = ["present", "absent", "halfday", "holiday", "sunday"];
                const randomStatus = statuses[Math.floor(Math.random() * statuses.length)];
                
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${staff.employeeId}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="flex items-center">
                            <img src="${staff.photoUrl}" alt="${staff.name}" class="w-8 h-8 rounded-full mr-3">
                            <div class="text-sm font-medium text-gray-900">${staff.name}</div>
                        </div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${getDepartmentName(staff.department)}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <select class="attendance-status border rounded px-2 py-1 text-sm ${getAttendanceClass(randomStatus)}" 
                                data-staff-id="${staff.id}" data-date="${date}">
                            <option value="present" ${randomStatus === "present" ? "selected" : ""}>Present</option>
                            <option value="absent" ${randomStatus === "absent" ? "selected" : ""}>Absent</option>
                            <option value="halfday" ${randomStatus === "halfday" ? "selected" : ""}>Half Day</option>
                            <option value="holiday" ${randomStatus === "holiday" ? "selected" : ""}>Holiday</option>
                            <option value="sunday" ${randomStatus === "sunday" ? "selected" : ""}>Sunday</option>
                        </select>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                        ${randomStatus === "present" || randomStatus === "halfday" ? "09:00 AM" : "-"}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                        ${randomStatus === "present" ? "06:00 PM" : randomStatus === "halfday" ? "01:00 PM" : "-"}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                        <button onclick="saveAttendance(${staff.id}, "${date}")" 
                                class="text-green-600 hover:text-green-900">
                            <i class="fas fa-save"></i> Save
                        </button>
                    </td>
                `;
                attendanceTableBody.appendChild(row);
            });
        }

        function saveAttendance(staffId, date) {
            const select = document.querySelector(`[data-staff-id="${staffId}"][data-date="${date}"]`);
            const status = select.value;
            
            if (!attendanceData[date]) {
                attendanceData[date] = {};
            }
            attendanceData[date][staffId] = status;
            
            showToast("Attendance saved successfully!");
        }

        function markAllPresent() {
            const date = document.getElementById("attendanceDate").value;
            const selects = document.querySelectorAll(".attendance-status");
            
            selects.forEach(select => {
                select.value = "present";
                select.className = `attendance-status border rounded px-2 py-1 text-sm ${getAttendanceClass("present")}`;
                
                const staffId = select.getAttribute("data-staff-id");
                if (!attendanceData[date]) {
                    attendanceData[date] = {};
                }
                attendanceData[date][staffId] = "present";
            });
            
            showToast("All staff marked as present!");
        }

        function bulkMarkAttendance(status) {
            const date = document.getElementById("attendanceDate").value;
            const selects = document.querySelectorAll(".attendance-status");
            
            selects.forEach(select => {
                select.value = status;
                select.className = `attendance-status border rounded px-2 py-1 text-sm ${getAttendanceClass(status)}`;
                
                const staffId = select.getAttribute("data-staff-id");
                if (!attendanceData[date]) {
                    attendanceData[date] = {};
                }
                attendanceData[date][staffId] = status;
            });
            
            showToast(`All staff marked as ${status.replace("halfday", "half day")}!`);
        }

        function loadPayroll() {
            const month = document.getElementById("payrollMonth").value;
            const year = document.getElementById("payrollYear").value;
            
            // Calculate payroll for demo
            let totalSalary = 0;
            let totalPresent = 0;
            let totalAbsent = 0;
            
            payrollTableBody.innerHTML = "";
            
            staffData.forEach(staff => {
                // Random calculations for demo
                const presentDays = Math.floor(Math.random() * 26) + 20;
                const absentDays = 26 - presentDays;
                const basicSalary = staff.salary;
                const perDaySalary = basicSalary / 26;
                const earnedSalary = perDaySalary * presentDays;
                const deductions = perDaySalary * absentDays;
                const bonus = Math.random() > 0.7 ? basicSalary * 0.1 : 0;
                const netSalary = earnedSalary + bonus;
                
                totalSalary += netSalary;
                totalPresent += presentDays;
                totalAbsent += absentDays;
                
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${staff.employeeId}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">${staff.name}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">₹${basicSalary.toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">${presentDays}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">${absentDays}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600">₹${deductions.toFixed(0).toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600">₹${bonus.toFixed(0).toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-900">₹${netSalary.toFixed(0).toLocaleString("en-IN")}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-2 py-1 text-xs rounded-full bg-yellow-100 text-yellow-800">Pending</span>
                    </td>
                `;
                payrollTableBody.appendChild(row);
            });
            
            // Update summary
            document.getElementById("totalSalaryAmount").textContent = totalSalary.toFixed(0).toLocaleString("en-IN");
            document.getElementById("totalPresentDays").textContent = totalPresent;
            document.getElementById("totalAbsentDays").textContent = totalAbsent;
            document.getElementById("payrollStaffCount").textContent = staffData.length;
            document.getElementById("totalPayable").textContent = totalSalary.toFixed(0).toLocaleString("en-IN");
        }

        function calculatePayroll() {
            loadPayroll();
            showToast("Payroll calculated successfully!");
        }

        function exportPayroll() {
            showToast("Payroll exported to Excel!");
        }

        function printPayroll() {
            window.print();
        }

        function processPayment() {
            showConfirmModal(
                "Process Payment",
                "Are you sure you want to process payments for all staff? This will mark all salaries as paid.",
                () => {
                    showToast("Payments processed successfully!");
                }
            );
        }

        function generateReport() {
            const reportType = document.getElementById("reportType").value;
            const reportContent = document.getElementById("reportContent");
            
            let html = "";
            
            if (reportType === "attendance") {
                html = `
                    <div>
                        <h3 class="text-lg font-bold text-gray-800 mb-4">Attendance Report</h3>
                        <div class="mb-6">
                            <p class="text-gray-600">Total Staff: ${staffData.length}</p>
                            <p class="text-gray-600">Average Attendance: 85%</p>
                            <p class="text-gray-600">Most Present Department: Mining</p>
                        </div>
                        <div class="h-64 bg-gray-100 rounded-lg flex items-center justify-center">
                            <p class="text-gray-500">Attendance Chart Visualization</p>
                        </div>
                    </div>
                `;
            } else if (reportType === "payroll") {
                html = `
                    <div>
                        <h3 class="text-lg font-bold text-gray-800 mb-4">Payroll Report</h3>
                        <div class="mb-6">
                            <p class="text-gray-600">Total Salary Paid: ₹${(staffData.reduce((sum, s) => sum + s.salary, 0)).toLocaleString("en-IN")}</p>
                            <p class="text-gray-600">Highest Salary: ₹${Math.max(...staffData.map(s => s.salary)).toLocaleString("en-IN")}</p>
                            <p class="text-gray-600">Lowest Salary: ₹${Math.min(...staffData.map(s => s.salary)).toLocaleString("en-IN")}</p>
                        </div>
                        <div class="h-64 bg-gray-100 rounded-lg flex items-center justify-center">
                            <p class="text-gray-500">Payroll Distribution Chart</p>
                        </div>
                    </div>
                `;
            }
            
            reportContent.innerHTML = html;
            showToast("Report generated successfully!");
        }

        function exportPDF() {
            showToast("Report exported as PDF!");
        }

        function exportExcel() {
            showToast("Report exported as Excel!");
        }

        function printReport() {
            window.print();
        }

        function saveSettings() {
            showToast("Settings saved successfully!");
        }

        function showConfirmModal(title, message, callback) {
            document.getElementById("confirmTitle").textContent = title;
            document.getElementById("confirmMessage").textContent = message;
            document.getElementById("confirmModal").classList.remove("hidden");
            
            window.confirmAction = function() {
                callback();
                closeConfirmModal();
            };
        }

        function closeConfirmModal() {
            document.getElementById("confirmModal").classList.add("hidden");
        }

        function showToast(message, type = "success") {
            const toast = document.getElementById("successToast");
            const toastMessage = document.getElementById("toastMessage");
            
            toastMessage.textContent = message;
            
            if (type === "error") {
                toast.classList.remove("bg-green-500");
                toast.classList.add("bg-red-500");
            } else {
                toast.classList.remove("bg-red-500");
                toast.classList.add("bg-green-500");
            }
            
            toast.classList.remove("hidden");
            
            setTimeout(() => {
                toast.classList.add("hidden");
            }, 3000);
        }

        // Helper functions
        function getDepartmentName(dept) {
            const names = {
                "mining": "Mining",
                "construction": "Construction",
                "administration": "Administration",
                "transport": "Transport",
                "security": "Security",
                "maintenance": "Maintenance",
                "quality": "Quality Control"
            };
            return names[dept] || dept;
        }

        function getDepartmentColor(dept) {
            const colors = {
                "mining": "bg-blue-100 text-blue-800",
                "construction": "bg-orange-100 text-orange-800",
                "administration": "bg-purple-100 text-purple-800",
                "transport": "bg-green-100 text-green-800",
                "security": "bg-red-100 text-red-800",
                "maintenance": "bg-yellow-100 text-yellow-800",
                "quality": "bg-indigo-100 text-indigo-800"
            };
            return colors[dept] || "bg-gray-100 text-gray-800";
        }

        function getStatusColor(status) {
            const colors = {
                "active": "bg-green-100 text-green-800",
                "inactive": "bg-red-100 text-red-800",
                "on-leave": "bg-yellow-100 text-yellow-800"
            };
            return colors[status] || "bg-gray-100 text-gray-800";
        }

        function getAttendanceClass(status) {
            const classes = {
                "present": "bg-green-100 text-green-800 border-green-300",
                "absent": "bg-red-100 text-red-800 border-red-300",
                "halfday": "bg-yellow-100 text-yellow-800 border-yellow-300",
                "holiday": "bg-blue-100 text-blue-800 border-blue-300",
                "sunday": "bg-purple-100 text-purple-800 border-purple-300"
            };
            return classes[status] || "bg-gray-100 text-gray-800 border-gray-300";
        }

        function formatDate(dateString) {
            const date = new Date(dateString);
            return date.toLocaleDateString("en-IN", {
                day: "numeric",
                month: "short",
                year: "numeric"
            });
        }

        function populateYearDropdown() {
            const select = document.getElementById("payrollYear");
            const currentYear = new Date().getFullYear();
            
            for (let i = currentYear - 5; i <= currentYear + 5; i++) {
                const option = document.createElement("option");
                option.value = i;
                option.textContent = i;
                if (i === currentYear) option.selected = true;
                select.appendChild(option);
            }
        }

        function loadSampleData() {
            // Add more sample staff for demonstration
            for (let i = 6; i <= 50; i++) {
                const departments = ["mining", "construction", "transport", "security", "administration"];
                const positions = {
                    "mining": ["Miner", "Supervisor", "Engineer", "Operator"],
                    "construction": ["Worker", "Foreman", "Engineer", "Supervisor"],
                    "transport": ["Driver", "Helper", "Supervisor"],
                    "security": ["Guard", "Supervisor"],
                    "administration": ["Clerk", "Accountant", "Manager"]
                };
                
                const dept = departments[Math.floor(Math.random() * departments.length)];
                const position = positions[dept][Math.floor(Math.random() * positions[dept].length)];
                
                staffData.push({
                    id: i,
                    employeeId: `TT${i.toString().padStart(3, "0")}`,
                    name: `Staff Member ${i}`,
                    department: dept,
                    position: position,
                    salary: Math.floor(Math.random() * 20000) + 20000,
                    contact: `98765${Math.floor(Math.random() * 90000) + 10000}`,
                    email: `staff${i}@example.com`,
                    address: "Gunia Mahagama, Jharkhand",
                    photoUrl: `https://picsum.photos/200?random=${i + 100}`,
                    status: Math.random() > 0.1 ? "active" : "inactive",
                    joiningDate: `2023-${Math.floor(Math.random() * 12) + 1}-${Math.floor(Math.random() * 28) + 1}`
                });
            }
        }

        function prevStaffPage() {
            if (currentStaffPage > 1) {
                currentStaffPage--;
                loadStaffTable();
            }
        }

        function nextStaffPage() {
            if (currentStaffPage * staffPerPage < staffData.length) {
                currentStaffPage++;
                loadStaffTable();
            }
        }

        // Initialize the application when DOM is loaded
        document.addEventListener("DOMContentLoaded", init);
    </script>
</body>
</html>
