<!DOCTYPE html>
<html lang="en" class="h-full bg-gray-900">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Sync App</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Simple transition for tab content */
        .tab-content {
            display: none;
            transition: opacity 0.3s ease;
        }
        .tab-content.active {
            display: block;
            opacity: 1;
        }
        /* Custom scrollbar for dark mode */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #2d3748; /* gray-800 */ }
        ::-webkit-scrollbar-thumb { background: #4a5568; /* gray-600 */ border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #718096; /* gray-500 */ }
    </style>
</head>
<body class="h-full text-gray-100 font-sans">

    <div class="container mx-auto max-w-2xl p-4">
        
        <!-- App Header -->
        <header class="mb-4">
            <h1 class="text-3xl font-bold text-center text-white">Team Sync</h1>
            <p class="text-center text-indigo-300">Your team's status and content list.</p>
        </header>

        <!-- User Info Section -->
        <div class="bg-gray-800 p-4 rounded-lg shadow-xl mb-6">
            <div class="mb-3">
                <label for="nameInput" class="block text-sm font-medium text-gray-300 mb-1">Your Display Name</label>
                <div class="flex space-x-2">
                    <input type="text" id="nameInput" class="flex-1 bg-gray-700 border border-gray-600 text-white rounded-md p-2 text-sm focus:ring-indigo-500 focus:border-indigo-500" placeholder="Enter your name..." value="Anonymous User">
                    <button id="setNameBtn" class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2 px-4 rounded-md shadow focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75 transition duration-150">
                        Set Name
                    </button>
                </div>
            </div>
            <div id="statusMessage" class="text-sm text-yellow-400">
                ⚠️ **Note:** This app is fully public and does not require sign-in.
            </div>
        </div>

        <!-- Tab Navigation -->
        <div class="mb-4 border-b border-gray-700">
            <nav class="flex -mb-px space-x-4" aria-label="Tabs">
                <button class="tab-btn active-tab whitespace-nowrap py-3 px-4 border-b-2 font-medium text-sm text-indigo-400 border-indigo-400" data-tab="availability">
                    Availability
                </button>
                <button class="tab-btn inactive-tab whitespace-nowrap py-3 px-4 border-b-2 font-medium text-sm text-gray-400 border-transparent hover:text-gray-200 hover:border-gray-500" data-tab="content">
                    Content List
                </button>
            </nav>
        </div>

        <!-- Loading & Error Messages -->
        <div id="messageArea" class="text-center p-4 my-4 bg-red-800 text-red-100 rounded-lg shadow hidden"></div>
        <div id="loadingArea" class="text-center p-4 my-4 text-gray-400">Initializing database connection...</div>


        <!-- Tab 1: Availability Content -->
        <div id="availability" class="tab-content active">
            <!-- A) Set Your Availability -->
            <section class="bg-gray-800 p-4 rounded-lg shadow-xl mb-6">
                <h2 class="text-lg font-semibold mb-3 text-white">Update Your Status</h2>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                    <button data-status="green" class="status-btn bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-4 rounded-lg shadow-md transition duration-150 transform hover:scale-105">
                        Very Free
                    </button>
                    <button data-status="orange" class="status-btn bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 px-4 rounded-lg shadow-md transition duration-150 transform hover:scale-105">
                        Free (Uncertain)
                    </button>
                    <button data-status="red" class="status-btn bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-lg shadow-md transition duration-150 transform hover:scale-105">
                        Busy
                    </button>
                </div>
            </section>

            <!-- A) See Other Users' Availability -->
            <section>
                <h2 class="text-xl font-semibold mb-3 text-white">Team Status</h2>
                <div id="teamStatusList" class="space-y-3">
                    <p id="statusLoading" class="text-gray-400">Loading team statuses...</p>
                    <!-- Statuses will be injected here -->
                </div>
            </section>
        </div>

        <!-- Tab 2: Content List Content -->
        <div id="content" class="tab-content">
            <!-- B) Add to Content List -->
            <section class="bg-gray-800 p-4 rounded-lg shadow-xl mb-6">
                <h2 class="text-lg font-semibold mb-3 text-white">Add to Content List</h2>
                <form id="addContentForm" class="space-y-3">
                    <div>
                        <label for="contentType" class="block text-sm font-medium text-gray-300">Content Type</label>
                        <select id="contentType" class="w-full bg-gray-700 border border-gray-600 text-white rounded-md p-2 mt-1 text-sm focus:ring-indigo-500 focus:border-indigo-500">
                            <option>Movie</option>
                            <option>Book</option>
                            <option>Song</option>
                            <option>Essay</option>
                            <option>Other</option>
                        </select>
                    </div>
                    <div>
                        <label for="contentTitle" class="block text-sm font-medium text-gray-300">Title</label>
                        <input type="text" id="contentTitle" class="w-full bg-gray-700 border border-gray-600 text-white rounded-md p-2 mt-1 text-sm focus:ring-indigo-500 focus:border-indigo-500" placeholder="e.g., 'Inception'" required>
                    </div>
                    <div>
                        <label for="contentDescription" class="block text-sm font-medium text-gray-300">Description</label>
                        <textarea id="contentDescription" rows="2" class="w-full bg-gray-700 border border-gray-600 text-white rounded-md p-2 mt-1 text-sm focus:ring-indigo-500 focus:border-indigo-500" placeholder="e.g., 'A mind-bending sci-fi movie'"></textarea>
                    </div>
                    <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2 px-4 rounded-md shadow focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75 transition duration-150">
                        Add Item
                    </button>
                </form>
            </section>

            <!-- B) Shared Content List -->
            <section>
                <h2 class="text-xl font-semibold mb-3 text-white">Shared List</h2>
                <div id="contentList" class="space-y-3">
                    <p id="contentLoading" class="text-gray-400">Loading content list...</p>
                    <!-- Content items will be injected here -->
                </div>
            </section>
        </div>

    </div>

    <!-- Firebase Library (Only Firestore is needed now) -->
    <script type="module">
        // --- HARDCODED FIREBASE CONFIG ---
        // This is a minimal configuration that enables basic Firestore access for a globally public app.
        const PUBLIC_FIREBASE_CONFIG = {
            // Using a dummy project ID as only the Firebase CDN imports are required for the public access method.
            projectId: "team-sync-public-db" 
        };

        // --- GLOBAL VARIABLES & DOM ELEMENTS ---
        let db;
        // Generate a random ID to act as a session/user ID for tracking status
        const globalUserId = 'user_' + Math.random().toString(36).substring(2, 9);
        let globalDisplayName = localStorage.getItem('displayName') || 'Anonymous User';
        let userStatusCollection, contentListCollection;

        const nameInput = document.getElementById('nameInput');
        const setNameBtn = document.getElementById('setNameBtn');
        const statusBtns = document.querySelectorAll('.status-btn');
        const teamStatusList = document.getElementById('teamStatusList');
        const addContentForm = document.getElementById('addContentForm');
        const contentList = document.getElementById('contentList');
        const contentType = document.getElementById('contentType');
        const contentTitle = document.getElementById('contentTitle');
        const contentDescription = document.getElementById('contentDescription');
        const messageArea = document.getElementById('messageArea');
        const loadingArea = document.getElementById('loadingArea');
        const statusLoading = document.getElementById('statusLoading');
        const contentLoading = document.getElementById('contentLoading');
        const tabBtns = document.querySelectorAll('.tab-btn');
        const tabContents = document.querySelectorAll('.tab-content');

        // --- IMPORT FIREBASE MODULES (Only what we need) ---
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getFirestore, 
            doc, 
            setDoc, 
            onSnapshot, 
            collection, 
            addDoc, 
            updateDoc, 
            deleteDoc, 
            serverTimestamp
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";


        // --- UTILITY FUNCTIONS ---
        function showMessage(msg, isError = true) {
            console.log(isError ? "Error:" : "Message:", msg);
            messageArea.textContent = msg;
            messageArea.className = `text-center p-4 my-4 rounded-lg shadow ${isError ? 'bg-red-800 text-red-100' : 'bg-green-800 text-green-100'}`;
            messageArea.style.display = 'block';
            setTimeout(() => { messageArea.style.display = 'none'; }, 3000);
        }
        
        // Helper to format timestamps
        function formatTimestamp(timestamp) {
            if (!timestamp) return 'Just now';
            if (typeof timestamp.toDate === 'function') {
                return new Date(timestamp.toDate()).toLocaleString();
            }
            return new Date().toLocaleString(); 
        }

        // --- TAB NAVIGATION ---
        tabBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                tabBtns.forEach(b => b.classList.replace('active-tab', 'inactive-tab'));
                btn.classList.replace('inactive-tab', 'active-tab');
                
                document.querySelectorAll('.tab-btn').forEach(button => {
                    if (button.classList.contains('active-tab')) {
                        button.classList.remove('text-gray-400', 'border-transparent', 'hover:text-gray-200', 'hover:border-gray-500');
                        button.classList.add('text-indigo-400', 'border-indigo-400');
                    } else {
                        button.classList.remove('text-indigo-400', 'border-indigo-400');
                        button.classList.add('text-gray-400', 'border-transparent', 'hover:text-gray-200', 'hover:border-gray-500');
                    }
                });

                const tab = btn.dataset.tab;
                tabContents.forEach(content => {
                    if (content.id === tab) {
                        content.classList.add('active');
                    } else {
                        content.classList.remove('active');
                    }
                });
            });
        });

        // --- FIREBASE INITIALIZATION ---
        async function initFirebase() {
            try {
                // Initialize Firebase with the minimal public config
                const app = initializeApp(PUBLIC_FIREBASE_CONFIG);
                db = getFirestore(app);

                // Define collection paths in a simple, globally shared location
                // All users share the same two collections: user-statuses and content-list
                userStatusCollection = collection(db, 'public-data', 'team-sync', 'user-statuses');
                contentListCollection = collection(db, 'public-data', 'team-sync', 'content-list');
                
                // Set initial UI state
                nameInput.value = globalDisplayName;
                loadingArea.style.display = 'none';

                // Setup real-time listeners
                setupStatusListener();
                setupContentListener();
                
            } catch (err) {
                console.error("Firebase Init Error:", err);
                showMessage("Failed to initialize database. Check console for details.");
                loadingArea.textContent = "Database Error.";
            }
        }
        
        // --- A) AVAILABILITY LOGIC ---

        // Handle clicking "Set Name"
        setNameBtn.addEventListener('click', () => {
            const newName = nameInput.value.trim();
            if (!newName) {
                nameInput.value = 'Anonymous User';
                globalDisplayName = 'Anonymous User';
            } else {
                globalDisplayName = newName;
            }
            // Save the name locally for persistence between reloads
            localStorage.setItem('displayName', globalDisplayName);
            showMessage("Name updated! Now set your status.", false);
        });

        // Handle clicking a status button
        statusBtns.forEach(btn => {
            btn.addEventListener('click', async () => {
                const status = btn.dataset.status;
                
                // Use the random session ID for the document ID
                const userDocRef = doc(userStatusCollection, globalUserId);
                
                try {
                    await setDoc(userDocRef, {
                        status: status,
                        lastUpdated: serverTimestamp(),
                        displayName: globalDisplayName, // Use the current local name
                        userId: globalUserId 
                    }, { merge: true }); // Use merge to keep other fields if any
                    updateStatusButtonUI(status);
                    showMessage(`Status set to ${status}!`, false);
                } catch (err) {
                    console.error("Error setting status:", err);
                    showMessage("Failed to update status.");
                }
            });
        });
        
        // Visually update which status button is active
        function updateStatusButtonUI(activeStatus) {
            statusBtns.forEach(btn => {
                if (btn.dataset.status === activeStatus) {
                    btn.classList.add('ring-4', 'ring-offset-2', 'ring-offset-gray-800', 'ring-white');
                } else {
                    btn.classList.remove('ring-4', 'ring-offset-2', 'ring-offset-gray-800', 'ring-white');
                }
            });
        }

        // Listen for real-time changes to *all* user statuses
        function setupStatusListener() {
            onSnapshot(userStatusCollection, (snapshot) => {
                statusLoading.style.display = 'none';
                teamStatusList.innerHTML = ''; // Clear list
                if (snapshot.empty) {
                    teamStatusList.innerHTML = '<p class="text-gray-400">No team members have set a status yet. Set yours above!</p>';
                    return;
                }
                
                snapshot.docs
                    .sort((a, b) => { // Sort to put current user first
                        if (a.id === globalUserId) return -1;
                        if (b.id === globalUserId) return 1;
                        return 0;
                    })
                    .forEach(doc => {
                        const user = doc.data();
                        const statusColorClass = {
                            green: 'bg-green-600',
                            orange: 'bg-orange-500',
                            red: 'bg-red-600'
                        }[user.status] || 'bg-gray-600';

                        const html = `
                            <div class="flex items-center bg-gray-700 p-3 rounded-lg shadow-md">
                                <span class="flex-shrink-0 w-4 h-4 rounded-full ${statusColorClass} mr-3 border-2 border-gray-900"></span>
                                <div class="flex-1 overflow-hidden">
                                    <span class="font-semibold text-white truncate">${user.displayName || 'Anonymous'}</span>
                                    ${doc.id === globalUserId ? '<span class="text-xs text-indigo-300 ml-2">(You)</span>' : ''}
                                </div>
                                <span class="text-xs text-gray-400 ml-2">${formatTimestamp(user.lastUpdated)}</span>
                            </div>
                        `;
                        teamStatusList.innerHTML += html;
                    });
            }, (err) => {
                console.error("Status listener error:", err);
                showMessage("Error loading team statuses.");
                statusLoading.textContent = "Error loading statuses.";
            });
        }

        // --- B) CONTENT LIST LOGIC ---

        // Handle form submission to add a new content item
        addContentForm.addEventListener('submit', async (e) => {
            e.preventDefault();

            const title = contentTitle.value.trim();
            const type = contentType.value;
            const description = contentDescription.value.trim();

            if (!title) return showMessage("Please enter a title.");

            try {
                await addDoc(contentListCollection, {
                    title: title,
                    type: type,
                    description: description,
                    isDone: false,
                    addedByName: globalDisplayName,
                    createdAt: serverTimestamp()
                });
                addContentForm.reset();
                showMessage("Item added!", false);
            } catch (err) {
                console.error("Error adding content:", err);
                showMessage("Failed to add content item.");
            }
        });

        // Listen for real-time changes to the content list
        function setupContentListener() {
            onSnapshot(contentListCollection, (snapshot) => {
                contentLoading.style.display = 'none';
                contentList.innerHTML = ''; // Clear list
                if (snapshot.empty) {
                    contentList.innerHTML = '<p class="text-gray-400">No content items added yet.</p>';
                    return;
                }

                // Sort by creation time (newest first)
                snapshot.docs
                    .sort((a, b) => (b.data().createdAt?.seconds || 0) - (a.data().createdAt?.seconds || 0))
                    .forEach(doc => {
                        const item = doc.data();
                        const id = doc.id;
                        const isDoneClass = item.isDone ? 'line-through text-gray-400' : 'text-white';
                        const doneButtonText = item.isDone ? 'Mark as Not Done' : 'Mark as Done';

                        const html = `
                            <div class="content-item bg-gray-700 p-4 rounded-lg shadow-md" data-id="${id}">
                                <div class="flex justify-between items-start mb-2">
                                    <div>
                                        <span class="text-xs font-semibold uppercase text-indigo-300">${item.type}</span>
                                        <h3 class="text-lg font-semibold ${isDoneClass}">${item.title}</h3>
                                    </div>
                                    <button class="delete-btn flex-shrink-0 text-gray-400 hover:text-red-400 ml-2 transition duration-150" title="Delete">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
                                    </button>
                                </div>
                                <p class="text-sm text-gray-300 mb-3 ${isDoneClass}">${item.description || 'No description.'}</p>
                                <div class="flex justify-between items-center border-t border-gray-600 pt-3 mt-3">
                                    <span class="text-xs text-gray-400">Added by: ${item.addedByName || 'Anonymous'}</span>
                                    <button class="toggle-btn text-sm font-medium ${item.isDone ? 'text-yellow-400 hover:text-yellow-300' : 'text-green-400 hover:text-green-300'} transition duration-150">
                                        ${doneButtonText}
                                    </button>
                                </div>
                            </div>
                        `;
                        contentList.innerHTML += html;
                    });
            }, (err) => {
                console.error("Content listener error:", err);
                showMessage("Error loading content list.");
                contentLoading.textContent = "Error loading content.";
            });
        }
        
        // Handle clicks on "Delete" or "Mark as Done" using event delegation
        contentList.addEventListener('click', async (e) => {
            const itemEl = e.target.closest('.content-item');
            if (!itemEl) return;
            const docId = itemEl.dataset.id;
            const itemDocRef = doc(contentListCollection, docId);

            try {
                // Handle "Toggle Done" button
                if (e.target.classList.contains('toggle-btn')) {
                    const docSnap = await getDoc(itemDocRef);
                    if (docSnap.exists()) {
                        const currentStatus = docSnap.data().isDone;
                        await updateDoc(itemDocRef, { isDone: !currentStatus });
                        showMessage(`Item marked as ${!currentStatus ? 'done' : 'not done'}!`, false);
                    }
                }

                // Handle "Delete" button
                if (e.target.classList.contains('delete-btn') || e.target.closest('.delete-btn')) {
                    await deleteDoc(itemDocRef);
                    showMessage("Item deleted!", false);
                }
            } catch (err) {
                console.error("Error updating/deleting item:", err);
                showMessage("Failed to update item.");
            }
        });

        // --- START THE APP ---
        document.addEventListener('DOMContentLoaded', () => {
             // Load the name from local storage on startup
             nameInput.value = localStorage.getItem('displayName') || 'Anonymous User';
             initFirebase();
        });

    </script>
</body>
</html>
