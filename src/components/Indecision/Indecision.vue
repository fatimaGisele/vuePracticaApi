<template>
  <div>
      <img :src="image" alt="imageRandom" />
      <div class="bg-dark"></div>
      <div class="indecision-container">
          <input type="text" 
          placeholder="Hazme una pregunta"
          name="question" 
          v-model="question"
          />
        <p>Debe finalizarla con ?</p>
        <div v-if="isValid">
          <h2>{{question}}</h2>
          <h1>{{answer}}</h1>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            question: '',
            answer: null,
            image:null,
            isValid: false
        }
    },
    methods:{
        async useApi(){
            try {
                this.answer = '...pensando'
                const { answer, image } = await (await fetch('https://yesno.wtf/api')).json();
                this.answer = answer ==='yes' ? 'Sii!' : 'No!'
                this.image = image
            } catch (error) {
                console.log(error);
            }
        }
    },
    watch:{
        question(value, oldValue){
            this.isValid = false
            if(!value.includes('?')) return
            this.isValid = true
            this.useApi();
        }
    }
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

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>