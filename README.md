# MutualPlay
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MutuaPlay • Listas de YouTube • Apoyo Mutuo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css">
    <script src="https://www.youtube.com/iframe_api"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Space+Grotesk:wght@500;600;700&display=swap');
        body { font-family: 'Inter', system_ui, sans-serif; }
        .logo-font { font-family: 'Space Grotesk', sans-serif; }
        .embed-container {
            position: relative;
            padding-bottom: 56.25%;
            background: #000;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(255, 0, 0, 0.4);
        }
        .embed-container iframe {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            border: 0;
        }
    </style>
</head>
<body class="bg-black text-white">

    <!-- NAVBAR (sin perfil ni Escucha al Creador - ya están en Géneros) -->
    <nav class="bg-black/90 backdrop-blur-lg border-b border-white/10 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-6 py-5 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-red-600 rounded-3xl flex items-center justify-center text-white text-3xl">▶</div>
                <span class="logo-font text-4xl tracking-[-2px]">MutuaPlay</span>
                <span class="text-red-500 font-medium text-xl">YouTube Edition</span>
            </div>

            <div class="hidden md:flex items-center gap-8 font-medium">
                <a onclick="showGenres()" id="navGenres" class="px-6 py-3 bg-white/10 hover:bg-white/20 rounded-3xl border border-white/20 hover:border-red-500 text-sm font-medium transition-colors text-center min-w-[130px]">Géneros</a>
                <a onclick="showGlobalPlaylist()" id="navGlobal" class="px-6 py-3 bg-white/10 hover:bg-white/20 rounded-3xl border border-white/20 hover:border-red-500 text-sm font-medium transition-colors text-center min-w-[130px]">Playlist Global</a>
                <a onclick="showDashboard()" id="navDashboard" class="px-6 py-3 bg-white/10 hover:bg-white/20 rounded-3xl border border-white/20 hover:border-red-500 text-sm font-medium transition-colors text-center min-w-[130px]">Mi Dashboard</a>
                <a onclick="showAdminPanel()" id="adminLink" class="hidden px-6 py-3 bg-white/10 hover:bg-white/20 rounded-3xl border border-white/20 hover:border-red-500 text-sm font-medium transition-colors text-center min-w-[130px] text-red-400 font-bold">🔥 ADMIN</a>
                <button onclick="showChangeRequests()" id="requestsBtn" class="hidden px-6 py-3 bg-yellow-600 hover:bg-yellow-700 text-white rounded-3xl font-semibold text-sm min-w-[130px]">Solicitudes</button>
                <a onclick="showAboutPage()" id="navAbout" class="px-6 py-3 bg-white/10 hover:bg-white/20 rounded-3xl border border-white/20 hover:border-red-500 text-sm font-medium transition-colors text-center min-w-[130px]">Acerca de la página</a>
            </div>

            <div class="flex items-center gap-8">
                <button onclick="showLoginModal()" id="loginBtn" class="bg-white/10 hover:bg-white/20 px-7 py-3.5 rounded-3xl font-semibold text-sm">Iniciar sesión</button>
                <button onclick="showUploadModal()" id="uploadBtn" class="hidden bg-red-600 hover:bg-red-700 text-white px-7 py-3.5 rounded-3xl font-semibold text-sm">Subir video a lista</button>
                <button onclick="logout()" id="logoutBtn" class="hidden bg-white/10 hover:bg-white/20 px-7 py-3.5 rounded-3xl font-semibold text-sm">Cerrar sesión</button>

                <select id="languageSelector" onchange="changeLanguage(this.value)" class="bg-black border border-white/30 rounded-3xl px-5 py-3.5 text-white text-sm cursor-pointer">
                    <option value="es">🇪🇸 Español</option>
                    <option value="en">🇬🇧 English</option>
                </select>
            </div>
        </div>
    </nav>

    <!-- GÉNEROS -->
    <section id="genresSection" class="py-16">
        <div class="max-w-7xl mx-auto px-6">
            <h2 id="genresTitle" class="text-4xl font-bold text-center mb-8">Elige tu género</h2>
            
            <!-- CONTROLES SOLO PARA ADMIN -->
            <div id="adminGenreControls" class="hidden text-center mb-6">
                <button onclick="addNewGenre()" class="bg-red-600 hover:bg-red-700 text-white px-6 py-3 rounded-3xl font-semibold flex items-center gap-2 mx-auto">
                    <i class="fas fa-plus"></i> AGREGAR NUEVO GÉNERO
                </button>
                <p class="text-xs text-gray-400 mt-2">Solo el administrador puede agregar géneros y cambiar imágenes</p>
            </div>
            
            <!-- PERFIL + BUSCADOR + BOTÓN ESCUCHA AL CREADOR (MISMA ALTURA) -->
            <div class="flex items-center justify-between gap-6 mb-8">
                <!-- PERFIL IZQUIERDA -->
                <div id="leftProfileArea" class="w-64"></div>
                
                <!-- BUSCADOR CENTRO -->
                <div class="flex-1 max-w-md">
                    <input id="genreSearch" type="text" placeholder="Buscar género..." 
                           class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white placeholder-gray-400 focus:outline-none focus:border-red-500">
                </div>
                
                <!-- BOTÓN DERECHA -->
                <div class="w-64 flex justify-end">
                    <button onclick="showCreatorPlaylist()" class="bg-red-600 hover:bg-red-700 text-white px-8 py-4 rounded-3xl font-bold flex items-center gap-3">
                        <i class="fas fa-user-circle text-2xl"></i> 
                        <span>ESCUCHA AL CREADOR</span>
                    </button>
                </div>
            </div>
            
            <div id="genresGrid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-6"></div>
        </div>
    </section>

    <!-- PLAYLIST POR GÉNERO -->
    <section id="genrePlaylistSection" class="py-16 hidden">
        <div class="max-w-7xl mx-auto px-6">
            <button onclick="showGenres()" class="mb-6 text-red-500 flex items-center gap-2">← Volver a géneros</button>
            <h2 id="currentGenreTitle" class="text-4xl font-bold mb-8"></h2>
            
            <button onclick="reproducirTodoGenero()" id="playAllGenreBtn" class="mb-8 bg-red-600 text-white px-8 py-4 rounded-3xl font-bold flex items-center gap-3">
                <i class="fas fa-play-circle"></i> REPRODUCIR TODO EL GÉNERO 
                <span id="genreCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="genreCountNum">0</span> veces</span>
            </button>
            
            <div id="genreSongsList" class="space-y-8"></div>
        </div>
    </section>

    <!-- PLAYLIST GLOBAL -->
    <section id="globalPlaylistSection" class="py-16 hidden bg-zinc-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 id="globalTitle" class="text-4xl font-bold mb-8 flex items-center gap-3"><i class="fas fa-globe"></i> PLAYLIST GLOBAL DE YOUTUBE</h2>
            
            <button onclick="reproducirTodoGlobal()" id="playAllGlobalBtn" class="mb-8 bg-red-600 text-white px-8 py-4 rounded-3xl font-bold flex items-center gap-3">
                <i class="fas fa-play-circle"></i> REPRODUCIR TODO 
                <span id="globalCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="globalCountNum">0</span> veces</span>
            </button>
            
            <div id="globalSongsList" class="space-y-8"></div>
        </div>
    </section>

    <!-- PLAYLIST ESCUCHA AL CREADOR -->
    <section id="creatorPlaylistSection" class="py-16 hidden bg-zinc-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 id="creatorTitle" class="text-4xl font-bold mb-8 flex items-center gap-3"><i class="fas fa-user-circle"></i> ESCUCHA AL CREADOR DE LA PÁGINA</h2>
            
            <button onclick="reproducirTodoCreator()" id="playAllCreatorBtn" class="mb-8 bg-red-600 text-white px-8 py-4 rounded-3xl font-bold flex items-center gap-3">
                <i class="fas fa-play-circle"></i> REPRODUCIR TODO 
                <span id="creatorCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="creatorCountNum">0</span> veces</span>
            </button>
            
            <div id="creatorSongsList" class="space-y-8"></div>
        </div>
    </section>

    <!-- ACERCA DE LA PÁGINA -->
    <section id="aboutSection" class="py-16 hidden bg-zinc-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 id="aboutTitle" class="text-4xl font-bold mb-8 text-center">Acerca de la página</h2>
            <div class="max-w-3xl mx-auto text-center">
                <p id="aboutDesc" class="text-gray-400 text-lg mb-12"></p>
                
                <div id="socialLinksContainer" class="flex flex-col gap-6 items-center">
                </div>

                <div id="communityPostBox" class="mt-12 bg-zinc-900 rounded-3xl p-6 max-w-2xl mx-auto">
                    <div class="mb-4">
                        <select id="postType" class="bg-black border border-white/30 rounded-3xl px-6 py-3 text-white w-full">
                            <option value="text">Solo texto</option>
                            <option value="image">Imagen</option>
                            <option value="video">Video de YouTube</option>
                        </select>
                    </div>
                    
                    <textarea id="newPostText" rows="3" placeholder="Escribe tu publicación..." class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white placeholder-gray-400"></textarea>
                    
                    <div id="imageUploadDiv" class="hidden mt-4">
                        <input type="file" id="postImageInput" accept="image/*" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white">
                    </div>
                    
                    <div id="videoUploadDiv" class="hidden mt-4">
                        <input id="postVideoLink" type="text" placeholder="Pega el link completo de YouTube" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white placeholder-gray-400">
                    </div>
                    
                    <div class="flex gap-3 mt-4">
                        <button onclick="createNewPost()" class="flex-1 bg-red-600 hover:bg-red-700 text-white px-8 py-3 rounded-3xl font-semibold">Publicar</button>
                        <button onclick="deleteAllPosts()" class="bg-red-800 hover:bg-red-900 text-white px-6 py-3 rounded-3xl font-semibold text-sm">Eliminar TODAS</button>
                    </div>
                </div>

                <div class="max-w-2xl mx-auto mt-6">
                    <div class="max-h-[600px] overflow-y-auto border border-white/20 rounded-3xl p-4 bg-zinc-900">
                        <div id="postsContainer" class="space-y-8"></div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- COMUNIDAD -->
    <section id="communitySection" class="py-16 hidden bg-zinc-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 class="text-4xl font-bold mb-8 text-center">Comunidad</h2>
            <div id="postsContainerCommunity" class="space-y-8 max-w-2xl mx-auto"></div>
        </div>
    </section>

    <!-- MODAL REPRODUCCIÓN EN CADENA -->
    <div onclick="if(event.target.id === 'chainModal') cerrarChainModal()" id="chainModal" class="hidden fixed inset-0 bg-black/95 flex items-center justify-center z-[10000]">
        <div class="bg-zinc-900 rounded-3xl w-full max-w-5xl mx-4 p-8 text-center">
            <h3 id="chainTitle" class="text-3xl font-bold mb-4">Reproduciendo en cadena</h3>
            <div id="chainPlayer" class="embed-container mb-8"></div>
            <div class="flex justify-between items-center text-lg mb-6 px-8">
                <span id="currentVideoInfo" class="font-medium"></span>
                <span id="progressText" class="text-red-500 font-bold">1 de X</span>
            </div>
            <div id="chainStatus" class="text-2xl font-bold text-green-500 mb-8"></div>
            <button onclick="cerrarChainModal()" class="px-12 py-4 bg-white/10 text-white rounded-3xl font-medium">Detener reproducción</button>
        </div>
    </div>

    <!-- DASHBOARD -->
    <section id="dashboardSection" class="py-16 hidden bg-zinc-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 id="dashboardTitle" class="text-4xl font-bold mb-8">Mi Dashboard</h2>
            <div id="dashboardUserName" class="text-2xl font-semibold mb-8 text-red-400"></div>
            
            <div class="grid md:grid-cols-1 gap-6 mb-12">
                <div class="bg-zinc-900 rounded-3xl p-8 text-center">
                    <div class="text-red-500 text-sm font-medium">Videos subidos</div>
                    <div id="statsSongs" class="text-7xl font-bold text-red-500">0</div>
                </div>
            </div>

            <h3 class="text-xl font-semibold mb-6">Calendario semanal</h3>
            
            <div class="flex items-center justify-between mb-4 bg-zinc-900 rounded-3xl px-6 py-3">
                <button onclick="changeCalendarWeek(-1)" class="flex items-center gap-2 text-red-400 hover:text-red-500 font-medium">
                    <i class="fas fa-chevron-left"></i> Semana anterior
                </button>
                <button onclick="goToCurrentWeek()" class="text-white font-semibold flex items-center gap-1 hover:text-red-400">
                    <i class="fas fa-calendar-day"></i> HOY
                </button>
                <button onclick="changeCalendarWeek(1)" class="flex items-center gap-2 text-red-400 hover:text-red-500 font-medium">
                    Semana siguiente <i class="fas fa-chevron-right"></i>
                </button>
            </div>

            <div id="userCalendar" class="grid grid-cols-7 gap-3 bg-zinc-900 rounded-3xl p-6"></div>

            <h3 id="myPublishedTitle" class="text-xl font-semibold mb-6 mt-12">Mis videos publicados</h3>
            <div id="mySongsList" class="grid md:grid-cols-2 gap-6"></div>
        </div>
    </section>

    <!-- ADMIN PANEL -->
    <section id="adminSection" class="py-16 hidden bg-red-950">
        <div class="max-w-7xl mx-auto px-6">
            <h2 class="text-4xl font-bold mb-8 text-red-400">🔥 PANEL DE ADMINISTRADOR</h2>
            
            <div class="bg-zinc-900 rounded-3xl p-8 mb-12">
                <h3 class="text-2xl font-bold mb-6 flex items-center gap-2"><i class="fas fa-toggle-on"></i> Configuración de Playlists</h3>
                
                <div class="flex flex-col gap-6">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <i class="fas fa-globe text-xl"></i>
                            <span class="font-medium">Playlist Global</span>
                        </div>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="toggleGlobal" checked onchange="togglePlaylist('global')" class="sr-only peer">
                            <div class="w-11 h-6 bg-gray-700 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-red-300 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-red-600"></div>
                        </label>
                    </div>

                    <div class="flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <i class="fas fa-sun text-xl"></i>
                            <span class="font-medium">Playlist del Día</span>
                        </div>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="toggleDaily" checked onchange="togglePlaylist('daily')" class="sr-only peer">
                            <div class="w-11 h-6 bg-gray-700 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-red-300 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-red-600"></div>
                        </label>
                    </div>

                    <div>
                        <label class="text-xs uppercase block mb-2">Género para Playlist del Día</label>
                        <select id="dailyGenreSelect" onchange="saveDailyGenre()" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4"></select>
                    </div>
                </div>
            </div>

            <div class="bg-zinc-900 rounded-3xl p-8 mb-12">
                <h3 class="text-2xl font-bold mb-4 flex items-center gap-2"><i class="fas fa-user-circle"></i> Subir canción a "Escucha al Creador de la Página"</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="text-xs uppercase">Título del video</label>
                        <input id="creatorUploadTitle" type="text" placeholder="Nombre del video" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                    <div>
                        <label class="text-xs uppercase">Link completo de YouTube</label>
                        <input id="creatorUploadLink" type="text" placeholder="https://youtube.com/watch?v=..." class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                    <div class="flex items-end">
                        <button onclick="uploadToCreatorPlaylist()" class="w-full bg-red-600 hover:bg-red-700 text-white py-4 rounded-3xl font-bold">SUBIR A LISTA DEL CREADOR</button>
                    </div>
                </div>
                <p class="text-xs text-gray-400 mt-3">Estas canciones solo aparecerán en la lista "Escucha al Creador de la Página" y NO en la Playlist Global ni en géneros.</p>
            </div>

            <div class="bg-zinc-900 rounded-3xl p-8 mb-12">
                <h3 class="text-2xl font-bold mb-6 flex items-center gap-2"><i class="fas fa-share-alt"></i> Gestionar enlaces de redes sociales</h3>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
                    <div>
                        <label class="text-xs uppercase">Nombre de la red</label>
                        <input id="newSocialName" type="text" placeholder="Instagram" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                    <div>
                        <label class="text-xs uppercase">Icono Font Awesome</label>
                        <input id="newSocialIcon" type="text" placeholder="fab fa-instagram" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                    <div>
                        <label class="text-xs uppercase">Color del icono</label>
                        <input id="newSocialColor" type="text" placeholder="text-pink-500" value="text-white" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                    <div>
                        <label class="text-xs uppercase">Link completo</label>
                        <input id="newSocialUrl" type="text" placeholder="https://instagram.com/..." class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    </div>
                </div>
                <button onclick="addSocialLink()" class="bg-red-600 hover:bg-red-700 text-white px-8 py-4 rounded-3xl font-bold w-full md:w-auto">AGREGAR NUEVO ENLACE DE RED SOCIAL</button>

                <div id="currentSocialLinksList" class="mt-10 space-y-4"></div>
            </div>

            <div class="mb-8 flex gap-3">
                <input id="adminSearch" type="text" placeholder="Buscar usuario por correo..." 
                       class="flex-1 bg-black border border-white/30 rounded-3xl px-6 py-4 text-white placeholder-gray-400 focus:outline-none focus:border-red-500"
                       onkeyup="filterAdminUsers()">
                <button onclick="filterAdminUsers()" 
                        class="bg-red-600 hover:bg-red-700 text-white px-8 py-4 rounded-3xl font-semibold flex items-center gap-2">
                    <i class="fas fa-search"></i> Buscar
                </button>
            </div>

            <div class="mb-8 flex gap-3">
                <div class="flex-1">
                    <label class="text-xs uppercase block mb-2 text-red-400">Número mínimo de casillas activas</label>
                    <input id="minCasillasActivas" type="number" placeholder="0" min="0"
                           class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white placeholder-gray-400 focus:outline-none focus:border-red-500"
                           onkeyup="filterAdminUsers()">
                </div>
                <button onclick="filterAdminUsers()" 
                        class="bg-red-600 hover:bg-red-700 text-white px-8 py-4 rounded-3xl font-semibold flex items-center gap-2 self-end">
                    <i class="fas fa-filter"></i> Filtrar
                </button>
            </div>

            <div id="adminUsersList" class="max-h-[700px] overflow-y-auto space-y-8 pr-4 rounded-3xl bg-zinc-900/50 p-6"></div>

            <div class="mt-12 bg-zinc-900 rounded-3xl p-8">
                <h3 class="text-2xl font-bold mb-4">Editar texto de "Acerca de la página"</h3>
                <textarea id="aboutEditText" rows="6" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-white"></textarea>
                <button onclick="saveAboutText()" class="mt-4 bg-red-600 hover:bg-red-700 text-white px-8 py-3 rounded-3xl font-semibold">Guardar cambios en Acerca de la página</button>
            </div>
        </div>
    </section>

    <!-- LOGIN / CREAR CUENTA MODAL -->
    <div onclick="if(event.target.id === 'loginModal') hideLoginModal()" id="loginModal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-[9999]">
        <div class="bg-zinc-900 rounded-3xl w-full max-w-md mx-4 p-8">
            <h3 id="loginModalTitle" class="text-3xl font-bold mb-8 text-center">Iniciar sesión</h3>
            <div class="space-y-6">
                <div class="text-center text-xs uppercase tracking-widest text-gray-400 my-4">Solo usuario y contraseña</div>
                <div id="loginForm">
                    <input id="loginUsername" type="text" placeholder="Nombre de usuario (máx. 10 caracteres)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4">
                    <div class="relative">
                        <input id="loginPass" type="password" placeholder="Contraseña (máx. 10 caracteres)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4">
                        <button onclick="togglePasswordVisibility('loginPass')" class="absolute right-6 top-4 text-gray-400"><i class="fas fa-eye" id="loginEye"></i></button>
                    </div>
                    <button onclick="emailLogin()" id="loginButton" class="w-full bg-red-600 text-white py-5 rounded-3xl font-bold text-lg">INICIAR SESIÓN</button>
                </div>
                <div id="registerForm" class="hidden">
                    <input id="registerUsername" type="text" maxlength="10" placeholder="Nombre de usuario (máx. 10 caracteres)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4">
                    <input id="registerName" type="text" placeholder="Nombre (solo letras)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4" oninput="validateNameInput(this)">
                    <input id="registerApellido" type="text" placeholder="Apellido (solo letras)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4" oninput="validateNameInput(this)">
                    
                    <div class="mb-4">
                        <label class="text-xs uppercase block mb-1">Edad (18 a 100)</label>
                        <div class="flex items-center gap-2">
                            <button onclick="changeAge(-1)" class="px-4 py-3 bg-white/10 hover:bg-white/20 rounded-3xl text-xl font-bold">-</button>
                            <input id="registerEdad" type="number" value="18" min="18" max="100" class="flex-1 bg-black border border-white/30 rounded-3xl px-6 py-4 text-center text-white" oninput="validateAgeInput(this)">
                            <button onclick="changeAge(1)" class="px-4 py-3 bg-white/10 hover:bg-white/20 rounded-3xl text-xl font-bold">+</button>
                        </div>
                    </div>
                    
                    <select id="registerNacionalidad" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4">
                        <option value="">Selecciona tu nacionalidad</option>
                        <option value="México">🇲🇽 México</option>
                        <option value="Argentina">🇦🇷 Argentina</option>
                        <option value="Colombia">🇨🇴 Colombia</option>
                        <option value="España">🇪🇸 España</option>
                        <option value="Estados Unidos">🇺🇸 Estados Unidos</option>
                        <option value="Brasil">🇧🇷 Brasil</option>
                        <option value="Perú">🇵🇪 Perú</option>
                        <option value="Chile">🇨🇱 Chile</option>
                        <option value="Venezuela">🇻🇪 Venezuela</option>
                        <option value="Ecuador">🇪🇨 Ecuador</option>
                        <option value="Bolivia">🇧🇴 Bolivia</option>
                        <option value="Francia">🇫🇷 Francia</option>
                        <option value="Alemania">🇩🇪 Alemania</option>
                        <option value="Italia">🇮🇹 Italia</option>
                        <option value="Reino Unido">🇬🇧 Reino Unido</option>
                        <option value="China">🇨🇳 China</option>
                        <option value="India">🇮🇳 India</option>
                        <option value="Japón">🇯🇵 Japón</option>
                        <option value="Corea del Sur">🇰🇷 Corea del Sur</option>
                        <option value="Rusia">🇷🇺 Rusia</option>
                        <option value="Canadá">🇨🇦 Canadá</option>
                        <option value="Australia">🇦🇺 Australia</option>
                        <option value="Sudáfrica">🇿🇦 Sudáfrica</option>
                        <option value="Nigeria">🇳🇬 Nigeria</option>
                        <option value="Egipto">🇪🇬 Egipto</option>
                    </select>
                    <div class="relative">
                        <input id="registerPass" type="password" maxlength="10" placeholder="Contraseña (máx. 10 caracteres)" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-4">
                        <button onclick="togglePasswordVisibility('registerPass')" class="absolute right-6 top-4 text-gray-400"><i class="fas fa-eye" id="registerEye"></i></button>
                    </div>
                    <button onclick="registerAccount()" id="registerButton" class="w-full bg-red-600 text-white py-5 rounded-3xl font-bold text-lg">CREAR CUENTA</button>
                </div>
                <div class="text-center">
                    <button onclick="toggleLoginRegister()" id="toggleButton" class="text-red-500 hover:underline text-sm">
                        ¿No tienes cuenta? Crear cuenta
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL OLVIDÉ CONTRASEÑA -->
    <div onclick="if(event.target.id === 'forgotModal') hideForgotModal()" id="forgotModal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-[100000]">
        <div class="bg-zinc-900 rounded-3xl w-full max-w-md mx-4 p-8">
            <h3 class="text-3xl font-bold mb-6 text-center">¿Olvidaste tu contraseña?</h3>
            <p class="text-center text-gray-400 mb-6">Ingresa tu correo y te enviaremos un enlace de recuperación a synthetic036@gmail.com</p>
            <input id="forgotEmail" type="email" placeholder="Tu correo" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 mb-6">
            <button onclick="sendPasswordReset()" class="w-full bg-red-600 text-white py-5 rounded-3xl font-bold">Enviar enlace de recuperación</button>
            <button onclick="hideForgotModal()" class="w-full text-gray-400 mt-4">Cancelar</button>
        </div>
    </div>

    <!-- SUBIR VIDEO MODAL -->
    <div onclick="if(event.target.id === 'uploadModal') hideUploadModal()" id="uploadModal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-[9999]">
        <div class="bg-zinc-900 rounded-3xl w-full max-w-lg mx-4 p-8 relative">
            <button onclick="hideUploadModal()" class="absolute top-6 right-6 text-4xl text-gray-400 hover:text-white">✕</button>
            <h3 id="uploadModalTitle" class="text-3xl font-bold mb-6">Subir video a la lista</h3>
            <div class="space-y-6">
                <div>
                    <label class="text-xs uppercase">Género</label>
                    <select id="uploadGenre" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4"></select>
                </div>
                <div>
                    <label class="text-xs uppercase">Título del video</label>
                    <input id="uploadTitle" type="text" placeholder="Nombre del video" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                </div>
                <div>
                    <label class="text-xs uppercase">Link completo de YouTube</label>
                    <input id="uploadLink" type="text" placeholder="https://youtube.com/watch?v=..." class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                </div>
                <button onclick="uploadSong()" id="uploadSubmitBtn" class="w-full bg-red-600 text-white py-5 rounded-3xl font-bold text-lg">SUBIR A LA PLAYLIST</button>
            </div>
        </div>
    </div>

    <!-- PERFIL MODAL -->
    <div onclick="if(event.target.id === 'profileModal') hideProfileModal()" id="profileModal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-[9999]">
        <div class="bg-zinc-900 rounded-3xl w-full max-w-md mx-4 p-8 relative">
            <button onclick="hideProfileModal()" class="absolute top-6 right-6 text-4xl text-gray-400 hover:text-white">✕</button>
            <h3 id="profileModalTitle" class="text-3xl font-bold mb-6">Mi Perfil</h3>
            <div class="flex flex-col items-center gap-6">
                <div class="relative">
                    <img id="profileAvatar" src="https://i.pravatar.cc/128?img=12" class="w-24 h-24 rounded-3xl object-cover border-4 border-red-600">
                    <label class="absolute bottom-0 right-0 bg-red-600 text-white text-xs px-3 py-1 rounded-3xl cursor-pointer hover:bg-red-700">
                        Cambiar foto
                        <input type="file" id="profilePhotoInput" accept="image/*" class="hidden" onchange="changeProfilePhoto(event)">
                    </label>
                </div>
                <input id="profileName" type="text" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-center text-xl">
                <input id="profileApellido" type="text" placeholder="Apellido" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-center">
                <input id="profileEdad" type="number" placeholder="Edad" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4 text-center">
                <select id="profileNacionalidad" class="w-full bg-black border border-white/30 rounded-3xl px-6 py-4">
                    <option value="">Selecciona tu nacionalidad</option>
                    <option value="México">🇲🇽 México</option>
                    <option value="Argentina">🇦🇷 Argentina</option>
                    <option value="Colombia">🇨🇴 Colombia</option>
                    <option value="España">🇪🇸 España</option>
                    <option value="Estados Unidos">🇺🇸 Estados Unidos</option>
                    <option value="Brasil">🇧🇷 Brasil</option>
                    <option value="Perú">🇵🇪 Perú</option>
                    <option value="Chile">🇨🇱 Chile</option>
                    <option value="Venezuela">🇻🇪 Venezuela</option>
                    <option value="Ecuador">🇪🇨 Ecuador</option>
                    <option value="Bolivia">🇧🇴 Bolivia</option>
                    <option value="Francia">🇫🇷 Francia</option>
                    <option value="Alemania">🇩🇪 Alemania</option>
                    <option value="Italia">🇮🇹 Italia</option>
                    <option value="Reino Unido">🇬🇧 Reino Unido</option>
                    <option value="China">🇨🇳 China</option>
                    <option value="India">🇮🇳 India</option>
                    <option value="Japón">🇯🇵 Japón</option>
                    <option value="Corea del Sur">🇰🇷 Corea del Sur</option>
                    <option value="Rusia">🇷🇺 Rusia</option>
                    <option value="Canadá">🇨🇦 Canadá</option>
                    <option value="Australia">🇦🇺 Australia</option>
                    <option value="Sudáfrica">🇿🇦 Sudáfrica</option>
                    <option value="Nigeria">🇳🇬 Nigeria</option>
                    <option value="Egipto">🇪🇬 Egipto</option>
                </select>
                <button onclick="saveProfile()" id="saveProfileBtn" class="w-full bg-red-600 text-white py-5 rounded-3xl font-bold">Guardar cambios</button>
            </div>
        </div>
    </div>

    <script>
        const allGenres = ["Rock","Pop","Hip Hop","Rap","Jazz","Blues","Classical","Electronic","Dance","House","Techno","Trance","Dubstep","Drum and Bass","Reggae","Ska","Soul","R&B","Funk","Disco","Country","Folk","Gospel","Opera","Metal","Punk","Alternative","Indie","Ambient","Industrial","World Music","Latin","Flamenco","Salsa","Cumbia","Merengue","Bachata","Tango","Mariachi","Banda","Norteño","Regional Mexicano","Reggaeton","Trap","K-pop","J-pop","Afrobeat","Highlife","Raï","Bollywood","Qawwali","New Age","Soundtrack","Children's Music","Christian","Instrumental","Experimental","Noise"];
        
        let currentGenres = JSON.parse(localStorage.getItem('mutuaplay_currentGenres')) || allGenres.slice();
        let genreImages = JSON.parse(localStorage.getItem('mutuaplay_genreImages')) || {};

        let genres = {};
        currentGenres.forEach(g => genres[g] = []);

        let currentUser = null;
        let isAdmin = false;
        let currentQueue = [];
        let currentQueueIndex = 0;
        let player = null;

        let globalCompletionCount = 0;
        let userGlobalCompletions = {};
        let userGenreCompletions = {};

        let registeredUsers = JSON.parse(localStorage.getItem('mutuaplay_registeredUsers')) || [];
        registeredUsers = registeredUsers.filter(u => u.email === "admin@proton.me");
        if (registeredUsers.length === 0) {
            registeredUsers = [{
                email: "admin@proton.me",
                password: "adminIOI2003",
                username: "admin@proton.me",
                name: "Administrador",
                apellido: "",
                edad: "",
                nacionalidad: "",
                isAdmin: true,
                photo: "https://i.pravatar.cc/128?img=12"
            }];
            localStorage.setItem('mutuaplay_registeredUsers', JSON.stringify(registeredUsers));
        }

        let userMaxUploads = {};
        let allSongs = [];
        let userStrikes = {};

        let creatorPlaylist = [];

        let creatorCompletionCount = parseInt(localStorage.getItem('mutuaplay_creatorCompletionCount')) || 0;
        let userCreatorCompletions = JSON.parse(localStorage.getItem('mutuaplay_userCreatorCompletions')) || {};

        let playlistJustCompleted = false;
        let pendingChangeRequests = [];

        let userGlobalDailyCompletions = JSON.parse(localStorage.getItem('mutuaplay_userGlobalDailyCompletions')) || {};

        let aboutText = `MutuaPlay es una página destinada a promocionar nuestras canciones en YouTube. Aquí nos apoyamos mutuamente entre todos los creadores para alcanzar más visualizaciones y conseguir una mayor audiencia. Cada usuario sube sus videos a listas por género y a la playlist global. Al escuchar completa la lista de otro, ayudamos a que su música crezca, y a cambio recibimos el mismo apoyo. Juntos crecemos más rápido.`;

        let socialLinks = JSON.parse(localStorage.getItem('mutuaplay_socialLinks')) || [
            { id: 1, name: "YouTube", icon: "fab fa-youtube", color: "text-red-600", url: "https://www.youtube.com/@Angel_DiazC" },
            { id: 2, name: "TikTok", icon: "fab fa-tiktok", color: "text-white", url: "https://www.tiktok.com/@angeldiaz_c" },
            { id: 3, name: "Spotify", icon: "fab fa-spotify", color: "text-green-500", url: "https://open.spotify.com/intl-es/artist/4aEkEMqD8J3qJ7c0YtGiuW?si=q3YsE3DKQC-QURSG91NKgg" }
        ];

        let communityPosts = [];

        let currentWeekOffset = 0;

        let globalPlaylistActive = JSON.parse(localStorage.getItem('mutuaplay_globalPlaylistActive')) || true;
        let dailyPlaylistActive = JSON.parse(localStorage.getItem('mutuaplay_dailyPlaylistActive')) || true;
        let dailySelectedGenre = localStorage.getItem('mutuaplay_dailySelectedGenre') || "Rock";

        let currentPlaylistType = null;

        const translations = {
            es: {
                genresTitle: "Elige tu género",
                globalTitle: "PLAYLIST GLOBAL DE YOUTUBE",
                creatorTitle: "ESCUCHA AL CREADOR DE LA PÁGINA",
                aboutTitle: "Acerca de la página",
                aboutDesc: aboutText,
                playAllGenre: "REPRODUCIR TODO EL GÉNERO",
                playAllGlobal: "REPRODUCIR TODO",
                playAllCreator: "REPRODUCIR TODO",
                uploadBtn: "Subir video a lista",
                requestsBtn: "Solicitudes",
                logoutBtn: "Cerrar sesión",
                loginBtn: "Iniciar sesión",
                dashboardTitle: "Mi Dashboard",
                myPublishedTitle: "Mis videos publicados",
                loginModalTitle: "Iniciar sesión",
                registerButton: "CREAR CUENTA",
                uploadModalTitle: "Subir video a la lista",
                uploadSubmitBtn: "SUBIR A LA PLAYLIST",
                profileModalTitle: "Mi Perfil",
                saveProfileBtn: "Guardar cambios",
                chainTitle: "Reproduciendo en cadena",
                communityTitle: "Comunidad",
                navGenres: "Géneros",
                navGlobal: "Playlist Global",
                navCreator: "Escucha al Creador",
                navDashboard: "Mi Dashboard",
                navAbout: "Acerca de la página",
                navCommunity: "Comunidad"
            },
            en: {
                genresTitle: "Choose your genre",
                globalTitle: "GLOBAL YOUTUBE PLAYLIST",
                creatorTitle: "LISTEN TO THE PAGE CREATOR",
                aboutTitle: "About the page",
                aboutDesc: aboutText,
                playAllGenre: "PLAY ALL GENRE",
                playAllGlobal: "PLAY ALL",
                playAllCreator: "PLAY ALL",
                uploadBtn: "Upload video to list",
                requestsBtn: "Requests",
                logoutBtn: "Log out",
                loginBtn: "Log in",
                dashboardTitle: "My Dashboard",
                myPublishedTitle: "My published videos",
                loginModalTitle: "Log in",
                registerButton: "CREATE ACCOUNT",
                uploadModalTitle: "Upload video to list",
                uploadSubmitBtn: "UPLOAD TO PLAYLIST",
                profileModalTitle: "My Profile",
                saveProfileBtn: "Save changes",
                chainTitle: "Playing in chain",
                communityTitle: "Community",
                navGenres: "Genres",
                navGlobal: "Global Playlist",
                navCreator: "Listen to Creator",
                navDashboard: "My Dashboard",
                navAbout: "About the page",
                navCommunity: "Community"
            }
        };

        let currentLang = "es";

        function changeLanguage(lang) {
            currentLang = lang;
            document.documentElement.lang = lang;

            document.getElementById('genresTitle').textContent = translations[lang].genresTitle;
            document.getElementById('globalTitle').innerHTML = `<i class="fas fa-globe"></i> ${translations[lang].globalTitle}`;
            document.getElementById('creatorTitle').innerHTML = `<i class="fas fa-user-circle"></i> ${translations[lang].creatorTitle}`;
            document.getElementById('aboutTitle').textContent = translations[lang].aboutTitle;
            document.getElementById('aboutDesc').innerHTML = translations[lang].aboutDesc;
            document.getElementById('playAllGenreBtn').innerHTML = `<i class="fas fa-play-circle"></i> ${translations[lang].playAllGenre} <span id="genreCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="genreCountNum">0</span> veces</span>`;
            document.getElementById('playAllGlobalBtn').innerHTML = `<i class="fas fa-play-circle"></i> ${translations[lang].playAllGlobal} <span id="globalCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="globalCountNum">0</span> veces</span>`;
            document.getElementById('playAllCreatorBtn').innerHTML = `<i class="fas fa-play-circle"></i> ${translations[lang].playAllCreator} <span id="creatorCounterInline" class="ml-4 bg-black/30 text-white text-sm px-4 py-1 rounded-3xl font-medium">Escuchada <span id="creatorCountNum">0</span> veces</span>`;
            document.getElementById('uploadBtn').textContent = translations[lang].uploadBtn;
            document.getElementById('requestsBtn').textContent = translations[lang].requestsBtn;
            document.getElementById('logoutBtn').textContent = translations[lang].logoutBtn;
            document.getElementById('loginBtn').textContent = translations[lang].loginBtn;
            document.getElementById('dashboardTitle').textContent = translations[lang].dashboardTitle;
            document.getElementById('myPublishedTitle').textContent = translations[lang].myPublishedTitle;
            document.getElementById('loginModalTitle').textContent = translations[lang].loginModalTitle;
            document.getElementById('registerButton').textContent = translations[lang].registerButton;
            document.getElementById('uploadModalTitle').textContent = translations[lang].uploadModalTitle;
            document.getElementById('uploadSubmitBtn').textContent = translations[lang].uploadSubmitBtn;
            document.getElementById('profileModalTitle').textContent = translations[lang].profileModalTitle;
            document.getElementById('saveProfileBtn').textContent = translations[lang].saveProfileBtn;
            document.getElementById('chainTitle').textContent = translations[lang].chainTitle;
            document.getElementById('navGenres').textContent = translations[lang].navGenres;
            document.getElementById('navGlobal').textContent = translations[lang].navGlobal;
            document.getElementById('navDashboard').textContent = translations[lang].navDashboard;
            document.getElementById('navAbout').textContent = translations[lang].navAbout;

            alert(`✅ Idioma cambiado a ${lang === 'en' ? 'English' : 'Español'}`);
        }

        function createSongCard(song) {
            const videoId = song.linkId || song.fullUrl.split('v=')[1] || song.fullUrl.split('/').pop();
            return `
            <div class="song-card bg-zinc-900 rounded-3xl p-6">
                <div class="flex justify-between mb-4">
                    <div><div class="font-bold text-2xl">${song.title}</div><div class="text-gray-400">${song.artist}</div></div>
                    <button onclick="likeSong(${song.id});event.stopImmediatePropagation()" class="text-3xl text-red-400">❤️ <span>${song.likes}</span></button>
                </div>
                <div class="embed-container mb-6">
                    <iframe src="https://www.youtube.com/embed/${videoId}" allowfullscreen></iframe>
                </div>
                <a href="https://youtube.com/watch?v=${videoId}" target="_blank" class="block w-full text-center bg-red-600 hover:bg-red-700 text-white font-bold py-4 rounded-3xl text-lg flex items-center justify-center gap-3">
                    <i class="fab fa-youtube text-2xl"></i> ABRIR EN YOUTUBE
                </a>
            </div>`;
        }

        function openGenrePlaylist(genre) {
            hideAllSections();
            const section = document.getElementById('genrePlaylistSection');
            section.classList.remove('hidden');
            document.getElementById('currentGenreTitle').innerHTML = genre;
            const container = document.getElementById('genreSongsList');
            container.innerHTML = '';
            genres[genre].forEach(song => container.innerHTML += createSongCard(song));
            const count = currentUser && userGenreCompletions[currentUser.email] && userGenreCompletions[currentUser.email][genre] ? userGenreCompletions[currentUser.email][genre] : 0;
            document.getElementById('genreCountNum').textContent = count;
        }

        function reproducirTodoGenero() {
            const genre = document.getElementById('currentGenreTitle').innerHTML;
            currentQueue = genres[genre] ? [...genres[genre]] : [];
            if (currentQueue.length === 0) return alert(currentLang === 'en' ? "No videos in this genre" : "No hay videos en este género");
            currentQueueIndex = 0;
            currentPlaylistType = 'genre';
            playlistJustCompleted = false;
            document.getElementById('chainModal').classList.remove('hidden');
            document.getElementById('chainTitle').innerHTML = `Reproduciendo en cadena: ${genre}`;
            reproducirSiguienteVideo();
        }

        function reproducirTodoGlobal() {
            if (dailyPlaylistActive) {
                reproducirTodoDaily();
                return;
            }
            currentQueue = [];
            Object.keys(genres).forEach(g => genres[g].forEach(song => currentQueue.push({...song})));
            if (currentQueue.length === 0) return alert(currentLang === 'en' ? "No videos in global playlist" : "No hay videos en la playlist global");
            currentQueueIndex = 0;
            currentPlaylistType = 'global';
            playlistJustCompleted = false;
            document.getElementById('chainModal').classList.remove('hidden');
            document.getElementById('chainTitle').innerHTML = `Reproduciendo en cadena: Playlist Global`;
            reproducirSiguienteVideo();
        }

        function reproducirTodoDaily() {
            currentQueue = genres[dailySelectedGenre] ? [...genres[dailySelectedGenre]] : [];
            if (currentQueue.length === 0) return alert(currentLang === 'en' ? "No videos in daily playlist" : "No hay videos en la playlist del día");
            currentQueueIndex = 0;
            currentPlaylistType = 'daily';
            playlistJustCompleted = false;
            document.getElementById('chainModal').classList.remove('hidden');
            document.getElementById('chainTitle').innerHTML = `Reproduciendo en cadena: Playlist del Día (${dailySelectedGenre})`;
            reproducirSiguienteVideo();
        }

        function reproducirTodoCreator() {
            currentQueue = [...creatorPlaylist];
            if (currentQueue.length === 0) return alert(currentLang === 'en' ? "No videos in creator playlist" : "No hay videos en la lista del creador");
            currentQueueIndex = 0;
            currentPlaylistType = 'creator';
            playlistJustCompleted = false;
            document.getElementById('chainModal').classList.remove('hidden');
            document.getElementById('chainTitle').innerHTML = `Reproduciendo en cadena: Escucha al Creador`;
            reproducirSiguienteVideo();
        }

        function reproducirSiguienteVideo() {
            if (currentQueueIndex >= currentQueue.length) {
                if (playlistJustCompleted) return;
                playlistJustCompleted = true;

                document.getElementById('chainStatus').innerHTML = `
                    <div class="text-6xl font-bold text-green-500">🎉 ${currentLang === 'en' ? 'THE LIST HAS FINISHED' : 'LA LISTA HA FINALIZADO'}</div>
                    <div class="text-3xl mt-4">${currentLang === 'en' ? 'Thank you for listening to the full playlist!' : '¡Gracias por escuchar completa la playlist!'}</div>
                `;

                const titleText = document.getElementById('chainTitle').innerHTML;
                const isGlobal = titleText.includes("Playlist Global");
                const isDaily = titleText.includes("Playlist del Día");
                const isCreator = titleText.includes("Escucha al Creador") || titleText.includes("Creador");

                if ((isGlobal && globalPlaylistActive) || (isDaily && dailyPlaylistActive)) {
                    globalCompletionCount++;
                    if (currentUser) userGlobalCompletions[currentUser.email] = (userGlobalCompletions[currentUser.email] || 0) + 1;

                    const today = new Date().toISOString().split('T')[0];
                    if (!userGlobalDailyCompletions[currentUser.email]) userGlobalDailyCompletions[currentUser.email] = {};
                    userGlobalDailyCompletions[currentUser.email][today] = (userGlobalDailyCompletions[currentUser.email][today] || 0) + 1;
                    localStorage.setItem('mutuaplay_userGlobalDailyCompletions', JSON.stringify(userGlobalDailyCompletions));

                    if (currentUser && !document.getElementById('dashboardSection').classList.contains('hidden')) {
                        renderWeeklyCalendar('userCalendar', currentUser.email, currentWeekOffset);
                    }
                } else if (isCreator) {
                    creatorCompletionCount++;
                    localStorage.setItem('mutuaplay_creatorCompletionCount', creatorCompletionCount);
                    if (currentUser) {
                        userCreatorCompletions[currentUser.email] = (userCreatorCompletions[currentUser.email] || 0) + 1;
                        localStorage.setItem('mutuaplay_userCreatorCompletions', JSON.stringify(userCreatorCompletions));
                    }
                } else if (currentPlaylistType === 'genre' && currentUser) {
                    const genre = document.getElementById('currentGenreTitle') ? document.getElementById('currentGenreTitle').innerHTML : null;
                    if (genre) {
                        if (!userGenreCompletions[currentUser.email]) userGenreCompletions[currentUser.email] = {};
                        userGenreCompletions[currentUser.email][genre] = (userGenreCompletions[currentUser.email][genre] || 0) + 1;
                    }
                }

                setTimeout(() => cerrarChainModal(), 3500);
                return;
            }

            const song = currentQueue[currentQueueIndex];
            const videoId = song.linkId || song.fullUrl.split('v=')[1] || song.fullUrl.split('/').pop();

            if (!player) {
                player = new YT.Player('chainPlayer', {
                    height: '100%',
                    width: '100%',
                    videoId: videoId,
                    playerVars: { 'autoplay': 1, 'controls': 1, 'rel': 0, 'modestbranding': 1 },
                    events: { 'onStateChange': onPlayerStateChange }
                });
            } else {
                player.loadVideoById(videoId);
            }

            document.getElementById('currentVideoInfo').innerHTML = `<strong>${song.title}</strong> - ${song.artist}`;
            document.getElementById('progressText').innerHTML = `${currentQueueIndex + 1} de ${currentQueue.length}`;
            document.getElementById('chainStatus').innerHTML = '🔊 Reproduciendo con sonido...';
        }

        function onPlayerStateChange(event) {
            if (event.data === YT.PlayerState.ENDED) {
                currentQueueIndex++;
                reproducirSiguienteVideo();
            }
        }

        function cerrarChainModal() {
            document.getElementById('chainModal').classList.add('hidden');
            if (player) player.stopVideo();
            currentQueue = [];
            playlistJustCompleted = false;
            currentPlaylistType = null;
        }

        function likeSong(id) { alert(currentLang === 'en' ? '❤️ Like registered' : '❤️ Like registrado'); }

        function showGenres() { 
            hideAllSections(); 
            document.getElementById('genresSection').classList.remove('hidden'); 
            
            const adminControls = document.getElementById('adminGenreControls');
            if (adminControls) {
                adminControls.style.display = (currentUser && currentUser.isAdmin) ? 'block' : 'none';
            }
            
            renderGenresGrid(); 
            renderUserProfileInGenres();
            
            const searchInput = document.getElementById('genreSearch');
            if (searchInput) {
                searchInput.oninput = function() {
                    const term = this.value.toLowerCase().trim();
                    const cards = document.querySelectorAll('#genresGrid > div');
                    cards.forEach(card => {
                        const h3 = card.querySelector('h3');
                        if (h3) {
                            const name = h3.textContent.toLowerCase();
                            card.style.display = name.includes(term) ? '' : 'none';
                        }
                    });
                };
            }
        }
        
        function renderUserProfileInGenres() {
            const container = document.getElementById('leftProfileArea');
            if (!container) return;
            
            if (!currentUser) {
                container.innerHTML = `
                    <div class="bg-zinc-900 rounded-3xl p-8 text-center cursor-pointer hover:border-red-500" onclick="showLoginModal()">
                        <div class="text-6xl mb-4">👤</div>
                        <div class="font-bold">Inicia sesión</div>
                        <div class="text-xs text-gray-400 mt-1">para ver tu perfil</div>
                    </div>
                `;
                return;
            }
            
            const photo = currentUser.photo || 'https://i.pravatar.cc/128?img=12';
            
            container.innerHTML = `
                <div onclick="showProfileModal()" class="bg-zinc-900 rounded-3xl overflow-hidden cursor-pointer hover:border-red-500 relative h-64">
                    <div class="absolute inset-0 bg-cover bg-center" style="background-image: url('${photo}')"></div>
                    <div class="absolute inset-0 bg-black/70"></div>
                    <div class="absolute bottom-0 left-0 right-0 p-6 text-center">
                        <div class="font-bold text-xl text-white drop-shadow-lg">${currentUser.name}</div>
                        <div class="text-xs text-gray-300">${currentUser.email}</div>
                    </div>
                    <div class="absolute top-4 right-4 bg-red-600 text-white text-xs px-3 py-1 rounded-3xl">Mi Perfil</div>
                </div>
            `;
        }
        
        function renderGenresGrid() {
            const container = document.getElementById('genresGrid');
            container.innerHTML = '';
            currentGenres.forEach(genre => {
                const songCount = genres[genre] ? genres[genre].length : 0;
                const imgSrc = genreImages[genre] || null;
                const card = document.createElement('div');
                card.className = `bg-zinc-900 rounded-3xl p-8 text-center cursor-pointer hover:border-red-500 relative overflow-hidden ${imgSrc ? 'h-64' : ''}`;
                
                let html = `<div class="absolute top-4 right-4 bg-red-600 text-white text-xs font-bold px-3 py-1 rounded-3xl z-10">${songCount}</div>`;
                
                if (imgSrc) {
                    html += `
                        <div class="absolute inset-0 bg-cover bg-center" style="background-image: url('${imgSrc}')"></div>
                        <div class="absolute inset-0 bg-black/70"></div>
                        <div class="relative z-10 flex flex-col items-center justify-end h-full pb-8">
                            <h3 class="font-bold text-2xl text-white drop-shadow-lg">${genre}</h3>
                        </div>
                    `;
                } else {
                    html += `
                        <div class="text-6xl mb-4">🎥</div>
                        <h3 class="font-bold text-xl">${genre}</h3>
                    `;
                }
                
                if (currentUser && currentUser.isAdmin) {
                    html += `<button onclick="changeGenreImage('${genre}', event)" class="mt-3 text-xs bg-blue-600 hover:bg-blue-700 px-4 py-1.5 rounded-3xl flex items-center gap-1 mx-auto relative z-10">
                        <i class="fas fa-camera"></i> Cambiar imagen
                    </button>`;
                }
                
                card.innerHTML = html;
                card.onclick = (e) => {
                    if (e.target.tagName === 'BUTTON') return;
                    openGenrePlaylist(genre);
                };
                container.appendChild(card);
            });
        }

        function hideAllSections() {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
        }

        function showGlobalPlaylist() {
            hideAllSections();
            document.getElementById('globalPlaylistSection').classList.remove('hidden');
            const container = document.getElementById('globalSongsList');
            container.innerHTML = '';
            Object.keys(genres).forEach(g => genres[g].forEach(song => container.innerHTML += createSongCard(song)));
            const count = currentUser && userGlobalCompletions[currentUser.email] ? userGlobalCompletions[currentUser.email] : 0;
            document.getElementById('globalCountNum').textContent = count;
            document.getElementById('globalTitle').innerHTML = `<i class="fas fa-globe"></i> PLAYLIST GLOBAL DE YOUTUBE`;
        }

        function showDailyPlaylist() {
            hideAllSections();
            document.getElementById('globalPlaylistSection').classList.remove('hidden');
            const container = document.getElementById('globalSongsList');
            container.innerHTML = '';
            if (genres[dailySelectedGenre]) {
                genres[dailySelectedGenre].forEach(song => container.innerHTML += createSongCard(song));
            }
            const count = currentUser && userGlobalCompletions[currentUser.email] ? userGlobalCompletions[currentUser.email] : 0;
            document.getElementById('globalCountNum').textContent = count;
            document.getElementById('globalTitle').innerHTML = `<i class="fas fa-sun"></i> PLAYLIST DEL DÍA - ${dailySelectedGenre}`;
        }

        function showCreatorPlaylist() {
            hideAllSections();
            document.getElementById('creatorPlaylistSection').classList.remove('hidden');
            const container = document.getElementById('creatorSongsList');
            container.innerHTML = '';
            creatorPlaylist.forEach(song => container.innerHTML += createSongCard(song));
            document.getElementById('creatorCountNum').textContent = creatorCompletionCount;
        }

        function showAboutPage() {
            hideAllSections();
            document.getElementById('aboutSection').classList.remove('hidden');
            document.getElementById('aboutDesc').innerHTML = aboutText;
            renderSocialLinks();
            
            const postBox = document.getElementById('communityPostBox');
            if (postBox) {
                postBox.style.display = (currentUser && currentUser.isAdmin) ? 'block' : 'none';
            }
            
            renderCommunityPosts();
            
            const postTypeSelect = document.getElementById('postType');
            if (postTypeSelect) {
                postTypeSelect.onchange = updatePostFormFields;
            }
        }
        
        function updatePostFormFields() {
            const type = document.getElementById('postType').value;
            const textArea = document.getElementById('newPostText');
            const imageDiv = document.getElementById('imageUploadDiv');
            const videoDiv = document.getElementById('videoUploadDiv');
            
            textArea.style.display = (type === 'text') ? 'block' : 'none';
            imageDiv.style.display = (type === 'image') ? 'block' : 'none';
            videoDiv.style.display = (type === 'video') ? 'block' : 'none';
            
            if (type === 'text') {
                textArea.placeholder = "Escribe tu publicación...";
            } else if (type === 'image') {
                textArea.placeholder = "(Opcional) Agrega un comentario a la imagen...";
            } else if (type === 'video') {
                textArea.placeholder = "(Opcional) Agrega un comentario al video...";
            }
        }

        function renderSocialLinks() {
            const container = document.getElementById('socialLinksContainer');
            container.innerHTML = '';
            socialLinks.forEach(link => {
                const a = document.createElement('a');
                a.href = link.url;
                a.target = "_blank";
                a.className = `flex items-center gap-4 bg-zinc-900 hover:bg-zinc-800 transition-colors w-full max-w-md px-8 py-6 rounded-3xl`;
                a.innerHTML = `
                    <i class="${link.icon} text-4xl ${link.color}"></i>
                    <div class="text-left">
                        <div class="font-bold text-xl">${link.name}</div>
                        <div class="text-gray-400">${link.url}</div>
                    </div>
                `;
                container.appendChild(a);
            });
        }

        function showCommunityPage() {
            hideAllSections();
            document.getElementById('communitySection').classList.remove('hidden');
            renderCommunityPosts();
        }

        function renderCommunityPosts() {
            const container = document.getElementById('postsContainer');
            container.innerHTML = '';
            if (communityPosts.length === 0) {
                container.innerHTML = `<p class="text-gray-400 text-center py-12">${currentLang === 'en' ? 'No posts yet. Be the first to share something!' : 'Aún no hay publicaciones. ¡Sé el primero en compartir algo!'}</p>`;
                return;
            }
            communityPosts.forEach((post, index) => {
                let contentHTML = '';
                
                if (post.text) {
                    contentHTML += `<p class="mt-3 text-gray-200">${post.text}</p>`;
                }
                if (post.image) {
                    contentHTML += `<img src="${post.image}" class="mt-4 max-w-full rounded-3xl border border-white/20">`;
                }
                if (post.video) {
                    let videoId = '';
                    if (post.video.includes('v=')) {
                        videoId = post.video.split('v=')[1].split('&')[0];
                    } else if (post.video.includes('youtu.be/')) {
                        videoId = post.video.split('youtu.be/')[1].split('?')[0];
                    }
                    if (videoId) {
                        contentHTML += `
                            <div class="embed-container mt-4">
                                <iframe src="https://www.youtube.com/embed/${videoId}" allowfullscreen></iframe>
                            </div>
                        `;
                    } else {
                        contentHTML += `<a href="${post.video}" target="_blank" class="text-red-400 underline">Ver video en YouTube</a>`;
                    }
                }
                
                let repliesHTML = '';
                if (post.replies && post.replies.length > 0) {
                    repliesHTML = post.replies.map(reply => `
                        <div class="ml-8 mt-3 bg-black/40 rounded-2xl px-5 py-3 text-sm">${reply}</div>
                    `).join('');
                }
                
                let deleteBtn = '';
                if (currentUser && currentUser.isAdmin) {
                    deleteBtn = `<button onclick="deletePost(${post.id}); event.stopImmediatePropagation()" class="text-red-500 hover:text-red-600 text-xs px-4 py-1 rounded-3xl border border-red-500/50">Eliminar</button>`;
                }
                
                const postHTML = `
                <div class="bg-zinc-900 rounded-3xl p-6">
                    <div class="flex justify-between items-start">
                        <div>
                            <div class="font-semibold">${post.author}</div>
                            <div class="text-xs text-gray-400">${post.time}</div>
                        </div>
                        ${deleteBtn}
                    </div>
                    ${contentHTML}
                    <div class="mt-6">${repliesHTML}</div>
                    <div class="mt-6 flex gap-3">
                        <input id="replyInput-${index}" type="text" placeholder="${currentLang === 'en' ? 'Reply...' : 'Responder...'}" class="flex-1 bg-black border border-white/30 rounded-3xl px-5 py-3 text-sm">
                        <button onclick="addReply(${index})" class="bg-white/10 hover:bg-white/20 px-6 py-3 rounded-3xl text-sm">Responder</button>
                    </div>
                </div>`;
                container.innerHTML += postHTML;
            });
        }

        function createNewPost() {
            if (!currentUser || !currentUser.isAdmin) {
                alert("Solo el administrador puede publicar.");
                return;
            }
            
            const type = document.getElementById('postType').value;
            const text = document.getElementById('newPostText').value.trim();
            
            let post = {
                id: Date.now(),
                author: currentUser.name,
                time: "ahora",
                replies: []
            };
            
            if (type === "text") {
                if (!text) {
                    alert("Escribe algo para publicar.");
                    return;
                }
                post.text = text;
            } 
            else if (type === "image") {
                const fileInput = document.getElementById('postImageInput');
                if (!fileInput.files[0]) {
                    alert("Selecciona una imagen para publicar.");
                    return;
                }
                const reader = new FileReader();
                reader.onload = function(e) {
                    post.image = e.target.result;
                    if (text) post.text = text;
                    communityPosts.unshift(post);
                    clearPostForm();
                    renderCommunityPosts();
                };
                reader.readAsDataURL(fileInput.files[0]);
                return;
            } 
            else if (type === "video") {
                const videoLink = document.getElementById('postVideoLink').value.trim();
                if (!videoLink) {
                    alert("Pega el link del video de YouTube.");
                    return;
                }
                post.video = videoLink;
                if (text) post.text = text;
            }
            
            communityPosts.unshift(post);
            clearPostForm();
            renderCommunityPosts();
        }
        
        function clearPostForm() {
            document.getElementById('newPostText').value = '';
            document.getElementById('postImageInput').value = '';
            document.getElementById('postVideoLink').value = '';
            document.getElementById('postType').value = 'text';
            updatePostFormFields();
        }

        function addReply(index) {
            if (!currentUser) return;
            const input = document.getElementById(`replyInput-${index}`);
            const replyText = input.value.trim();
            if (!replyText) return;
            communityPosts[index].replies.push(`${currentUser.name}: ${replyText}`);
            input.value = '';
            renderCommunityPosts();
        }

        function deletePost(postId) {
            if (!currentUser || !currentUser.isAdmin) return;
            if (confirm("¿Eliminar esta publicación?")) {
                communityPosts = communityPosts.filter(p => p.id !== postId);
                renderCommunityPosts();
            }
        }
        
        function deleteAllPosts() {
            if (!currentUser || !currentUser.isAdmin) return;
            if (confirm("¿Eliminar TODAS las publicaciones? Esta acción no se puede deshacer.")) {
                communityPosts = [];
                renderCommunityPosts();
            }
        }

        function logout() {
            if (confirm(currentLang === 'en' ? 'Log out?' : '¿Cerrar sesión?')) {
                currentUser = null;
                isAdmin = false;
                localStorage.removeItem('mutuaplay_currentUser');
                document.getElementById('loginBtn').classList.remove('hidden');
                document.getElementById('uploadBtn').classList.add('hidden');
                document.getElementById('logoutBtn').classList.add('hidden');
                document.getElementById('adminLink').classList.add('hidden');
                document.getElementById('requestsBtn').classList.add('hidden');
                hideAllSections();
                document.getElementById('genresSection').classList.remove('hidden');
                renderGenresGrid();
            }
        }

        function renderWeeklyCalendar(containerId, userEmail, offset = currentWeekOffset) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            const daysNames = ['Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb', 'Dom'];
            
            const today = new Date();
            let monday = new Date(today);
            const dayOfWeek = today.getDay();
            const diffToMonday = dayOfWeek === 0 ? 6 : dayOfWeek - 1;
            monday.setDate(today.getDate() - diffToMonday);
            monday.setDate(monday.getDate() + (offset * 7));
            
            const todayStr = today.toISOString().split('T')[0];
            
            for (let i = 0; i < 7; i++) {
                const currentDay = new Date(monday);
                currentDay.setDate(monday.getDate() + i);
                const dateStr = currentDay.toISOString().split('T')[0];
                
                const completions = userGlobalDailyCompletions[userEmail] && userGlobalDailyCompletions[userEmail][dateStr] ? userGlobalDailyCompletions[userEmail][dateStr] : 0;
                const isToday = dateStr === todayStr;
                
                let bgColor = 'bg-gray-600';
                let symbol = '○';
                let textColor = 'text-gray-400';
                
                if (isToday) {
                    if (completions > 0) {
                        bgColor = 'bg-green-500';
                        textColor = 'text-white';
                        symbol = completions;
                    } else {
                        bgColor = 'bg-red-500';
                        textColor = 'text-white';
                        symbol = '○';
                    }
                }
                
                const dayHTML = `
                <div class="text-center">
                    <div class="text-xs font-medium text-gray-400">${daysNames[i]}${isToday ? ' <span class="text-red-400 font-bold">(Hoy)</span>' : ''}</div>
                    <div class="text-sm font-semibold mt-1">${currentDay.getDate()} <span class="text-[10px] text-gray-500">${currentDay.getFullYear()}</span></div>
                    <div class="mt-3 h-10 w-10 mx-auto flex items-center justify-center rounded-2xl ${bgColor} ${textColor} text-xl font-bold ${isToday ? 'ring-2 ring-offset-2 ring-offset-zinc-900 ring-white/70' : ''}">
                        ${symbol}
                    </div>
                </div>`;
                container.innerHTML += dayHTML;
            }
        }

        function changeCalendarWeek(delta) {
            let newOffset = currentWeekOffset + delta;
            if (newOffset < 0) newOffset = 0;
            currentWeekOffset = newOffset;
            if (currentUser) {
                renderWeeklyCalendar('userCalendar', currentUser.email, currentWeekOffset);
            }
        }

        function goToCurrentWeek() {
            currentWeekOffset = 0;
            if (currentUser) {
                renderWeeklyCalendar('userCalendar', currentUser.email, 0);
            }
        }

        function togglePlaylist(type) {
            if (type === 'global') {
                globalPlaylistActive = !globalPlaylistActive;
                localStorage.setItem('mutuaplay_globalPlaylistActive', JSON.stringify(globalPlaylistActive));
            } else if (type === 'daily') {
                dailyPlaylistActive = !dailyPlaylistActive;
                localStorage.setItem('mutuaplay_dailyPlaylistActive', JSON.stringify(dailyPlaylistActive));
            }
            updateNavLinks();
        }

        function saveDailyGenre() {
            dailySelectedGenre = document.getElementById('dailyGenreSelect').value;
            localStorage.setItem('mutuaplay_dailySelectedGenre', dailySelectedGenre);
        }

        function updateNavLinks() {
            const navGlobal = document.getElementById('navGlobal');
            if (globalPlaylistActive) {
                navGlobal.textContent = 'Playlist Global';
                navGlobal.onclick = showGlobalPlaylist;
            } else if (dailyPlaylistActive) {
                navGlobal.textContent = 'Playlist del Día';
                navGlobal.onclick = showDailyPlaylist;
            }
        }

        function showAdminPanel() {
            if (!currentUser || !currentUser.isAdmin) return alert(currentLang === 'en' ? "Only the administrator can access" : "Solo el administrador puede acceder");
            hideAllSections();
            document.getElementById('adminSection').classList.remove('hidden');

            let html = `<h3 class="text-2xl font-bold mb-6">${currentLang === 'en' ? 'Registered Users' : 'Usuarios Registrados'}</h3>`;

            registeredUsers.forEach(user => {
                const max = userMaxUploads[user.email] || 2;
                const userSongs = allSongs.filter(s => s.uploader === user.email);
                const globalTimes = userGlobalCompletions[user.email] || 0;
                const userGenres = userGenreCompletions[user.email] || {};
                const creatorTimes = userCreatorCompletions[user.email] || 0;

                const isMainAdmin = user.email === "admin@proton.me";

                html += `
                <div class="bg-zinc-900 rounded-3xl p-6 mb-6">
                    <div class="flex justify-between mb-4">
                        <div>
                            <img src="${user.photo || 'https://i.pravatar.cc/128?img=12'}" class="w-12 h-12 rounded-2xl inline-block mr-3">
                            <span class="font-bold">${user.name} ${user.apellido || ''}</span> 
                            <span class="text-gray-400">(${user.email})</span>
                            ${user.isAdmin ? `<span class="ml-2 bg-red-500 text-white text-xs px-3 py-1 rounded-3xl">ADMIN</span>` : ''}
                        </div>
                        <div>
                            Máximo canciones: 
                            <input type="number" id="max-${user.email}" value="${max}" class="w-16 bg-black border border-white/30 rounded-3xl px-3 py-1 text-center">
                            <button onclick="saveUserMax('${user.email}')" class="ml-3 bg-red-600 text-white px-4 py-1 rounded-3xl text-sm">Guardar</button>
                        </div>
                    </div>

                    <div class="flex gap-6 text-sm mb-4">
                        <div><span class="text-gray-400">Nacionalidad:</span> ${user.nacionalidad || '—'}</div>
                        <div><span class="text-gray-400">Edad:</span> ${user.edad || '—'}</div>
                    </div>

                    <div class="text-sm text-gray-400 mb-3">Escuchas completas:</div>
                    <div class="grid grid-cols-2 gap-6 mb-6">
                        <div>Lista Global: <span class="font-bold text-red-500">${globalTimes}</span></div>
                        <div>Lista del Creador: <span class="font-bold text-red-500">${creatorTimes}</span></div>
                        <div>
                            <div class="text-gray-400 mb-2">Listas de Géneros:</div>
                            <div class="grid grid-cols-2 gap-2 text-xs max-h-48 overflow-y-auto pr-2">
                                ${currentGenres.map(g => {
                                    const count = userGenres[g] || 0;
                                    return `<div class="flex justify-between bg-black/40 px-3 py-1 rounded-2xl"><span>${g}</span><span class="font-bold text-red-400">${count}</span></div>`;
                                }).join('')}
                            </div>
                        </div>
                    </div>

                    <div class="text-sm text-gray-400 mb-3">Canciones subidas: <span class="font-bold text-red-500">${userSongs.length}</span></div>
                    <details class="mt-6">
                        <summary class="text-sm text-gray-400 cursor-pointer flex items-center gap-2 hover:text-white">Ver canciones (${userSongs.length}) <span class="text-xs">▼</span></summary>
                        <div class="space-y-2 mt-4">
                            ${userSongs.map(s => `
                                <div class="flex justify-between bg-black/50 rounded-2xl px-4 py-3">
                                    <span>${s.title}</span>
                                    <button onclick="deleteSong(${s.id});event.stopImmediatePropagation()" class="text-red-500 text-xs">Eliminar Canción</button>
                                </div>
                            `).join('')}
                        </div>
                    </details>

                    <div class="mt-8">
                        <div class="text-sm text-gray-400 mb-3">Calendario semanal</div>
                        <div class="flex items-center justify-between mb-4 bg-zinc-900 rounded-3xl px-6 py-3">
                            <button onclick="changeAdminCalendarWeek('${user.email}', -1)" class="flex items-center gap-2 text-red-400 hover:text-red-500 font-medium">
                                <i class="fas fa-chevron-left"></i> Semana anterior
                            </button>
                            <button onclick="goToCurrentWeekAdmin('${user.email}')" class="text-white font-semibold flex items-center gap-1 hover:text-red-400">
                                <i class="fas fa-calendar-day"></i> HOY
                            </button>
                            <button onclick="changeAdminCalendarWeek('${user.email}', 1)" class="flex items-center gap-2 text-red-400 hover:text-red-500 font-medium">
                                Semana siguiente <i class="fas fa-chevron-right"></i>
                            </button>
                        </div>
                        <div id="adminCalendar-${user.email}" class="grid grid-cols-7 gap-3"></div>
                    </div>

                    ${!isMainAdmin ? `
                    <div class="mt-6 flex gap-3 justify-end">
                        <button onclick="toggleAdminStatus('${user.email}');event.stopImmediatePropagation()" class="bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded-3xl text-sm flex items-center gap-2">
                            <i class="fas fa-shield-alt"></i> ${user.isAdmin ? 'Quitar Admin' : 'Hacer Administrador'}
                        </button>
                        <button onclick="deleteAccount('${user.email}');event.stopImmediatePropagation()" class="bg-red-600 hover:bg-red-700 text-white px-5 py-2 rounded-3xl text-sm flex items-center gap-2">
                            <i class="fas fa-trash"></i> Eliminar cuenta
                        </button>
                    </div>` : ''}
                </div>`;
            });

            document.getElementById('adminUsersList').innerHTML = html;

            const select = document.getElementById('dailyGenreSelect');
            select.innerHTML = '';
            currentGenres.forEach(g => {
                const opt = document.createElement('option');
                opt.value = g;
                opt.textContent = g;
                if (g === dailySelectedGenre) opt.selected = true;
                select.appendChild(opt);
            });

            document.getElementById('toggleGlobal').checked = globalPlaylistActive;
            document.getElementById('toggleDaily').checked = dailyPlaylistActive;

            registeredUsers.forEach(user => {
                const calContainer = document.getElementById(`adminCalendar-${user.email}`);
                if (calContainer) renderWeeklyCalendar(`adminCalendar-${user.email}`, user.email, 0);
            });

            renderCurrentSocialLinks();
            updateNavLinks();
        }

        function changeAdminCalendarWeek(userEmail, delta) {
            let offset = window[`adminOffset_${userEmail}`] || 0;
            let newOffset = offset + delta;
            if (newOffset < 0) newOffset = 0;
            window[`adminOffset_${userEmail}`] = newOffset;
            renderWeeklyCalendar(`adminCalendar-${user.email}`, userEmail, newOffset);
        }

        function goToCurrentWeekAdmin(userEmail) {
            window[`adminOffset_${userEmail}`] = 0;
            renderWeeklyCalendar(`adminCalendar-${user.email}`, userEmail, 0);
        }

        function showDashboard() {
            if (!currentUser) return alert(currentLang === 'en' ? "Log in first" : "Inicia sesión primero");
            hideAllSections();
            document.getElementById('dashboardSection').classList.remove('hidden');

            document.getElementById('dashboardUserName').innerHTML = `👤 ${currentUser.name}`;

            const userSongs = allSongs.filter(s => s.uploader === currentUser.email);
            document.getElementById('statsSongs').textContent = userSongs.length;

            renderWeeklyCalendar('userCalendar', currentUser.email, currentWeekOffset);

            const container = document.getElementById('mySongsList');
            container.innerHTML = '';
            if (userSongs.length === 0) {
                container.innerHTML = `<p class="text-gray-400">${currentLang === 'en' ? 'You have no published videos yet.' : 'Aún no tienes videos publicados.'}</p>`;
            } else {
                userSongs.forEach(song => {
                    container.innerHTML += `
                    <div class="bg-zinc-900 rounded-3xl p-6 flex justify-between items-center">
                        <div>
                            <div class="font-bold">${song.title}</div>
                        </div>
                        <button onclick="requestSongChange(${song.id})" class="bg-yellow-600 hover:bg-yellow-700 text-white text-xs px-5 py-2 rounded-3xl">${currentLang === 'en' ? 'Change song' : 'Cambiar canción'}</button>
                    </div>`;
                });
            }
        }

        function renderCurrentSocialLinks() {
            const container = document.getElementById('currentSocialLinksList');
            container.innerHTML = `<p class="text-sm text-gray-400 mb-4">Enlaces actuales:</p>`;
            
            socialLinks.forEach((link, index) => {
                const div = document.createElement('div');
                div.className = "flex items-center justify-between bg-black/50 rounded-3xl px-6 py-4";
                div.innerHTML = `
                    <div class="flex items-center gap-4">
                        <i class="${link.icon} text-3xl ${link.color}"></i>
                        <div>
                            <span class="font-semibold">${link.name}</span>
                            <span class="text-xs text-gray-400 ml-3">${link.url}</span>
                        </div>
                    </div>
                    <button onclick="deleteSocialLink(${index}); event.stopImmediatePropagation()" class="text-red-500 hover:text-red-600 px-4 py-2 rounded-3xl text-sm">Eliminar</button>
                `;
                container.appendChild(div);
            });
        }

        function addSocialLink() {
            const name = document.getElementById('newSocialName').value.trim();
            const icon = document.getElementById('newSocialIcon').value.trim();
            const color = document.getElementById('newSocialColor').value.trim();
            const url = document.getElementById('newSocialUrl').value.trim();

            if (!name || !icon || !url) {
                alert("Por favor completa nombre, icono y link");
                return;
            }

            const newLink = {
                id: Date.now(),
                name: name,
                icon: icon,
                color: color || "text-white",
                url: url
            };

            socialLinks.push(newLink);
            localStorage.setItem('mutuaplay_socialLinks', JSON.stringify(socialLinks));

            document.getElementById('newSocialName').value = '';
            document.getElementById('newSocialIcon').value = '';
            document.getElementById('newSocialUrl').value = '';

            alert("✅ Enlace de red social agregado");
            renderCurrentSocialLinks();
            
            if (!document.getElementById('aboutSection').classList.contains('hidden')) {
                renderSocialLinks();
            }
        }

        function deleteSocialLink(index) {
            if (confirm("¿Eliminar este enlace de red social?")) {
                socialLinks.splice(index, 1);
                localStorage.setItem('mutuaplay_socialLinks', JSON.stringify(socialLinks));
                renderCurrentSocialLinks();
                if (!document.getElementById('aboutSection').classList.contains('hidden')) {
                    renderSocialLinks();
                }
            }
        }

        function toggleAdminStatus(email) {
            const user = registeredUsers.find(u => u.email === email);
            if (user) {
                user.isAdmin = !user.isAdmin;
                localStorage.setItem('mutuaplay_registeredUsers', JSON.stringify(registeredUsers));
                alert(user.isAdmin ? '✅ Usuario convertido en Administrador' : '✅ Se quitaron los permisos de administrador');
                showAdminPanel();
            }
        }

        function filterAdminUsers() {
            const searchTerm = document.getElementById('adminSearch').value.toLowerCase().trim();
            const minCasillas = parseInt(document.getElementById('minCasillasActivas').value) || 0;
            const userDivs = document.querySelectorAll('#adminUsersList > div');
            userDivs.forEach(div => {
                const text = div.textContent.toLowerCase();
                const globalMatch = text.match(/lista global:\s*(\d+)/i);
                const globalCount = globalMatch ? parseInt(globalMatch[1]) : 0;
                const matchesSearch = !searchTerm || text.includes(searchTerm);
                const matchesCasillas = globalCount >= minCasillas;
                div.style.display = (matchesSearch && matchesCasillas) ? 'block' : 'none';
            });
        }

        function saveAboutText() {
            aboutText = document.getElementById('aboutEditText').value.trim();
            alert(currentLang === 'en' ? 'About page text updated!' : '¡Texto de Acerca de la página guardado!');
            if (!document.getElementById('aboutSection').classList.contains('hidden')) {
                document.getElementById('aboutDesc').innerHTML = aboutText;
            }
        }

        function deleteAccount(email) {
            if (confirm('¿Eliminar completamente la cuenta de ' + email + '?')) {
                allSongs = allSongs.filter(s => s.uploader !== email);
                Object.keys(genres).forEach(g => {
                    genres[g] = genres[g].filter(s => s.uploader !== email);
                });
                registeredUsers = registeredUsers.filter(u => u.email !== email);
                localStorage.setItem('mutuaplay_registeredUsers', JSON.stringify(registeredUsers));
                alert('Cuenta eliminada');
                showAdminPanel();
            }
        }

        function saveUserMax(email) {
            const input = document.getElementById('max-' + email);
            userMaxUploads[email] = parseInt(input.value);
            alert('Límite actualizado para ' + email);
            showAdminPanel();
        }

        function deleteSong(id) {
            if (confirm('¿Eliminar esta canción permanentemente?')) {
                allSongs = allSongs.filter(s => s.id !== id);
                Object.keys(genres).forEach(g => {
                    genres[g] = genres[g].filter(s => s.id !== id);
                });
                alert('Canción eliminada');
                showAdminPanel();
            }
        }

        function uploadToCreatorPlaylist() {
            if (!currentUser || !currentUser.isAdmin) {
                alert("Solo el administrador puede subir a esta lista");
                return;
            }
            const title = document.getElementById('creatorUploadTitle').value.trim() || "Video del creador";
            let link = document.getElementById('creatorUploadLink').value.trim();
            if (!link) {
                alert("Ingresa un link de YouTube");
                return;
            }
            let linkId = link.includes('v=') ? link.split('v=')[1].split('&')[0] : link.split('/').pop();

            const newSong = { 
                id: Date.now(), 
                title: title, 
                artist: "Ángel Díaz C (Creador)", 
                platform: "youtube", 
                linkId: linkId, 
                fullUrl: link, 
                completed: false, 
                likes: 0, 
                uploader: "admin@proton.me",
                views: 0 
            };

            creatorPlaylist.push(newSong);
            localStorage.setItem('mutuaplay_creatorPlaylist', JSON.stringify(creatorPlaylist));

            document.getElementById('creatorUploadTitle').value = '';
            document.getElementById('creatorUploadLink').value = '';

            alert(currentLang === 'en' ? '✅ Video subido a la lista del creador!' : '✅ ¡Video subido a la lista del creador!');
            
            if (!document.getElementById('creatorPlaylistSection').classList.contains('hidden')) {
                showCreatorPlaylist();
            }
        }

        function showLoginModal() { document.getElementById('loginModal').classList.remove('hidden'); }
        function hideLoginModal() { document.getElementById('loginModal').classList.add('hidden'); }

        function toggleLoginRegister() {
            const loginForm = document.getElementById('loginForm');
            const registerForm = document.getElementById('registerForm');
            const toggleBtn = document.getElementById('toggleButton');
            if (loginForm.classList.contains('hidden')) {
                loginForm.classList.remove('hidden');
                registerForm.classList.add('hidden');
                toggleBtn.innerHTML = currentLang === 'en' ? 'Already have an account? Log in' : '¿No tienes cuenta? Crear cuenta';
            } else {
                loginForm.classList.add('hidden');
                registerForm.classList.remove('hidden');
                toggleBtn.innerHTML = currentLang === 'en' ? 'Don\'t have an account? Create one' : '¿Ya tienes cuenta? Iniciar sesión';
            }
        }

        function togglePasswordVisibility(fieldId) {
            const field = document.getElementById(fieldId);
            const eye = document.getElementById(fieldId === 'loginPass' ? 'loginEye' : 'registerEye');
            if (field.type === "password") {
                field.type = "text";
                eye.classList.replace('fa-eye', 'fa-eye-slash');
            } else {
                field.type = "password";
                eye.classList.replace('fa-eye-slash', 'fa-eye');
            }
        }

        function emailLogin() {
            const username = document.getElementById('loginUsername').value.trim();
            const pass = document.getElementById('loginPass').value.trim();

            if (!username || !pass) {
                alert(currentLang === 'en' ? "Please enter username and password" : "Por favor ingresa nombre de usuario y contraseña");
                return;
            }

            const user = registeredUsers.find(u => u.username === username && u.password === pass);
            hideLoginModal();

            if (user) {
                currentUser = { 
                    name: user.name, 
                    apellido: user.apellido || '', 
                    edad: user.edad || '', 
                    nacionalidad: user.nacionalidad || '', 
                    email: user.email, 
                    username: user.username,
                    isAdmin: user.isAdmin || false, 
                    photo: user.photo || 'https://i.pravatar.cc/128?img=12' 
                };
                loginSuccess(user.isAdmin || false);
            } else {
                alert(currentLang === 'en' ? "User not found. Create an account" : "Usuario no encontrado. Crea una cuenta");
            }
        }

        function registerAccount() {
            const username = document.getElementById('registerUsername').value.trim();
            const name = document.getElementById('registerName').value.trim();
            const apellido = document.getElementById('registerApellido').value.trim();
            const edad = parseInt(document.getElementById('registerEdad').value);
            const nacionalidad = document.getElementById('registerNacionalidad').value;
            const pass = document.getElementById('registerPass').value.trim();

            if (!username || !name || !apellido || !edad || !nacionalidad || !pass) {
                alert("Todos los campos son obligatorios.");
                return;
            }

            if (edad < 18 || edad > 100) {
                alert("La edad debe estar entre 18 y 100 años.");
                return;
            }

            const letterRegex = /^[a-zA-ZáéíóúñÁÉÍÓÚÑ ]+$/;
            if (!letterRegex.test(name) || !letterRegex.test(apellido)) {
                alert("NÚMEROS EN NOMBRE Y APELLIDO NO SON ACEPTABLES SOLO LETRAS....");
                return;
            }

            if (username.length > 10 || pass.length > 10) {
                alert("El nombre de usuario y la contraseña deben tener máximo 10 caracteres.");
                return;
            }

            const existing = registeredUsers.find(u => u.username === username);
            if (existing) {
                alert(currentLang === 'en' ? "Username not available" : "Nombre de usuario no disponible");
                return;
            }

            const newUser = {
                email: username + "@mutuaplay.com",
                password: pass,
                username: username,
                name: name,
                apellido: apellido,
                edad: edad,
                nacionalidad: nacionalidad,
                isAdmin: false,
                photo: 'https://i.pravatar.cc/128?img=12'
            };

            registeredUsers.push(newUser);
            localStorage.setItem('mutuaplay_registeredUsers', JSON.stringify(registeredUsers));

            currentUser = { 
                name: name, 
                apellido: apellido || '', 
                edad: edad || '', 
                nacionalidad: nacionalidad || '', 
                email: username + "@mutuaplay.com", 
                username: username,
                isAdmin: false, 
                photo: 'https://i.pravatar.cc/128?img=12' 
            };

            loginSuccess(false);
            hideLoginModal();
        }

        function validateNameInput(input) {
            const value = input.value;
            if (/[0-9]/.test(value)) {
                alert("NÚMEROS EN NOMBRE Y APELLIDO NO SON ACEPTABLES SOLO LETRAS....");
                input.value = value.replace(/[0-9]/g, '');
            }
        }

        function validateAgeInput(input) {
            let val = parseInt(input.value);
            if (val < 18) input.value = 18;
            if (val > 100) input.value = 100;
        }

        function changeAge(delta) {
            const input = document.getElementById('registerEdad');
            let val = parseInt(input.value) || 18;
            val += delta;
            if (val < 18) val = 18;
            if (val > 100) val = 100;
            input.value = val;
        }

        function showForgotPassword() {
            hideLoginModal();
            document.getElementById('forgotModal').classList.remove('hidden');
        }

        function hideForgotModal() {
            document.getElementById('forgotModal').classList.add('hidden');
        }

        function sendPasswordReset() {
            const email = document.getElementById('forgotEmail').value.trim();
            if (email) {
                hideForgotModal();
                alert(currentLang === 'en' ? `A password recovery link has been sent to synthetic036@gmail.com` : `Se ha enviado un enlace de recuperación a synthetic036@gmail.com`);
            } else {
                alert(currentLang === 'en' ? "Enter your email" : "Ingresa tu correo");
            }
        }

        function loginSuccess(admin) {
            document.getElementById('loginBtn').classList.add('hidden');
            document.getElementById('uploadBtn').classList.remove('hidden');
            document.getElementById('logoutBtn').classList.remove('hidden');
            // Ya no hay userInfo en navbar, el perfil está en Géneros

            if (admin) {
                document.getElementById('requestsBtn').classList.remove('hidden');
            } else {
                document.getElementById('requestsBtn').classList.add('hidden');
            }

            if (admin) {
                isAdmin = true;
                document.getElementById('adminLink').classList.remove('hidden');
            }

            localStorage.setItem('mutuaplay_currentUser', JSON.stringify(currentUser));

            alert(currentLang === 'en' ? '✅ Welcome ' + currentUser.name + '!' : '✅ ¡Bienvenido ' + currentUser.name + '!');
            updateNavLinks();
            showGlobalPlaylist();
        }

        function showUploadModal() {
            if (!currentUser) return alert(currentLang === 'en' ? "Log in first" : "Inicia sesión primero");
            const modal = document.getElementById('uploadModal');
            modal.classList.remove('hidden');
            const select = document.getElementById('uploadGenre');
            select.innerHTML = '';
            currentGenres.forEach(g => {
                const opt = document.createElement('option');
                opt.value = g;
                opt.textContent = g;
                select.appendChild(opt);
            });
        }
        function hideUploadModal() { document.getElementById('uploadModal').classList.add('hidden'); }

        function uploadSong() {
            const genre = document.getElementById('uploadGenre').value;
            const title = document.getElementById('uploadTitle').value || "Nuevo video";
            let link = document.getElementById('uploadLink').value.trim();
            let linkId = link.includes('v=') ? link.split('v=')[1].split('&')[0] : link.split('/').pop();

            const max = userMaxUploads[currentUser.email] || 2;
            const currentCount = allSongs.filter(s => s.uploader === currentUser.email).length;

            if (currentCount >= max) {
                alert(currentLang === 'en' ? "You can't upload more songs. Contact the administrator to upload more." : "No puedes subir más canciones. Ponte en contacto con el administrador para que puedas subir más canciones.");
                hideUploadModal();
                return;
            }

            const newSong = { id: Date.now(), title: title, artist: currentUser.name, platform: "youtube", linkId: linkId, fullUrl: link, completed: false, likes: 0, uploader: currentUser.email, views: 0 };
            if (!genres[genre]) genres[genre] = [];
            genres[genre].push(newSong);
            allSongs.push(newSong);

            hideUploadModal();
            alert(currentLang === 'en' ? '✅ Video uploaded successfully!' : '✅ ¡Video subido correctamente!');

            document.getElementById('uploadTitle').value = '';
            document.getElementById('uploadLink').value = '';

            showGlobalPlaylist();
            
            if (!document.getElementById('genresSection').classList.contains('hidden')) {
                renderGenresGrid();
            }
        }

        function showProfileModal() {
            if (!currentUser) return;
            document.getElementById('profileModal').classList.remove('hidden');
            document.getElementById('profileName').value = currentUser.name || '';
            document.getElementById('profileApellido').value = currentUser.apellido || '';
            document.getElementById('profileEdad').value = currentUser.edad || '';
            document.getElementById('profileNacionalidad').value = currentUser.nacionalidad || '';
            document.getElementById('profileAvatar').src = currentUser.photo || 'https://i.pravatar.cc/128?img=12';
        }
        function hideProfileModal() { document.getElementById('profileModal').classList.add('hidden'); }

        function changeProfilePhoto(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(ev) {
                    currentUser.photo = ev.target.result;
                    document.getElementById('profileAvatar').src = currentUser.photo;
                    
                    if (!document.getElementById('genresSection').classList.contains('hidden')) {
                        renderUserProfileInGenres();
                    }
                };
                reader.readAsDataURL(file);
            }
        }

        function saveProfile() {
            currentUser.name = document.getElementById('profileName').value;
            currentUser.apellido = document.getElementById('profileApellido').value;
            currentUser.edad = document.getElementById('profileEdad').value;
            currentUser.nacionalidad = document.getElementById('profileNacionalidad').value;
            hideProfileModal();
            
            if (!document.getElementById('genresSection').classList.contains('hidden')) {
                renderUserProfileInGenres();
            }
            
            alert(currentLang === 'en' ? '✅ Profile saved' : '✅ Perfil guardado');
        }

        function requestSongChange(id) {
            const song = allSongs.find(s => s.id === id);
            if (song && currentUser) {
                pendingChangeRequests.push({
                    id: Date.now(),
                    title: song.title,
                    user: currentUser.email,
                    songId: id
                });
                alert(currentLang === 'en' ? "✅ Song change request sent to administrator" : "✅ Solicitud de cambio de canción enviada al administrador");
            }
        }

        function showChangeRequests() {
            if (!currentUser || !currentUser.isAdmin) {
                alert(currentLang === 'en' ? "Only the administrator can see requests" : "Solo el administrador puede ver las solicitudes");
                return;
            }
            let html = `<h3 class="text-2xl font-bold mb-6">${currentLang === 'en' ? 'Song Change Requests' : 'Solicitudes de Cambio de Canción'}</h3>`;
            
            html += `<button onclick="acceptAllChangeRequests()" class="mb-6 bg-green-600 hover:bg-green-700 text-white px-8 py-3 rounded-3xl font-semibold flex items-center gap-2">
                <i class="fas fa-check"></i> ${currentLang === 'en' ? 'Accept all requests' : 'Aceptar todas las solicitudes'}
            </button>`;

            html += `<div class="space-y-4">`;
            if (pendingChangeRequests.length === 0) {
                html += `<p class="text-gray-400">${currentLang === 'en' ? 'No pending requests.' : 'No hay solicitudes pendientes.'}</p>`;
            } else {
                pendingChangeRequests.forEach((req, index) => {
                    html += `
                    <div class="bg-zinc-900 rounded-3xl p-6 flex justify-between items-center">
                        <div>
                            <span class="font-medium">${req.title}</span> 
                            <span class="text-gray-400">(${req.user})</span>
                        </div>
                        <div class="flex gap-3">
                            <button onclick="acceptChangeRequest(${index});event.stopImmediatePropagation()" class="bg-green-600 hover:bg-green-700 text-white px-5 py-2 rounded-3xl text-sm">${currentLang === 'en' ? 'Accept' : 'Aceptar'}</button>
                            <button onclick="denyChangeRequest(${index});event.stopImmediatePropagation()" class="bg-red-600 hover:bg-red-700 text-white px-5 py-2 rounded-3xl text-sm">${currentLang === 'en' ? 'Deny' : 'Negar'}</button>
                        </div>
                    </div>`;
                });
            }
            html += `</div>`;
            document.getElementById('adminUsersList').innerHTML = html;
            hideAllSections();
            document.getElementById('adminSection').classList.remove('hidden');
        }

        function acceptAllChangeRequests() {
            if (pendingChangeRequests.length === 0) return alert(currentLang === 'en' ? "No pending requests" : "No hay solicitudes pendientes");
            if (confirm(currentLang === 'en' ? `Accept ALL ${pendingChangeRequests.length} requests?` : `¿Aceptar TODAS las ${pendingChangeRequests.length} solicitudes?`)) {
                let count = 0;
                while (pendingChangeRequests.length > 0) {
                    const req = pendingChangeRequests[0];
                    if (req && req.songId) {
                        allSongs = allSongs.filter(s => s.id !== req.songId);
                        Object.keys(genres).forEach(g => {
                            genres[g] = genres[g].filter(s => s.id !== req.songId);
                        });
                        count++;
                    }
                    pendingChangeRequests.shift();
                }
                alert(currentLang === 'en' ? `✅ Accepted ${count} requests. All songs were deleted.` : `✅ Se aceptaron ${count} solicitudes. Todas las canciones fueron eliminadas.`);
                showChangeRequests();
            }
        }

        function acceptChangeRequest(index) {
            const req = pendingChangeRequests[index];
            if (req && req.songId) {
                allSongs = allSongs.filter(s => s.id !== req.songId);
                Object.keys(genres).forEach(g => {
                    genres[g] = genres[g].filter(s => s.id !== req.songId);
                });
                alert(currentLang === 'en' ? `✅ Request accepted. Song "${req.title}" was deleted.` : `✅ Solicitud aceptada. La canción "${req.title}" fue eliminada.`);
                pendingChangeRequests.splice(index, 1);
                showChangeRequests();
            }
        }

        function denyChangeRequest(index) {
            const req = pendingChangeRequests[index];
            if (req) {
                alert(currentLang === 'en' ? `Request for "${req.title}" DENIED` : `Solicitud de cambio de "${req.title}" NEGADA`);
                pendingChangeRequests.splice(index, 1);
                showChangeRequests();
            }
        }

        function addNewGenre() {
            if (!currentUser || !currentUser.isAdmin) {
                alert("Solo el administrador puede agregar géneros.");
                return;
            }
            const name = prompt("Nombre del nuevo género:");
            if (!name || name.trim() === '') return;
            const trimmed = name.trim();
            if (currentGenres.includes(trimmed)) {
                alert("Ese género ya existe.");
                return;
            }
            currentGenres.push(trimmed);
            localStorage.setItem('mutuaplay_currentGenres', JSON.stringify(currentGenres));
            if (!genres[trimmed]) genres[trimmed] = [];
            if (!genreImages[trimmed]) genreImages[trimmed] = null;
            localStorage.setItem('mutuaplay_genreImages', JSON.stringify(genreImages));
            renderGenresGrid();
            alert("✅ Nuevo género agregado: " + trimmed);
        }

        function changeGenreImage(genre, event) {
            event.stopImmediatePropagation();
            if (!currentUser || !currentUser.isAdmin) return;
            
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'image/*';
            input.onchange = function() {
                const file = input.files[0];
                if (!file) return;
                const reader = new FileReader();
                reader.onload = function(ev) {
                    genreImages[genre] = ev.target.result;
                    localStorage.setItem('mutuaplay_genreImages', JSON.stringify(genreImages));
                    renderGenresGrid();
                    alert("✅ Imagen actualizada para '" + genre + "'. Todos los usuarios verán el cambio.");
                };
                reader.readAsDataURL(file);
            };
            input.click();
        }

        window.onload = function() {
            renderGenresGrid();
            console.log('%c✅ Alineación a misma altura + eliminación de duplicados en navbar (perfil y Escucha al Creador)', 'color:#ff0000; font-weight:bold');
            updateNavLinks();
        };
    </script>
</body>
</html>
```
