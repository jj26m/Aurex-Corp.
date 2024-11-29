<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aurex Corporation</title>
    <style>
        body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
        }

        header {
            background-color: red;
            padding: 10px;
            text-align: center;
        }

        header h1 {
            color: black;
        }

        .button {
            background-color: red;
            color: black;
            padding: 15px;
            margin: 10px;
            border: none;
            cursor: pointer;
            text-align: center;
            display: inline-block;
            font-size: 16px;
        }

        .button:hover {
            background-color: #d50000;
        }

        .form-container {
            margin: 20px;
            padding: 20px;
            background-color: #333;
            border-radius: 5px;
        }

        footer {
            text-align: center;
            margin-top: 20px;
        }

        footer p {
            color: white;
        }
    </style>
</head>
<body>

    <header>
        <h1>Aurex Corporation</h1>
    </header>

    <div id="main-menu">
        <button class="button" id="f1-button">Formula 1</button>
        <button class="button" id="energy-button">Aurex Energy</button>
    </div>

    <div id="f1-menu" style="display: none;">
        <button class="button" id="register-team">Registrar Equipo</button>
        <button class="button" id="register-driver">Registrar Piloto</button>
        <button class="button" id="registered-teams">Equipos Registrados</button>
        <button class="button" id="registered-drivers">Pilotos Registrados</button>
    </div>

    <div id="energy-menu" style="display: none;">
        <button class="button" id="tropical-mango">Tropical Mango (Original)</button>
        <button class="button" id="purple-pulse">Purple Pulse (Uva)</button>
    </div>

    <div id="form-container" style="display: none;" class="form-container"></div>

    <footer>
        <p>Desarrollado por: jj26m</p>
    </footer>

    <script>
        document.getElementById('f1-button').addEventListener('click', function() {
            document.getElementById('f1-menu').style.display = 'block';
            document.getElementById('main-menu').style.display = 'none';
        });

        document.getElementById('energy-button').addEventListener('click', function() {
            document.getElementById('energy-menu').style.display = 'block';
            document.getElementById('main-menu').style.display = 'none';
        });

        document.getElementById('register-team').addEventListener('click', function() {
            showForm('Registrar Equipo', 'team');
        });

        document.getElementById('register-driver').addEventListener('click', function() {
            showForm('Registrar Piloto', 'driver');
        });

        document.getElementById('tropical-mango').addEventListener('click', function() {
            showForm('Hacer Pedido - Tropical Mango', 'order', 'Tropical Mango');
        });

        document.getElementById('purple-pulse').addEventListener('click', function() {
            showForm('Hacer Pedido - Purple Pulse', 'order', 'Purple Pulse');
        });

        function showForm(formType, type, flavor = null) {
            let formHTML = '';
            if (type === 'team') {
                formHTML = `
                    <h3>${formType}</h3>
                    <form id="team-form">
                        <label>Nombre del Equipo:</label><br><input type="text" name="teamName"><br>
                        <label>Dueño (use @):</label><br><input type="text" name="owner"><br>
                        <label>Miembros:</label><br><input type="text" name="members"><br>
                        <label>Color:</label><br><input type="text" name="color"><br>
                        <label>Logo (Archivo):</label><br><input type="file" name="logo"><br><br>
                        <button type="submit" class="button">Enviar</button>
                    </form>
                `;
            } else if (type === 'driver') {
                formHTML = `
                    <h3>${formType}</h3>
                    <form id="driver-form">
                        <label>Nombre IC:</label><br><input type="text" name="icName"><br>
                        <label>Edad IC:</label><br><input type="text" name="icAge"><br>
                        <label>Usuario:</label><br><input type="text" name="user"><br>
                        <label>Número de Piloto:</label><br><input type="text" name="driverNumber"><br>
                        <label>Foto (Archivo):</label><br><input type="file" name="driverPhoto"><br><br>
                        <button type="submit" class="button">Enviar</button>
                    </form>
                `;
            } else if (type === 'order') {
                formHTML = `
                    <h3>${formType}</h3>
                    <form id="order-form">
                        <label>Nombre IC:</label><br><input type="text" name="icName"><br>
                        <label>Domicilio:</label><br><input type="text" name="address"><br>
                        <label>Sabor:</label><br><input type="text" value="${flavor}" disabled><br><br>
                        <button type="submit" class="button">Enviar</button>
                    </form>
                `;
            }

            document.getElementById('form-container').innerHTML = formHTML;
            document.getElementById('form-container').style.display = 'block';
        }

        document.getElementById('team-form')?.addEventListener('submit', function(e) {
            e.preventDefault();
            let formData = new FormData(this);
            sendToDiscord(formData, 'team');
        });

        document.getElementById('driver-form')?.addEventListener('submit', function(e) {
            e.preventDefault();
            let formData = new FormData(this);
            sendToDiscord(formData, 'driver');
        });

        document.getElementById('order-form')?.addEventListener('submit', function(e) {
            e.preventDefault();
            let formData = new FormData(this);
            sendToDiscord(formData, 'order');
        });

        function sendToDiscord(formData, type) {
            const webhookUrls = {
                team: 'https://discord.com/api/webhooks/1311859474004312064/FsFuWn6JBkIbK8bDkdr0ZQugA6LoRqvr0FYJQqa3x7V-I8YGz2QMhn-hrCSNxo19iS3G',
                driver: 'https://discord.com/api/webhooks/1311858886243778560/bNo814xfHYVQR6v-yJLpagg1gZovujZXMPFGVOPhQ8O4QmslJNskOKJDTchfOPpp8XET',
                order: 'https://discord.com/api/webhooks/1311858890866167859/OJ6WepXD0diVgVgnyygL3yRV0txYYpsInWAGCc4ug_UoxPgJXLiqSWh_oIxvnW3BLpdE'
            };

            let data = {};
            formData.forEach((value, key) => {
                data[key] = value;
            });

            const payload = {
                content: `Nuevo formulario de ${type}`,
                embeds: [{
                    title: type === 'team' ? 'Nuevo Equipo' : type === 'driver' ? 'Nuevo Piloto' : 'Nuevo Pedido',
                    fields: Object.keys(data).map((key) => ({
                        name: key.replace(/([A-Z])/g, ' $1').toUpperCase(),
                        value: data[key] || 'No proporcionado'
                    }))
                }]
            };

            fetch(webhookUrls[type], {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(response => response.json())
            .then(data => {
                alert('Formulario enviado con éxito');
            })
            .catch(error => {
                alert('Error al enviar el formulario');
            });
        }
    </script>

</body>
</html>
