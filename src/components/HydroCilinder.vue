<template>
    <div>
        <h2>Расчет гидроцилиндра</h2>        
        <div class="data">
            <fieldset>
                <legend>Исходные данные:</legend>
                <label for="pressure">Давление в системе, бар: </label>
                <input type="text" id="pressure" v-model.number="pressure">
                <span class="error" v-if="typeof(this.pressure) !== 'number'">Введите число</span>
                <label for="plungerDiametr">Диаметр поршня, мм<sup>2</sup>: </label>
                <input type="text" id="plungerDiametr" v-model.number="plungerDiametr">
                <span class="error" v-if="typeof(this.plungerDiametr) !== 'number'">Введите число</span>
                <label for="workLength">Длина хода, мм: </label>
                <input type="text" id="workLength" v-model.number="workLength">
                <span class="error" v-if="typeof(this.workLength) !== 'number'">Введите число</span>                    
                <label for="stockDiametr">Диаметр штока, мм<sup>2</sup>: </label>
                <input type="text" id="stockDiametr" v-model.number="stockDiametr">
                <span class="error" v-if="typeof(this.stockDiametr) !== 'number'">Введите число</span>
                <button class="btn" @click="calculateHC">Рассчитать</button>
            </fieldset>
        </div>        
    </div>
</template>

<script>
export default {
    props: ['finalData'],
    emits: ['calculateHC'],
    data () {
        return {
            word: 'Слово',
            pressure: 0,
            plungerDiametr: 0,
            workLength: 0,
            stockDiametr: 0,
            calcHC: {
                valuePlunger: 0,
                valueStock: 0,
                workingTime: 0,
                emptyTime: 0,
                force: 0
            }            
        }
    },
    methods: {
        calculateHC () {
            this.calcHC.valuePlunger = (Math.pow(this.plungerDiametr, 2)  * Math.PI * this.workLength / 4) / 1000000

            this.calcHC.valueStock = (((Math.pow(this.plungerDiametr, 2)  * Math.PI / 4) - (Math.pow(this.stockDiametr, 2)  * Math.PI / 4)) * this.workLength) / 1000000

            this.calcHC.workingTime =  this.calcHC.valuePlunger * 60 / this.finalData.expenditure

            this.calcHC.emptyTime =  this.calcHC.valueStock * 60 / this.finalData.expenditure

            this.calcHC.force = (this.pressure * Math.pow(this.plungerDiametr, 2)  * Math.PI / 40) / 9.8

            this.$emit('calculateHC', this.calcHC)
        }
    }
}
</script>
