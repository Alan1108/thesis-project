<template>
  <div class="contacto">
    <!-- Contenido de la página de Contacto -->
    <div class="contenido">
      <!-- Columna izquierda -->
      
      <div class="tabla-contacto">
            <!-- Columna izquierda -->
            <div class="columna-izquierda">
              <div v-if="captchaLoading">Cargando Captcha...</div>
              <div v-else>
              <!-- Contenido de la columna izquierda -->
              <h1>Contactanos!</h1>
              <!-- Formulario de contacto -->
              <form @submit.prevent="submitForm">
                <table class="form-table">
                  <tbody>
                    <!-- Fila para Nombre -->
                    <tr>
                      <td><strong>Nombre*</strong></td>
                      <td style="padding-right: 3%;"><input type="text" placeholder="Nombre" v-model="nombre" required></td>
                      <td><input type="text" placeholder="Apellido" v-model="apellido" required></td>
                    </tr>
                    <!-- Fila para Correo -->
                    <tr>
                      <td><strong>Correo*</strong></td>
                      <td colspan="2"><input type="email" placeholder="Correo" v-model="correo" required></td>
                    </tr>
                    <!-- Fila para Asunto -->
                    <tr>
                      <td><strong>Asunto*</strong></td>
                      <td colspan="2"><input type="text" placeholder="Asunto" v-model="asunto" required></td>
                    </tr>
                    <!-- Fila para Mensaje -->
                    <tr>
                      <td><strong>Mensaje*</strong></td>
                      <td colspan="2"><textarea placeholder="Mensaje" v-model="mensaje" rows="4" required></textarea></td>
                    </tr>
                    <!-- Fila para Captcha -->
                    <tr class="captcha">
                      <td><strong>Verificación*</strong></td>
                      <td colspan="2">
                        <div
                          class="h-captcha"
                          data-sitekey="ce6cb88f-3f12-4cdf-98a1-ea9eaf406c2b"
                          @verify="onVerify"
                          style="max-width: 10px;"
                        ></div>
                      </td>
                    </tr>                                      
                  </tbody>
                </table>
                <h2>¿Con quién deseas contactarte?</h2>
                <div class="radio-options">
                  <label><input type="radio" value="Desarrollo" v-model="departamento">Departamento de Desarrollo</label><br>
                  <label><input type="radio" value="Biotecnología" v-model="departamento">Departamento de Biotecnología</label>
                </div>

                <div class="boton-enviar">
                  <button type="submit" class="enviar-btn">Enviar</button>
                </div>
  
              </form>
            </div>
          </div>

            <!-- Columna derecha -->
            <div class="columna-derecha" >
              
              <p class="error-message" v-if="!notificacionerror">Error en el Servidor, su mensaje no fue enviado.</p>
              
              <p class="success-message" v-if="!notificacionenviado">Mensaje enviado con exito!</p> 
              <div class="titulo">
                <img src="../img/pregunta.png" style="max-width: 40%;padding-bottom: 10px;">
                <h1>¿Tienes algo que aportar?</h1>
                <p>Comunícate con el departamento de Biotecnología para compartir tus ideas y contribuciones. </p>
                
                <h1>¿Necesitas ayuda con algo?</h1>
                <p>Si necesitas asistencia técnica o tienes problemas con el sistema, ponte en contacto con el departamento de Desarrollo.</p>
              </div>
            </div>
          </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PaginaContacto',
  data() {
    return {
      notificacionerror: true,
      notificacionenviado: true,
      nombre: '',
      apellido: '',
      correo: '',
      asunto: '',
      mensaje: '',
      captcha: '',
      departamento: '',
      captchaLoading: true
    };
  },
  methods: {
    submitForm() {
      const emailSubject = `${this.nombre} ${this.apellido} con correo ${this.correo} tiene la siguiente consulta: ${this.asunto}`;
      const emailMessage = this.mensaje;
      const toEmail = this.departamento === 'Desarrollo' ? 'alan.ibm1108@gmail.com' : 'edisonmauricio00@gmail.com';

      const requestBody = {
        to_email: toEmail,
        email_subject: emailSubject,
        email_message: emailMessage
      };

      fetch('https://sample-f5jz4k4tfa-ue.a.run.app/email/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(requestBody)
      })
      .then(response => {
        if (response.ok) {
          console.log('¡El correo se ha enviado correctamente!');
          this.notificacionenviado = false;
          // Aquí puedes realizar alguna acción adicional si el correo se envió con éxito
        } else {
          console.error('Error al enviar el correo');
          this.notificacionerror=false;
          // Aquí puedes manejar el caso de error, por ejemplo, mostrar un mensaje al usuario
        }
      })
      .catch(error => {
        console.error('Error en la solicitud:', error);
        // Aquí puedes manejar el error en caso de que la solicitud falle
      });
    },
    onVerify(response) {
      // Verifica la respuesta de hCaptcha
      if (response) {
        // La respuesta es válida, continúa con el envío del formulario
        this.submitForm();
      } else {
        // La respuesta es inválida, muestra un mensaje de error o toma otra acción
        console.error('Error de verificación de hCaptcha');
      }
    }
  },
  mounted() {
    // Carga el script de hCaptcha
    this.captchaLoading = true;
    const script = document.createElement('script');
    script.src = 'https://hcaptcha.com/1/api.js';
    script.async = true;
    script.defer = true;
    script.onload = () => {
      // Una vez que el script del captcha se haya cargado, establece captchaLoading en falso
      this.captchaLoading = false;
      console.log('Captcha cargado');
    };
    document.head.appendChild(script);
  }
}
</script>

<style>
.contenido {
  display: flex; /* Usa flexbox para distribuir los elementos */
  flex-direction: column;
}


h1{
  font-size: 48px;
}

.form-table {
  width: 95%; /* La tabla ocupa todo el ancho disponible */
  border-collapse: collapse; /* Fusionar bordes de celda */
}

.form-table td {
  padding: 10px; /* Espaciado interno de celda */
}

.form-table strong {
  display: inline-block; /* Alinea los elementos strong dentro de las celdas */
  margin-bottom: 5px; /* Espaciado inferior */
  width: 100px; /* Ancho fijo para etiquetas */
}

.form-table input[type="text"],
.form-table input[type="email"],
.form-table textarea {
  width: 100%; /* Ancho ajustable para campos de entrada */
  padding: 10px; /* Espaciado interno de campo de entrada */
}

.form-table textarea {
  height: 100px; /* Altura fija para textarea */
}

button {
  background-color: #15323a; /* Color de fondo del botón */
  color: white; /* Color del texto del botón */
  border: none; /* Sin borde */
  font-size: 18px;
  border-radius: 5px; /* Borde redondeado */
  padding: 10px 20px; /* Espaciado interno del botón */
  
  cursor: pointer; /* Cursor al pasar el ratón */
}

button:hover {
  background-color: #2380a8; /* Cambio de color al pasar el ratón */
}

.radio-options {
  text-align: center; /* Alinea las opciones de radio a la izquierda */
}

.boton-enviar {
  margin-top: 20px;
  margin-bottom: 30px;
  align-items: center;
}


.titulo {
  margin-top: 0px; /* Espacio superior para el título */
  text-align: center; /* Alinear al centro */
}

.titulo h1 {
  margin: 10;
}

.tabla-contacto {
  display: flex;
  text-align:center;
  flex-direction: row;
}

.columna-izquierda {
  width: 50%; /* Ocupa el 50% del ancho */
  padding: 10px; /* Espaciado interno */
  background-color: #cff1ff; /* Color de fondo de la columna izquierda */
}

.columna-derecha {
  width: 50%; /* Ocupa el 50% del ancho */
  padding: 5%; /* Espaciado interno */
  background-color: #ffffff; /* Color de fondo de la columna derecha */
}
.columna-derecha h1{
  font-size: 34px;
}
.columna-derecha p{
  font-size: 20px;
}
input, textarea{
  border: 2px solid #15323a;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.2);
  border-radius: 15px;
}
.error-message {
  background-color: #f8d7da; /* Fondo rojo */
  border: 1px solid #dc3545; /* Contorno rojo */
  color: #721c24; /* Texto rojo oscuro */
  padding: 10px;
  border-radius: 5px;
}
.success-message {
  background-color: #d7f8da; /* Fondo rojo */
  border: 1px solid #35dc6d; /* Contorno rojo */
  color: #1c7227; /* Texto rojo oscuro */
  padding: 10px;
  border-radius: 5px;
}
@media only screen and (max-width: 768px) {
    .tabla-contacto {
      display: flex;
      flex-direction: column;
    }
    .captcha{
      display: flex;
      flex-direction: column;
      padding-left: 10%;
    }
    .columna-izquierda{
      width: 100%;
      padding: 0px;
    }
    .columna-derecha{
      width: 100%;
      padding-top: 10px;
      
    }
    .columna-derecha h1{
  font-size: 20px;padding-right: 7%;
}
.columna-derecha p{
  font-size: 18px;padding-right: 7%;
  
}
  }
</style>

  