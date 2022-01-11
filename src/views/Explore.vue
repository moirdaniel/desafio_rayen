<template>
  <div>
    <div class="container">
      <br /><br />
      <br /><br />
      <br />
      <h1><strong>Descubrir Usuarios</strong></h1>
      <br />
      <div>
        <div v-for="item of usuarios" v-bind:key="item.id">
          <div class="col-3">
            <div class="card">
              <div class="card-body text-center">
                <div
                  class="cero"
                  v-bind:style="{ background: item.color }"
                ></div>
                <p>{{ item.nombre }}</p>
                <a href="#" class="btn btn-primary btn-sm">Ver perfil</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <h1><strong>Explorar</strong></h1>
      <br />
      <div>
        <div v-for="item of fotos" v-bind:key="item.url">
          <div class="">
            <img v-bind:src="item.url"  width="300" alt="" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Collage from "vue-collage";

export default {
  components: {
    Collage,
  },
  data() {
    return {
      usuarios: [],
      fotos: [],
      fotos2: [],
      token: this.$store.getters.token,
      urlBase: "http://localhost:3000",
    };
  },
  created() {
    this.BuscarUsuarios();
    this.BuscarFotos();
  },
  methods: {
    navigate(post) {
      this.$router.push(`/post/${post.post_id}`);
    },

    ColorRandom() {
      var hexadecimal = [
        "0",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
      ];

      var color_aletorio = "#";
      for (var i = 0; i < 6; i++) {
        var p = this.Aleatorio(0, hexadecimal.length);
        color_aletorio += hexadecimal[p];
      }

      return color_aletorio;
    },

    Aleatorio(i, s) {
      var numPosibilidades = s - i;
      var aleat = Math.random() * numPosibilidades;
      aleat = Math.floor(aleat);
      return parseInt(i) + aleat;
    },

    BuscarUsuarios() {
      const config = {
        method: "get",
        url: this.urlBase + "/api/usuarios/explore",
        headers: {
          Authorization: "Bearer " + this.token,
        },
      };

      axios(config)
        .then((response) => {
          this.usuarios = [];

          for (var item of response.data) {
            var usr = {
              nombre: item.username,
              color: this.ColorRandom(),
              id: item.id,
            };
            this.usuarios.push(usr);
          }
        })
        .catch((error) => {
          console.log("error", error);
        });
    },

    BuscarFotos() {
      var base = this.urlBase;

      var config = {
        method: "get",
        url: base + "/api/posts/explore",
      };

      axios(config)
        .then((response) => {
          this.fotos = [];
          for (var item of response.data) {
            var img = {
              url: base + item.url,
            };
            console.log(img);
            this.fotos.push(img);
           this.fotos2.push(base + item.url);
          }
          //console.log(this.imagenesArray);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.cero {
  width: 100px;
  height: 100px;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}
</style>