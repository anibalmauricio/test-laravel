<template>
    <div class="card mb-3">
        <div class="card-header">En qué estás pensando ahora?</div>

        <div class="card-body">
            
            <form v-on:submit.prevent="newThought()">
                <div class="form-group">
                    <label for="thought">Ahora estás pensando en:</label>
                    <input type="text" class="form-control" name="thought" v-model="descripcion">
                </div>
                <button type="submit" class="btn btn-primary">Enviar pensamiento</button>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                descripcion: ''
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods: {
            newThought() {
                const params = {
                    descripcion: this.descripcion
                };
                axios.post('/thoughts', params)
                    .then((response) => {
                        console.log(response);
                        this.descripcion = '';
                        const thought = response.data;
                        this.$emit('new', thought);
                    });
                
            }
        }
    }
</script>
