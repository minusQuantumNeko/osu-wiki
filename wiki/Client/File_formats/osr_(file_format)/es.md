---
outdated_since: 3beefe4f5a50484b1a97ebdf019fd3581d4ca0c4
outdated_translation: true
---

# .osr (formato de archivo)

**.osr** es un formato de archivo que contiene información sobre una repetición de osu!. Para usarlo, se necesita el beatmap especificado por el archivo en la carpeta «Songs».

## Tipos de datos

| Nombre | Bytes | Descripción |
| :-- | :-- | :-- |
| Byte | 1 | Un único valor de 8 bits. |
| Short | 2 | Un valor little endian de 2 bytes. |
| Entero | 4 | Un valor little endian de 4 bytes. |
| Long | 8 | Un valor little endian de 8 bytes. |
| LEB128 | Variable | Un entero de longitud variable. Véase [LEB128](https://en.wikipedia.org/wiki/LEB128). |
| Cadena | Variable | Tiene tres partes; un solo byte que será 0x00, que indica que las dos partes siguientes no están presentes, o 0x0b (11 decimal), que indica que las dos partes siguientes están presentes. Si es 0x0b, habrá un ULEB128, que representa la longitud en bytes de la siguiente cadena, y luego la cadena en sí, codificada en UTF-8. Véase [UTF-8](https://es.wikipedia.org/wiki/UTF-8) |

## Formato

Los desplazamientos de bytes no se incluyen en esta tabla debido a los valores de longitud variable.

| Tipo de dato | Descripción |
| :-- | :-- |
| Byte | Modo de juego de la repetición (0 = osu! Standard, 1 = Taiko, 2 = Catch the Beat, 3 = osu!mania) |
| Entero | Versión del juego cuando se creó la repetición (por ejemplo, 20131216) |
| Cadena | HashMD5 del beatmap de osu! |
| Cadena | Nombre del jugador |
| Cadena | Hash MD5 de la repetición de osu! (incluye ciertas propiedades de la repetición). |
| Short | Número de 300s |
| Short | Número de 100s en standard, 150s en Taiko, 100s en CTB, 100s en mania |
| Short | Número de 50s en standard, frutas pequeñas en CTB, 50s en mania |
| Short | Número de Gekis en standard, Max 300s en mania |
| Short | Número de Katus en standard, 200s en mania |
| Short | Número de fallos |
| Entero | Puntuación total mostrada en el informe de la puntuación |
| Short | Mayor combo mostrado en el informe de la puntuación |
| Byte | Combo perfecto/completo (1 = sin fallos, sin sliders rotos y sin sliders terminados antes de tiempo) |
| Entero | Mods usados. Véase a continuación la lista de valores de los mods. |
| Cadena | Gráfico de la barra de vida: pares u/v separados por comas, donde u es el tiempo en milisegundos en la canción y v es un valor en coma flotante de 0 o 1 que representa la cantidad de vida que tienes en el momento dado (0 = la barra de vida está vacía, 1 = la barra de vida está llena) |
| Long | Marca de tiempo ([Pasos de Windows](https://learn.microsoft.com/es-es/dotnet/api/system.datetime.ticks)) |
| Entero | Longitud en bytes de los datos comprimidos de la repetición |
| Byte Array | Datos de la repetición comprimidos |
| Long | ID de la puntuación en línea |
| Double | Información adicional sobre los mods. Solo presente si [Target Practice](/wiki/Gameplay/Game_modifier/Target_Practice) está activado. |

**Información adicional de los mods:**

| Mod | Información almacenada |
| :-- | :-- |
| Target Practice | Precisión total de todos los golpes. Divide esto por la cantidad de objetivos en el mapa para encontrar la precisión que se muestra en el juego. |

Los datos restantes contienen información sobre el movimiento del ratón y las pulsaciones de teclas en una cadena [LZMA](https://es.wikipedia.org/wiki/LZMA).

Cuando se descomprime, el texto contiene datos separados por comas. Cada pieza denota una acción, representada por 4 números en la forma: `w | x | y | z.`

| Parte | Tipo de dato | Descripción |
| :-- | :-- | :-- |
| w | Long | Tiempo en milisegundos desde la acción anterior |
| x | Float | Coordenadas x del cursor de 0 a 512 |
| y | Float | Coordenadas y del cursor de 0 a 384 |
| z | Entero | Combinación bit a bit de teclas/botones del ratón presionados (M1 = 1, M2 = 2, K1 = 4, K2 = 8, Smoke = 16) (K1 siempre se usa con M1; K2 siempre se usa con M2: 1+4=5; 2+8=10) |

En las repeticiones creadas en la versión `20130319` o posteriores, la semilla RNG entera de 32 bits usada para la puntuación se codificará en un cuadro de adicional de la repetición al final de la cadena LZMA, con el formato `-12345|0|0|seed`.

## Mods

Como se ven en la [API de osu!](https://github.com/ppy/osu-api/wiki#mods).

| Mod | Valor (BitOffset) | Comentario |
| :-- | :-- | :-- |
| None | 0 |  |
| NoFail | 1 (0) |  |
| Easy | 2 (1) |  |
| TouchDevice | 4 (2) | Reemplaza al mod NoVideo no usado |
| Hidden | 8 (3) |  |
| HardRock | 16 (4) |  |
| SuddenDeath | 32 (5) |  |
| DoubleTime | 64 (6) |  |
| Relax | 128 (7) |  |
| HalfTime | 256 (8) |  |
| Nightcore | 512 (9) | Siempre usado con DT: 512 + 64 = 576. Reemplaza al mod Taiko no usado |
| Flashlight | 1024 (10) |  |
| Autoplay | 2048 (11) |  |
| SpunOut | 4096 (12) |  |
| Relax2 | 8192 (13) | Autopilot |
| Perfect | 16384 (14) |  |
| Key4 | 32768 (15) |  |
| Key5 | 65536 (16) |  |
| Key6 | 131072 (17) |  |
| Key7 | 262144 (18) |  |
| Key8 | 524288 (19) |  |
| keyMod | 1015808 | k4+k5+k6+k7+k8 |
| FadeIn | 1048576 (20) |  |
| Random | 2097152 (21) |  |
| LastMod | 4194304 (22) | Cinema |
| TargetPractice | 8388608 (23) | Solo en osu!cuttingedge |
| Key9 | 16777216 (24) |  |
| Coop | 33554432 (25) |  |
| Key1 | 67108864 (26) |  |
| Key3 | 134217728 (27) |  |
| Key2 | 268435456 (28) |  |
| ScoreV2 | 536870912 (29) |  |
| Mirror | 1073741824 (30) |  |
