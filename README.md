- 👋 Hi, I’m @fercho4tuzo
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
fercho4tuzo/fercho4tuzo is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Filtro de Datos Excel</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>
    <h1>Filtro de Datos desde Excel</h1>
    <input type="file" id="fileInput" accept=".xlsx, .xls" />

    <div>
        <label for="unidadResponsable">Unidad Responsable:</label>
        <input type="text" id="unidadResponsable" placeholder="Filtrar por unidad...">

        <label for="trabajador">Trabajador:</label>
        <input type="text" id="trabajador" placeholder="Filtrar por trabajador...">

        <label for="periodoLicencia">Período de Licencia:</label>
        <input type="text" id="periodoLicencia" placeholder="Filtrar por período...">

        <button id="filterButton">Filtrar</button>
    </div>

    <table id="dataTable">
        <thead>
            <tr>
                <th>Unidad Responsable</th>
                <th>Trabajador</th>
                <th>Período de Licencia</th>
            </tr>
        </thead>
        <tbody>
            <!-- Los datos se llenarán aquí dinámicamente -->
        </tbody>
    </table>

    <script>
        let excelData = [];

        document.getElementById('fileInput').addEventListener('change', handleFile, false);
        document.getElementById('filterButton').addEventListener('click', filterData);

        function handleFile(event) {
            const file = event.target.files[0];
            const reader = new FileReader();

            reader.onload = function (e) {
                const data = new Uint8Array(e.target.result);
                const workbook = XLSX.read(data, { type: 'array' });

                // Suponiendo que los datos están en la primera hoja
                const sheetName = workbook.SheetNames[0];
                const worksheet = workbook.Sheets[sheetName];
                excelData = XLSX.utils.sheet_to_json(worksheet);
                displayData(excelData);
            };

            reader.readAsArrayBuffer(file);
        }

        function displayData(data) {
            const tableBody = document.getElementById('dataTable').querySelector('tbody');
            tableBody.innerHTML = '';

            data.forEach(row => {
                const tr = document.createElement('tr');

                tr.innerHTML = `
                    <td>${row['Unidad Responsable'] || ''}</td>
                    <td>${row['Trabajador'] || ''}</td>
                    <td>${row['Período de Licencia'] || ''}</td>
                `;

                tableBody.appendChild(tr);
            });
        }

        function filterData() {
            const unidadResponsable = document.getElementById('unidadResponsable').value.toLowerCase();
            const trabajador = document.getElementById('trabajador').value.toLowerCase();
            const periodoLicencia = document.getElementById('periodoLicencia').value.toLowerCase();

            const filteredData = excelData.filter(row => {
                return (
                    (!unidadResponsable || (row['Unidad Responsable'] || '').toLowerCase().includes(unidadResponsable)) &&
                    (!trabajador || (row['Trabajador'] || '').toLowerCase().includes(trabajador)) &&
                    (!periodoLicencia || (row['Período de Licencia'] || '').toLowerCase().includes(periodoLicencia))
                );
            });

            displayData(filteredData);
        }
    </script>
</body>
</html>

