# Rama 01 — Identidad del Proyecto

## Nombre

**Piano-studio** (repositorio `Piano-proyectado`, GitHub: `cryaut/Piano-proyectado`).
La interfaz lo identifica como *Piano-studio v1.0*.

## Descripción corta

Piano virtual profesional y motor de práctica que corre completamente en el navegador:
síntesis multi-capa con samples reales, física de velocity, modos de juego con
scoring, grabación/exportación MIDI y un editor de piano roll.

## Problema que resuelve

- Tocar un piano físico requiere hardware costoso y espacio.
- Los sintetizadores web suenan "sintéticos" y no modelan dinámica.
- Practicar sin feedback no mide precisión ni progreso.

## Usuarios objetivo

- Estudiantes de piano que practican con teclado QWERTY o controladores MIDI.
- Profesores que crean lecciones con el editor de piano roll.
- Cualquier persona que quiera un piano con sonido de sample de cola en el navegador.

## Objetivo principal

Convertirse en un *practice workstation* de navegador: baja latencia, scoring
confiable, grabación, import/export y un editor de piano roll capaz de crear
lecciones jugables.

## Diferenciadores

- 3 capas de velocity con samples Salamander Grand Piano reales.
- Simulación de física de martillo sobre teclado QWERTY (velocidad de depresión).
- Cadena de efectos DSP propia (reverb por IR procedural, compresión, chorus, etc.).
- Soporte Web MIDI + WebHID (teclados magnéticos IROK/MG75).

## Estado actual

Prototipo funcional en desarrollo activo (ver rama 11).
