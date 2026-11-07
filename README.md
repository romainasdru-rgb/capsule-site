
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Capsule Sonore & Visuelle</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background-color: #0b0b0b;
            color: #f0f0f0;
            font-family: 'Courier New', Courier, monospace;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }

        /* En-tête */
        header {
            background-color: #0b0b0b;
            border-bottom: 1px solid #1a1a1a;
            padding: 15px 30px;
            display: grid;
            grid-template-columns: 1fr auto 1fr;
            align-items: center;
            height: 100px;
            flex-shrink: 0;
        }

        .site-title {
            text-align: center;
        }

        .site-title h1 {
            font-size: 1.4rem;
            letter-spacing: 5px;
            font-weight: bold;
            color: #ffffff;
            text-transform: uppercase;
        }

        .site-title p {
            font-size: 0.75rem;
            letter-spacing: 2px;
            color: #777777;
            margin-top: 4px;
        }

        .header-right {
            display: flex;
            justify-content: flex-end;
        }

        .soundcloud-player {
            width: 350px;
            height: 75px;
            border: none;
        }

        /* Triptyque ajusté */
        .triptych-container {
            display: flex;
            flex: 1;
            gap: 12px;
            padding: 15px;
            width: 100%;
            height: calc(100vh - 150px);
            min-height: 450px;
        }

        .triptych-card {
            position: relative;
            flex: 1;
            min-width: 0;
            min-height: 0;
            text-decoration: none;
            overflow: hidden;
            border: 1px solid #1c1c1c;
            transition: transform 0.4s ease, border-color 0.4s ease;
            display: block;
            cursor: pointer;
        }

        .triptych-card:hover {
            border-color: #ff4500;
            transform: translateY(-4px);
        }

        .img-wrapper {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #0b0b0b;
        }

        .img-wrapper img {
            width: 100%;
            height: 100%;
            object-fit: contain;
            transform: scale(0.84); /* Réduction de 16% de la taille des illustrations d'accueil */
            filter: brightness(0.75) contrast(1.1);
            transition: filter 0.4s ease, transform 0.4s ease;
        }

        .triptych-card:hover .img-wrapper img {
            filter: brightness(0.95) contrast(1.15);
            transform: scale(0.87); /* Conserve l'effet de zoom proportionnel au survol */
        }

        .card-overlay {
            position: absolute;
            bottom: 25px;
            left: 0;
            width: 100%;
            text-align: center;
            pointer-events: none;
            z-index: 2;
        }

        .card-overlay h2 {
            color: #ffffff;
            font-size: 1.1rem;
            letter-spacing: 4px;
            text-shadow: 0 2px 10px rgba(0,0,0,0.8);
        }

        /* Footer fixe "IF U WANT MORE" */
        .more-section {
            background-color: #0b0b0b;
            border-top: 1px solid #1a1a1a;
            padding: 12px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 50px;
            flex-shrink: 0;
            font-size: 0.8rem;
            letter-spacing: 2px;
        }

        .more-title {
            color: #777;
            text-transform: uppercase;
        }

        .more-links {
            display: flex;
            gap: 25px;
        }

        .more-links a {
            color: #ccc;
            text-decoration: none;
            transition: color 0.3s ease;
            letter-spacing: 2px;
        }

        .more-links a:hover {
            color: #ff4500;
        }

        /* Modales */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: rgba(11, 11, 11, 0.96);
            z-index: 100;
            padding: 40px 20px;
            overflow-y: auto;
        }

        .modal-content {
            max-width: 800px;
            margin: 40px auto;
            border: 1px solid #222;
            padding: 35px;
            background-color: #0f0f0f;
            position: relative;
        }

        .close-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            color: #ff4500;
            font-size: 0.9rem;
            letter-spacing: 2px;
            cursor: pointer;
            background: none;
            border: none;
            font-family: inherit;
        }

        .modal-title {
            font-size: 1.4rem;
            letter-spacing: 4px;
            margin-bottom: 25px;
            border-bottom: 1px solid #222;
            padding-bottom: 10px;
            color: #fff;
        }

        .modal-body {
            line-height: 1.8;
            color: #ccc;
            font-size: 0.95rem;
        }

        .salon-intro {
            margin-bottom: 30px;
            color: #aaa;
            font-style: italic;
        }

        .playlist-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            margin-top: 20px;
        }

        .playlist-card {
            background-color: #141414;
            border: 1px solid #1f1f1f;
            padding: 20px;
            transition: border-color 0.3s ease;
        }

        .playlist-card:hover {
            border-color: #ff4500;
        }

        .playlist-card h3 {
            font-size: 1.1rem;
            color: #fff;
            letter-spacing: 2px;
            margin-bottom: 8px;
        }

        .playlist-card p {
            font-size: 0.85rem;
            color: #888;
            line-height: 1.5;
        }

        .playlist-tags {
            margin-top: 12px;
            font-size: 0.75rem;
            color: #ff4500;
            letter-spacing: 1px;
        }

        /* Liste des Visuels & Gestion du ratio / orientation des photos */
        .visuals-list {
            display: flex;
            flex-direction: column;
            gap: 40px;
            margin-top: 20px;
        }

        .visual-item {
            border: 1px solid #1f1f1f;
            background-color: #141414;
            padding: 20px;
            transition: border-color 0.3s ease;
        }

        .visual-item:hover {
            border-color: #ff4500;
        }

        .visual-item img {
            width: 100%;
            height: auto;
            max-height: 85vh;
            object-fit: contain;
            image-orientation: from-image;
            display: block;
            margin: 0 auto 15px auto;
            filter: brightness(0.9) contrast(1.05);
        }

        .visual-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.85rem;
            color: #888;
            letter-spacing: 1px;
        }

        .visual-title {
            color: #fff;
            font-weight: bold;
        }

        @media (max-width: 768px) {
            body { overflow-y: auto; height: auto; }
            header { display: flex; flex-direction: column; height: auto; gap: 15px; padding: 20px; text-align: center; }
            .soundcloud-player { width: 100%; }
            .triptych-container { flex-direction: column; height: auto; padding: 15px; }
            .triptych-card { height: 60vh; }
            .more-section { flex-direction: column; height: auto; gap: 10px; text-align: center; padding: 15px; }
        }
    </style>
</head>
<body>

    <header>
        <div class="header-left"></div>
        <div class="site-title">
            <h1>Anoname_Vr</h1>
            <p>[ CAPSULE SONORE & VISUELLE ]</p>
        </div>
        <div class="header-right">
            <iframe class="soundcloud-player" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//soundcloud.com/longlivejulio/beamer-boy&color=%23ff5500&auto_play=false&hide_related=true&show_comments=false&show_user=true&show_reposts=false&show_teaser=false"></iframe>
        </div>
    </header>

    <main class="triptych-container">
        <div class="triptych-card" onclick="openModal('modal-ecrits')">
            <div class="img-wrapper"><img src="1.png" alt="Écrits"></div>
            <div class="card-overlay"><h2>[ ÉCRITS ]</h2></div>
        </div>
        <div class="triptych-card" onclick="openModal('modal-visuels')">
            <div class="img-wrapper"><img src="2.png" alt="Visuels"></div>
            <div class="card-overlay"><h2>[ VISUELS ]</h2></div>
        </div>
        <div class="triptych-card" onclick="openModal('modal-salon')">
            <div class="img-wrapper"><img src="3.png" alt="Salon"></div>
            <div class="card-overlay"><h2>[ SALON ]</h2></div>
        </div>
    </main>

    <footer class="more-section">
        <div class="more-title">[ IF U WANT MORE ]</div>
        <div class="more-links">
            <a href="https://open.spotify.com/user/b3pc4m5woh2y1b2j6oigkxjsi?si=d722889e0b7b4e31" target="_blank">[ SPOTIFY ]</a>
            <a href="https://www.instagram.com/a_noname_vr/" target="_blank">[ INSTAGRAM ]</a>
        </div>
    </footer>

    <!-- Modale Écrits -->
    <div id="modal-ecrits" class="modal" onclick="closeModalOnOuterClick(event, 'modal-ecrits')">
        <div class="modal-content">
            <button class="close-btn" onclick="closeModal('modal-ecrits')">[ FERMER X ]</button>
            <h2 class="modal-title">[ ÉCRITS ]</h2>
            <div class="modal-body">
                <p>— Textes, fragments et pensées en flux continu.</p>
            </div>
        </div>
    </div>

    <!-- Modale Visuels -->
    <div id="modal-visuels" class="modal" onclick="closeModalOnOuterClick(event, 'modal-visuels')">
        <div class="modal-content">
            <button class="close-btn" onclick="closeModal('modal-visuels')">[ FERMER X ]</button>
            <h2 class="modal-title">[ VISUELS ]</h2>
            <div class="modal-body">
                <p class="salon-intro">— Travaux d'illustrations et recherches graphiques.</p>
                
                <div class="visuals-list">
                    <div class="visual-item"><img src="4.png" alt="04"><div class="visual-info"><span class="visual-title">[ 04 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="5.png" alt="05"><div class="visual-info"><span class="visual-title">[ 05 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="6.png" alt="06"><div class="visual-info"><span class="visual-title">[ 06 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="7.png" alt="07"><div class="visual-info"><span class="visual-title">[ 07 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="8.png" alt="08"><div class="visual-info"><span class="visual-title">[ 08 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="9.png" alt="09"><div class="visual-info"><span class="visual-title">[ 09 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="10.png" alt="10"><div class="visual-info"><span class="visual-title">[ 10 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="11.png" alt="11"><div class="visual-info"><span class="visual-title">[ 11 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="12.png" alt="12"><div class="visual-info"><span class="visual-title">[ 12 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="13.png" alt="13"><div class="visual-info"><span class="visual-title">[ 13 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="14.png" alt="14"><div class="visual-info"><span class="visual-title">[ 14 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="15.png" alt="15"><div class="visual-info"><span class="visual-title">[ 15 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="16.png" alt="16"><div class="visual-info"><span class="visual-title">[ 16 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="17.png" alt="17"><div class="visual-info"><span class="visual-title">[ 17 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="18.png" alt="18"><div class="visual-info"><span class="visual-title">[ 18 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="19.png" alt="19"><div class="visual-info"><span class="visual-title">[ 19 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="20.png" alt="20"><div class="visual-info"><span class="visual-title">[ 20 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="21.png" alt="21"><div class="visual-info"><span class="visual-title">[ 21 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="22.png" alt="22"><div class="visual-info"><span class="visual-title">[ 22 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="23.png" alt="23"><div class="visual-info"><span class="visual-title">[ 23 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="24.png" alt="24"><div class="visual-info"><span class="visual-title">[ 24 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="25.png" alt="25"><div class="visual-info"><span class="visual-title">[ 25 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                    <div class="visual-item"><img src="26.png" alt="26"><div class="visual-info"><span class="visual-title">[ 26 ]</span><span>— ARCHIVE GRAPHIQUE</span></div></div>
                </div>

            </div>
        </div>
    </div>

    <!-- Modale Salon -->
    <div id="modal-salon" class="modal" onclick="closeModalOnOuterClick(event, 'modal-salon')">
        <div class="modal-content">
            <button class="close-btn" onclick="closeModal('modal-salon')">[ FERMER X ]</button>
            <h2 class="modal-title">[ SALON ]</h2>
            <div class="modal-body">
                <p class="salon-intro">— Espace de refuge, d'écoutes immersives et de sélections thématiques.</p>
                <div class="playlist-grid">
                    <div class="playlist-card">
                        <h3>SELECTION PUNK & EMO RAP</h3>
                        <p>Une plongée atmosphérique entre mélodies sombres et textures brutes.</p>
                        <div class="playlist-tags">#LILPEEP #ALTERNATIVE #ATMOSPHERE</div>
                    </div>
                    <div class="playlist-card">
                        <h3>02. AMBIANCES & FLOW CONTINU</h3>
                        <p>Morceaux ancrés dans les sonorités rap nocturnes et immersives.</p>
                        <div class="playlist-tags">#FRENCHRAP #HYPNOTIC #DARK</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        function openModal(id) {
            document.getElementById(id).style.display = 'block';
            document.body.style.overflow = 'hidden';
        }
        
        function closeModal(id) {
            document.getElementById(id).style.display = 'none';
            document.body.style.overflow = 'auto';
        }

        function closeModalOnOuterClick(event, id) {
            if (event.target === document.getElementById(id)) {
                closeModal(id);
            }
        }

        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                const modals = document.querySelectorAll('.modal');
                modals.forEach(m => {
                    m.style.display = 'none';
                });
                document.body.style.overflow = 'auto';
            }
        });
    </script>

</body>
</html>
