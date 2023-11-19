<template>

  <div class="game">
    <div class="simon">
      <div class="wrap">
        <div class="red simon-button" ref="1" @click="click(1)"></div>
        <div class="blue simon-button" ref="2" @click="click(2)"></div>
      </div>
      <div class="wrap">
        <div class="yellow simon-button" ref="3" @click="click(3)"></div>
        <div class="green simon-button" ref="4" @click="click(4)"></div>
      </div>
    </div>
    <button class="btn-start" @click="startGame">
      {{buttonText}}
    </button>
    <div class="setting">
      <p>Round: {{round}}</p>
      <hr>
      <p>Difficulty level</p>
      <label v-for="item in ['Easy', 'Medium', 'Hard']" :key="item">
        <input
            type="radio"
            v-model="level"
            name="level"
            @click="time = levelTime[item]"
            :value="item">
        {{item}}
      </label>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      level: 'Easy',
      round: 0,
      inGame: false,
      time: 1500,
      timeoutID: null,
      buttonText: 'Start',
      levelTime: {
        Easy: '1500',
        Medium: '1000',
        Hard: '400'
      },
      orderList: [],
      userList: [],
      audio: {
        1: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound1.mp3'),
        2: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound2.mp3'),
        3: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound3.mp3'),
        4: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound4.mp3')
      }
    }
  },

  methods: {
    click (id) {
      this.audio[id].play()
      this.$refs[id].style.opacity = '0.5'
      setTimeout(() => {
        this.$refs[id].style.opacity = '1'
      }, 200)
      if (this.inGame) {
        clearTimeout(this.timeoutID)
        this.userList.push(id)
        this.checkUserList(this.userList.length - 1)
      }
    },

    startGame () {
      this.orderList = []
      this.userList = []
      this.time = this.levelTime[this.level]
      this.step()
    },

    step () {
      this.userList = []
      this.playOrderList()
      this.buttonText = 'Listen'
    },

    playOrderList () {
      this.inGame = false
      this.orderList.push(this.random())

      this.orderList.forEach((id, i) => {
        setTimeout(() => {
          this.click(id)
        }, 700 * i)
      })
      setTimeout(() => {
        this.inGame = true
        this.buttonText = 'Repeat'
        this.timer()
      }, this.orderList.length * 700)
    },

    checkUserList (item) {
      if (this.userList[item] !== this.orderList[item]) {
        return this.gameOver()
      }
      if (this.userList.length === this.orderList.length) {
        this.round++
        setTimeout(() => {
          this.step()
        }, 1000)
      } else {
        this.timer()
      }
    },

    timer () {
      this.timeoutID = setTimeout(() => {
        this.gameOver()
      }, this.time)
    },

    random () {
      const num = 1 + Math.random() * (4 + 1 - 1)
      return Math.floor(num)
    },

    gameOver () {
      this.buttonText = 'Game Over'
      setTimeout(() => {
        this.buttonText = 'Start'
      }, 1000)
      clearTimeout(this.timeoutID)
      this.round = 0
      this.inGame = false
      this.userList = []
      this.orderList = []
    },
  }

}
</script>

<style lang="scss">
.simon {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}
.setting {
  font-family: Trattatello, fantasy;
  font-weight: normal;
  color: #2F4F4F;
}
.btn-start {
  width: 208px;
  height: 32px;
  box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  text-transform: uppercase;
  font-family: Trattatello, fantasy;
  color: #FFf;
  background: #708090;
  border: 1px solid #2F4F4F;
  letter-spacing: 2px;
  font-size: 16px;
  text-align: center;
}
.simon-button {
  width: 100px;
  height: 100px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  opacity: 1;
  color: #2F4F4F;
  margin: 1px 1px 0 0;
  border: 2px solid black;
}
.yellow {
  background-color: #f1c40f;
  border-radius: 0px 10px 0px 10px;
}

.blue {
  background-color: #3498db;
  border-radius: 0px 10px 0px 10px;
}

.green {
  background-color: #2ecc71;
  border-radius: 10px 0px 10px 0px;
}

.red {
  background-color: #e74c3c;
  border-radius: 10px 0px 10px 0px;
}
</style>