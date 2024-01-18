<template>

  <div>
    <img :src="url" alt="no se pudo cargar" />
    <label for="">{{ texto }}</label>
  </div>
</template>

<script>
export default {
  data() {
    return {
      texto: this.datoTexto,
      url: this.datoUrl,
    };
  },
  props: {
    datoUrl: {
      type: String,
      default: null,
    },
    datoTexto: {
      type: String,
      default: "xxxxxxxx",
    },
  },
  watch:{
    datoUrl(value, oldvalue){
        this.url=value
    },
    datoTexto(value, oldvalue){
        this.texto=value
    }
  },

};
</script>

<style scoped>
img {
  width: 200px;
  width: 220px;
}
div{
  display: grid;
  justify-content: center;
}
</style>