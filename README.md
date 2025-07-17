<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Malla Curricular - Medicina Veterinaria y Zooctenia</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f4f8;
            margin: 0;
            padding: 20px;
            color: #333;
        }
        h1 {
            text-align: center;
            color: #2a9d8f;
        }
        .container {
            display: grid;
            grid-template-columns: repeat(10, 1fr);
            gap: 20px;
            margin: 20px;
        }
        .semester {
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            padding: 15px;
        }
        .semester h2 {
            text-align: center;
            color: #e76f51;
        }
        .subject {
            background-color: #e9c46a;
            padding: 10px;
            border-radius: 6px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .subject:hover {
            background-color: #f4a261;
        }
        .subject.aprobada {
            background-color: #2a9d8f;
            color: white;
            cursor: default;
        }
        .subject.bloqueada {
            background-color: #f4a261;
            cursor: not-allowed;
        }
        .tooltip {
            visibility: hidden;
            background-color: rgba(0, 0, 0, 0.8);
            color: #fff;
            padding: 5px;
            border-radius: 5px;
            position: absolute;
            z-index: 1;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .subject.bloqueada:hover .tooltip {
            visibility: visible;
            opacity: 1;
        }
    </style>
</head>
<body>
    <h1>Malla Curricular - Medicina Veterinaria y Zooctenia</h1>

    <div class="container">
        <!-- SEMESTRES (cada columna de la malla) -->
        <div class="semester" id="semestre1">
            <h2>PRIMER SEMESTRE</h2>
            <div class="subject" onclick="toggleSubject(this)" data-id="bio-celular" data-prereqs="[]">
                BIOLOGÍA CELULAR Y MOLECULAR
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="intro-sistemas" data-prereqs="[]">
                INTRODUCCIÓN A SISTEMAS ORGÁNICOS
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="practicas-basicas1" data-prereqs="[]">
                PRÁCTICAS BÁSICAS I
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="practicas-basicas2" data-prereqs="[]">
                PRÁCTICAS BÁSICAS II
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="bioestadistica" data-prereqs="[]">
                BIOESTADÍSTICA
            </div>
        </div>

        <!-- SEMESTRE 2 -->
        <div class="semester" id="semestre2">
            <h2>SEGUNDO SEMESTRE</h2>
            <div class="subject" onclick="toggleSubject(this)" data-id="digestivo" data-prereqs="['bio-celular']">
                DIGESTIVO: BIOLOGÍA, ISO
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="locomotor" data-prereqs="['bio-celular']">
                LOCOMOTOR: BIOLOGÍA, ISO
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="genetica-general" data-prereqs="['bio-celular', 'bioestadistica']">
                GENÉTICA GENERAL: BIOLOGÍA, BIOESTADÍSTICA
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="reproductor-endocrino" data-prereqs="['bio-celular']">
                REPRODUCTOR Y ENDOCRINO: BIOLOGÍA, ISO
            </div>
            <div class="subject" onclick="toggleSubject(this)" data-id="fundamentos-reproduccion" data-prereqs="['practicas-basicas1', 'practicas-basicas2']">
                FUNDAMENTOS EN REPRODUCCIÓN ANIMAL: PRÁCTICAS I, PRÁCTICAS II
            </div>
        </div>

        <!-- Agregar más semestres aquí con la misma estructura... -->
    </div>

    <script>
        // Recuperar el progreso guardado
        window.onload = function() {
            const subjects = document.querySelectorAll('.subject');
            subjects.forEach(subject => {
                const id = subject.getAttribute('data-id');
                const isApproved = localStorage.getItem(id) === 'true';
                if (isApproved) {
                    subject.classList.add('aprobada');
                }

                // Verificar si hay prerequisitos no aprobados
                const prereqs = JSON.parse(subject.getAttribute('data-prereqs'));
                if (prereqs.some(prereq => !localStorage.getItem(prereq))) {
                    subject.classList.add('bloqueada');
                    subject.innerHTML += '<span class="tooltip">Debe aprobar las materias prerequisito.</span>';
                }
            });
        };

        // Función para alternar la aprobación de una materia
        function toggleSubject(subject) {
            if (subject.classList.contains('bloqueada') || subject.classList.contains('aprobada')) {
                return;
            }
            const id = subject.getAttribute('data-id');
            const isApproved = subject.classList.contains('aprobada');
            if (isApproved) {
                subject.classList.remove('aprobada');
                localStorage.removeItem(id);
            } else {
                subject.classList.add('aprobada');
                localStorage.setItem(id, 'true');
            }
        }
    </script>
</body>
</html>
