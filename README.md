# Simulador web: control por linealización instantánea con NNARX

Simulador en línea del control por **linealización instantánea** (Nørgaard et al., 2000) con un modelo neuronal **NNARX**, sin RLS, sobre el modelo de 47 estados de la planta termoeléctrica de 600 MW de Usoro (1977).

**Abrir el simulador:** https://marioabdel01-arch.github.io/simulador-nnarx-usoro/

## Qué permite

- **Pruebas:** las dos rampas de Amin et al. (1984), 90 → 70 % y 70 → 90 % a 10 %/min, y las cuatro cargas de Usoro (1977): V.1 100 → 77,5 %, V.2 77,5 → 50 %, V.3 50 → 77,5 % y V.4 77,5 → 100 %, a 15 %/min.
- **Modelos:** NNARX 3×4, que recibe la demanda de carga, y NNARX 3×3, que no la recibe.
- **Controladores:**
  - LI: la ley de mínima varianza ponderada de Amin sobre la red linealizada en cada periodo;
  - LI-I: con acción integral;
  - el PI original de Usoro.
- **Lectura:** las salidas y los mandos con el PI de fondo, y los índices IAE e ITAE frente al PI.
- **Exportación:** CSV y PNG.

## Cómo funciona

Cada 20 s se deriva la red respecto al mando en el punto de operación actual. Con ese modelo lineal local se calcula el mando que minimiza ‖ŷ(t+1) − w‖² + λ‖u − u_ref‖². La red queda fija: no hay estimación en línea.

## Planta

Todas las corridas, también las del PI, usan la planta con la hipótesis de Amin et al. (1984): el recalentado se controla solo con la inclinación de quemadores, sin atemperación. Arrancan en el estado estable de cada nivel. Por eso, en V.1–V.4 el PI no coincide con el del simulador de la reproducción, que corre la planta completa de Usoro con sus condiciones iniciales tabuladas.

## Verificación

Es una traducción a JavaScript del código de MATLAB (`stc-neuronal`). Se compararon 24 corridas: las 6 pruebas con el PI, LI y LI-I con el 3×4, más las 2 rampas de Amin con el 3×3. En todas, los IAE coinciden con los de MATLAB/Octave con una diferencia relativa menor a 10⁻⁷.

La planta es la del [simulador de la reproducción de Amin et al. (1984)](https://marioabdel01-arch.github.io/simulador-amin-usoro/).

## Autores

Mario Abdel Guerra Pérez y Enrique Quintero-Mármol Márquez, TecNM/CENIDET. Licencia MIT.
