<template>
    <div class="form-group col-xs-12" id="xyz">
        <label
            v-text = "label">
        </label>
        <input
            type="text"
            class ="pikaday form-control"
            :name = "name"
            ref="input_date"
            :value = "value == '' ? '' : getValue"
            :format = "format"
            :mode = "mode"
            @click="onclick"
        >
    </div>
</template>

<script>
import 'pikaday/css/pikaday.css';
import Pikaday from 'pikaday';
import moment from 'moment';
export default {
    props : {
        name : { default : ''},
        label : { default : ''},
        format : { default : 'DD/MM/YYYY'},
        mode : { default : 'AD'},
        startRange: { default : 2000 },
        endRange: { default : 2600 },
    },
    data() {
        return {
            value : ''
        }
    },
    computed :{
       getValue() {
           return this.mode == 'AD' ? moment(new Date(this.value)).format('DD/MM/YYYY') : moment(new Date(this.value)).add(543,'years').format('DD/MM/YYYY');
       },
       getYears (){
            return this.mode == 'AD' ? moment().format('YYYY') : moment().add(543,'years').format('YYYY');
        },
    },
    mounted() {
        var pikaday = new Pikaday({
            field : this.$refs.input_date,
            format : this.format,
            yearRange : [this.startRange,this.endRange],
            onSelect: (getDate) => {
                var date = this.mode == 'AD' ? moment(getDate).format('DD/MM/YYYY') : moment(getDate).add(-543,'years').format('DD/MM/YYYY');
                this.$emit('input', this.$refs.input_date.value);
                this.$emit('update',date);
            }
        }).gotoYear(this.getYears)
    },
    methods: {
        onclick () {
            console.log(document.querySelector("select.pika-select.pika-select-year").parentElement)
            let yearInt = parseInt(document.querySelector("select.pika-select.pika-select-year").parentElement.innerHTML)
            let modYear = yearInt.toString() + " (" + (yearInt+543) + ")"
            document.querySelector("select.pika-select.pika-select-year").parentElement.innerHTML = modYear
        }
    }
}
</script>
