<template>
  <div class="pokemon">
    <div class="card">
      <div class="card-image column is-half is-offset-one-quarter">
        <figure class="image is-128x128 column is-half is-offset-one-quarter">
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media mt-5">
          <div class="media-left is-1">
            <p style="float: left">HP - {{ this.pokemon.hp }}</p>
            <progress
              class="progress is-primary"
              :value="this.pokemon.hp"
              max="200"
            >
              HP
            </progress>
            <p style="float: left">ATTACK - {{ this.pokemon.attack }}</p>
            <progress
              class="progress is-danger"
              :value="this.pokemon.attack"
              max="200"
            >
              ATTACK
            </progress>
            <p style="float: left">DEFENSE - {{ this.pokemon.defense }}</p>
            <progress
              class="progress is-info is-"
              :value="this.pokemon.defense"
              max="200"
            >
              DEFENSE
            </progress>
          </div>
          <div class="media-content is-10">
            <p class="title is-4">{{ name | upper }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
          <div class="media-right is-1">
            <div id="divCheckbox" style="visibility: hidden">TESTE</div>
          </div>
        </div>

        <div class="content">
          <button class="button is-medium is-fullwidth" @click="mudarSprite">
            Mudar sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.pokemon.hp = res.data.stats[0].base_stat;
      this.pokemon.attack = res.data.stats[1].base_stat;
      this.pokemon.defense = res.data.stats[2].base_stat;
      this.currentImg = this.pokemon.front;
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
        hp: 0,
        attack: 0,
        defense: 0,
      },
    };
  },
  props: {
    name: String,
    url: String,
    num: Number,
  },
  filters: {
    upper: function (value) {
      let newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<style>
.pokemon {
  margin-top: 10px;
}

#divCheckbox {
  visibility: hidden;
  width: 107px;
}
</style>