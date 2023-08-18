<script>
export default {
  data() {
    return {
      defLetter: 'B',
      defNumber: '0',
      newNumber: null,
      selectedTags: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75],
      BingoInformation: {
        "B": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
        "I": [16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30],
        "N": [31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45],
        "G": [46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60],
        "O": [61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75],
      }
    }
  },
  methods: {
    randomCalc(steps) {
      let item = Math.random() * steps;
      return Math.floor(item);
    },
    randomLetter() {
      let keys = Object.keys(this.BingoInformation);
      let randomKeyIndex = Math.floor(Math.random() * keys.length);
      return keys[randomKeyIndex];
    },
    startBingo() {
      let finalSteps = this.selectedTags.length;
      this.animateNumber(finalSteps, 10);
    },
    RestoreBingo(){
      this.selectedTags = [...Array.from({ length: 75 }, (_, index) => index + 1)];
    },
    selectedLetter(){
      let letter = '';
      if (this.defNumber >= 1 && this.defNumber <= 15) {
        letter = 'B';
      } else if (this.defNumber >= 16 && this.defNumber <= 30) {
        letter = 'I';
      } else if (this.defNumber >= 31 && this.defNumber <= 45) {
        letter = 'N';
      } else if (this.defNumber >= 46 && this.defNumber <= 60) {
        letter = 'G';
      } else {
        letter = 'O';
      }
      this.defLetter = letter;
    },

    isNumberSelected(number) {
      let findNumber = this.selectedTags.indexOf(number)
      return findNumber === this.defNumber ? this.startBingo() : this.selectedTags.splice(findNumber, 1)
    },

    isBoardSelected(number) {
      return !this.selectedTags.includes(number)
    },

    async animateNumber(steps, delay) {
      let counter = 0;
      const delayAsync = (ms) => new Promise(resolve => setTimeout(resolve, ms));
      const animate = async () => {
        if (counter < 15) {
          let numDelay = this.randomCalc(steps);
          this.defNumber = this.selectedTags[numDelay]
          this.defLetter = this.randomLetter();
          counter++;
          await delayAsync(delay);
          await animate();
        }
      };

      await animate();
      this.selectedLetter();
      this.newNumber = this.defNumber; // Set the newNumber after animation is complete
      this.isNumberSelected(this.newNumber);
    }
  }
}

</script>

<template>
  <div class="wrapper flex flex-col justify-between gap-y-10">
    <section class="mt-12">
      <h1 class="text-center text-slate-300 text-5xl font-bold">Bingo Vue</h1>
    </section>
    <main>
      <section class="flex flex-col gap-y-8 letters">
        <ul class="flex gap-x-4 justify-center">
          <li class="text-7xl font-bold">{{ defLetter }}</li>
          <li class="text-7xl font-bold">{{ defNumber }}</li>
        </ul>
        <div class="flex flex-col justify-center md:justify-normal md:flex-row md:gap-x-8 gap-y-4 items-center">
          <button @click="startBingo"
            class="bg-amber-500 hover:bg-amber-400 transition-colors duration-300 text-black rounded-lg font-bold px-10 text-2xl py-4 flex-auto" :class="{'ui-disabled': !this.selectedTags.length, 'bg-black/60' : !this.selectedTags.length,  'text-gray-600' : !this.selectedTags.length}">Sortear
            Números</button>
            <button @click="RestoreBingo" v-if="!this.selectedTags.length"
            class="bg-amber-500 hover:bg-amber-400 transition-colors duration-300 text-black rounded-lg font-bold px-10 text-2xl py-4 flex-auto">Refrescar</button>
        </div>
      </section>
      <section class="mt-10">
        <div class="board">
          <ul class="flex flex-row md:flex-col">
            <li>
              <p class="key">B</p>
              <p v-for="numbers in BingoInformation.B" :class="{ selected: isBoardSelected(numbers) }" class="numbers">
                {{ numbers }}</p>
            </li>
            <li>
              <p class="key">I</p>
              <p v-for="numbers in BingoInformation.I" :class="{ selected: isBoardSelected(numbers) }" class="numbers">
                {{ numbers }}</p>
            </li>
            <li>
              <p class="key">N</p>
              <p v-for="numbers in BingoInformation.N" :class="{ selected: isBoardSelected(numbers) }" class="numbers">
                {{ numbers }}</p>
            </li>
            <li>
              <p class="key">G</p>
              <p v-for="numbers in BingoInformation.G" :class="{ selected: isBoardSelected(numbers) }" class="numbers">
                {{ numbers }}</p>
            </li>
            <li>
              <p class="key">O</p>
              <p v-for="numbers in BingoInformation.O" :class="{ selected: isBoardSelected(numbers) }" class="numbers">
                {{ numbers }}</p>
            </li>
          </ul>
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped lang="scss">
.selected {
  background-color: rgb(245, 188, 66) !important;
  color: black;
  font-weight: 600;
}

.board {
  // clip: auto;
  border: 1px solid black;
  margin: auto;
  width: fit-content;
  border-radius: 10px;
  overflow: clip;

  li {
    align-items: center;
    display: flex;
    justify-content: center;
    gap: 0;
  }

  .key {
    color: black;
    font-weight: bold;
    width: 50px;
    height: 50px;
    background-color: hsl(0, 0%, 60%);
    border: 1px solid black;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .numbers {
    border-collapse: collapse;
    align-items: center;
    background: rgb(70, 70, 70);
    border: 1px solid black;
    display: flex;
    height: 50px;
    justify-content: center;
    width: 50px;
  }
}

.letters {
  li {
    align-items: center;
    aspect-ratio: 1/1;
    background: rgb(46, 46, 46);
    border-radius: 12px;
    display: flex;
    height: 200px;
    justify-content: center;
    width: 200px;
  }
}

@media screen and (max-width: 600px) {
  .letters li {
    padding-inline: 15px;
    height: 100px;
    width: 100px;
  }
}

@media screen and (max-width: 768px) {
  .board li {
    flex-direction: column;
  }
}</style>