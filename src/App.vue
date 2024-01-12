<template>
  <div class="intentos">
    <label for="">Puntaje: {{ puntajeFinal }}</label>
    <label for="">Intento: {{ intentos }}</label>
  </div>
  <div class="container">
    <EFinal1 :datoUrl="imagen1" :datoTexto="respuesta1" />
    <EFinal1 :datoUrl="imagen2" :datoTexto="respuesta2" />
    <EFinal1 :datoUrl="imagen3" :datoTexto="respuesta3" />
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
      mostrar:TextTrackCue
    };
  },
  methods: {
    btnJugar() {
      if (this.intentos <= 5) {
        for (var i = 0; i < 3; i++) {
          this.consumirApi(i);
        }
        console.log(this.puntaje);
        this.intentos++;
      }else{

      }
    },
    comprobarPuntaje() {
      console.log("Hola");
      if (this.puntaje === 3) {
        this.puntajeFinal += 5;
      } else {
        this.puntajeFinal += this.puntaje;
        console.log(this.puntajeFinal);
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
      this.comprobarPuntaje();
    },
    
  },
};
</script>

<style>
#app {
  display: grid;
  justify-content: center;
  align-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
