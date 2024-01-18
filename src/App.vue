<template>
  <div v-if="mostrarJuego">
    <div class="intentos">
      <label for="">Puntaje: {{ puntajeFinal }}</label>
      <label for="">Intento: {{ intentos }}</label>
    </div>
    <div class="container">
      <EFinal1 :datoUrl="imagen1" :datoTexto="respuesta1" />
      <EFinal1 :datoUrl="imagen2" :datoTexto="respuesta2" />
      <EFinal1 :datoUrl="imagen3" :datoTexto="respuesta3" />
    </div>
  </div>
  <div v-if="!mostrarJuego">
    <div class="perdedor" v-if="!final">
      <h1>Has utilizado tus 5 intentos</h1>
      <p>El juego ha terminado, intentalo nuevamente</p>
    </div>
    <div class="ganador" v-if="final">
      <h1>Puntaje: {{ puntajeFinal }}</h1>
      <p>Felicitaciones has ganado un premio de $10.000,00</p>
    </div>
  </div>

  <div>
    <button @click="btnJugar">{{ textoBtn }}</button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import EFinal1 from "./components/EFinal1.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    EFinal1,
  },
  data() {
    return {
      puntaje: 0,
      intentos: 0,
      textoBtn: "Jugar",
      imagen1:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
      respuesta1: "xxxxxxxxxx",
      imagen2:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
      respuesta2: "xxxxxxxxxx",
      imagen3:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
      respuesta3: "xxxxxxxxxx",
      puntajeFinal: 0,
      mostrarJuego: true,
      final: false,
    };
  },
  methods: {
    async btnJugar() {
      if (this.intentos < 5) {
        if (this.intentos === -1) {
          this.textoBtn = "Jugar";
          this.mostrarJuego = true;
          this.puntajeFinal = 0;
          this.imagen1 =
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg";
          this.respuesta1 = "xxxxxxxxxx";
          this.imagen2 =
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg";
          this.respuesta2 = "xxxxxxxxxx";
          this.imagen3 =
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg";
          this.respuesta3 = "xxxxxxxxxx";
          this.intentos=0
        } else {
          for (var i = 0; i < 3; i++) {
            await this.consumirApi(i);
          }
          this.intentos++;
          this.comprobarPuntaje();
        }
      } else {
        this.mostrarJuego = false;
        this.textoBtn = "Nuevo Juego";
        this.intentos = -1;
        if (this.puntajeFinal < 10) {
          this.final = false;
        }else{
          this.final = true;
        }
      }
    },
    comprobarPuntaje() {
      if (this.puntaje === 3) {
        this.puntajeFinal += 5;
      } else {
        this.puntajeFinal += this.puntaje;
      }
      this.puntaje = 0;
    },
    async consumirApi(value) {
      const { answer, image } = await fetch("https://yesno.wtf/api").then(
        (respuesta) => respuesta.json()
      );
      switch (value) {
        case 0:
          this.imagen1 = image;
          this.respuesta1 = answer;
          break;
        case 1:
          this.imagen2 = image;
          this.respuesta2 = answer;
          break;
        case 2:
          this.imagen3 = image;
          this.respuesta3 = answer;
          break;
        default:
          break;
      }
      if (answer === "yes") {
        this.puntaje++;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  display: flex;
  justify-content: center;
}
button {
  height: 40px;
  width: 120px;
  margin: 30px;
  font-size: 18px;
  border-radius: 0px;
  border: black solid 3px;
}
.intentos {
  margin: 50px;
}
.intentos label {
  margin-left: 100px;
  margin-right: 100px;
}
.ganador {
  color: blue;
}
.perdedor {
  color: red;
}
</style>
