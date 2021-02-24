<template>
  <div>
    <div v-if="!calculated">
      <div class="home_page">
        <div class="header_section">
          <h1 class="header">FLAMES</h1>
          <h3 class="sub_header">A Relationship Calculator</h3>
        </div>
        <div class="form">
          <input type="text" class="your_name" placeholder="Your Name" v-model="myName" required>
          <input type="text" class="partner_name" placeholder="Your Partner's Name" v-model="myPartner" required>
          <input type="submit" value="Let's Go" class="submit" @click="getFlames">
        </div>
      </div>
    </div>
    <div v-if="calculated" class="show_result">
      <div class="result">{{result}}</div>
      <button @click="toggleCalculate">Go Back</button>
    </div>
  </div>
</template>
<script>
export default {
  
  data() {
    return {
      myName: '',
      myPartner: '',
      calculated: false,
      flamesMap: {
        F : 'Friendship',
        L : 'Love',
        A : 'Affection',
        M : 'Marriage',
        E : 'Enemies',
        S : 'Siblings'
      },
      result: ''
    };
  },
  
  methods: {
    getFlames() {
        let smallString = '';
        let largeString = '';
        if(this.myName.length > this.myPartner.length) {
          smallString = this.myPartner.toLowerCase().trim().replace(/ /g,'');
          largeString = this.myName.toLowerCase().trim().replace(/ /g,'');
        } else {
          smallString = this.myName.toLowerCase().trim().replace(/ /g,'');
          largeString = this.myPartner.toLowerCase().trim().replace(/ /g,'');
        }
        let Length = smallString.length + largeString.length;
        var i = 0;
        while(i < smallString.length) {
          if(largeString.search(smallString.charAt(i)) > -1) {
            Length = Length - 2;
            largeString = largeString.replace(smallString.charAt(i),'');
            smallString = smallString.replace(smallString.charAt(i),'');
            i=0;
          } else i++;
        }
        let flames =  ['F','L','A','M','E','S'];
        do {
          flames = this.modifyFlames(Length, flames);
        } while (flames.length > 1);
        this.calculated = true;
        this.result = this.flamesMap[flames[0]];
    },
    toggleCalculate() {
      this.calculated = false;
    },
    modifyFlames(Length, flamesLeft) {
      let tempQueue = [];
      let index = Length % flamesLeft.length;
      if(index == 0) index = flamesLeft.length;
      let char = flamesLeft[index - 1];
      while(flamesLeft[0] !== char) {
          tempQueue.push(flamesLeft[0]);
          flamesLeft.shift();
      }
      flamesLeft.shift();
      for(var i = 0; i < tempQueue.length; i++) {
        flamesLeft.push(tempQueue[i]);
      }
      return flamesLeft
    }
  }
  }
</script>
<style lang="scss">
  body {
    margin: 0;
    padding: 0;
    background-color: #25306c;
  }
  .home_page {
    color: white;
    .header_section {
      padding-top: 6rem;
      text-align: center;
      .header, .sub_header   {
        margin: 0;
      }
      .sub_header {
        margin-bottom: 2rem;
      }
    }
    .form {
      text-align: center;
      input {
        outline: none;
        padding: 0.5rem;
        border: 0px;
        border-radius: 1rem;
        background-color: white;
        &:focus{
          outline: none;
        }
      }
    }
    input {
      text-align: center;
      background-color: white;
    }
  }
  .show_result {
    color: white;
    text-align: center;
    padding-top: 10rem;
    .result {
      font-size: 20rem;
    }
    button {
      background-color: white;
    }
  }
</style>