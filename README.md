<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Registro de Bulto</title>
  <!-- Librería QRious desde CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></
  <style>
    body {
      font-family: sans-serif;
      max-width: 400px;
      margin: auto;
      text-align: center;
    }
    #qr {
      margin: 20px auto;
    }
    .mensaje {
      margin: 20px;
      font-size: 1.2em;
      color: green;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <h2>Bulto: <span id="idBulto">XYZ123</span></h2>
  <canvas id="qr"></canvas>
  <p>Fecha de apertura: <strong id="fechaApertura">–</strong></p>
  <p>Fecha hoy: <strong id="fechaEscaneo">–</strong></p>
  <div class="mensaje" id="mensajeEnvio">Ok de Envío ✅ – M. Muñoz</div>

  <script>
    // 1. Obtener ID del bulto desde la URL o usar "XYZ123" por defecto
    const params = new URLSearchParams(window.location.search);
    const id = params.get('id') || 'XYZ123';
    document.getElementById('idBulto').textContent = id;

    // 2. Consultar backend para obtener la fecha de apertura
    fetch(`/api/get-bulto?id=${id}`)
      .then(res => res.json())
      .then(data => {
        document.getElementById('fechaApertura').textContent = data.fechaApertura;
        // 3. Generar el QR solo una vez, apuntando a /scan?id=…
        qr.value = `${window.location.origin}/scan?id=${id}`;
      })
      .catch(err => {
        console.error('No se pudo obtener fecha de apertura', err);
        document.getElementById('fechaApertura').textContent = 'Desconocida';
      });

    // 4. Crear instancia QRious para renderizar el QR en el canvas
    const qr = new QRious({
      element: document.getElementById('qr'),
      size: 200,
      level: 'H'
    });

    // 5. Mostrar la fecha de escaneo (local, actual)
    document.getElementById('fechaEscaneo').textContent =
      new Date().toLocaleDateString();

    // Información sobre QRious y uso básico
    // QRious es una librería JavaScript de código abierto para
    // generar QR en canvas. Ejemplo básico:
    // var qr = new QRious({ element: canvasEl,
