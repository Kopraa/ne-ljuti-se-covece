<template>
  <div class="app">
    <p>Kocka je pala na: {{ DiceNumber }}</p>
    <button @click="RollDice">Baci kockicu</button>
    <img v-for="(Path, Index) in ImagePaths" v-bind:key="Index" :src="Path" :alt="'Player ' + Index" /> <!-- require('@/assets/images/players/' + Index + '.png')" -->
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

let FieldsCollection: Array<HTMLDivElement> = [];
let CurrentPlayer = 0; // Igrač koji je trenutno na redu da baca kockicu i igra

export default defineComponent({
  name: "App",
  data() {
    return {
      StartingPositions: [1, 13, 25, 37], // Startne pozicije po bojama(igračima)
      DiceNumber: 0, // Broj koji je bacen
      ImagePaths: [ // Putanje do slika igrača
        "@/assets/images/players/1.png",
        "@/assets/images/players/2.png",
        "@/assets/images/players/3.png",
        "@/assets/images/players/4.png"
      ],
    };
  },
  mounted() {
    // Generišemo polja
    for (let i = 1; i < 49; i++) {
      let Field = document.createElement("div");
      if (!Field) return;
      // Visina i širina polja
      Field.style.width = "40px";
      Field.style.height = "40px";
      // Pozadina
      Field.style.background = "orange";
      // Fiksiramo ih
      Field.style.position = "absolute";
      // Granice da se jasnije vide
      Field.style.border = "1px solid black";
      Field.style.borderRadius = "50%";

      // Brojevi polja, za debug možda posle neka upotreba
      Field.innerHTML = `${i}`;

      // Prvi red, svaki sledeći if predstavlja jedan red
      if (i > 0 && i < 7) {
        Field.style.top = 5 + 40 * i + "px";
        Field.style.left = "320px";
      }
      if (i > 6 && i < 12) {
        Field.style.top = "245px";
        Field.style.left = 320 + 40 * (i - 6) + "px";
      }
      if (i > 11 && i < 14) {
        Field.style.top = 245 + 40 * (i - 11) + "px";
        Field.style.left = "520px";
      }
      if (i > 13 && i < 19) {
        Field.style.top = "325px";
        Field.style.left = 520 - 40 * (i - 13) + "px";
      }
      if (i > 18 && i < 24) {
        Field.style.top = 325 + 40 * (i - 18) + "px";
        Field.style.left = "320px";
      }
      if (i > 23 && i < 26) {
        Field.style.top = "525px";
        Field.style.left = 320 - 40 * (i - 23) + "px";
      }
      if (i > 25 && i < 31) {
        Field.style.top = 525 - 40 * (i - 25) + "px";
        Field.style.left = "240px";
      }
      if (i > 30 && i < 36) {
        Field.style.top = "325px";
        Field.style.left = 240 - 40 * (i - 30) + "px";
      }
      if (i > 35 && i < 38) {
        Field.style.top = 325 - 40 * (i - 35) + "px";
        Field.style.left = "40px";
      }
      if (i > 37 && i < 43) {
        Field.style.top = "245px";
        Field.style.left = 40 + 40 * (i - 37) + "px";
      }
      if (i > 42 && i < 48) {
        Field.style.top = 245 - 40 * (i - 42) + "px";
        Field.style.left = "240px";
      }
      if (i == 48) {
        Field.style.top = "45px";
        Field.style.left = "280px";
      }

      // Dodajemo element
      const Main = document.getElementById("app");
      if (Main) Main.append(Field);

      // Pushamo polja u array da možemo posle da im pristupimo
      FieldsCollection.push(Field);
    }
  },

  methods: {
    RollDice() {
      this.DiceNumber = Math.floor(Math.random() * 6) + 1;
    },
  },
});
</script>

<style lang="scss">
#app {
  width: 100%;
  height: 100%;
}
</style>
