<template>
<div id="app"></div>
  <div class="wrapper__login">

    <div class="logo"><img src="../assets/Logos/logoLightblue.png" alt="GameApp"></div>

		<div class="contain content1">

      <div class="cabecero">
        <h2>{{ msg1 }}</h2>
        <p>Es rápido y fácil.</p>
      </div>

			<form class="form">
				<fieldset>

					<div class="col">
						<label>NOMBRE <span class="tooltip">?</span></label>
						<input type="text" id="nombre"  placeholder="Nombre" required>

						<label>USUARIO <span class="tooltip">?</span></label>
						<input type="text" id="usuario" placeholder="Username" required>

						<label>EMAIL <span class="tooltip">?</span></label>
						<input type="email" placeholder="Email" required>
					</div>

					<div class="col">
            <label>APELLIDO <span class="tooltip">?</span></label>
						<input type="text" placeholder="Apellido">

            <label>CONTRASEÑA <span class="tooltip">?</span></label>
						<input type="password"  placeholder="Password" required>

					</div>

				</fieldset>
        <p>Al hacer clic en "Registrarte", aceptas nuestras <span class="resaltado">Condiciones</span>, la <span class="resaltado">Política de datos</span> y la <span class="resaltado">Política de cookies</span>. Es posible que te enviemos notificaciones por SMS, que puedes desactivar cuando quieras.</p>
   
				<Button title="Siguiente" class="regular"/>

			</form>

		</div>
	</div>
</template>

<script>
import Button from '../components/Button.vue'

export default {
  name: 'UserCreate',
  components: { Button },
  data: function(){
    return {
      msg1: 'Registro'
    }
  }
}
</script>

<style scope>
.registro { 
  display: flex;
}
.logo {
  background: #f1f1f1;
  border-radius: 50%;
  height: 9em;
  width: 9em;
}
.logo img {
  padding: 5px 0 0 40px;
  width: 7em;
}
.cabecero {
  margin: 0 auto;
}
.cabecero h2 {
  font-family: var(--ff-secondary);
  font-size: 2em;
  font-weight: 700;
  letter-spacing: var(--spacing);
}
.cabecero p {
  float: left;
  padding: 16px 0;
  width: 96%;
}
.close {
  display:flex;
  position: absolute;
  float: right;
  right: 20px;
  top: 100px
}
.cabecero p {
  display: flex;
  margin-bottom: 0px;
}
.col {
  display: inline-block;
  width: 49%;
  margin-bottom: 10px;
  padding: 0 5px;
  vertical-align: top;
  text-align: left;
}  
.form .resaltado {
  color:var(--light-blue);
}
 
@media (max-width: 480px){
  .logo { display: none;}
  .logo img { display: none;}
  .cabecero p { font-size: 16px;}
  .form input[type="text"].small {width: 29%;}
}
</style>