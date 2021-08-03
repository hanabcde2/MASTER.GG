<template>
    <v-container>
        <v-layout row>
            <v-flex d-flex justify-center align-center>
                <v-card color="#02024C" class=" pa-3 white--text" min-width="510" max-width="510" min-height="410" >
                    <v-card-title>
                        <span class="text-h5 font-weight-black">FANDAQ</span>
                    </v-card-title>
                    <v-card-subtitle class="pt-5 white--text">
                        <p class="text-h5 ma-0">팬닥 로그인</p>
                        <span class="text-caption">팬닥에 처음 오셨나요?<router-link class="purple--text pl-1" to="/">회원가입하기</router-link></span>
                    </v-card-subtitle>
                    <v-card-text>
                        
                        <validation-observer ref="observer" v-slot="{invalid}">
                            <form @submit.prevent="onSubmit">
                            <validation-provider
                                name="E-Mail" 
                                :rules="{
                                    required:true,
                                    email:true
                                }" 
                                v-slot="{errors}"
                            >
                                <v-text-field
                                    dark v-model="email" 
                                    :error-messages="errors" 
                                    type="email" 
                                    placeholder="아이디를 입력해주세요" 
                                    dense outlined 
                                />
                                
                            </validation-provider>
                            <validation-provider   name="Password" 
                                :rules="{
                                    required:true,
                                    max:14,
                                    min:8,
                                }"  v-slot="{ errors }"
                            >
                                <v-text-field 
                                    dark v-model="password"
                                    
                                    :error-messages="errors"
                                    type="password" 
                                    placeholder="비밀번호를 입력해주세요" 
                                    dense outlined 
                                />

                            </validation-provider >
                            <v-btn  class="indigo darken-4" :disabled="invalid" type="sumbit" dark width="100%">로그인</v-btn>
                            </form>
                        </validation-observer>
                        
                        <v-card color="#02024C" class="mt-3 d-flex align-center justify-space-between" elevation="0" width="100%">
                            <div>
                                <v-checkbox v-model="idSave" color="purple" class="mt-0 mb-1 " hide-details>
                                    <template #[`label`] >
                                        <span class="text-body-2 white--text">아이디 저장하기</span>
                                    </template>
                                </v-checkbox>
                            </div>
                            <div >
                                <router-link id="password" to="/"><span class="text-body-2 white--text">비밀번호 찾기 <v-icon class="mb-1 white--text">mdi-chevron-right</v-icon></span></router-link>
                            </div>
                        </v-card>
                    </v-card-text>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
import { extend,ValidationProvider,ValidationObserver } from 'vee-validate';
import { max, required,email,min } from 'vee-validate/dist/rules';
extend('max', {
    ...max,
    message: '{_field_}는 {length}자를 초과할 수 없습니다.'

})
extend('min', {
    ...min,
    message: '{_field_}는 {length}자 미만으로 할 수 없습니다.'

})
extend('required', {
    ...required,
    message: '{_field_}는 필수값 입니다'

})
extend('email', {
    ...email,
    message: '잘못 입력된 이메일 주소입니다.'

})
export default {
    components:{
        ValidationProvider,
        ValidationObserver
    },
    data:() => ({
        valid:true,
        idSave:true,
        email: '',
        password:'',
    }),
    methods:{
        onSubmit(v){
            console.log(this.formData);
            console.log(v);
            this.$refs.observer.validate().then(result =>{
                if(result){
                    console.log(`
                    양식 제출
                    email : ${this.email}
                    password : ${this.password}
                    `)
                }
            })
        }
    }
}
</script>

<style lang="scss" scoped>
#password{
    text-decoration: none;
}
</style>