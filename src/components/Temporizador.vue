E o temporizador ficaria assim:

<template>
  <section
    class="is-flex is-align-items-center is-justify-content-space-between"
  >
    <Cronometro :tempoEmSegundos="tempoEmSegundos" />
    <Botao
      @clicado="iniciar"
      icone="fas fa-play"
      texto="PLAY"
      :desabilitado="cronometroRodando"
    />
    <Botao
      @clicado="finalizar"
      icone="fas fa-stop"
      texto="STOP"
      :desabilitado="!cronometroRodando"
    />
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Botao from "./Botao.vue";
import Cronometro from "./Cronometro.vue";

export default defineComponent({
  name: "Temporizador",
  emits: ["aoFinalizarTemporizador"],
  components: {
    Cronometro,
    Botao,
  },
  data() {
    return {
      tempoEmSegundos: 0,
      cronometro: 0,
      cronometroRodando: false,
    };
  },
  methods: {
    iniciar() {
      //Começar a contagem dos segundos
      this.cronometro = setInterval(() => {
        this.tempoEmSegundos += 1;
      }, 1000);
      this.cronometroRodando = true;
    },
    finalizar() {
      this.cronometroRodando = false;
      clearInterval(this.cronometro);
      this.$emit("aoFinalizarTemporizador", this.tempoEmSegundos);
      this.tempoEmSegundos = 0;
    },
  },
});
</script>
