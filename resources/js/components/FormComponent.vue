<template>
    <div class="card">
        <div class="card-header">¿En qué estas pensando ahora?</div>

        <div class="card-body">
            <!-- @if (session('status'))
                <div class="alert alert-success" role="alert">
                    {{ session('status') }}
                </div>
            @endif -->

            <form  @submit.prevent="newThought()">
                <div class="form-group">
                    <label for="thought">Ahora pienso en:</label>
                    <input type="text" name="thought" class="form-control"
                    v-model="description">
                </div>
                <button type="submit">Enviar pensamiento</button>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                description:''
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods:{
            newThought(){
                if(this.description.trim() === ''){
                    alert('Debes completar el campo antes de guardar');
                    return;
                }
                const params ={
                    description: this.description
                };
                this.description = '';
                console.log(params);

                axios.post('/controldoc/public/thoughts', params).then(Response=>{
                    const thought = Response.data;
                    console.log(params);
                    console.log(thought);
                    this.$emit('new', thought);
                });
            }
        }
    }
</script>
