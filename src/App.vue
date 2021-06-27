<template>
    <div style="margin-bottom:1rem;">
        <button class="btn btn-info " @click="zacladka = 0;">Валюты</button>
        <button class="btn btn-success" @click="zacladka = 1;">Конвертер</button>
    </div>
    <template  v-if="zacladka==0">
        <label>Поиск валюты:</label>
        <input type="text" size="50" maxlength="50" v-model="textSearch" style="margin-bottom:1rem;" />
        <div class="container-fluid">
            <div class="row justify-content-center">
                <div v-for="valute in valutes" :key="valute" class="col-5" style="margin-top:1rem; ">
                        <Card style="height:100%"  v-if="valute.check" :Name="valute.Name" :CharCode="valute.CharCode" :Previous="(valute.Previous-valute.Value).toFixed(4)" :Value="valute.Value.toFixed(4)" />
                </div>
            </div>
        </div>

    </template>
    <template v-else>
        <div class="container-fluid">
            <div class="row justify-content-center">
                <div class="col-md-4">
                    <div class="container-fluid">
                        <div class="row">
                            <div class="col-4">
                                <select class="form-select ms-auto" v-model="selectFrom">
                                    <option selected v-for="valute in valutesName" v-bind:key="valute.id">{{valute}}</option>
                                </select>
                            </div>
                            <div class="col-8">
                                <input class="form-control" type="number" v-model="inputFrom"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-md-1">
                    <div  style="margin-top:auto; background-color:chartreuse; border-radius:32px 32px;" @click="Rivers">
                        <img  src="https://img.icons8.com/small/32/000000/resize-horizontal.png" />
                    </div>
                </div>
                <div class="col-md-4">
                    <div class="container-fluid">
                        <div class="row">
                            <div class="col-4">
                                <select class="form-select ms-auto" v-model="selectTo">
                                    <option selected v-for="valute in valutesName" v-bind:key="valute.id">{{valute}}</option>
                                </select>
                            </div>
                            <div class="col-8">
                                <input readonly="readonly" class="form-control" type="number"  v-model="inputTo"/>
                            </div>
                        </div>
                    </div>
                </div>
                
            </div>
        </div>
    </template>
</template>
<script>
    import Card from './components/Card.vue'
    import axios from 'axios'
    export default {
        name: 'App',
        components: { Card },
        created: function () {

            this.getWorldMoney();
            
        },
        watch: {
            textSearch: function () {
                var l_textSearch = this.textSearch.toLowerCase();
                for (let key in this.valutes) {
                    if (this.textSearch.length == 0) { this.valutes[key]['check'] = true; }
                    else {
                        var st_all = this.valutes[key].CharCode + " " + this.valutes[key].Name;
                        st_all = st_all.toLocaleLowerCase();
                        this.valutes[key]['check'] = st_all.includes(l_textSearch);
                    }
                }
            },
            selectFrom: function () {
                this.updateValute();
            },
            inputFrom: function () {
                this.updateValute();
            },
            selectTo: function () {
                this.updateValute();
            }
        },
        data() {
            return {
                url: {
                    cbrJsDaily: "https://www.cbr-xml-daily.ru/daily_json.js"
                },
                valutes: [],
                valutesName: [],
                selectFrom: "",
                selectTo: "",
                inputFrom: 0,
                inputTo: 0,
                textSearch: "",
                zacladka: 1,
            }
        },
        methods: {
            Rivers() {
                let t = this.selectTo;
                this.selectTo = this.selectFrom;
                this.selectFrom = t;
            },
            updateValute() {
                if (this.selectTo.length > 0) {
                    this.inputTo = this.inputFrom * this.valutes[this.selectFrom].Value / this.valutes[this.selectTo].Value;
                    this.inputTo = this.inputTo.toFixed(4);
                }
                else {
                    this.inputTo = 0;
                }
            },
            getWorldMoney() {
                axios.get(this.url.cbrJsDaily)
                    .then((response) => {
                        this.valutes = response.data.Valute;
                        for (let key in this.valutes) { this.valutes[key].check = true; }
                        this.valutesName = Object.keys(response.data.Valute);
                    })
                    .catch((error) => {
                        // handle error
                        console.log(error);
                    })
                    .then(() => {
                        // always executed
                    });
            }
        }
    }
</script>

<style>
    body{
        background-color:aquamarine;
    }
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
        
    }
    .btn{
        margin-left:1rem;
    }
    input {
        text-decoration: underline;
        text-decoration-color: aqua;
        text-decoration-thickness:3px;
    }
    select {
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
    }
</style>
