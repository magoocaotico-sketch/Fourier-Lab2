Autores: Victor Manuel Chacón Hidalgo.	                 Fecha: 28-1-2026
         José Diego Veranes Malo de Molina.              Versión 1.0


# Fourier Lab: Blue Edition

![Version](https://img.shields.io/badge/version-1.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)

Una aplicación web interactiva para visualizar y manipular señales en el dominio del tiempo y la frecuencia, con simulación de filtros analógicos en tiempo real.

## 🎯 Características Principales

- **Generador de Funciones**: Crea señales personalizadas combinando hasta 5 armónicos
- **Presets de Formas de Onda**: Seno, cuadrada, triangular y sierra
- **Filtros Analógicos (VCF)**: Butterworth y Chebyshev con orden configurable
- **Visualización en Tiempo Real**: Osciloscopio y diagramas de Bode
- **Interfaz Intuitiva**: Diseño tipo sintetizador modular con estética retro

## 📸 Capturas de Pantalla

La aplicación presenta tres visualizaciones principales:
- **Osciloscopio**: Muestra la señal de entrada (verde) y salida filtrada (azul)
- **Diagrama de Magnitud**: Respuesta en frecuencia del filtro con barras de espectro
- **Diagrama de Fase**: Respuesta de fase del filtro

## 🚀 Inicio Rápido

### Requisitos
- Navegador web moderno (Chrome, Firefox, Safari, Edge)
- No requiere instalación de dependencias

### Instalación

1. Clona el repositorio:
```bash
git clone https://github.com/tu-usuario/fourier-lab.git
cd fourier-lab
```

2. Abre el archivo `index.html` en tu navegador:
```bash
# En Linux/Mac
open index.html

# En Windows
start index.html
```

O simplemente arrastra el archivo `index.html` a tu navegador.

### Estructura de Archivos

```
fourier-lab/
│
├── index.html          # Estructura HTML principal
├── styles.css          # Estilos y tema visual
├── script.js           # Lógica de la aplicación
└── README.md          # Este archivo
```

## 🎮 Guía de Uso

### Generador de Funciones

1. **Seleccionar Preset**: Haz clic en los botones (Sen, Cuad, Tri, Sierr) para cargar formas de onda predefinidas
2. **Ajustar Armónicos**: Cada armónico (H1-H5) tiene controles para:
   - **Amp**: Amplitud del armónico (0-100)
   - **Hz**: Frecuencia del armónico (50-5000 Hz)

### Configuración del Filtro

- **Tipo de Filtro**:
  - **Butterworth**: Respuesta plana en la banda de paso
  - **Chebyshev**: Mayor pendiente pero con rizado en la banda de paso

- **Frecuencia de Corte (Fc)**: Define el punto de atenuación del filtro (100-5000 Hz)
- **Orden (Polos n)**: Controla la pendiente del filtro (1-8 polos)

### Controles Adicionales

- **Botón ▮**: Pausa/reanuda la animación del osciloscopio
- **CLIP Indicator**: LED rojo que alerta cuando la señal satura
- **RESET SISTEMA**: Restaura todos los parámetros a valores por defecto

## 🔧 Características Técnicas

### Procesamiento de Señales

- **Síntesis Aditiva**: Generación de señales mediante suma de armónicos
- **Filtros IIR**: Implementación de filtros Butterworth y Chebyshev
- **Función de Transferencia**: Cálculo en tiempo real de magnitud y fase
- **Diagramas de Bode**: Visualización logarítmica de la respuesta en frecuencia

### Tecnologías Utilizadas

- **HTML5 Canvas**: Renderizado de gráficas en tiempo real
- **JavaScript ES6+**: Lógica de procesamiento de señales
- **CSS Grid**: Layout responsivo y adaptable
- **ResizeObserver API**: Redimensionamiento dinámico de canvas

## 🎨 Paleta de Colores

```css
--signal-in: #00ff00      /* Verde neón - Señal de entrada */
--signal-out: #00aaff     /* Azul cian - Señal de salida */
--spec-yellow: #ffff00    /* Amarillo - Magnitud */
--phase-red: #ff003c      /* Rojo - Fase */
--grid-color: #002200     /* Verde oscuro - Rejilla */
```

## 📐 Arquitectura del Código

### Estructura de Estado

```javascript
state = {	
    harmonics: [
        { amp: 80, freq: 200 },
        { amp: 0, freq: 400 },
        // ... hasta 5 armónicos
    ],
    filter: { 
        fc: 1000,      // Frecuencia de corte
        n: 2,          // Orden del filtro
        type: 'butterworth'
    },
    animating: true,
    time: 0
}
```

### Funciones Principales

- `setPreset(type)`: Carga formas de onda predefinidas
- `calculateTransferFunction(f)`: Calcula H(jω) para una frecuencia dada
- `renderTimeDomain()`: Dibuja el osciloscopio
- `renderBode()`: Dibuja los diagramas de magnitud y fase
- `animate()`: Loop principal de animación

## 🎓 Aplicaciones Educativas

Esta herramienta es ideal para:

- **Cursos de Procesamiento de Señales**: Visualización de conceptos de Fourier
- **Diseño de Filtros**: Experimentación con diferentes topologías
- **Síntesis de Audio**: Comprensión de formas de onda complejas
- **Análisis de Sistemas**: Estudio de respuesta en frecuencia

## 🐛 Resolución de Problemas

### El canvas no se muestra correctamente
- Verifica que tu navegador soporte HTML5 Canvas
- Intenta recargar la página con Ctrl+F5

### La animación es lenta
- Reduce el número de armónicos activos
- Disminuye el orden del filtro
- Usa el botón de pausa cuando no necesites animación

### Los controles no responden
- Asegúrate de que JavaScript esté habilitado
- Verifica la consola del navegador por errores

## 📝 Notas de Versión

### v1.0 (Versión Actual)
- Implementación inicial de 5 armónicos
- Filtros Butterworth y Chebyshev
- Visualización en tiempo real
- Interfaz estilo sintetizador modular
- Diseño responsivo

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Para cambios importantes:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/NuevaCaracteristica`)
3. Commit tus cambios (`git commit -m 'Añade nueva característica'`)
4. Push a la rama (`git push origin feature/NuevaCaracteristica`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo LICENSE para más detalles.

## 🙏 Agradecimientos

- Inspirado en sintetizadores modulares clásicos
- Basado en teoría de procesamiento digital de señales
- Diseño visual influenciado por equipos de audio profesional

## 📬 Contacto

Para preguntas, sugerencias o reportar bugs, por favor abre un issue en el repositorio.

---

**Desarrollado con 💙 para la comunidad de procesamiento de señales**

