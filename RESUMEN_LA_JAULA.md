# LA JAULA — Resumen del proyecto (para continuar en Claude Code)

Juego de lucha 2D tipo Street Fighter en una jaula de MMA, para PC (navegador). Hecho por Pedro.

## Archivos (carpeta `Descargas\jaula.game`)
- `la-jaula.html` → el juego completo. Un solo archivo HTML + JavaScript (canvas), con todos los sprites incrustados en base64. Se abre con doble clic en el navegador.
- `Ranger_sprites.zip`, `Zarpa_sprites.zip`, `Grillete_sprites.zip`, `Titanio-X_sprites.zip` → sprites de cada personaje (carpetas x1 de 160x160 y x3 de 480x480, más `<nombre>_sprites.json` con frames, fps, bucle y suelo/aire de cada animación).
- `Fichas_luchadores.zip` → fichas de presentación 1040x640 (`ficha_<nombre>.png`) para la pantalla de selección.
- `ranger_lanzamiento.png` → imagen de referencia del lanzamiento de cuchillo.

## Cómo está organizado el código
- Todos los valores ajustables están en el objeto `CONFIG`, al principio del script.
- Dificultades de la CPU en `NIVELES` (Fácil, Normal, Difícil, Súper difícil).
- Golpes básicos en `ATAQUES`: tiempos y cajas de golpe [desde delante, desde altura, hasta delante, hasta altura].
- Personajes en `PERSONAJES` (ranger, zarpa, grillete, titanio-x). Si se añade uno ahí, sale solo en la selección. Fichas en `FICHAS`.
- Sprites: se usa la versión x1, escalada x1,8 sin suavizado. Los personajes miran a la derecha y se voltea la animación entera.
- Lógica y dibujo están separados: la lógica decide el estado y `eleccion()` decide qué frame se dibuja.

## Controles
- **Jugador 1:** W saltar · A/D moverse · S agacharse · F puñetazo · G patada · Mayús izq. mortal en el aire · P pausa.
- **Jugador 2:** flechas · L puñetazo · Ñ patada · Mayús dcha.
- **Bloqueo:** mantener "atrás". De pie bloquea golpes altos; agachado bloquea todo lo de suelo. Los ataques en salto no se pueden bloquear.

## Reglas generales
- Vida: 1000. Al mejor de 3 rondas, 99 s por ronda.
- Daños básicos: puño alto 60, bajo 50, en salto 70 · patada alta 80, baja 70, voladora 90, grulla 80.
- Lanzan por el aire: puño alto, patada alta y cualquier golpe entre dos que están saltando.
- **Recuperación con Mayús (solo contra la CPU):** sin Mayús gira y cae de cabeza (KO). Con Mayús 0,6 s cae de pie; con menos, cae de espaldas o boca abajo y se levanta.
- **2 jugadores:** sin Mayús; quien sale lanzado cae siempre de espaldas y se levanta.
- No se puede golpear al que está en el suelo ni mientras se levanta, más 0,4 s al ponerse de pie. No se ve ningún aviso de esto.
- KO por vida a 0: cámara lenta, vuela recto, rebota y suena "K.O." (voz grave del navegador).
- Golpe simultáneo: los dos reciben su daño y retroceden, sin salir volando.

## Barra de energía y especiales
- Barra azul en cargas, se rellena sola (unos 3,3 s por carga). Cada especial gasta 1 carga; sin energía sale el golpe normal.
- Ranger tiene 5 cargas; los demás, 3.
- **Regla general de especiales:** 200 de daño si conecta; 30 si se bloquea, sin ningún otro efecto. El bloqueo de un especial no puede dejar la vida a 0 (se queda en 1).
- Combinaciones: máximo 0,5 s entre teclas.

## Personajes
- **Ranger** (puños con cuchillos de adorno)
  - Especial CUCHILLO: atrás, atrás+abajo, F. Quita 150 (excepción). El cuchillo vuela a 500 px/s.
- **Zarpa** (garras)
  - Especial TELETRANSPORTE: adelante ×3, F. Aparece delante del rival y el zarpazo lo manda contra la pared; luego cae con las reglas normales. Invulnerable en los frames 2 y 3.
- **Grillete** (pesado)
  - Anda y ataca a 0,75 de velocidad y hace +20 de daño en golpes básicos.
  - Especial TIRÓN: S, adelante, F. La cadena va a unos 292 px/s con alcance de media pantalla. Si engancha: lo arrastra, cabezazo de 250 y lo manda volando. Si falla: la cadena se suelta y sale del mapa. Si la bloquean: quita 30 y vuelve.
- **Titanio-X** (robot)
  - Especial PUÑOS COHETE: S, adelante (sin F). Los puños vuelan a la velocidad del cuchillo y quitan 200.
- Todos: patada alta giratoria lenta (antiaéreo, golpea en los frames 4-5) y barrido agachado (delante en el frame 2, detrás en el frame 4).

## Menús
- Menú principal: 1 JUGADOR / 2 JUGADORES / DIFICULTAD.
- 1 jugador: se elige personaje con A/D y F o Intro; la CPU coge personaje al azar (si coincide, sale en otro color). La CPU usa especiales según la dificultad (nunca en Fácil).
- 2 jugadores: primero los nombres de los jugadores (se recuerdan en el navegador); luego cada uno elige con su cursor (J1: A/D y F · J2: flechas y L).
- Cuenta atrás 3, 2, 1 en la primera ronda.

## Pendientes o ideas comentadas
- Posible voz de "K.O." grabada en vez de la del navegador.
- Comprobar en un teclado español real la tecla Ñ del jugador 2.
