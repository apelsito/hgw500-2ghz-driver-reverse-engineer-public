# Realtek 2.4 GHz HGW500

Reconstrucción documentada del driver de **2,4 GHz Realtek RTL8192EE** para el MitraStar HGW-500BNA-QCv2 A2, ARC700 y Linux `5.10.260-openmitrastar-radiohw`.

Repositorio privado: `apelsito/Realtek-2.4GHz-HGW500`. Equipo: HGW-500BNA-QCv2 A2.

El repositorio conserva las fuentes originales, los cambios del port, las operaciones recuperadas desde los módulos y las herramientas de comprobación offline. **Reconstrucción exacta offline conseguida:** 27 unidades y cuatro módulos se han compilado dos veces, con SHA-256 idéntico a la referencia. Una compilación o un análisis estático no certifican asociación, tráfico ni aceptación del firmware en el equipo físico.

## Estructura

| Carpeta | Contenido |
|---|---|
| `original/linux-5.10.260/` | Subconjunto upstream original: rtlwifi, RTL8192EE, btcoexist y API mac80211 |
| `original/port-overlay/` | Fuentes disponibles del port antes de esta reconstrucción, conservadas byte a byte |
| `original/kernel-api/` | Cabeceras originales necesarias para identificar las tablas y sus offsets |
| `src/overlay/` | Receta y cambios necesarios para reconstruir el árbol del driver |
| `src/baseline-exact/` | Fuentes que reconstruyen exactamente los cuatro módulos de referencia |
| `build-context/f363/` | Cabeceras y contexto mínimo de compilación; no contiene objetos ni módulos heredados |
| `scripts/` | Importación, conservación de comentarios, inventario de ops, reconstrucción y REA |
| `tests/` | Pruebas offline de las herramientas |
| `docs/` | Documentación, comentarios originales y límites de verificación |
| `evidence/` | Manifiestos, hashes y resultados; fuentes y binarios de referencia se distinguen |
| `.work/` | Compilaciones y ELF de análisis locales; excluidos de Git |

## Comentarios originales

[`docs/ORIGINAL_COMMENTS.md`](docs/ORIGINAL_COMMENTS.md) contiene **todos los comentarios literales** de los archivos C, cabeceras, Makefile y Kconfig importados en `original/`, con sus archivos y líneas. El índice [`evidence/original-comments.json`](evidence/original-comments.json) conserva también offsets de byte y hashes.

Los comentarios originales proceden de fuentes disponibles. Los binarios compilados no retienen comentarios; el pseudocódigo generado por REA se conserva separado y no se presenta como código original.

```powershell
python scripts/extract_original_comments.py --check
python scripts/verify_original_sources.py
python -m unittest discover -s tests -v
```

## Referencia del driver

Referencia extraída previamente de la imagen F367r9 y contrastada con el contexto F363 retenido:

| Módulo | Bytes | SHA-256 |
|---|---:|---|
| `rtl8192ee.ko` | 102736 | `f014ad53084e1eb3cf7a9083913388ac907f0d254478dd63b0bc9ca054394e85` |
| `rtl_pci.ko` | 45680 | `6f85efa424c6aa5b693982ae074d268d2312a3f9262238e8c6f5eda16199df63` |
| `rtlwifi.ko` | 77412 | `9b7988816abb5065cd5a2a39defba608d073646aac15c75e244ac5cd95e096ee` |

Estos tres módulos suman 225828 bytes, 370 símbolos de función definidos y 2326 relocaciones RELA. Los binarios originales se mantienen fuera de Git. La pila completa incluye mac80211, cfg80211, btcoexist y módulos criptográficos: este inventario no atribuye todo ese código al módulo RTL8192EE.

## Reconstrucción

El archivo upstream Linux 5.10.260 está fijado por SHA-256 `47a8297131505b7d38338d39ad8a9d592507d33c695b27b1bbe55f25869a6b84`. `scripts/import_original_sources.py` extrae únicamente el subconjunto declarado y comprueba su identidad.

`scripts/reconstruct_baseline.py --help` describe los insumos explícitos: archivo upstream, contexto de compilación histórico retenido y prefijo del compilador ARC. Trabaja en directorios nuevos, realiza dos compilaciones y compara unidades con los objetos de referencia. El árbol completo del kernel F358 con sus parches no está recuperado; los resultados informan por separado si las unidades del driver son idénticas y si quedan diferencias.

El contexto mínimo está en `build-context/f363/`. La evidencia [`evidence/reconstruction-exact-paired.json`](evidence/reconstruction-exact-paired.json) conserva los hashes de ambas compilaciones y la comparación exacta. Véase [`docs/RECONSTRUCTION.md`](docs/RECONSTRUCTION.md) para reproducirlo con el archivo upstream y el toolchain ARC fijados.

REA recorrió 370 símbolos de función, equivalentes a 366 entradas únicas, con pseudocódigo y ensamblador conservados. Las tablas de ops contienen 91 callbacks poblados; el registro de timers, trabajo diferido, tasklets, IRQ e IO añade 20 destinos distintos. Esos inventarios documentan operaciones concretas. La recuperación exacta del binario se acredita por compilación y hashes, de forma independiente de las limitaciones del pseudocódigo REA.

## Alcance y conservación

Los scripts del repositorio trabajan offline. No acceden al router, UART, radio, TFTP ni flash. Los ELF derivados para REA son exclusivamente de análisis y no pueden cargarse ni empaquetarse como firmware.

Este repositorio no contiene firmware OEM, imágenes personalizadas del equipo, dumps de particiones, calibración, contraseñas, PSK o claves privadas. La autorización para reconstrucción y publicación privada no sustituye a los requisitos de instalación y aceptación física.

## Licencias y procedencia

Las fuentes Linux conservan sus avisos SPDX y copyright. Véase [`NOTICE.md`](NOTICE.md), el `COPYING` original y `LICENSES/preferred/GPL-2.0`. Los hashes y la procedencia están en los manifiestos de `original/`. Los componentes de procedencia distinta mantienen sus avisos propios.
