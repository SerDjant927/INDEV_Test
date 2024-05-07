<template>
  <div class="container">
    <div class="button-container">
      <input type="text" v-model="inputValue" @keyup.enter="addToArray" class="input" />
      <button @click="addToArray" :disabled="inputValue === ''" class="button">Добавить</button>
      <button @click="analyzeArray" class="button">Проанализировать массив</button>
      <button @click="resetArray" class="button">Сбросить результат</button>
    </div>
    <div class="result">
      <span>{{ array.join(', ') }}</span>
    </div>
    <div class="missing-numbers">
      <span v-if="missingNumbers.length > 0" class="missing-numbers-label">Пропущенные числа:</span>
      <span v-else class="missing-numbers-label">Пропущенные числа: нет</span>
      <span v-for="number in missingNumbers" :key="number" class="missing-number">{{ number }}</span>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputValue: '',
      array: [],
      missingNumbers: [],
    };
  },
  methods: {
    addToArray() {
      if (/^-?\d+$/.test(this.inputValue)) {
        const number = parseInt(this.inputValue);
        if (number >= 0) {
          if (number <= 100) {
            if (this.array.length === 0 || number < Math.min(...this.array.map(Number))) {
              if (/^0\d+$/.test(this.inputValue) || /^0$/.test(this.inputValue)) {
                alert('Число не может начинаться с нуля');
              } else {
                this.array.unshift(this.inputValue);
              }
            } else if (number > Math.max(...this.array.map(Number)) && number <= 100) {
              this.array = this.array.filter((num) => num <= number);
              this.array.push(this.inputValue);
            } else {
              alert('Число уже присутствует в массиве или превышает максимально допустимое значение');
            }
            this.inputValue = '';
          } else {
            alert('Возможна нехватка памяти (вводи числа меньше 100)');
          }
        } else {
          alert('Не вводи, подумой');
        }
      } else {
        alert('Не вводи буквы, прочие символы или дробные значения');
      }
    },
    analyzeArray() {
      const minNumber = Math.min(...this.array.map(Number));
      const maxNumber = Math.max(...this.array.map(Number));
      this.missingNumbers = Array.from({ length: maxNumber - minNumber + 1 }, (_, i) => i + minNumber)
        .filter((num) => !this.array.includes(String(num)));
    },
    resetArray() {
      this.array = [];
      this.missingNumbers = [];
    },
  },
};
</script>

<style scoped>
* {
  transition: all 0.3s;
  font-size: 1.3vw;
}

.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.input {
  width: 100%;
  padding: 5px;
}

.button-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  row-gap: 10px;
}

.button {
  display: inline-block;
  padding: 5px 10px;
  background-color: #4cafa9;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 8px;
}

.button:hover {
  opacity: 0.7;
}

.result {
  margin: 10px 0;
}

.missing-numbers {
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  row-gap: 5px;
  column-gap: 5px;
}

.missing-numbers-label {
  font-weight: 700;
  display: block;
  width: 100%;
}

.missing-number {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  padding: 2px 5px;
  background-color: #ff0000;
  color: #ffffff;
}

@media(max-width:768px) {
  * {
    font-size: 16px;
  }
}
</style>