# Apuntes de ciencias de datos

Material de estudio y práctica sobre redes neuronales: desde el perceptrón y el descenso del gradiente hasta las redes convolucionales (CNN) y la transferencia de aprendizaje. Incluye cuadernos Jupyter y una aplicación web para reconocer dígitos escritos a mano.

## Contenido

- `Perceptron/`: explicación de pesos, sesgo, funciones de activación, error y aprendizaje, junto con cuadernos de perceptrones y redes neuronales con scikit-learn, Keras y TensorFlow. Empieza por [la guía del perceptrón](Perceptron/Readme.md).
- `Avanzada_Cuaderno_8_CNN_Transferencia_de_aprendizaje.ipynb`: ejercicios de clasificación de imágenes y transferencia de aprendizaje con VGG16.
- `Grafana/`: cuaderno de entrenamiento de una CNN para dígitos MNIST, cuaderno de predicción y aplicación en `app.py`. A pesar del nombre de la carpeta, la interfaz está hecha con Gradio.
- `Prueba GPU.ipynb`: ejecuta `nvidia-smi` para consultar una GPU NVIDIA y su controlador.
- `imagenes/`: ilustraciones de apoyo para los apuntes.

## Cómo funciona la aplicación

1. El usuario dibuja un dígito del 0 al 9 en el lienzo de Gradio.
2. La aplicación convierte el dibujo a escala de grises, recorta el trazo y lo escala conservando su proporción.
3. Lo coloca en una imagen de 28 × 28 píxeles, lo centra por su centro de masa y normaliza sus valores entre 0 y 1.
4. La CNN cargada desde `numerosC2.keras` calcula las probabilidades de las diez clases.
5. La interfaz muestra las tres clases con mayor probabilidad y una vista de la imagen que recibe la red.

## Preparación local

Necesitas Git y Python con `pip` y soporte para entornos virtuales. Usa una versión de Python compatible con TensorFlow. Se requiere conexión a Internet para instalar dependencias y descargar los datos y pesos utilizados por los cuadernos. Una GPU es opcional para la app y puede acelerar el entrenamiento.

```bash
git clone git@github.com:jus7code/grafanarod.git
cd grafanarod
python -m venv .venv
```

Activa el entorno en Linux o macOS:

```bash
source .venv/bin/activate
```

En Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Instala las dependencias de la aplicación:

```bash
python -m pip install -r Grafana/requirements.txt
```

## Generar el modelo de dígitos

**El archivo `numerosC2.keras` no está incluido en el repositorio.** Debes generarlo antes de iniciar `app.py`.

1. Abre el cuaderno `Grafana/Avanzada_Cuaderno_8_A2_CNN_neuronales_convolucionales_Números_Modo_2_(Uso_de_RNC).ipynb` en Google Colab.
2. Ejecuta las celdas en orden, completa la conexión a Google Drive cuando se solicite y entrena el modelo con MNIST.
3. Ejecuta las celdas de guardado. El cuaderno contempla guardar los archivos en la carpeta `Cuaderno8_CNN` de Mi unidad.
4. Descarga `numerosC2.keras` y cópialo a `Grafana/`, junto a `app.py`.

También puedes utilizar un modelo compatible ya entrenado: debe aceptar entradas de forma `(1, 28, 28, 1)` y devolver diez probabilidades correspondientes a los dígitos del 0 al 9.

## Ejecutar la aplicación

Con el entorno virtual activado y el modelo en su ubicación:

```bash
cd Grafana
python app.py
```

Abre en el navegador la dirección local que Gradio imprima en la terminal. Dibuja un dígito grande y centrado para ver su clasificación. Para detener el servidor, pulsa `Ctrl+C`.

Ejecuta el comando desde `Grafana/`: el código busca `numerosC2.keras` en el directorio de trabajo actual.

## Ejecutar los cuadernos

Puedes abrir los archivos `.ipynb` en Google Colab, o usar Jupyter localmente:

```bash
# Desde la raíz del repositorio, con el entorno virtual activado
python -m pip install jupyterlab matplotlib scikit-learn pillow
python -m jupyter lab
```

Selecciona un cuaderno y ejecuta sus celdas en orden. Las dependencias de TensorFlow, NumPy, OpenCV y Gradio ya se instalan con `Grafana/requirements.txt`.

Algunos cuadernos contienen integraciones con Google Drive, comandos de Colab y rutas absolutas bajo `/content/`. En una ejecución local debes adaptar esas rutas a la ubicación de tus datos. El cuaderno de transferencia de aprendizaje también requiere revisar las rutas de entrenamiento y validación después de extraer el conjunto de gatos y perros. Si usas la alternativa de conexión a Drive mediante OAuth, instala además `google-auth-oauthlib` y `google-api-python-client` y sigue las indicaciones del cuaderno.

`Grafana/App_Prediccion_Digitos.ipynb` permite probar la predicción desde un cuaderno y recupera el modelo de Google Drive.

## Problemas frecuentes

- **No se encuentra `numerosC2.keras`:** genera o descarga el modelo y colócalo dentro de `Grafana/`; inicia la app desde esa carpeta.
- **Falta un módulo:** activa el entorno virtual e instala las dependencias con `python -m pip install -r Grafana/requirements.txt` desde la raíz.
- **No se encuentra un conjunto de imágenes:** comprueba la descarga, la extracción y las rutas utilizadas por el cuaderno.
- **`nvidia-smi` no está disponible:** ese cuaderno requiere una GPU NVIDIA con su controlador; la app puede ejecutarse en CPU.

Las dependencias del proyecto no fijan versiones y los cuadernos pueden necesitar ajustes según el entorno. No se incluye un modelo preentrenado ni una configuración de GPU automática.
