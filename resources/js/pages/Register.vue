<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-sm-6 mt-4">

                <h2>Register</h2>
                <p class="text-danger" v-for="error in errors" :key="error">
                    <span v-for="err in error" :key="err">{{ err }}</span>
                </p>

                <center style="display: none;" class="messageshow">
                    <div class="alert alert-primary d-flex align-items-center" role="alert">
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" class="bi bi-exclamation-triangle-fill flex-shrink-0 me-2" viewBox="0 0 16 16" role="img" aria-label="Warning:">
    <path d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
  </svg>
  <div id="show_response_msg">
        
  </div>
</div>
                </center>
                <form @submit.prevent="register" id="userFrm">
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input type="text" class="form-control" id="name" v-model="form.name">
                    </div>
                    <div class="form-group">
                        <label for="email">Email Address:</label>
                        <input type="email" class="form-control" id="email" v-model="form.email">
                    </div>

                    <div class="form-group">
                        <label for="password">Password:</label>
                        <input type="password" class="form-control" id="password" v-model="form.password">
                    </div>
                    <div class="form-group">
                        <label for="c_password">Confirm Password:</label>
                        <input type="password" class="form-control" id="c_password" v-model="form.c_password">
                    </div>

                    <button type="submit" class="btn btn-primary">Register</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue'
import { useRouter } from "vue-router"
import { useStore } from 'vuex'
export default {
    setup() {
        const router = useRouter()
        const store = useStore()
        let form = reactive({
            name: '',
            email: '',
            password: '',
            c_password: '',
        });
        let errors = ref([])
        //const success_message: '',
        const register = async () => {
            await axios.post('/api/register', form).then(res => {
                if (res.data.success) {
                    router.push({ name: 'Dashboard' })
                    $("#show_response_msg").html(res.data.message);
                    successMsg();
                    store.dispatch('setToken', res.data.data.token)
                    
                }
            }).catch(e => {
                //errors.value = e.data.message
            })
        }
        return {
            form,
            register,
            errors
        }

        function successMsg() {
            $('#userFrm')[0].reset();
            $(".messageshow").show();
             

        }
    },

}
</script>
