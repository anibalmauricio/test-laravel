<template>
    <div class="card mb-3">
        <div class="card-header" v-if="thought.created_at == thought.updated_at">Publicado en {{ thought.created_at }}</div>
        <div class="card-header" v-else>Publicado en {{ thought.created_at }} - Última actualización: {{ thought.updated_at }}</div>

        <div class="card-body">
            <input v-if="editMode" type="text" class="form-control" v-model="thought.descripcion">
            <p v-else>{{ thought.descripcion }}</p>
        </div>

        <div class="card-footer">
            <button v-if="editMode" type="button" class="btn btn-success" v-on:click="onClickUpdate">Guardar Cambios</button>
            <button v-else type="button" class="btn btn-primary" v-on:click="onClickEdit">Editar</button>
            <button type="button" class="btn btn-danger" v-on:click="onClickDelete">Elliminar</button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['thought'],
        data() {
            return {
                editMode: false
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods: {
            onClickEdit() {
                this.editMode = true;
            },
            onClickDelete() {
                axios.delete(`/thoughts/${this.thought.id}`).then(() => {
                    this.$emit('delete');
                });
            },
            onClickUpdate() {
                const params = {
                    descripcion: this.thought.descripcion
                };
                axios.put('/thoughts/' + this.thought.id, params).then((response) => {
                    this.editMode = false;
                    const thought = response.data;
                    this.$emit('update', this.thought);
                    
                });
            }
        }
    }
</script>
