<template>
  <div class="app">
    <div class="dice">
      <p>Kocka je pala na: {{ DiceNumber }}</p>
      <button @click="RollDice">Baci kockicu</button>
    </div>
    <div class="field-background">
      <div class="player-1-base">
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
      </div>
      <div class="player-2-base">
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
      </div>
      <div class="player-3-base">
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
      </div>
      <div class="player-4-base">
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
        <div class="base"></div>
      </div>
      <div
        class="img"
        v-for="(Image, Index) in PlayerImages"
        v-bind:key="Index"
      >
        <img
          :src="require(`@/assets/images/players/${Image}.png`)"
          :alt="'Player ' + Index"
          :class="'player' + Index"
        />
        <img
          :src="require(`@/assets/images/players/${Image}.png`)"
          :alt="'Player ' + Index"
          :class="'player' + Index"
        />
        <img
          :src="require(`@/assets/images/players/${Image}.png`)"
          :alt="'Player ' + Index"
          :class="'player' + Index"
        />
        <img
          :src="require(`@/assets/images/players/${Image}.png`)"
          :alt="'Player ' + Index"
          :class="'player' + Index"
        />
        <!-- require('@/assets/images/players/' + Index + '.png')" -->
      </div>
    </div>
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
      PlayerImages: ["yellow", "blue", "green", "red"],
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
      //Field.innerHTML = `${i}`;

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

      if (this.StartingPositions.includes(i)) {
        Field.style.background = "#00000030"
      }

      // Dodajemo element
      const Main = document.getElementById("app");
      if (Main) Main.append(Field);

      // Pushamo polja u array, možda zatreba
      FieldsCollection.push(Field);
    }
  },

  methods: {
    RollDice() {
      this.DiceNumber = Math.floor(Math.random() * 6) + 1;
    },

    Move(Player: number, Value: number) {
      if (Value < 0 || Value > FieldsCollection.length)
        return alert("Neispravan broj polja." + "0-" + FieldsCollection.length);

      const Field = FieldsCollection[Value];
    },
  },
});
</script>

<style lang="scss">
#app {
  width: 100%;
  height: 100%;
}

.dice {
  position: absolute;
  left: 600px;
  top: 40px;
}

.field-background {
  position: relative;
  width: 528px;
  height: 535px;
  margin: 40px 26px;
  border: 5px ridge gray;
  padding: 0px;
}

.player-1-base {
  position: absolute;
  top: 40px;
  left: 40px;
  width: 140px;
  height: 140px;
  background: yellow;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-2-base {
  position: absolute;
  top: 40px;
  right: 40px;
  width: 140px;
  height: 140px;
  background: blue;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-3-base {
  position: absolute;
  bottom: 40px;
  right: 40px;
  width: 140px;
  height: 140px;
  background: red;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-4-base {
  position: absolute;
  bottom: 40px;
  left: 40px;
  width: 140px;
  height: 140px;
  background: green;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.base {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin: 0 auto;
  background: #00000090;
}

.player0 {
  width: 40px;
  height: 40px;
  position: absolute;
  top: 0px;
  left: 0px;
}
.player1 {
  width: 40px;
  height: 40px;
  position: absolute;
  top: 0px;
  right: 0px;
}
.player2 {
  width: 40px;
  height: 40px;
  position: absolute;
  bottom: 0px;
  left: 0px;
}
.player3 {
  width: 40px;
  height: 40px;
  position: absolute;
  bottom: 0px;
  right: 0px;
}
</style>
