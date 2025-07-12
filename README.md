<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Malla Curricular - Lic. en Nutrición</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
<h1>Malla Curricular - Licenciatura en Nutrición</h1>
  <p>Hacé clic en una materia para aprobarla. Las correlativas se activan automáticamente.</p>
  <div id="contenedor-malla"></div>
  <script src="script.js"></script>
</body>
</html>
body {font-family: sans-serif;padding: 20px;  background-color: #f4f4f4;}

h1, p {
  text-align: center;
}

#contenedor-malla {
  display: flex;
  flex-direction: column;
  gap: 24px;
  margin-top: 30px;
}

.año {
  background-color: #e0f2f1;
  border: 2px solid #00695c;
  padding: 15px;
  border-radius: 10px;
}

.año h2 {
  margin-top: 0;
  color: #00695c;
}

.cuatrimestre {
  margin-top: 10px;
}

.materia {
  display: inline-block;
  margin: 6px;
  padding: 10px 15px;
  border: 2px solid #0288d1;
  background-color: #e1f5fe;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.materia:hover {
  background-color: #b3e5fc;
}

.aprobada {
  background-color: #c8e6c9;
  border-color: #388e3c;
  text-decoration: line-through;
  color: #555;
}

.bloqueada {
  background-color: #eeeeee;
  border-color: #bdbdbd;
  color: #aaa;
  cursor: not-allowed;
}
const materias = [
  {
    id: "seminario1",
    nombre: "Seminario I: Problemática Sanitaria y Socioeconómica Regional del NOA",
    año: "1º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: [],
  },
  {
    id: "quimica",
    nombre: "Química Biológica",
    año: "1º Año",
    cuatrimestre: "Anual",
    correlativas: [],
  },
  {
    id: "anatomia",
    nombre: "Anatomía y Fisiología",
    año: "1º Año",
    cuatrimestre: "Anual",
    correlativas: [],
  },
  {
    id: "educacion",
    nombre: "Fundamentos de la Práctica Educativa",
    año: "1º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: [],
  },
  {
    id: "alimentos",
    nombre: "Alimentos",
    año: "1º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: [],
  },
  {
    id: "sociologia",
    nombre: "Sociología y Antropología Nutricional",
    año: "1º Año",
    cuatrimestre: "2º Cuatrimestre",
    correlativas: ["seminario1"],
  },
  {
    id: "estadistica",
    nombre: "Estadística Descriptiva",
    año: "1º Año",
    cuatrimestre: "2º Cuatrimestre",
    correlativas: ["seminario1"],
  },

  {
    id: "fisiopatologia",
    nombre: "Fisiopatología",
    año: "2º Año",
    cuatrimestre: "Anual",
    correlativas: ["quimica", "anatomia"],
  },
  {
    id: "fisiologia_nino",
    nombre: "Fisiología y Fisiopatología del Niño y la Embarazada",
    año: "2º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: ["quimica", "anatomia"],
  },
  {
    id: "tecnica_dietetica",
    nombre: "Técnica Dietética",
    año: "2º Año",
    cuatrimestre: "Anual",
    correlativas: ["quimica", "alimentos"],
  },
  {
    id: "microbio",
    nombre: "Microbiología de los Alimentos",
    año: "2º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: ["alimentos"],
  },
  {
    id: "nutricion_basica",
    nombre: "Nutrición Básica",
    año: "2º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: ["quimica", "anatomia", "alimentos"],
  },
  {
    id: "bioestadistica",
    nombre: "Bioestadística",
    año: "2º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: ["estadistica"],
  },
  {
    id: "comunicacion",
    nombre: "Comunicación en Nutrición",
    año: "2º Año",
    cuatrimestre: "1º Cuatrimestre",
    correlativas: ["educacion", "alimentos", "sociologia"],
  },
  {
    id: "alimentacion_normal",
    nombre: "Alimentación Normal",
    año: "2º Año",
    cuatrimestre: "2º Cuatrimestre",
    correlativas: ["nutricion_basica"],
  },
  {
    id: "evaluacion",
    nombre: "Evaluación del Estado Nutricional",
    año: "2º Año",
    cuatrimestre: "2º Cuatrimestre",
    correlativas: ["nutricion_basica"],
  },
];

function renderMaterias() {
  const contenedor = document.getElementById("contenedor-malla");
  const años = [...new Set(materias.map(m => m.año))];

  años.forEach(año => {
    const divAño = document.createElement("div");
    divAño.classList.add("año");
    divAño.innerHTML = `<h2>${año}</h2>`;

    const cuatris = [...new Set(materias.filter(m => m.año === año).map(m => m.cuatrimestre))];

    cuatris.forEach(cuatr => {
      const divCuatri = document.createElement("div");
      divCuatri.classList.add("cuatrimestre");
      divCuatri.innerHTML = `<h3>${cuatr}</h3>`;

      materias
        .filter(m => m.año === año && m.cuatrimestre === cuatr)
        .forEach(materia => {
          const div = document.createElement("div");
          div.classList.add("materia");
          div.textContent = materia.nombre;
          div.dataset.id = materia.id;

          const aprobadas = JSON.parse(localStorage.getItem("aprobadas") || "[]");
          if (aprobadas.includes(materia.id)) div.classList.add("aprobada");

          div.addEventListener("click", () => {
            if (div.classList.contains("bloqueada")) return;
            div.classList.toggle("aprobada");
            guardarEstado();
            actualizarEstado();
          });

          divCuatri.appendChild(div);
        });

      divAño.appendChild(divCuatri);
    });

    contenedor.appendChild(divAño);
  });

  actualizarEstado();
}

function guardarEstado() {
  const aprobadas = [...document.querySelectorAll(".materia.aprobada")].map(m => m.dataset.id);
  localStorage.setItem("aprobadas", JSON.stringify(aprobadas));
}

function actualizarEstado() {
  const aprobadas = JSON.parse(localStorage.getItem("aprobadas") || "[]");

  materias.forEach(materia => {
    const div = document.querySelector(`.materia[data-id="${materia.id}"]`);
    const todasAprobadas = materia.correlativas.every(correl => aprobadas.includes(correl));

    if (!todasAprobadas) {
      div.classList.add("bloqueada");
      div.classList.remove("aprobada");
    } else {
      div.classList.remove("bloqueada");
    }
  });

  guardarEstado();
}

document.addEventListener("DOMContentLoaded", renderMaterias);
