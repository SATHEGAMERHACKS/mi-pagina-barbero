<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BarberApp - SPA Vanilla JS</title>
    <style>
        /* Estilos CSS completos */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a2a3a, #0d1b2a);
            color: #e0e0e0;
            min-height: 100vh;
        }

        #app {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Pantalla de Selección de Rol */
        .role-selection {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            gap: 20px;
        }

        .role-selection h1 {
            color: #fff;
            font-size: 3rem;
            margin-bottom: 2rem;
            text-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }

        .role-buttons {
            display: flex;
            gap: 30px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .role-btn {
            padding: 20px 40px;
            font-size: 1.2rem;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            color: white;
            font-weight: bold;
            min-width: 200px;
            position: relative;
            overflow: hidden;
        }

        .role-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: 0.5s;
        }

        .role-btn:hover::before {
            left: 100%;
        }

        .role-btn.cliente { background: #3498db; }
        .role-btn.barbero { background: #e74c3c; }
        .role-btn.admin { background: #2ecc71; }

        .role-btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }

        /* Header */
        .header {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            backdrop-filter: blur(12px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header h1 {
            color: #fff;
            font-size: 2rem;
        }

        .back-btn {
            padding: 10px 20px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }

        .back-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        /* Vistas */
        .view {
            display: block;
        }

        .view.hidden {
            display: none;
        }

        /* Grid de opciones */
        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .option-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            color: white;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .option-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }

        .option-card h3 {
            font-size: 1.5rem;
            margin-bottom: 10px;
            color: #fff;
        }

        .option-card p {
            color: #ccc;
        }

        /* Lista de barberos */
        .barbers-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .barber-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            color: white;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .barber-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
        }

        .barber-card h4 {
            margin-bottom: 10px;
            color: #fff;
        }

        .barber-card p {
            color: #aaa;
            margin-bottom: 5px;
        }

        /* Tabla de horarios */
        .schedule-table {
            width: 100%;
            margin-top: 20px;
            border-collapse: separate;
            border-spacing: 0;
            border-radius: 15px;
            overflow: hidden;
        }

        .schedule-table th,
        .schedule-table td {
            padding: 15px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .schedule-table th {
            background: rgba(255, 255, 255, 0.1);
            font-weight: bold;
            color: #fff;
        }

        .schedule-table td {
            background: rgba(255, 255, 255, 0.05);
        }

        .time-slot {
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            border: none;
            width: 100%;
            color: white;
        }

        .time-slot.available {
            background: #2ecc71;
        }

        .time-slot.available:hover {
            background: #27ae60;
            transform: scale(1.05);
        }

        .time-slot.unavailable {
            background: #e74c3c;
            cursor: not-allowed;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 1000;
            backdrop-filter: blur(8px);
        }

        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: rgba(30, 30, 40, 0.9);
            padding: 30px;
            border-radius: 15px;
            max-width: 500px;
            width: 90%;
            position: relative;
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
        }

        .modal-header {
            margin-bottom: 20px;
        }

        .modal-header h2 {
            color: #fff;
            font-size: 1.8rem;
        }

        .close-modal {
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 2rem;
            cursor: pointer;
            color: #aaa;
            transition: color 0.3s;
            font-weight: bold;
        }

        .close-modal:hover {
            color: #fff;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #ddd;
        }

        .form-group input,
        .form-group select {
            width: 100%;
            padding: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            font-size: 1rem;
            background: rgba(255, 255, 255, 0.05);
            color: white;
            transition: all 0.3s;
        }

        .form-group input:focus,
        .form-group select:focus {
            background: rgba(255, 255, 255, 0.1);
            border-color: rgba(255, 255, 255, 0.3);
            outline: none;
        }

        .btn-submit {
            width: 100%;
            padding: 15px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
            letter-spacing: 0.5px;
        }

        .btn-submit:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
        }

        /* Toast */
        .toast {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background: rgba(30, 30, 40, 0.9);
            color: white;
            padding: 20px 30px;
            border-radius: 8px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.3);
            transform: translateX(400px);
            transition: transform 0.3s;
            z-index: 2000;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .toast.show {
            transform: translateX(0);
        }

        /* Listas */
        .list-container {
            background: rgba(255, 255, 255, 0.08);
            border-radius: 15px;
            padding: 20px;
            margin-top: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .list-item {
            padding: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .list-item:last-child {
            border-bottom: none;
        }

        .list-item h4 {
            color: #fff;
            margin-bottom: 5px;
        }

        .list-item p {
            color: #aaa;
            font-size: 0.9rem;
        }

        .list-item .status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .status.pending { background: #f39c12; color: white; }
        .status.confirmed { background: #2ecc71; color: white; }
        .status.cancelled { background: #e74c3c; color: white; }
        .status.completed { background: #3498db; color: white; }

        .action-buttons {
            display: flex;
            gap: 10px;
        }

        .btn-action {
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
            color: white;
            font-weight: 600;
        }

        .btn-confirm { background: #2ecc71; }
        .btn-confirm:hover { background: #27ae60; }

        .btn-cancel { background: #e74c3c; }
        .btn-cancel:hover { background: #c0392b; }

        .btn-complete { background: #3498db; }
        .btn-complete:hover { background: #2980b9; }

        /* Dashboard Admin */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            color: white;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.12);
        }

        .stat-card h3 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: #fff;
        }

        .stat-card p {
            color: #aaa;
        }

        .chart-container {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .chart-container h3 {
            color: #fff;
            margin-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            padding-bottom: 10px;
        }

        .chart-bar {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }

        .chart-label {
            width: 150px;
            font-weight: bold;
            color: #ddd;
        }

        .chart-progress {
            flex: 1;
            height: 25px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            overflow: hidden;
            margin: 0 10px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .chart-fill {
            height: 100%;
            transition: width 0.5s ease;
        }

        .chart-value {
            min-width: 40px;
            text-align: right;
            font-weight: bold;
            color: #ddd;
        }

        /* Categorías de productos */
        .category-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .category-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            color: white;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .category-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
        }

        .category-card h4 {
            margin-bottom: 10px;
            color: #fff;
        }

        .category-card p {
            color: #aaa;
        }

        /* Productos */
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .product-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s;
        }

        .product-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.12);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
        }

        .product-card h4 {
            color: #fff;
            margin-bottom: 10px;
            font-size: 1.3rem;
        }

        .product-card p {
            color: #aaa;
            margin-bottom: 5px;
        }

        .product-card .price {
            font-size: 1.5rem;
            color: #2ecc71;
            font-weight: bold;
            margin: 10px 0;
        }

        .product-card .stock {
            color: #f39c12;
            margin-bottom: 15px;
            font-weight: bold;
        }

        .btn-add-cart {
            width: 100%;
            padding: 10px;
            background: #2ecc71;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }

        .btn-add-cart:hover {
            background: #27ae60;
            transform: translateY(-2px);
        }
    </style>
</head>
<body>
    <div id="app"></div>

    <!-- Modal -->
    <div id="modal" class="modal">
        <div class="modal-content">
            <span class="close-modal" onclick="closeModal()">&times;</span>
            <div id="modal-body"></div>
        </div>
    </div>

    <!-- Toast -->
    <div id="toast" class="toast"></div>

    <script>
        // ======= ESTADO CENTRALIZADO (Single Source of Truth) =======
        const state = {
            currentRole: null, // 'cliente' | 'barbero' | 'admin' | null
            currentView: 'role-selection', // 'role-selection' | 'cliente-main' | 'agendar-cita' | 'ver-productos' | 'barbero-main' | 'admin-main'
            currentFlow: null, // 'agendar' | 'productos' | null
            
            // Datos de la aplicación
            barbers: [
                { id: 1, name: 'Carlos Mendoza', specialty: 'Clásico', experience: '5 años' },
                { id: 2, name: 'Andrea Ramos', specialty: 'Moderno', experience: '3 años' },
                { id: 3, name: 'Juan Pérez', specialty: 'Vintage', experience: '7 años' }
            ],
            selectedBarber: null,
            
            // Horarios: día -> hora -> estado
            schedule: {
                'Lunes': { '9:00': 'available', '10:00': 'available', '11:00': 'unavailable', '12:00': 'available' },
                'Martes': { '9:00': 'available', '10:00': 'unavailable', '11:00': 'available', '12:00': 'available' },
                'Miércoles': { '9:00': 'unavailable', '10:00': 'available', '11:00': 'available', '12:00': 'unavailable' },
                'Jueves': { '9:00': 'available', '10:00': 'available', '11:00': 'unavailable', '12:00': 'available' },
                'Viernes': { '9:00': 'available', '10:00': 'unavailable', '11:00': 'available', '12:00': 'available' },
                'Sábado': { '9:00': 'available', '10:00': 'available', '11:00': 'available', '12:00': 'available' },
                'Domingo': { '9:00': 'unavailable', '10:00': 'unavailable', '11:00': 'unavailable', '12:00': 'unavailable' }
            },
            selectedSlot: null, // { day, time }
            
            // Citas
            appointments: [],
            
            // Productos
            categories: [
                { id: 1, name: 'Cuidado Capilar', description: 'Shampoos y acondicionadores' },
                { id: 2, name: 'Styling', description: 'Ceras y geles' },
                { id: 3, name: 'Barba', description: 'Aceites y bálsamos' }
            ],
            products: [
                { id: 1, name: 'Shampoo Anticaspa', price: 150, stock: 25, category: 1 },
                { id: 2, name: 'Acondicionador Hidratante', price: 180, stock: 18, category: 1 },
                { id: 3, name: 'Cera Matte', price: 220, stock: 12, category: 2 },
                { id: 4, name: 'Gel Fuerte', price: 160, stock: 30, category: 2 },
                { id: 5, name: 'Aceite de Barba', price: 200, stock: 15, category: 3 },
                { id: 6, name: 'Bálsamo Aftershave', price: 190, stock: 8, category: 3 }
            ],
            selectedCategory: null,
            
            // Notificaciones
            notifications: [],
            
            // Ventas
            sales: []
        };

        // ======= PERSISTENCIA CON localStorage =======
        function loadStore() {
            const stored = localStorage.getItem('barberAppState');
            if (stored) {
                const parsed = JSON.parse(stored);
                // Fusionar con el estado inicial para no perder estructura
                Object.assign(state, parsed);
            }
        }

        function saveStore() {
            localStorage.setItem('barberAppState', JSON.stringify(state));
        }

        // ======= FUNCIÓN SETSTATE (Única forma de modificar el estado) =======
        function setState(patch) {
            // Merge del patch con el estado actual
            Object.assign(state, patch);
            
            // Guardar en localStorage
            saveStore();
            
            // Re-renderizar la aplicación
            render();
            
            // Re-attach event handlers
            attachHandlers();
        }

        // ======= RENDERIZADO DECLARATIVO =======
        function render() {
            const app = document.getElementById('app');
            
            if (state.currentView === 'role-selection') {
                app.innerHTML = renderRoleSelection();
            } else if (state.currentView === 'cliente-main') {
                app.innerHTML = renderClienteMain();
            } else if (state.currentView === 'agendar-cita') {
                if (!state.selectedBarber) {
                    app.innerHTML = renderBarberSelection();
                } else {
                    app.innerHTML = renderScheduleTable();
                }
            } else if (state.currentView === 'ver-productos') {
                if (!state.selectedCategory) {
                    app.innerHTML = renderCategories();
                } else {
                    app.innerHTML = renderProducts();
                }
            } else if (state.currentView === 'barbero-main') {
                app.innerHTML = renderBarberoMain();
            } else if (state.currentView === 'admin-main') {
                app.innerHTML = renderAdminMain();
            }
        }

        // ======= VISTAS =======
        function renderRoleSelection() {
            return `
                <div class="role-selection">
                    <h1>BarberApp</h1>
                    <div class="role-buttons">
                        <button class="role-btn cliente" onclick="selectRole('cliente')">
                            Cliente
                        </button>
                        <button class="role-btn barbero" onclick="selectRole('barbero')">
                            Barbero
                        </button>
                        <button class="role-btn admin" onclick="selectRole('admin')">
                            Admin
                        </button>
                    </div>
                </div>
            `;
        }

        function renderHeader(title) {
            return `
                <div class="header">
                    <h1>${title}</h1>
                    <button class="back-btn" onclick="goBack()">← Volver</button>
                </div>
            `;
        }

        function renderClienteMain() {
            return `
                ${renderHeader('Cliente')}
                <div class="options-grid">
                    <div class="option-card" onclick="navigateTo('agendar-cita')">
                        <h3>📅</h3>
                        <h3>Agendar Cita</h3>
                        <p>Reserva tu hora con el barbero de tu preferencia</p>
                    </div>
                    <div class="option-card" onclick="navigateTo('ver-productos')">
                        <h3>🛍️</h3>
                        <h3>Ver Productos</h3>
                        <p>Explora nuestros productos para el cuidado personal</p>
                    </div>
                </div>
            `;
        }

        function renderBarberSelection() {
            return `
                ${renderHeader('Selecciona un Barbero')}
                <div class="barbers-list">
                    ${state.barbers.map(barber => `
                        <div class="barber-card" onclick="selectBarber(${barber.id})">
                            <h4>${barber.name}</h4>
                            <p><strong>Especialidad:</strong> ${barber.specialty}</p>
                            <p><strong>Experiencia:</strong> ${barber.experience}</p>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderScheduleTable() {
            const barber = state.barbers.find(b => b.id === state.selectedBarber);
            const hours = ['9:00', '10:00', '11:00', '12:00'];
            const days = ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo'];
            
            return `
                ${renderHeader(`Horarios - ${barber.name}`)}
                <div class="table-container">
                    <table class="schedule-table">
                        <thead>
                            <tr>
                                <th>Hora</th>
                                ${days.map(day => `<th>${day}</th>`).join('')}
                            </tr>
                        </thead>
                        <tbody>
                            ${hours.map(hour => `
                                <tr>
                                    <td><strong>${hour}</strong></td>
                                    ${days.map(day => {
                                        const status = state.schedule[day][hour];
                                        return `
                                            <td>
                                                <button class="time-slot ${status}" 
                                                        onclick="selectTimeSlot('${day}', '${hour}')"
                                                        ${status === 'unavailable' ? 'disabled' : ''}>
                                                    ${status === 'available' ? 'Disponible' : 'Ocupado'}
                                                </button>
                                            </td>
                                        `;
                                    }).join('')}
                                </tr>
                            `).join('')}
                        </tbody>
                    </table>
                </div>
            `;
        }

        function renderCategories() {
            return `
                ${renderHeader('Categorías de Productos')}
                <div class="category-grid">
                    ${state.categories.map(category => `
                        <div class="category-card" onclick="selectCategory(${category.id})">
                            <h4>${category.name}</h4>
                            <p>${category.description}</p>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderProducts() {
            const category = state.categories.find(c => c.id === state.selectedCategory);
            const products = state.products.filter(p => p.category === category.id);
            
            return `
                ${renderHeader(category.name)}
                <div class="products-grid">
                    ${products.map(product => `
                        <div class="product-card">
                            <h4>${product.name}</h4>
                            <p class="price">$${product.price}</p>
                            <p class="stock">Stock: ${product.stock} unidades</p>
                            <button class="btn-add-cart" onclick="addToCart(${product.id})">
                                Agregar al carrito
                            </button>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderBarberoMain() {
            const barberAppointments = state.appointments.filter(a => a.barberId === state.selectedBarber);
            
            return `
                ${renderHeader('Panel del Barbero')}
                
                <h2>Notificaciones</h2>
                <div class="list-container">
                    ${state.notifications.slice(0, 5).map(notif => `
                        <div class="list-item">
                            <div>
                                <h4>${notif.title}</h4>
                                <p>${notif.message}</p>
                            </div>
                        </div>
                    `).join('')}
                    ${state.notifications.length === 0 ? '<p style="text-align: center; color: #999;">No hay notificaciones</p>' : ''}
                </div>
                
                <h2>Gestión de Citas</h2>
                <div class="list-container">
                    ${barberAppointments.map(appointment => `
                        <div class="list-item">
                            <div>
                                <h4>${appointment.clientName}</h4>
                                <p>${appointment.day} ${appointment.time} - ${appointment.service}</p>
                            </div>
                            <div class="action-buttons">
                                <span class="status ${appointment.status}">${appointment.status}</span>
                                ${appointment.status === 'pending' ? `
                                    <button class="btn-action btn-confirm" onclick="updateAppointment(${appointment.id}, 'confirmed')">Confirmar</button>
                                    <button class="btn-action btn-cancel" onclick="updateAppointment(${appointment.id}, 'cancelled')">Cancelar</button>
                                ` : ''}
                                ${appointment.status === 'confirmed' ? `
                                    <button class="btn-action btn-cancel" onclick="updateAppointment(${appointment.id}, 'cancelled')">Cancelar</button>
                                    <button class="btn-action btn-complete" onclick="updateAppointment(${appointment.id}, 'completed')">Completar</button>
                                ` : ''}
                            </div>
                        </div>
                    `).join('')}
                    ${barberAppointments.length === 0 ? '<p style="text-align: center; color: #999;">No hay citas</p>' : ''}
                </div>
            `;
        }

        function renderAdminMain() {
            const totalAppointments = state.appointments.length;
            const activeClients = new Set(state.appointments.map(a => a.clientEmail)).size;
            const totalProducts = state.products.length;
            const lowStockProducts = state.products.filter(p => p.stock < 10).length;
            
            // Stats para gráficos
            const barberStats = {};
            state.barbers.forEach(barber => {
                barberStats[barber.name] = state.appointments.filter(a => a.barberId === barber.id && a.status === 'completed').length;
            });
            
            const maxStat = Math.max(...Object.values(barberStats), 1);
            
            return `
                ${renderHeader('Panel de Administración')}
                
                <div class="dashboard-grid">
                    <div class="stat-card">
                        <h3>${totalAppointments}</h3>
                        <p>Total de Citas</p>
                    </div>
                    <div class="stat-card">
                        <h3>${activeClients}</h3>
                        <p>Clientes Activos</p>
                    </div>
                    <div class="stat-card">
                        <h3>${totalProducts}</h3>
                        <p>Productos en Stock</p>
                    </div>
                    <div class="stat-card">
                        <h3>${lowStockProducts}</h3>
                        <p>Productos con Bajo Stock</p>
                    </div>
                </div>
                
                <h2>Barberos</h2>
                <div class="list-container">
                    ${state.barbers.map(barber => `
                        <div class="list-item">
                            <div>
                                <h4>${barber.name}</h4>
                                <p>${barber.specialty} - ${barber.experience}</p>
                            </div>
                        </div>
                    `).join('')}
                </div>
                
                <h2>Flujo de Clientes por Barbero</h2>
                <div class="chart-container">
                    ${Object.entries(barberStats).map(([name, count]) => `
                        <div class="chart-bar">
                            <div class="chart-label">${name}</div>
                            <div class="chart-progress">
                                <div class="chart-fill" style="width: ${(count / maxStat) * 100}%; background: #3498db;"></div>
                            </div>
                            <div class="chart-value">${count}</div>
                        </div>
                    `).join('')}
                </div>
                
                <h2>Flujo de Compras Recientes</h2>
                <div class="list-container">
                    ${state.sales.slice(0, 5).map(sale => `
                        <div class="list-item">
                            <div>
                                <h4>${sale.productName}</h4>
                                <p>${sale.date} - ${sale.clientName}</p>
                            </div>
                            <div>
                                <strong>$${sale.amount}</strong>
                            </div>
                        </div>
                    `).join('')}
                    ${state.sales.length === 0 ? '<p style="text-align: center; color: #999;">No hay ventas registradas</p>' : ''}
                </div>
            `;
        }

        // ======= MANEJADORES DE EVENTOS =======
        function selectRole(role) {
            setState({ 
                currentRole: role,
                currentView: role === 'cliente' ? 'cliente-main' : 
                           role === 'barbero' ? 'barbero-main' : 'admin-main'
            });
        }

        function navigateTo(view) {
            setState({ 
                currentView: view,
                currentFlow: view === 'agendar-cita' ? 'agendar' : 
                            view === 'ver-productos' ? 'productos' : null
            });
        }

        function goBack() {
            if (state.currentView === 'agendar-cita' && state.selectedBarber) {
                setState({ selectedBarber: null });
            } else if (state.currentView === 'ver-productos' && state.selectedCategory) {
                setState({ selectedCategory: null });
            } else if (state.currentView.includes('-main')) {
                setState({ 
                    currentView: 'role-selection',
                    currentRole: null,
                    currentFlow: null,
                    selectedBarber: null,
                    selectedCategory: null
                });
            } else {
                setState({ 
                    currentView: state.currentRole + '-main',
                    currentFlow: null,
                    selectedBarber: null,
                    selectedCategory: null
                });
            }
        }

        function selectBarber(barberId) {
            setState({ selectedBarber: barberId });
        }

        function selectTimeSlot(day, time) {
            setState({ 
                selectedSlot: { day, time },
                modalType: 'appointment-form'
            });
            openModal();
        }

        function selectCategory(categoryId) {
            setState({ selectedCategory: categoryId });
        }

        function addToCart(productId) {
            const product = state.products.find(p => p.id === productId);
            showToast(`Producto "${product.name}" agregado al carrito`);
            
            // Registrar venta
            const newSale = {
                id: Date.now(),
                productName: product.name,
                amount: product.price,
                date: new Date().toLocaleDateString(),
                clientName: 'Cliente Demo'
            };
            
            setState({ 
                sales: [...state.sales, newSale]
            });
        }

        function updateAppointment(appointmentId, newStatus) {
            const appointment = state.appointments.find(a => a.id === appointmentId);
            showToast(`Cita ${newStatus} para ${appointment.clientName}`);
            
            const updatedAppointments = state.appointments.map(a => 
                a.id === appointmentId ? { ...a, status: newStatus } : a
            );
            
            setState({ appointments: updatedAppointments });
        }

        // ======= MODAL =======
        function openModal() {
            const modal = document.getElementById('modal');
            const modalBody = document.getElementById('modal-body');
            
            if (state.modalType === 'appointment-form') {
                modalBody.innerHTML = `
                    <div class="modal-header">
                        <h2>Confirmar Cita</h2>
                    </div>
                    <form onsubmit="confirmAppointment(event)">
                        <div class="form-group">
                            <label>Nombre Completo</label>
                            <input type="text" id="clientName" required>
                        </div>
                        <div class="form-group">
                            <label>Correo Electrónico</label>
                            <input type="email" id="clientEmail" required>
                        </div>
                        <div class="form-group">
                            <label>Teléfono</label>
                            <input type="tel" id="clientPhone" required>
                        </div>
                        <div class="form-group">
                            <label>Servicio</label>
                            <select id="service" required>
                                <option value="Corte de cabello">Corte de cabello</option>
                                <option value="Corte + Barba">Corte + Barba</option>
                                <option value="Afeitado">Afeitado</option>
                            </select>
                        </div>
                        <button type="submit" class="btn-submit">Reservar Cita</button>
                    </form>
                `;
            }
            
            modal.classList.add('active');
        }

        function closeModal() {
            const modal = document.getElementById('modal');
            modal.classList.remove('active');
            setState({ modalType: null, selectedSlot: null });
        }

        function confirmAppointment(event) {
            event.preventDefault();
            
            const clientName = document.getElementById('clientName').value;
            const clientEmail = document.getElementById('clientEmail').value;
            const clientPhone = document.getElementById('clientPhone').value;
            const service = document.getElementById('service').value;
            
            // Marcar horario como no disponible
            const newSchedule = { ...state.schedule };
            newSchedule[state.selectedSlot.day][state.selectedSlot.time] = 'unavailable';
            
            // Crear nueva cita
            const newAppointment = {
                id: Date.now(),
                barberId: state.selectedBarber,
                clientName,
                clientEmail,
                clientPhone,
                service,
                day: state.selectedSlot.day,
                time: state.selectedSlot.time,
                status: 'pending',
                date: new Date().toISOString()
            };
            
            // Crear notificación para el barbero
            const newNotification = {
                id: Date.now(),
                title: 'Nueva cita registrada',
                message: `Nueva cita de ${clientName} - ${state.selectedSlot.day} ${state.selectedSlot.time}`,
                date: new Date().toISOString()
            };
            
            setState({
                appointments: [...state.appointments, newAppointment],
                notifications: [...state.notifications, newNotification],
                schedule: newSchedule,
                modalType: null,
                selectedSlot: null
            });
            
            closeModal();
            showToast('Cita creada y notificada');
        }

        // ======= TOAST =======
        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.classList.add('show');
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // ======= ATTACH HANDLERS =======
        function attachHandlers() {
            // Cerrar modal al hacer clic fuera
            const modal = document.getElementById('modal');
            if (modal) {
                modal.addEventListener('click', (e) => {
                    if (e.target === modal) {
                        closeModal();
                    }
                });
            }
        }

        // ======= INICIALIZACIÓN =======
        // Cargar datos al inicio
        loadStore();
        
        // Renderizar aplicación
        render();
        
        // Adjuntar handlers
        attachHandlers();
    </script>
</body>
</html>
