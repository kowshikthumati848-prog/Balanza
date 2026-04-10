<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BALANZA | Luxury Balanced Sleeper</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        .balanza-green { background-color: #1b4332; }
        .text-balanza { color: #1b4332; }
        .seat { transition: all 0.3s ease; cursor: pointer; border: 2px solid #1b4332; }
        .seat.selected { background-color: #1b4332; color: white; }
        .seat.booked { background-color: #e2e8f0; border-color: #cbd5e1; cursor: not-allowed; opacity: 0.6; }
        .airplane-cabin { border-radius: 8px 30px 30px 8px; }
    </style>
</head>
<body class="bg-slate-50 font-sans">

<div id="app">
    <nav class="bg-white shadow-sm sticky top-0 z-50">
        <div class="container mx-auto px-6 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="p-2 balanza-green rounded-lg text-white"><i class="fas fa-bus"></i></div>
                <div>
                    <h1 class="text-2xl font-black text-balanza leading-none">BALANZA</h1>
                    <p class="text-[10px] uppercase tracking-[0.2em] font-bold text-green-700">Balanced • Safe • Modern</p>
                </div>
            </div>
            <div class="hidden md:flex space-x-8 items-center font-bold text-sm uppercase">
                <a href="#" class="text-balanza">Safety Tech</a>
                <a href="#" class="text-gray-500 hover:text-balanza">Experience</a>
                <button class="balanza-green text-white px-6 py-2 rounded-full shadow-lg">Login / Sign Up</button>
            </div>
        </div>
    </nav>

    <section class="balanza-green py-12 px-6">
        <div class="container mx-auto max-w-6xl">
            <h2 class="text-white text-3xl font-bold mb-8 text-center">India's Safest Overnight Travel</h2>
            <div class="bg-white p-2 rounded-xl shadow-2xl flex flex-col lg:flex-row gap-2">
                <div class="flex-1 flex items-center px-4 py-3 border-r border-gray-100">
                    <i class="fas fa-bus-alt mr-3 text-gray-400"></i>
                    <input type="text" v-model="search.from" placeholder="From Station" class="w-full outline-none font-medium">
                </div>
                <div class="flex-1 flex items-center px-4 py-3 border-r border-gray-100">
                    <i class="fas fa-map-marker-alt mr-3 text-gray-400"></i>
                    <input type="text" v-model="search.to" placeholder="To Station" class="w-full outline-none font-medium">
                </div>
                <div class="flex-1 flex items-center px-4 py-3">
                    <i class="fas fa-calendar-alt mr-3 text-gray-400"></i>
                    <input type="date" class="w-full outline-none font-medium">
                </div>
                <button @click="showBuses = true" class="bg-orange-500 hover:bg-orange-600 text-white px-10 py-4 rounded-lg font-black uppercase tracking-widest transition">
                    Search
                </button>
            </div>
        </div>
    </section>

    <section v-if="showBuses" class="container mx-auto px-6 py-12">
        <div class="flex flex-col lg:flex-row gap-8">
            
            <div class="lg:w-2/3">
                <div class="bg-white rounded-2xl shadow-md border border-gray-100 overflow-hidden mb-6">
                    <div class="p-6 flex flex-col md:flex-row justify-between items-center gap-6">
                        <div>
                            <span class="bg-green-100 text-green-800 text-[10px] font-bold px-2 py-1 rounded uppercase">Safest Choice</span>
                            <h3 class="text-xl font-bold mt-2">BALANZA LUXE - 9600 Series</h3>
                            <p class="text-sm text-gray-500">Dual-Side Balanced • Airplane Recliners</p>
                        </div>
                        <div class="flex items-center space-x-12">
                            <div class="text-center">
                                <p class="text-2xl font-black">21:00</p>
                                <p class="text-xs text-gray-400 uppercase font-bold">{{search.from || 'Origin'}}</p>
                            </div>
                            <div class="text-gray-300 flex flex-col items-center">
                                <span class="text-[10px] font-bold text-gray-400 italic">08h 30m</span>
                                <div class="w-20 h-[2px] bg-gray-200 relative">
                                    <div class="absolute -top-1 -right-1 w-2 h-2 rounded-full bg-gray-200"></div>
                                </div>
                            </div>
                            <div class="text-center">
                                <p class="text-2xl font-black">05:30</p>
                                <p class="text-xs text-gray-400 uppercase font-bold">{{search.to || 'Dest'}}</p>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="text-sm text-gray-400 line-through">₹3,200</p>
                            <p class="text-2xl font-black text-balanza">₹2,500</p>
                            <button @click="toggleSeatMap" class="mt-2 text-xs font-bold text-orange-500 uppercase border-b-2 border-orange-500">View Seats</button>
                        </div>
                    </div>

                    <div v-if="seatMapVisible" class="bg-slate-50 p-8 border-t border-dashed border-gray-200">
                        <div class="flex flex-col md:flex-row gap-12">
                            <div class="flex-1">
                                <h4 class="font-bold mb-6 text-center text-gray-400 uppercase tracking-widest text-xs">Select Your Private Cabin</h4>
                                <div class="max-w-[300px] mx-auto bg-white p-6 rounded-[40px] border-4 border-gray-200 shadow-inner">
                                    <div class="flex justify-end mb-10 px-4 text-gray-300"><i class="fas fa-dharmachakra fa-2x"></i></div>
                                    
                                    <div class="grid grid-cols-2 gap-x-16 gap-y-4">
                                        <div v-for="n in 6" :key="'L'+n" 
                                             @click="selectSeat('L'+n)"
                                             :class="['seat airplane-cabin h-14 flex items-center justify-center font-bold text-xs', isSelected('L'+n) ? 'selected' : '', isBooked('L'+n) ? 'booked' : '']">
                                            L{{n}}
                                        </div>
                                        <div v-for="n in 6" :key="'R'+n" 
                                             @click="selectSeat('R'+n)"
                                             :class="['seat airplane-cabin h-14 flex items-center justify-center font-bold text-xs', isSelected('R'+n) ? 'selected' : '', isBooked('R'+n) ? 'booked' : '']">
                                            R{{n}}
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="md:w-1/3 bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                                <h4 class="font-bold mb-4 border-b pb-2 uppercase text-xs">Booking Details</h4>
                                <div v-if="selectedSeats.length > 0">
                                    <div class="flex justify-between mb-2">
                                        <span class="text-gray-500 text-sm">Cabins:</span>
                                        <span class="font-bold">{{ selectedSeats.join(', ') }}</span>
                                    </div>
                                    <div class="flex justify-between mb-4">
                                        <span class="text-gray-500 text-sm">Amount:</span>
                                        <span class="font-bold text-balanza">₹{{ selectedSeats.length * 2500 }}</span>
                                    </div>
                                    <button class="w-full bg-orange-500 text-white py-3 rounded-xl font-bold hover:bg-orange-600">Proceed to Payment</button>
                                </div>
                                <p v-else class="text-gray-400 text-sm italic">Please select a cabin to continue.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="lg:w-1/3 space-y-6">
                <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-balanza">
                    <h4 class="font-bold text-balanza mb-3"><i class="fas fa-shield-alt mr-2"></i> Safety Innovation</h4>
                    <p class="text-sm text-gray-600">Our 1x1 layout ensures **Equal Weight Distribution** [cite: 17, 19], preventing the tipping risks found in traditional sleeper buses[cite: 25, 65].</p>
                </div>
                <div class="bg-white p-6 rounded-2xl shadow-sm border-l-4 border-balanza">
                    <h4 class="font-bold text-balanza mb-3"><i class="fas fa-couch mr-2"></i> Recliner Cabins</h4>
                    <p class="text-sm text-gray-600">Airplane-style reclining sofa pods for a **cramp-free** overnight journey[cite: 16, 53].</p>
                </div>
            </div>
        </div>
    </section>
</div>

<script>
    const { createApp } = Vue
    createApp({
        data() {
            return {
                showBuses: false,
                seatMapVisible: false,
                search: { from: '', to: '' },
                selectedSeats: [],
                bookedSeats: ['L2', 'R4', 'R5'] // Simulated booked seats
            }
        },
        methods: {
            toggleSeatMap() { this.seatMapVisible = !this.seatMapVisible },
            selectSeat(id) {
                if(this.bookedSeats.includes(id)) return;
                const index = this.selectedSeats.indexOf(id);
                if (index > -1) this.selectedSeats.splice(index, 1);
                else this.selectedSeats.push(id);
            },
            isSelected(id) { return this.selectedSeats.includes(id) },
            isBooked(id) { return this.bookedSeats.includes(id) }
        }
    }).mount('#app')
</script>

</body>
</html>
