<template>
  <main
    class="columns is-gapless is-multiline"
    :class="{ 'modo-escuro': modoEscuroAtivo }"
  >
    <div class="column is-one-quarter">
      <BarraLateral @aoTemaAlterado="trocarTema" />
    </div>
    <div class="column is-three-quarter conteudo">
      <Formulario @aoSalvarTarefa="salvarTarefa" />
      <div class="lista">
        <Botao
          class="btnClear is-danger is-light"
          texto="Limpar Tarefas"
          @click="limparTarefas"
          :desabilitado="this.tarefas.length === 0 ? true : false"
        />

        <article
          v-if="tarefasForamLimpas && tarefas.length === 0"
          class="message is-primary"
        >
          <div class="message-header">
            <p>Ok</p>
            <button
              class="delete"
              aria-label="delete"
              @click="fecharNotificacao"
            ></button>
          </div>
          <div class="message-body">Suas tarefas foram limpas!</div>
        </article>
        <Box v-else-if="tarefas.length === 0">Não há tarefas registradas</Box>

        <Tarefa
          v-for="(tarefa, index) in tarefas"
          :key="index"
          :tarefa="tarefa"
        />
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import BarraLateral from "@/components/BarraLateral.vue";
import Formulario from "@/components/Formulario.vue";
import Tarefa from "@/components/Tarefa.vue";
import Box from "@/components/Box.vue";
import Botao from "@/components/Botao.vue";
import ITarefa from "./interfaces/ITarefa";

export default defineComponent({
  name: "App",
  components: {
    BarraLateral,
    Formulario,
    Tarefa,
    Box,
    Botao,
  },
  data() {
    return {
      tarefas: [] as ITarefa[],
      modoEscuroAtivo: false,
      tarefasForamLimpas: false,
    };
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) {
      this.tarefas.push(tarefa);
    },
    trocarTema(modoEscuroAtivo: boolean) {
      this.modoEscuroAtivo = modoEscuroAtivo;
    },
    limparTarefas() {
      this.tarefas = [];
      this.tarefasForamLimpas = true;
    },
    fecharNotificacao() {
      this.tarefasForamLimpas = false;
    },
  },
});
</script>

<style>
.lista {
  padding: 1.25rem;
}

main {
  --bg-primario: #fff;
  --texto-primario: #000;
  --card-primario: #faf0ca;
}

main.modo-escuro {
  --bg-primario: #404040;
  --texto-primario: rgb(245, 237, 237);
  --card-primario: #8899a6;
}

.conteudo {
  background-color: var(--bg-primario);
}

.btnClear {
  margin-bottom: 10px;
}
</style>
