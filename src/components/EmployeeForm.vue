<template>
    <div id="employee-form">
        <form v-on:submit.prevent="handleSubmit">
            <!-- <form @submit.prevent="handleSubmit"> -->
            <label for="">Employee Name</label>
            <input type="text" v-model="employee.name" :class="{'has-error':submitting&&invalidName}"
            @focus="clearStatus" @keypress="clearStatus" ref="firstInput"
            />

            <label for="">Employee Email</label>
            <input type="text" v-model="employee.email" :class="{'has-error':submitting&&invalidEmail}"
            @focus="clearStatus" @keypress="clearStatus"/>

            <p v-if="submitting && error" class="error-message"> Pleae Fill The fileds!</p>
            <p v-if="success" class="success-message">Employee Successfully Added!</p>
            <button>Add Employee</button>
        </form>
    </div>
</template>
<script>
export default {
    name:'employee-form',
    data(){
        return{
            submitting:false,
            success:false,
            error:false,
            employee:{
                name:'',
                email:''
            }
        }
    },
    methods:{
        handleSubmit(){
            // alert('Form Submitted!')
            this.submitting=true;
            this.clearStatus();

            if (this.invalidName || this.invalidEmail) {
                this.error=true;
                return;
            } 
            this.$emit('add:employee',this.employee);
            this.$refs.firstInput.focus();

            this.employee = {
                 name:'',
                 email:''
            };
            this.success=true;
            this.error=false;
            this.submitting=false;
        },
        clearStatus(){
            this.success=false;
            this.error=false;
        }
    },
    computed:{
        invalidName(){
            return this.employee.name==='';
        },
        invalidEmail(){
            return this.employee.email==='';
        }
    }
}
</script>
<style scoped>
form{
    margin-bottom:2rem;
}
</style>