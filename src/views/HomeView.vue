<template>
  <div class="home">

    <v-text-field
      v-model="nuevoTituloTarea"
      @click:append="agregarTarea"
      @keyup.enter="agregarTarea"
      class="pa-3"
      outlined
      hide-details
      clearable
      :label="editandoId ? 'Editando tarea' : 'Nueva tarea'"
      :append-icon="editandoId ? 'mdi-content-save' : 'mdi-plus'"
    ></v-text-field>

    <v-list flat>
      <div 
        v-for="tarea in tareas"
        :key="tarea.id"
      >
        <v-list-item 
          @click="TareaHecho(tarea.id)"
          :class="{ 'red lighten-5' : tarea.hecho }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="tarea.hecho"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{'text-decoration-line-through': tarea.hecho}"
              >
                {{ tarea.titulo }}
              </v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn 
                icon
                @click.stop="BorrarTarea(tarea.id)"
              >
                <v-icon color="red lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>

            <v-list-item-action>
              <v-btn 
                icon
                @click.stop="EditarTarea(tarea.id)"
              >
                <v-icon color="blue lighten-1">mdi-pencil</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      tareas: [
        { id: 1, titulo: "Levantarse", hecho: false },
        { id: 2, titulo: "Desayunar", hecho: false },
        { id: 3, titulo: "Ir a la escuela", hecho: false }
      ],
      nuevoTituloTarea: '',
      editandoId: null // controla si se está editando una tarea
    }
  },
  methods: {
    TareaHecho(id) {
      const tarea = this.tareas.find(t => t.id === id)
      if (tarea) tarea.hecho = !tarea.hecho
    },

    BorrarTarea(id) {
      if (confirm("Deseas eliminar el registro?")) {
        this.tareas = this.tareas.filter(t => t.id !== id)
      }
    },

    agregarTarea() {
      if (!this.nuevoTituloTarea.trim()) return

      // Si estamos editando una tarea, actualiza su título
      if (this.editandoId !== null) {
        const tarea = this.tareas.find(t => t.id === this.editandoId)
        if (tarea) tarea.titulo = this.nuevoTituloTarea.trim()
        this.editandoId = null
        this.nuevoTituloTarea = ''
        return
      }

      // Si no, agrega una nueva tarea
      const nTarea = {
        id: Date.now(),
        titulo: this.nuevoTituloTarea.trim(),
        hecho: false
      }

      this.tareas.push(nTarea)
      this.nuevoTituloTarea = ''
    },

    EditarTarea(id) {
      const tarea = this.tareas.find(t => t.id === id)
      if (tarea) {
        this.nuevoTituloTarea = tarea.titulo
        this.editandoId = id
      }
    }
  }
}
</script>

<style scoped>
.text-decoration-line-through {
  text-decoration: line-through;
}
</style>
