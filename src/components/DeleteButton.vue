<template>
    <v-btn
            color="error"
            elevation="4"
            
            @click="deleteTask"
            >
                <v-icon>{{'mdi-close-circle'}}</v-icon>
                Delete
    </v-btn>

    
</template>
  
<script>

export default{
    props: {
        tasks: Array,
        delTitle: String
    },
    methods: {
        deleteTask(){
            const deleteTitle = this.delTitle
            this.$emit('task-deleted', deleteTitle)

            
            
        }
    },
    data(){
        return{
            
        }
    }
}

</script>



  