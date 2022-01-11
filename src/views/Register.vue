<template lang="pug">
  main
    section.full.flex
      div.column.image
        img(src="@/assets/img/9364675fb26a.png")
      div.column.flex.text
        div.content
          h1.text-center <strong>Desafiogram</strong> 
          p Registrate para que veas el clon de Intagram.
          form(@submit.prevent="register")
          
            ValidationProvider(rules="required" v-slot="v")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="text" name="Nombre de usuario" placeholder="Nombre" v-model="nombre" :class="{ error: v.errors[0] }")
            ValidationProvider(rules="required" v-slot="v")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="text" name="Username" placeholder="Username" v-model="username" :class="{ error: v.errors[0] }")
            ValidationProvider(rules="email|required" v-slot="v")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="email" name="E-mail" placeholder="E-mail" v-model="email" :class="{ error: v.errors[0] }")
            ValidationProvider(rules="required" v-slot="v")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="text" name="Bio" placeholder="Bio" v-model="bio" :class="{ error: v.errors[0] }")
            ValidationProvider(v-slot="v" rules="required")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="password" name="Contraseña" placeholder="Contraseña" v-model="password" :class="{ error: v.errors[0] }")
            ValidationProvider(v-slot="v" rules="required")
              span.error(v-if="v.errors[0]") {{ v.errors[0] }}
              input(type="password" name="Confirmar contraseña" placeholder="Confirma tu contraseña" v-model="password2" :class="{ error: v.errors[0] }")
            button(type="submit") Registrarse
          p.text-center o si ya tienes una cuenta
          p.text-center
            router-link(to="/") Entrar con tu cuenta
</template>

<script>
import { ValidationProvider } from 'vee-validate'
import { extend } from 'vee-validate';
import { required, email } from 'vee-validate/dist/rules';

import axios from 'axios'

extend('required', {
  ...required,
  message: '{_field_} es obligatorio'
})

extend('email', {
  ...email,
  message: '{_field_} es inválido'
})

export default {
  data() {
    return {
      nombre:"",
      bio:"",
      email: "",
      username: "",
      password: "",
      password2: ""
    }
  },
  components: {
    ValidationProvider
  },
  methods: {
    register() {
      
      if(this.password != this.password2) {
        alert("Las contraseñas no coinciden!")
        return
      }

      const dataSend = JSON.stringify({
        "nombre": this.nombre,
        "username": this.username,
        "password": this.password,
        "email": this.email,
        "bio": this.bio
      });
      
      const config = {
        method: 'post',
        url: 'http://localhost:3000/api/usuarios/signup',
        headers: {
          'Content-Type': 'application/json'
        },
        data : dataSend
      };
      
      axios(config)
      .then( (response) => {
        //console.log("response", (response));
        
        if(response.data.token.length == 0 || response.data.usuario._id.length == 0)
        {
          alert("Error al crear Usuario")
          return
        }
        
        alert('Usuario Creado con exito');
        
        setTimeout( ()=> {
          this.$router.push('/Login')
        }, 5000);

      })
      .catch( (error) => {
        console.log("error",error);
      });

    }
  }
}
</script>

<style lang="stylus" scoped>
  .image
    width 60%
    flex-basis 60%
    position relative
    img
      width 100%
      display block
      height 100%
      object-fit cove
    p
      position absolute
      bottom 30px
      left 30px
      color white
      font-weight 600
  div.text
    align-self center
    flex-basis 40%
    align-items center
    justify-content center
    div.content
      width 320px
  ul.social
    position absolute
    bottom 40px
    margin 0
    padding 0
    li
      display inline-block
      margin 0 10px
      a
        font-size 30px
        font-weight normal
  span.error
    padding 0 20px
    color #F23C3C
    font-size 10px
    font-weight bold
    margin-bottom 5px
    display block
</style>