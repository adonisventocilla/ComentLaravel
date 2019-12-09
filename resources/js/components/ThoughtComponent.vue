<template>
    <div class="card">
        <div class="card-header">Publicado en {{ thought.created_at }} - Última actualización: {{ thought.updated_at }} </div>

        <div class="card-body">
            <input v-if="editMode" type="text" class="form-control" v-model="thought.description">
            <p v-else>{{ thought.description }}</p>
        </div>
        <div class="card-footer">
            <button v-if="editMode" class="btn btn-primary" v-on:click="onClickUpdate()">Guardar Cambios</button>
            <button v-else class="btn btn-primary" v-on:click="onClickEdit()">Editar</button>
            <button class="btn btn-danger" v-on:click="onClickDelete()">Eliminar</button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['thought'],
        data(){
            return{
                editMode: false,
            };
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods : {
            onClickDelete() {
                console.log('delete');
                axios.delete(`/controldoc/public/thoughts/${this.thought.id}`).then(()=>{
                    console.log('delete');
                    this.$emit('delete');
                });
            },
            onClickEdit(){
                this.editMode = true;
            },
            onClickUpdate(){

                console.log(this.thought.description);
                const params ={
                    description: this.thought.description
                };

                axios.put(`/controldoc/public/thoughts/${this.thought.id}`, params).then(Response=>{
                    this.editMode = false;
                    const thought = Response.data;
                    console.log('update');
                    this.$emit('update', thought);
            });

            }
        }
    }
</script>
