<template>
    <div class="familias">
      <h1>Familias de Polínicos</h1>
      <p style="font-size: 18px;padding-bottom: 10px;">A continuación están las 9 familias de polinicos que identifica el sistema mediante la carga de imágenes. Conoce sus nombres, su distribución geográfica y el recurso utilizado para su identificación.</p>
      
      <!-- Aquí se divide la tabla en partes -->
        <table class="tabla-familias">
            <thead>
                <tr>
                <th style="width: 9%;text-align: center;">No.</th>
                <th style="width: 12%;text-align: center;">Familia</th>
                <th style="width: 35%;text-align: center;">Distribución Geográfica</th>
                <th style="width: 32%;text-align: center;">Grano de Polen</th>
                <th style="width: 12%;text-align: center;">Recurso Utilizado</th>
                </tr>
            </thead>
            <tbody>
                <!-- Filas de la tabla -->
                <tr v-for="familia in familiasMostradas" :key="familia.id" :class="{ 'fila-par': familia.id % 2 === 0, 'fila-impar': familia.id % 2 !== 0 }">
                <td style="width: 9%; text-align: center;">{{ familia.id }}</td>
                <td style="width: 12%; text-align: center;">{{ familia.nombre }}</td>
                <td style="width: 35%;">{{ familia.distribucion }}</td>
                <td style="width: 32%;"><img :src="familia.imagen" alt="Grano de Polen" class="imagen-polen"></td>
                <td style="width: 12%; text-align: center;">{{ familia.recurso }}</td>
                </tr>
            </tbody>
        </table>
        <div class="paginacion">
            <button class="botonpaginado" @click="paginaAnterior" :disabled="pagina === 1">
                <img src="../img/flecha.png" style="transform: rotate(180deg)"> Anterior</button>
            <button class="botonpaginado" @click="paginaSiguiente" :disabled="pagina === totalPaginas">Siguiente
                <img src="../img/flecha.png">
            </button>
        </div>
    </div>
  </template>
  
  <script>
    //import AizoaceaeImage from '../img/familias/Aizoaceae.jpg';
    //import AlismataceaeImage from '../img/familias/Alismataceae.jpg';
    //import Arecaceae from '../img/familias/Arecaceae.jpg';
    //import Anacardaceae from '../img/familias/Anacardaceae.jpg';
    import Asteraceae from '../img/familias/Asteraceae.jpg';
    //import Berberidaceae from '../img/familias/Berberidaceae.jpg';
    //import Burseraceae from '../img/familias/Burseraceae.jpg';
    //import Cyperaceae from '../img/familias/Cyperaceae.jpg';
    //import Cytinaceae from '../img/familias/Cytinaceae.jpg';
    import Euphorbiaceae  from '../img/familias/Euphorbiaceae.jpg';
    import Fabaceae from '../img/familias/Fabaceae.jpg';
    import Lardizabalaceae from '../img/familias/Lardizabalaceae.jpg';
    //import Loranthaceae from '../img/familias/Loranthaceae.jpg';
    import Lythraceae from '../img/familias/Lythraceae.jpg';
    import Melastomataceae from '../img/familias/Melastomataceae.jpg';
    import Molluginaceae from '../img/familias/Molluginaceae.jpg';
    //import Oleaceae from '../img/familias/Oleaceae.jpg';
    import Papaveraceae from '../img/familias/Papaveraceae.jpg';
    //import Piperaceae from '../img/familias/Piperaceae.jpg';
    import Plantaginaceae  from '../img/familias/Plantaginaceae.jpg';
    //import Plumbaginaceae from '../img/familias/Plumbaginaceae.jpg';
    //import Poaceae from '../img/familias/Poaceae.jpg';
    //import Polygalaceae from '../img/familias/Polygalaceae.jpg';
    //import Rosaceae from '../img/familias/Rosaceae.jpg';
    //import Salicaceae from '../img/familias/Salicaceae.jpg';
    //import Vitaceae from '../img/familias/Vitaceae.jpg';

  export default {
    
    name: 'PaginaFamilias',
    data() {
      return {
        pagina: 1,
        tamanoPagina: 5,
        familias: [
            
            {
                id: 1,
                nombre: 'Asteraceae',
                distribucion: 'Todos los ambientes y continentes, excepto la Antártida.',
                imagen: Asteraceae,
                recurso: 'Polen y néctar'
            },
            {
                id: 2,
                nombre: 'Euphorbiaceae',
                distribucion: 'Zonas tropicales y selvas, desiertos secos y calurosos.',
                imagen: Euphorbiaceae,
                recurso: 'Polen y néctar'
            },
            {
                id: 3,
                nombre: 'Fabaceae',
                distribucion: 'Cordillera de los Andes, América del Sur y sus zonas tropicales.',
                imagen: Fabaceae,
                recurso: 'Polen y néctar'
            },
            {
                id: 4,
                nombre: 'Lardizabalaceae',
                distribucion: 'Bosques tropicales, bosques mixtos templado-cálidos de Asia y América del Sur (zona oriental).',
                imagen: Lardizabalaceae,
                recurso: 'Polen'
            },
            {
                id: 5,
                nombre: 'Lythraceae',
                distribucion: 'Bosques abiertos, bosques tropicales y subtropicales, tierras rocosas, pastizales, riberas de arroyos, manglares, costas asiáticas y americanas.',
                imagen: Lythraceae,
                recurso: 'Polen'
            },
            {
                id: 6,
                nombre: 'Melastomataceae',
                distribucion: 'Zonas tropicales de Asia, América y África (pantropical).',
                imagen: Melastomataceae,
                recurso: 'Polen y néctar'
            },
            {
                id: 7,
                nombre: 'Molluginaceae',
                distribucion: 'Regiones tropicales y subtropicales de ambos hemisferios. Hábitat seco, abierto y hostil.',
                imagen: Molluginaceae,
                recurso: 'Polen'
            },
            {
                id: 8,
                nombre: 'Papaveraceae',
                distribucion: 'Asia central y occidental, altas montañas de Europa, América a lo largo de la costa del Pacífico, en climas áridos a cálidos.',
                imagen: Papaveraceae,
                recurso: 'Polen y néctar'
            },
            {
                id: 9,
                nombre: 'Plantaginaceae ',
                distribucion: 'Zonas templadas a tropicales, considerada una familia cosmopolita.',
                imagen: Plantaginaceae ,
                recurso: 'Polen y néctar'
            },
        ]
      };
    },
    computed: {
    // Calcula el índice de inicio de la página actual
        indiceInicio() {
        return (this.pagina - 1) * this.tamanoPagina;
        },
        // Calcula el índice de fin de la página actual
        indiceFin() {
        return this.indiceInicio + this.tamanoPagina;
        },
        // Obtiene las familias que se muestran en la página actual
        familiasMostradas() {
        return this.familias.slice(this.indiceInicio, this.indiceFin);
        },
        // Calcula el número total de páginas
        totalPaginas() {
        return Math.ceil(this.familias.length / this.tamanoPagina);
        }
    },
    methods: {
    // Función para ir a la página anterior
    paginaAnterior() {
      if (this.pagina > 1) {
        this.pagina--;
      }
    },
    // Función para ir a la página siguiente
    paginaSiguiente() {
      if (this.pagina < this.totalPaginas) {
        this.pagina++;
      }
    },
    
  }
  };
  </script>
  
  <style scoped>
  .familias {
    margin-top: 20px;
    padding-left: 10%;
    padding-right: 10%;
    padding-bottom: 10%;
  }
  
  .tabla-familias {
    width: 100%;
    border-collapse: collapse;
    
  }
  
  .tabla-familias th,
  .tabla-familias td {
    border: 1px solid #2380a8;
    padding: 8px;
    text-align: left;
  }
  
  .tabla-familias th {
    background-color: #15323a;
    color: white;
    font-weight: bold;
  }
  
  .fila-par {
    background-color: #cff1ff;
  }
  
  .fila-impar {
    background-color: #ffffff;
  }
  
  .imagen-polen {
    width: 100%;
    height: auto;
    max-width: 480px;
    max-height: 270px;
  }
  .paginacion {
  margin-top: 20px;
}

.paginacion button {
  margin-right: 10px;
}

.botonpaginado{
    background-color: #15323a;
    font-size: 18px;
}
.botonpaginado:hover {
  background-color: #2380a8;
}
.botonpaginado img{
    max-width: 10%;
}
@media screen and (max-width: 768px) {
    .tabla-familias th:nth-child(1),
    .tabla-familias td:nth-child(1),
    .tabla-familias th:nth-child(5),
    .tabla-familias td:nth-child(5) {
        display: none;
    }
    .tabla-familias th:nth-child(2),
    .tabla-familias td:nth-child(2) {
        width: 5%;
        font-size: 12px;
    }
    .tabla-familias th:nth-child(3),
    .tabla-familias td:nth-child(3) {
        width: 10%;
        font-size: 12px;
    }
    .tabla-familias th:nth-child(4),
    .tabla-familias td:nth-child(4) {
        width: 50%;
    }
    .paginacion {
  padding: 10px;
  display: flex;
}
.paginacion button {
  margin-right: 10px;
  padding: 10px;
}
}
  </style>
  