<template>
  <div>
    <q-field stack-label>

      <div class="q-gutter-sm">
        <q-checkbox v-model="val" @click="$emit('feito', index)" />
      </div>

      <div class="self-center full-width no-outline" tabindex="0">
        <p>{{ tarefa }}</p>
      </div>

      <q-btn flat round color="primary" @click="$emit('deletar', index)" icon="delete" />
    </q-field>
  </div>
</template>

<script>
export default {
  name: "ToDoItem",
  props: {
    tarefa: { type: String },
    realizado: { type: Boolean },
    index: { type: Number },
  },
  data() {
    return {
      val: this.realizado,
    };
  },
};
</script>
