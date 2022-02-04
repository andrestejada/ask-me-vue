<template>
  <img v-if="image" :src="image" alt="imagen">
  <div class="bg-dark" ></div>
  <div class="indecision-container">
    <input type="text" placeholder="Haz me una pregunta" v-model="question" >
    <p>Recuerda terminar con signo de interrogación (?) </p>
    <div>
      <h2 v-if="isValidQuestion" >{{question}}</h2>
      <h1>{{answer}}</h1>
    </div>
  </div>
  
</template>

<script lang="ts" >
import { defineComponent } from "@vue/runtime-core"

export default defineComponent({
  name:'Indesicion',
  data(){
    return{
      question:'',
      image:null,
      isValidQuestion:false,
      answer:'',
    } as DataIndesicion
  },
  methods:{
    async getAnswer(){
      this.answer='Por favor espere...'
      const res:Res = await fetch('https://yesno.wtf/api').then(r=>r.json());
        const {answer,image} = res;
        this.image= image;
        this.answer = answer === 'yes' ? 'SI!' : 'NO!';   
    }
  },
  watch:{
    question(value:string){
      if(!value.includes('?')) return
      this.getAnswer();
      this.isValidQuestion=true;
    }
  }

});

interface DataIndesicion{
  question:string;
  image:null | string;
  isValidQuestion:boolean;
  answer:string;
}

interface Res{
  answer: string,
  forced: boolean,
  image: string
}
</script>

  <style scoped>

    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>
