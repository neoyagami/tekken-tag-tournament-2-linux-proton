# Tekken Tag Tournament 2 PC en Linux (experimental)

Prueba comunitaria para instalar y ejecutar mediante Proton la recompilación
estática de [Tekken Tag Tournament 2 para PC](https://github.com/shihabal3amri/tekken-tag-tournament-2-pc).

El proyecto upstream solo declara compatibilidad con Windows. Este launcher no
incluye el juego, el instalador, Title Updates, Proton ni otros binarios.

## Requisitos

- Una copia propia de `Tekken Tag Tournament 2 (USA, Europe)` para Xbox 360.
- Title Update 3: `TU_174Q22P_0000008000000.00000000000O2`.
- `ttt2_setup.exe` v0.1.7 de la release oficial.
- Steam y Proton Experimental.
- Al menos 13 GB libres para la instalación.

El launcher solo acepta el instalador cuyo SHA-256 oficial es:

```text
2649442f5a82122bbb432cad93f612b241aa842c4dfb68330a7ad14b97638941
```

## Uso

Coloca `ttt2_setup.exe`, la ISO y TU3 junto a `ttt2-linux`. Después ejecuta:

```bash
chmod +x ttt2-linux
./ttt2-linux status
./ttt2-linux install
```

En el selector de archivos de Windows, los archivos de Linux aparecen bajo la
unidad `Z:`. El launcher imprime las rutas exactas antes de abrir el instalador.
Conserva la carpeta de instalación predeterminada para que el juego quede dentro
del prefijo dedicado.

Después de completar el instalador:

```bash
./ttt2-linux run
```

Para añadirlo al menú de aplicaciones y crear un icono en el escritorio:

```bash
./ttt2-linux install-desktop
```

Si están disponibles `wrestool` e `icotool`, el comando extrae automáticamente
el icono de 256×256 del ejecutable instalado. La imagen queda en `data/icons/`
y no se incorpora al repositorio.

No es necesario añadir el ejecutable manualmente como juego ajeno a Steam. El
port inicia un proceso auxiliar, carga `steam_api64.dll` y se conecta al cliente
nativo de Steam como Spacewar (App 480) incluso cuando se lanza con este script.

Variables opcionales:

- `TTT2_PROTON`: ruta a otro ejecutable `proton`.
- `TTT2_ISO`: ruta a la imagen del juego.
- `TTT2_TU3`: ruta al archivo TU3.
- `TTT2_DATA_ROOT`: ubicación del prefijo y logs.
- `TTT2_CACHE_ROOT`: ubicación de la copia verificada del instalador.
- `TTT2_PROTON_LOG=0`: desactiva el log detallado de Proton.
- `TTT2_USE_XALIA=1`: reactiva Xalia; se deshabilita por defecto porque produjo
  excepciones repetidas de identificador de ventana durante la instalación.

## Alcance de esta prueba

El juego usa D3D11, que Proton traduce mediante DXVK. No se aplican los parches
de ICU, `mscms.dll`, VKD3D ni `rexgpu-xenos` necesarios en otros ports. El modo
offline y la integración de Steamworks/Spacewar fueron comprobados en Bazzite
con Proton Experimental 11 y una NVIDIA RTX 4070. Funcionan la identificación
de la cuenta, consulta de sesiones, creación de lobbies y Steam Networking. Aún
falta comprobar una pelea completa entre dos equipos Linux distintos.

La v0.1.7 incorpora rollback netcode, invitaciones de amigos desde las salas y
servidores comunitarios para Ranked Match, leaderboards, equipos, replays y
ghosts. Ambos jugadores deben usar la misma versión del port.

## Resultado comprobado

- Instalador v0.1.7 ejecutado correctamente mediante Proton.
- ISO y TU3 validados y 13 GB de datos generados.
- D3D11 traducido mediante DXVK en la RTX 4070.
- Salida XAudio2 funcional.
- Perfil local, menú principal y configuración de controles funcionales.
- Steamworks inicializado como Spacewar/App 480.
- NAT abierto, búsqueda de sesiones y publicación de lobbies funcionales.
- Runtime de rollback y nueva entrada `SERVER` confirmados en v0.1.7.
- La precarga bajo Proton conserva miles de shaders DXBC utilizables, pero
  `vkd3d-shader` descarta algunas variantes con avisos `Invalid shader`; el
  juego inicia y carga la caché sin recompilar shaders durante el menú.
- Xalia produce excepciones de identificador de ventana en el instalador; se
  desactiva por defecto con `PROTON_USE_XALIA=0`.

Los logs del juego están en:

```text
data/proton-prefix/pfx/drive_c/users/steamuser/AppData/Local/TTT2Recomp/log.txt
```

## Privacidad y distribución

`data/`, `cache/`, las imágenes de disco, Title Updates, archivos ZIP/7Z y el
instalador oficial están ignorados por Git. No deben publicarse. Este repositorio
solo distribuye el launcher y su documentación; Tekken, Proton, Steamworks y el
port upstream conservan sus respectivas licencias y propietarios.
