<template>
  <div class="productos">
    <v-container>
      <v-tabs
        v-model="tab"
        color="primary"
        class="mb-6"
      >
        <v-tab>INICIO</v-tab>
        <v-tab>PRODUCTO</v-tab>
        <v-tab>ACERCA</v-tab>
          </v-tabs>

      <v-tabs-items v-model="tab">
        <!-- Tab de INICIO -->
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <h2>Bienvenido</h2>
              <p>Selecciona la pestaña de Producto para gestionar artículos</p>
            </v-card-text>
          </v-card>
        </v-tab-item>

        <!-- Tab de PRODUCTO -->
        <v-tab-item>
          <v-card flat>
            <!-- Campo para agregar nueva tarea/artículo -->
            <v-card-text>
              <v-text-field
                v-model="nuevoArticulo.titulo"
                label="Nueva tarea"
                outlined
                append-icon="mdi-plus"
                @click:append="mostrarDialogo"
                @keyup.enter="mostrarDialogo"
                hide-details
                class="mb-6"
              ></v-text-field>

              <h3 class="mb-4">Artículos</h3>

              <!-- Lista de artículos en formato de tarjetas -->
              <v-row>
                <v-col
                  v-for="articulo in articulos"
                  :key="articulo.id"
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-card outlined>
                    <v-card-text>
                      <div class="overline mb-2">{{ articulo.categoria }}</div>
                      
                      <v-img
                        :src="articulo.imagen"
                        height="150"
                        class="mb-3 grey lighten-2"
                      ></v-img>

                      <h3 class="mb-2">{{ articulo.titulo }}</h3>
                      <p class="text--secondary mb-2">
                        {{ articulo.descripcion }}
                      </p>

                      <div class="overline">{{ articulo.tipo }}</div>
                    </v-card-text>

                    <v-card-actions class="justify-end">
                      <v-btn
                        icon
                        color="blue"
                        @click="editarArticulo(articulo)"
                      >
                        <v-icon>mdi-pencil</v-icon>
                      </v-btn>
                      <v-btn
                        icon
                        color="red"
                        @click="borrarArticulo(articulo.id)"
                      >
                        <v-icon>mdi-delete</v-icon>
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-tab-item>

        <!-- Tab de ACERCA -->
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <h2>Acerca de</h2>
              <p>Sistema de gestión de artículos y tareas</p>
            </v-card-text>
          </v-card>
        </v-tab-item>

        <!-- Tab de EXAMEN UWU -->
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <h2>Examen</h2>
              <p>Contenido del examen</p>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs-items>
    </v-container>

    <!-- Diálogo para agregar/editar artículo -->
    <v-dialog v-model="dialogo" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h5">{{ editandoId ? 'Editar Artículo' : 'Nuevo Artículo' }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-model="nuevoArticulo.titulo"
                  label="Título"
                  outlined
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <v-textarea
                  v-model="nuevoArticulo.descripcion"
                  label="Descripción"
                  outlined
                  rows="3"
                ></v-textarea>
              </v-col>

              <v-col cols="12">
                <v-select
                  v-model="nuevoArticulo.categoria"
                  :items="categorias"
                  label="Categoría"
                  outlined
                ></v-select>
              </v-col>

              <v-col cols="12">
                <v-select
                  v-model="nuevoArticulo.tipo"
                  :items="tipos"
                  label="Tipo"
                  outlined
                ></v-select>
              </v-col>

              <v-col cols="12">
                <v-text-field
                  v-model="nuevoArticulo.imagen"
                  label="URL de Imagen (opcional)"
                  outlined
                  hint="Deja en blanco para imagen por defecto"
                  persistent-hint
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="grey"
            text
            @click="cerrarDialogo"
          >
            Cancelar
          </v-btn>
          <v-btn
            color="primary"
            @click="guardarArticulo"
          >
            Guardar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'ProductosView',
  data() {
    return {
      tab: 1, // Comienza en la pestaña de PRODUCTO
      dialogo: false,
      editandoId: null,
      nuevoArticulo: {
        titulo: '',
        descripcion: '',
        categoria: 'OVERLINE',
        tipo: 'BUTTON',
        imagen: ''
      },
      articulos: [
        {
          id: 1,
          titulo: 'Levantarse',
          descripcion: 'Greyhound divisively hello coldly wonderfully',
          categoria: 'OVERLINE',
          tipo: 'BUTTON',
          imagen: ''
        }
      ],
      categorias: ['OVERLINE', 'HEADING', 'SUBTITLE', 'BODY'],
      tipos: ['BUTTON', 'CARD', 'LIST', 'CHIP']
    }
  },
  methods: {
    mostrarDialogo() {
      this.dialogo = true
    },

    cerrarDialogo() {
      this.dialogo = false
      this.editandoId = null
      this.nuevoArticulo = {
        titulo: '',
        descripcion: '',
        categoria: 'OVERLINE',
        tipo: 'BUTTON',
        imagen: ''
      }
    },

    guardarArticulo() {
      if (!this.nuevoArticulo.titulo.trim()) {
        alert('Por favor ingresa un título')
        return
      }

      if (this.editandoId !== null) {
        // Editar artículo existente
        const index = this.articulos.findIndex(a => a.id === this.editandoId)
        if (index !== -1) {
          this.articulos[index] = {
            ...this.articulos[index],
            ...this.nuevoArticulo
          }
        }
      } else {
        // Agregar nuevo artículo
        const nuevoArt = {
          id: Date.now(),
          ...this.nuevoArticulo
        }
        this.articulos.push(nuevoArt)
      }

      this.cerrarDialogo()
    },

    editarArticulo(articulo) {
      this.editandoId = articulo.id
      this.nuevoArticulo = { ...articulo }
      this.dialogo = true
    },

    borrarArticulo(id) {
      if (confirm('¿Deseas eliminar este artículo?')) {
        this.articulos = this.articulos.filter(a => a.id !== id)
      }
    }
  }
}
</script>

<style scoped>
.productos {
  padding: 20px;
}
</style>
