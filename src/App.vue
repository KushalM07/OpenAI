<template>
  <div>
    <div class="card">
      <div class="card-border">
        <div class="container">
          <h1 style="margin: 20px;">Generate Solutions using Open AI</h1>
          <div class="Text-Area">
            <textarea id="text_area" ref='text_area' cols="30"  rows="10"></textarea>
          </div>
          <div class="Query-Area">
            <input type="text" ref='query_area' id="query_area" placeholder="enter your Query">
          </div>
          <div class="Btns">
            <button id="btn1" ref="myButton" @click="GenerateText">Generate Text</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data() {
    return {
      StopGenertion:false,
      API_Key: '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',
      API_URL: 'https://api.openai.com/v1/chat/completions'
    }
  },
  methods: {
    GenerateText() {
      if (this.$refs.query_area.value === '') {
        alert('Invalid Input');
        this.$refs.query_area.value = '';
        return false;
      }
      else {
        this.$refs.myButton.disabled = true;
        this.$refs.myButton.innerText = 'Generating....'
        const headers = {
          ' Authorization': `Bearer ${this.API_Key}`,
          'Content-Type': 'application/json'
        };
        const data = {
          model: 'gpt-3.5-turbo',
          messages: [{ role: 'user', content: this.$refs.query_area.value }]
        };
        if(this.StopGenertion)
        {
          console.log()
          return;
        }
        axios.post(this.API_URL, data, { headers })
          .then(response => {
            console.log(response.data);
            this.$refs.text_area.value = response.data.choices[0].message.content
            this.$refs.myButton.disabled = false;
            this.$refs.myButton.innerText = 'Generate Text'
            this.$refs.query_area.value = '';
          })
          .catch(error => {
            console.error(error);
          });
      }
      
    },

  }
}
</script>

<style scoped>
.card {
  display: flex;
   /* background-color: #504fb5;  */
  height: 100%;
  justify-content: center;
}

.card-border {
  border: outset;
  color: black;
  box-shadow: rgba(151, 65, 252, 0.2) 0 15px 30px -5px;
  background-image: linear-gradient(144deg,#AF40FF, #5B42F3 50%,#00DDEB);
  border-radius: 20px;
  
}

.container {
  width: auto;
  height: 100%;
  /* background-color: #504fb5 */

}

.Text-Area {
  display: flex;
  height: 28rem;
  justify-content: center;
}

#text_area {
  resize: none;
  height: 70%;
  border-radius: 10px;
  width: 90%;
}

.Query-Area {
  display: flex;
  justify-content: center;
}

#query_area {
  display: flex;
  position: relative;
  margin: 5px;
  bottom: 130px;
  width: 90%;
  height: 20px;
  padding: 10px;
  border-radius: 50px;
}

.Btns {
  display: flex;
  justify-content: center;
  position: relative;
  bottom: 125px;
  margin: 20px;
  width: 100%;
}

#btn1 {
  display: flex;
  width: 80%;
  background-color: black;
  color: white;
  height: 30px;
  font-weight: bold;
  border-radius: 12px;
  justify-content: center;
  align-items: center;
  margin-right: 30px;
}
#btn1:hover {
  display: flex;
  width: 80%;
  background-color: white;
  transition: 1s;
  color: black;
  height: 30px;
  border-radius: 12px;
  justify-content: center;
  align-items: center;
  margin-right: 30px;
}
</style>
