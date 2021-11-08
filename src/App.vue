<template>
  <div class="app">
    <!-- Podaci o igračima -->
    <div class="players" v-if="UsernamesModal">
      <p>Unesite ime igrača:</p>
      <input
        class="user-input"
        v-model="PlayerData[0].Username"
        type="text"
        placeholder="Ime žutog igrača"
      />
      <input
        class="user-input"
        v-model="PlayerData[1].Username"
        type="text"
        placeholder="Ime plavog igrača"
      />
      <input
        class="user-input"
        v-model="PlayerData[2].Username"
        type="text"
        placeholder="Ime crvenog igrača"
      />
      <input
        class="user-input"
        v-model="PlayerData[3].Username"
        type="text"
        placeholder="Ime zelenog igrača"
      />
      <button @click="GameStarted = true">Pokreni igru</button>
    </div>

    <!-- Kockica -->
    <div class="dice" v-if="GameStarted">
      <p>Kocka je pala na: {{ DiceNumber }}</p>
      <p>Trenutni igrač: {{ PlayerNames[CurrentPlayer] }}</p>
      <!-- <p>Preostalo bacanja: {{ PlayerNames[CurrentPlayer] }}</p> -->

      <button @click="RollDice">Baci kockicu</button>
      <img v-if="DiceNumber > 0" :src="DiceImages[DiceNumber - 1]" alt="Dice Image" />
    </div>

    <!-- Tabla -->
    <div id="field-background" class="field-background" v-if="GameStarted">
      <div class="player-1-base">
        <div
          class="base"
          v-for="(Data, Index) in PlayerData[0].Pawns"
          :key="Index"
        >
          <img
            :src="require('@/assets/images/players/' + Data.Image + '.png')"
            :id="Data.ID"
            @click="HandleClick(Data.ID, this.CurrentPlayer)"
          />
        </div>
      </div>

      <div class="player-2-base">
        <div
          class="base"
          v-for="(Data, Index) in PlayerData[1].Pawns"
          :key="Index"
        >
          <img
            :src="require('@/assets/images/players/' + Data.Image + '.png')"
            :id="Data.ID"
            @click="HandleClick(Data.ID, this.CurrentPlayer)"
          />
        </div>
      </div>

      <div class="player-3-base">
        <div
          class="base"
          v-for="(Data, Index) in PlayerData[2].Pawns"
          :key="Index"
        >
          <img
            :src="require('@/assets/images/players/' + Data.Image + '.png')"
            :id="Data.ID"
            @click="HandleClick(Data.ID, this.CurrentPlayer)"
          />
        </div>
      </div>

      <div class="player-4-base">
        <div
          class="base"
          v-for="(Data, Index) in PlayerData[3].Pawns"
          :key="Index"
        >
          <img
            :src="require('@/assets/images/players/' + Data.Image + '.png')"
            :id="Data.ID"
            @click="HandleClick(Data.ID, this.CurrentPlayer)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

let FieldsCollection: Array<HTMLDivElement> = [];

enum PlayerColors {
  Yellow = 0,
  Blue = 1,
  Green = 2,
  Red = 3,
}

export default defineComponent({
  name: "App",
  data() {
    return {
      GameStarted: true,
      UsernamesModal: false,
      CurrentPlayer: PlayerColors.Yellow, // 0 = Yellow, 1 = Blue, 2 = Red, 3 = Green
      CurrentThrows: 0,
      StartingPositions: [37, 1, 13, 25], // Startne pozicije po bojama(igračima)
      DiceNumber: 0, // Broj koji je bacen
      DiceImages: [
        require("@/assets/images/dice/1.png"),
        require("@/assets/images/dice/2.png"),
        require("@/assets/images/dice/3.png"),
        require("@/assets/images/dice/4.png"),
        require("@/assets/images/dice/5.png"),
        require("@/assets/images/dice/6.png"),
      ],
      PlayerNames: ["Žuti", "Plavi", "Zeleni", "Crveni"],
      PlayerData: [
        {
          // Player 1
          Username: "",
          Score: 0,
          Pawns: [
            {
              ID: "yellow-1",
              Position: 0,
              Image: "yellow",
              InBase: true,
              InHome: false,
            },
            {
              ID: "yellow-2",
              Position: 0,
              Image: "yellow",
              InBase: true,
              InHome: false,
            },
            {
              ID: "yellow-3",
              Position: 0,
              Image: "yellow",
              InBase: true,
              InHome: false,
            },
            {
              ID: "yellow-4",
              Position: 0,
              Image: "yellow",
              InBase: true,
              InHome: false,
            },
          ],
        },
        {
          // Player 2
          Username: "",
          Score: 0,
          Pawns: [
            {
              ID: "blue-1",
              Position: 0,
              Image: "blue",
              InBase: true,
              InHome: false,
            },
            {
              ID: "blue-2",
              Position: 0,
              Image: "blue",
              InBase: true,
              InHome: false,
            },
            {
              ID: "blue-3",
              Position: 0,
              Image: "blue",
              InBase: true,
              InHome: false,
            },
            {
              ID: "blue-4",
              Position: 0,
              Image: "blue",
              InBase: true,
              InHome: false,
            },
          ],
        },
        {
          // Player 3
          Username: "",
          Score: 0,
          Pawns: [
            {
              ID: "red-1",
              Position: 0,
              Image: "red",
              InBase: true,
              InHome: false,
            },
            {
              ID: "red-2",
              Position: 0,
              Image: "red",
              InBase: true,
              InHome: false,
            },
            {
              ID: "red-3",
              Position: 0,
              Image: "red",
              InBase: true,
              InHome: false,
            },
            {
              ID: "red-4",
              Position: 0,
              Image: "red",
              InBase: true,
              InHome: false,
            },
          ],
        },
        {
          // Player 4
          Username: "",
          Score: 0,
          Pawns: [
            {
              ID: "green-1",
              Position: 0,
              Image: "green",
              InBase: true,
              InHome: false,
            },
            {
              ID: "green-2",
              Position: 0,
              Image: "green",
              InBase: true,
              InHome: false,
            },
            {
              ID: "green-3",
              Position: 0,
              Image: "green",
              InBase: true,
              InHome: false,
            },
            {
              ID: "green-4",
              Position: 0,
              Image: "green",
              InBase: true,
              InHome: false,
            },
          ],
        },
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
      Field.style.background = "lightblue";
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

      // Ukoliko je polje neka od startnih pozicija menjamo mu boju
      if (this.StartingPositions.includes(i)) {
        Field.innerText = "S";
        Field.style.background = "#00000030";
      }
      //Field.style.display = "none";
      FieldsCollection[i] = Field; // Kolekcija polja
      Field.id = `field-${i}`; // ID polja

      // Dodajemo element
      const Main = document.getElementById("app");
      if (Main) Main.append(Field);
    }
  },

  methods: {
    RollDice() {
      this.DiceNumber = Math.floor(Math.random() * 6) + 1;
      this.CurrentThrows++;

      let MaxThrows;
      // Proveravamo da li su svi piuni u bazi, ukoliko jesu dozovljavamo 3 bacanja
      this.PlayerData[this.CurrentPlayer].Pawns.every(
        (Pawn) => Pawn.InBase == true
      )
        ? (MaxThrows = 3)
        : (MaxThrows = 1);

      // Menjamo igrača
      if (this.CurrentThrows >= MaxThrows) {
        this.CurrentThrows = 0;
        this.CurrentPlayer++;
        if (this.CurrentPlayer > 3) this.CurrentPlayer = 0;
      }
    },

    HandleClick(PionID: string, Player: number) {
      const CurrentPionEl = document.getElementById(PionID);
      const PionObject = this.GetPionById(PionID);
      if (!PionObject) return alert("Pion nije pronađen. ID: " + PionID);

      if (
        PionObject.Image == "yellow" &&
        this.CurrentPlayer != PlayerColors.Yellow
      )
        return alert("Nije vaš pion!");

      if (PionObject.Image == "blue" && this.CurrentPlayer != PlayerColors.Blue)
        return alert("Nije vaš pion!");

      if (PionObject.Image == "red" && this.CurrentPlayer != PlayerColors.Red)
        return alert("Nije vaš pion!");

      if (
        PionObject.Image == "green" &&
        this.CurrentPlayer != PlayerColors.Green
      )
        return alert("Nije vaš pion!");

      if (PionObject.InBase && this.DiceNumber != 6)
        return alert("Pion je u bazi, treba da bacite 6!");

      const StartingFieldNumber = this.StartingPositions[Player];
      const StartingField = FieldsCollection[StartingFieldNumber];

      if (!StartingField || !CurrentPionEl)
        return alert(
          `Error: StartingField: ${StartingField} | CurrentPion: ${CurrentPionEl}`
        );

      // Da ne bi mogli da pomeramo piuna više puta
      this.DiceNumber = 0;

      if (PionObject.InBase) {
        // Postavljamo na startnu poziciju
        CurrentPionEl.style.position = "absolute";
        CurrentPionEl.style.top = StartingField.style.top;
        CurrentPionEl.style.left = StartingField.style.left;
        PionObject.InBase = false;
        PionObject.Position = StartingFieldNumber;
      } else {
        // Ako piun nije u bazi, pomeramo ga na novu poziciju
        const NextStepCount = PionObject.Position + this.DiceNumber;
        let NextField = FieldsCollection[PionObject.Position + this.DiceNumber];

        if (NextStepCount > FieldsCollection.length) {
          NextField = FieldsCollection[NextStepCount - FieldsCollection.length];
          PionObject.Position = NextStepCount - FieldsCollection.length;
          CurrentPionEl.style.position = "absolute";
          CurrentPionEl.style.top = NextField.style.top;
          CurrentPionEl.style.left = NextField.style.left;
          return;
        }

        PionObject.Position = NextStepCount;
        CurrentPionEl.style.position = "absolute";
        CurrentPionEl.style.top = NextField.style.top;
        CurrentPionEl.style.left = NextField.style.left; // Postavljamo na novu poziciju
      }
    },

    GetPionById(PionID: string) {
      for (let i = 0; i < this.PlayerData.length; i++) {
        if (this.PlayerData[i].Pawns.find((Pawn) => Pawn.ID == PionID))
          return this.PlayerData[i].Pawns.find((Pawn) => Pawn.ID == PionID);
      }
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
  img {
    width: 150px;
    height: 150px;
    /*
    animation: shake 0.5s;
    animation-iteration-count: 5; */
  }
}

.selected {
  border: brown;
}

.field-background {
  width: 528px;
  height: 535px;
  margin: 33px 24px;
  border: 5px ridge gray;
  padding: 0px;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  justify-items: center;
  grid-gap: 130px;
}

.player-1-base {
  width: 140px;
  height: 140px;
  background: yellow;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-2-base {
  width: 140px;
  height: 140px;
  background: blue;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-3-base {
  width: 140px;
  height: 140px;
  background: red;
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.player-4-base {
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
  background: #fff;

  img {
    width: 40px;
    height: 40px;
    z-index: 1;
    &:hover {
      border: 2px solid black;
      border-radius: 50%;
    }
  }
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

@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}
</style>


