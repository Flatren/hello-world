<template>
    <div class="left-text card">
        
        <div class="container-fluid">
            <div class="row">
                <div class="col-12 name">{{d_Name}}</div>
            </div>
            <div class="row justify-content-between">
                <div class="col-md-7">
                    <span class="value"> {{valueOne}} </span>
                    <span class="charcode"> {{d_CharCode}} </span>
                    <span class="harr" @click="Rivers"> &harr; </span>
                    <span class="value"> {{parseFloat(d_Value).toFixed(4)}} </span>
                    <span class="charcode"> {{charCodeTwo}} </span>
                </div>
                <div class="col-md-3" style="text-align:right">
                    
                    <div class="def" :class="[parseFloat(Previous).toFixed(4)<0?'col-red':'col-green']">
                        <span class="s-size" v-if="parseFloat(Previous)<0"> &darr;</span>
                        <span class="s-size" v-else> &uarr;</span>
                        {{ Math.abs(Previous).toFixed(4) }}
                    </div>
                </div>
            </div>
            
           
        </div>
    </div>
</template>

<script>

    export default {
        name: 'Card',
        props: {
            Name: String,
            CharCode: String,
            Previous: String,
            Value:String
            },
        data() {
            return {
                charCodeTwo: "RUB",
                valueOne: 1,
                d_Value: this.Value,
                d_CharCode: this.CharCode,
                d_Name: this.Name

            }
        },
        methods: {
            Rivers()
            {
                var t = this.d_CharCode;
                this.d_CharCode = this.charCodeTwo;
                this.charCodeTwo = t;
                this.d_Value = 1 / this.d_Value;
            }
        }
    }
</script>

<style scoped>
    .left-text{
        text-align:left;
    }
    .card {
        background-color: #899189;
        border-radius: 30px;
        padding: 1rem;
        color: antiquewhite;
        font-size: 2rem;
    }
    .name {
        font-size: 1rem;
    }
    .harr {
        font-size: 3rem;
    }
    .s-size {
        font-size: 3rem;
    }
    .col-green {
        color: green;
    }
    .col-red {
        color: red;
    }
    .def {
        
    }
    .charcode {
    }
</style>