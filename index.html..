// MISSILE MAYHEM 3D
// FIXED GAME.JS

let scene;
let camera;
let renderer;

let player;

let missiles = [];

let running = false;
let alive = true;

let score = 0;
let lives = 3;
let wave = 1;

let playerY = 0;
let velocityY = 0;

let ducking = false;

let spawnTimer = 0;
let waveMissiles = 0;
let waveComplete = false;
let waveDelay = 0;

let cameraShake = 0;
let lastTime = performance.now();


// ==========================================
// START THE 3D GAME
// ==========================================

function setupGame() {

    scene = new THREE.Scene();

    scene.background =
        new THREE.Color(0x020611);

    scene.fog =
        new THREE.Fog(
            0x020611,
            30,
            180
        );


    // CAMERA

    camera =
        new THREE.PerspectiveCamera(
            70,
            window.innerWidth /
            window.innerHeight,
            0.1,
            500
        );

    camera.position.set(
        0,
        5,
        13
    );


    // RENDERER

    renderer =
        new THREE.WebGLRenderer({
            antialias: true
        });

    renderer.setSize(
        window.innerWidth,
        window.innerHeight
    );

    renderer.setPixelRatio(
        Math.min(
            window.devicePixelRatio,
            2
        )
    );

    renderer.shadowMap.enabled = true;


    document
        .getElementById("game")
        .appendChild(renderer.domElement);


    // LIGHTS

    const ambient =
        new THREE.AmbientLight(
            0x8899bb,
            1.5
        );

    scene.add(ambient);


    const light =
        new THREE.DirectionalLight(
            0xffffff,
            2
        );

    light.position.set(
        -10,
        20,
        10
    );

    light.castShadow = true;

    scene.add(light);


    const redLight =
        new THREE.PointLight(
            0xff3322,
            5,
            40
        );

    redLight.position.set(
        0,
        5,
        0
    );

    scene.add(redLight);


    // STARS

    createStars();


    // MOON

    const moon =
        new THREE.Mesh(

            new THREE.SphereGeometry(
                7,
                32,
                32
            ),

            new THREE.MeshBasicMaterial({
                color: 0xfff1bd
            })

        );

    moon.position.set(
        35,
        35,
        -100
    );

    scene.add(moon);


    // GROUND

    const ground =
        new THREE.Mesh(

            new THREE.PlaneGeometry(
                180,
                500
            ),

            new THREE.MeshStandardMaterial({
                color: 0x17202e
            })

        );

    ground.rotation.x =
        -Math.PI / 2;

    ground.position.y =
        -2;

    ground.position.z =
        -100;

    ground.receiveShadow = true;

    scene.add(ground);


    // ROAD LINES

    for (
        let z = 0;
        z > -300;
        z -= 10
    ) {

        const line =
            new THREE.Mesh(

                new THREE.BoxGeometry(
                    0.25,
                    0.05,
                    4
                ),

                new THREE.MeshBasicMaterial({
                    color: 0x3c4d68
                })

            );

        line.position.set(
            0,
            -1.94,
            z
        );

        scene.add(line);

    }


    // PLAYER

    createPlayer();


    // RESIZE

    window.addEventListener(
        "resize",
        resizeGame
    );


    // START GAME BUTTON

    const startButton =
        document.getElementById(
            "startButton"
        );

    if (startButton) {

        startButton.addEventListener(
            "click",
            startGame
        );

    }


    // RESTART BUTTON

    const restartButton =
        document.getElementById(
            "restartButton"
        );

    if (restartButton) {

        restartButton.addEventListener(
            "click",
            startGame
        );

    }


    // KEYBOARD

    document.addEventListener(
        "keydown",
        keyboardDown
    );

    document.addEventListener(
        "keyup",
        keyboardUp
    );


    // MOBILE BUTTONS

    setupMobileControls();


    // DRAW FIRST FRAME

    renderer.render(
        scene,
        camera
    );


    // START ANIMATION LOOP

    requestAnimationFrame(
        gameLoop
    );

}


// ==========================================
// STARS
// ==========================================

function createStars() {

    const geometry =
        new THREE.BufferGeometry();

    const positions = [];

    for (
        let i = 0;
        i < 800;
        i++
    ) {

        positions.push(
            (Math.random() - 0.5) * 250,
            Math.random() * 100,
            (Math.random() - 0.5) * 250
        );

    }

    geometry.setAttribute(
        "position",
        new THREE.Float32BufferAttribute(
            positions,
            3
        )
    );


    const material =
        new THREE.PointsMaterial({
            color: 0xffffff,
            size: 0.35
        });


    const stars =
        new THREE.Points(
            geometry,
            material
        );


    stars.name =
        "stars";


    scene.add(stars);

}


// ==========================================
// MISSILE
// ==========================================

function createMissile() {

    const missile =
        new THREE.Group();


    const material =
        new THREE.MeshStandardMaterial({
            color: 0xff3028,
            metalness: 0.6,
            roughness: 0.3
        });


    // BODY

    const body =
        new THREE.Mesh(

            new THREE.CylinderGeometry(
                0.55,
                0.55,
                3,
                16
            ),

            material

        );

    body.rotation.z =
        Math.PI / 2;

    missile.add(body);


    // NOSE

    const nose =
        new THREE.Mesh(

            new THREE.ConeGeometry(
                0.55,
                1.2,
                16
            ),

            material

        );

    nose.rotation.z =
        -Math.PI / 2;

    nose.position.x =
        2;

    missile.add(nose);


    // FINS

    for (
        const side of [-1, 1]
    ) {

        const fin =
            new THREE.Mesh(

                new THREE.BoxGeometry(
                    0.8,
                    0.12,
                    0.8
                ),

                new THREE.MeshStandardMaterial({
                    color: 0x8e1118
                })

            );

        fin.position.x =
            -1;

        fin.position.z =
            side * 0.5;

        missile.add(fin);

    }


    // FLAME

    const flame =
        new THREE.Mesh(

            new THREE.ConeGeometry(
                0.4,
                1.5,
                12
            ),

            new THREE.MeshBasicMaterial({
                color: 0xffaa22
            })

        );

    flame.rotation.z =
        Math.PI / 2;

    flame.position.x =
        -2.2;

    missile.add(flame);


    // GLOW

    const glow =
        new THREE.PointLight(
            0xff4422,
            5,
            10
        );

    glow.position.x =
        -2;

    missile.add(glow);


    return missile;

}


// ==========================================
// PLAYER
// ==========================================

function createPlayer() {

    player =
        new THREE.Group();


    player.position.set(
        0,
        0,
        5
    );


    scene.add(player);


    // HEAD

    const head =
        new THREE.Mesh(

            new THREE.SphereGeometry(
                0.55,
                20,
                20
            ),

            new THREE.MeshStandardMaterial({
                color: 0x5eeaff,
                emissive: 0x164d5a
            })

        );

    head.position.y =
        2.5;

    player.add(head);


    // BODY

    const body =
        new THREE.Mesh(

            new THREE.BoxGeometry(
                0.8,
                1.3,
                0.6
            ),

            new THREE.MeshStandardMaterial({
                color: 0x26aabd
            })

        );

    body.position.y =
        1.5;

    player.add(body);


    // LEGS

    for (
        const side of [-1, 1]
    ) {

        const leg =
            new THREE.Mesh(

                new THREE.BoxGeometry(
                    0.25,
                    0.9,
                    0.25
                ),

                new THREE.MeshStandardMaterial({
                    color: 0x197e94
                })

            );

        leg.position.set(
            side * 0.25,
            0.6,
            0
        );

        player.add(leg);

    }


    // ARMS

    for (
        const side of [-1, 1]
    ) {

        const arm =
            new THREE.Mesh(

                new THREE.BoxGeometry(
                    0.22,
                    1,
                    0.22
                ),

                new THREE.MeshStandardMaterial({
                    color: 0x5eeaff
                })

            );

        arm.position.set(
            side * 0.58,
            1.5,
            0
        );

        player.add(arm);

    }


    // PLAYER MISSILE

    const missile =
        createMissile();

    missile.scale.set(
        0.8,
        0.8,
        0.8
    );

    missile.position.y =
        0.1;

    player.add(missile);

}


// ==========================================
// START GAME
// ==========================================

function startGame() {

    console.log(
        "MISSILE MAYHEM STARTED!"
    );


    // Hide start screen

    const startScreen =
        document.getElementById(
            "startScreen"
        );

    if (startScreen) {

        startScreen.style.display =
            "none";

    }


    // Hide game over

    const gameOver =
        document.getElementById(
            "gameOver"
        );

    if (gameOver) {

        gameOver.style.display =
            "none";

    }


    // Delete old missiles

    for (
        const missile of missiles
    ) {

        scene.remove(
            missile.mesh
        );

    }

    missiles = [];


    // Reset everything

    score = 0;

    lives = 3;

    wave = 1;

    playerY = 0;

    velocityY = 0;

    ducking = false;

    spawnTimer = 0;

    waveMissiles = 0;

    waveComplete = false;

    waveDelay = 0;

    cameraShake = 0;

    alive = true;

    running = true;


    // Update HUD

    document.getElementById(
        "score"
    ).textContent = "0";


    document.getElementById(
        "lives"
    ).textContent = "3";


    document.getElementById(
        "wave"
    ).textContent = "1";


    player.position.y = 0;

    player.scale.y = 1;


    lastTime =
        performance.now();


    showWave(
        "WAVE 1",
        "GET READY!"
    );

}


// ==========================================
// WAVE SETTINGS
// ==========================================

function getSettings() {

    return {

        amount:
            5 + (wave - 1) * 2,

        speed:
            18 + (wave - 1) * 2.5,

        spawn:
            Math.max(
                0.32,
                0.9 - (wave - 1) * 0.04
            )

    };

}


// ==========================================
// SPAWN MISSILE
// ==========================================

function spawnMissile() {

    const settings =
        getSettings();


    const missile =
        createMissile();


    const lane =
        Math.floor(
            Math.random() * 3
        ) - 1;


    // HIGH = DUCK
    // LOW = JUMP

    const high =
        Math.random() < 0.35;


    missile.position.set(

        lane * 3,

        high
            ? 3.1
            : 0,

        -110

    );


    missile.rotation.y =
        Math.PI;


    scene.add(
        missile
    );


    missiles.push({

        mesh: missile,

        speed:
            settings.speed *
            (
                0.85 +
                Math.random() * 0.3
            ),

        high: high,

        checked: false

    });


    waveMissiles++;

}


// ==========================================
// JUMP
// ==========================================

function jump() {

    if (
        !running ||
        !alive
    ) return;


    if (
        playerY <= 0.05
    ) {

        velocityY =
            12;

        ducking =
            false;

    }

}


// ==========================================
// DUCK
// ==========================================

function duckStart() {

    if (
        !running ||
        !alive
    ) return;


    ducking = true;

}


function duckStop() {

    ducking = false;

}


// ==========================================
// PLAYER UPDATE
// ==========================================

function updatePlayer(delta) {

    velocityY -=
        30 * delta;


    playerY +=
        velocityY * delta;


    if (
        playerY <= 0
    ) {

        playerY = 0;

        velocityY = 0;

    }


    player.position.y =
        playerY;


    const targetScale =
        ducking
            ? 0.55
            : 1;


    player.scale.y +=
        (
            targetScale -
            player.scale.y
        ) *
        Math.min(
            delta * 15,
            1
        );

}


// ==========================================
// COLLISION
// ==========================================

function missileHitsPlayer(missile) {

    const dx =
        Math.abs(
            missile.mesh.position.x
            -
            player.position.x
        );


    const dz =
        Math.abs(
            missile.mesh.position.z
            -
            player.position.z
        );


    if (
        dx > 2.2 ||
        dz > 2.5
    ) {

        return false;

    }


    // LOW MISSILE

    if (
        !missile.high
    ) {

        return playerY < 1.3;

    }


    // HIGH MISSILE

    if (
        missile.high
    ) {

        return !ducking;

    }


    return false;

}


// ==========================================
// EXPLOSION
// ==========================================

function explosion(position) {

    const geometry =
        new THREE.SphereGeometry(
            1,
            16,
            16
        );


    const material =
        new THREE.MeshBasicMaterial({
            color: 0xff7722,
            transparent: true,
            opacity: 1
        });


    const blast =
        new THREE.Mesh(
            geometry,
            material
        );


    blast.position.copy(
        position
    );


    scene.add(
        blast
    );


    let size = 1;


    function animateExplosion() {

        size += 0.18;

        material.opacity -= 0.07;

        blast.scale.setScalar(
            size
        );


        if (
            material.opacity <= 0
        ) {

            scene.remove(
                blast
            );

            geometry.dispose();

            material.dispose();

            return;

        }


        requestAnimationFrame(
            animateExplosion
        );

    }


    animateExplosion();

}


// ==========================================
// PLAYER HIT
// ==========================================

function playerHit() {

    lives--;


    document.getElementById(
        "lives"
    ).textContent =
        lives;


    explosion(
        player.position.clone()
    );


    cameraShake =
        0.5;


    if (
        lives <= 0
    ) {

        endGame();

    }

}


// ==========================================
// UPDATE MISSILES
// ==========================================

function updateMissiles(delta) {

    for (
        let i = missiles.length - 1;
        i >= 0;
        i--
    ) {

        const missile =
            missiles[i];


        missile.mesh.position.z +=
            missile.speed * delta;


        // Check collision

        if (
            !missile.checked &&
            missile.mesh.position.z > 2
        ) {

            missile.checked = true;


            if (
                missileHitsPlayer(
                    missile
                )
            ) {

                playerHit();

            }
            else {

                score++;


                document.getElementById(
                    "score"
                ).textContent =
                    score;

            }

        }


        // Remove missile

        if (
            missile.mesh.position.z > 20
        ) {

            scene.remove(
                missile.mesh
            );

            missiles.splice(
                i,
                1
            );

        }

    }

}


// ==========================================
// SPAWN / WAVES
// ==========================================

function updateWave(delta) {

    if (
        waveComplete
    ) {

        waveDelay -=
            delta;


        if (
            waveDelay <= 0
        ) {

            wave++;

            document.getElementById(
                "wave"
            ).textContent =
                wave;


            waveMissiles = 0;

            waveComplete = false;

            showWave(
                "WAVE " + wave,
                "GET READY!"
            );

        }


        return;

    }


    const settings =
        getSettings();


    spawnTimer +=
        delta;


    if (
        waveMissiles <
        settings.amount &&
        spawnTimer >=
        settings.spawn
    ) {

        spawnMissile();

        spawnTimer = 0;

    }


    if (
        waveMissiles >=
        settings.amount &&
        missiles.length === 0
    ) {

        waveComplete = true;

        waveDelay = 2;

    }

}


// ==========================================
// WAVE MESSAGE
// ==========================================

function showWave(
    title,
    subtitle
) {

    const message =
        document.getElementById(
            "waveMessage"
        );


    document.getElementById(
        "waveNumber"
    ).textContent =
        title;


    document.getElementById(
        "waveText"
    ).textContent =
        subtitle;


    message.classList.add(
        "show"
    );


    setTimeout(
        function() {

            message.classList.remove(
                "show"
            );

        },
        1500
    );

}


// ==========================================
// GAME OVER
// ==========================================

function endGame() {

    running = false;

    alive = false;

    ducking = false;


    document.getElementById(
        "finalScore"
    ).textContent =
        "Score: " + score;


    document.getElementById(
        "finalWave"
    ).textContent =
        "Wave: " + wave;


    document.getElementById(
        "gameOver"
    ).style.display =
        "flex";

}


// ==========================================
// KEYBOARD
// ==========================================

function keyboardDown(event) {

    if (
        event.code ===
        "ArrowUp"
    ) {

        event.preventDefault();

        jump();

    }


    if (
        event.code ===
        "ArrowDown"
    ) {

        event.preventDefault();

        duckStart();

    }

}


function keyboardUp(event) {

    if (
        event.code ===
        "ArrowDown"
    ) {

        event.preventDefault();

        duckStop();

    }

}


// ==========================================
// MOBILE CONTROLS
// ==========================================

function setupMobileControls() {

    const jumpButton =
        document.getElementById(
            "jumpButton"
        );


    const duckButton =
        document.getElementById(
            "duckButton"
        );


    if (
        jumpButton
    ) {

        jumpButton.addEventListener(
            "pointerdown",
            function(event) {

                event.preventDefault();

                jump();

            }
        );

    }


    if (
        duckButton
    ) {

        duckButton.addEventListener(
            "pointerdown",
            function(event) {

                event.preventDefault();

                duckButton.setPointerCapture(
                    event.pointerId
                );

                duckStart();

            }
        );


        duckButton.addEventListener(
            "pointerup",
            function(event) {

                event.preventDefault();

                duckStop();

            }
        );


        duckButton.addEventListener(
            "pointercancel",
            duckStop
        );

    }

}


// ==========================================
// CAMERA
// ==========================================

function updateCamera(delta) {

    if (
        cameraShake > 0
    ) {

        cameraShake -=
            delta;


        camera.position.x =
            (
                Math.random() - 0.5
            ) *
            cameraShake;


        camera.position.y =
            5 +
            (
                Math.random() - 0.5
            ) *
            cameraShake;

    }
    else {

        camera.position.x *=
            0.9;


        camera.position.y +=
            (
                5 -
                camera.position.y
            ) * 0.1;

    }

}


// ==========================================
// GAME LOOP
// ==========================================

function gameLoop(time) {

    requestAnimationFrame(
        gameLoop
    );


    const delta =
        Math.min(
            (time - lastTime) /
            1000,
            0.05
        );


    lastTime =
        time;


    if (
        running
    ) {

        updatePlayer(delta);

        updateMissiles(delta);

        updateWave(delta);

        updateCamera(delta);

    }


    // Animate stars

    const stars =
        scene.getObjectByName(
            "stars"
        );


    if (
        stars
    ) {

        stars.rotation.y +=
            delta * 0.01;

    }


    renderer.render(
        scene,
        camera
    );

}


// ==========================================
// RESIZE
// ==========================================

function resizeGame() {

    if (
        !camera ||
        !renderer
    ) return;


    camera.aspect =
        window.innerWidth /
        window.innerHeight;


    camera.updateProjectionMatrix();


    renderer.setSize(
        window.innerWidth,
        window.innerHeight
    );

}


// ==========================================
// START EVERYTHING
// ==========================================

window.addEventListener(
    "load",
    function() {

        setupGame();

    }
);
