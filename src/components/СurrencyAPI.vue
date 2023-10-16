<template>
    <div class="container">
        <div v-if="isLoad" class="loading">
            Loading data...
        </div>

        <div v-else class="data">
            <div class="image-and-button">
                <p><input type="number" v-model="count" @input="updateInput"/>$ в рублях</p>
                <h1>= {{ result }}</h1>
                <p class="info">* данные могут быть неточными и устаревшими на несколько часов</p>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'СurrencyAPI',
    data() {
        return{
            number: 0,
            count: 0,
            result: 0,
            isLoad: true
        }
    },
    methods: {
        updateInput() {
            if((this.count+0.00001).toString().length > 7) {
                this.count = parseFloat(this.count.toString().slice(0, 7).toString())
            }
            if(!isNaN(this.count)) {
                this.result = this.count * this.number
            }
            else {
                this.result = 0
            }
            this.result = Math.round(this.result * 10000) / 10000
        }
    },
    mounted() {
        const url = 'https://currency-exchange.p.rapidapi.com/exchange?from=USD&to=RUB&q=4';
        const options = {
            method: 'GET',
            headers: {
                'X-RapidAPI-Key': 'e26e24ff50msh687af2f427a0a92p146497jsna9fafbd84d5a',
                'X-RapidAPI-Host': 'currency-exchange.p.rapidapi.com'
            }
        };
        fetch(url, options)
        .then((res) => res.json())
        .then((res) => {
            console.log(res)
            this.number = res
            this.isLoad = false
        })
    },
}
</script>

<style scoped>
.container{
    height: 240px;
    width: 300px;
    margin: auto;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 1px solid rgba(255, 255, 255, 0.8);
    border-radius: 50px;
    background-color: rgba(0,0,0, .3);
    color: black;
}
.image-and-button{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.title {
    font-size: 2em;
    color: purple;
    margin: 15px;
    font-weight: bold;
}
.data {
    padding: 20px;
}
.loading {
    color: rgba(255, 255, 255, 0.8);
    padding-bottom: 30px;
}

input{
    width: 76px;
    height: 24px;
    padding: 0 10px 0 10px;
    font-size: 18px;
    transition: all 0.4s ease;
    font-weight: 400;
    box-sizing: content-box;
    background: transparent;
    outline: none;
    border: none;
    color: rgba(255, 255, 255, 0.8);
    font-family: Comfortaa, Avenir, Helvetica, Arial, sans-serif;
    border-bottom: 1px solid rgba(255, 255, 255, 0.3);
    text-align: center;
}
input:focus{
    border-bottom: 1px solid rgba(0,0,0, .3);
    outline: none;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
h1, p{
    transition: all 0.4s ease;
    color: rgba(255, 255, 255, 0.8);
}
p{
    font-size: 18px;
}
.info{
    color: rgba(255, 255, 255, 0.4);
    font-style: italic;
    text-align: center;
    animation-name: text;
    font-size: 10px;
}
</style>