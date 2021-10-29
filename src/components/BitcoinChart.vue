<template>
    <v-container>
        <p>Aquí va a ir mi gráfica de bitcoin con {{cripto}} {{moneda}} {{plazo}}</p>
            <v-progress-circular
      indeterminate
      color="primary"
      v-if="loading"
    ></v-progress-circular>

        <apexchart v-if="!loading" width="100%" type="line" :options="options" :series="series" v-bind:key="series[0].data"></apexchart>
    </v-container>
</template>

<script>
export default {
    props: ['cripto','moneda','plazo'],

    watch:{
        series:function(newSeries,oldSeries){
            console.log(newSeries);
            console.log(oldSeries);
        },
        cripto:function(newCripto,oldCripto){
            console.log(newCripto)
            console.log(oldCripto)
            this.cargarDatos();
        },
        moneda:function(newCripto,oldCripto){
            console.log(newCripto)
            console.log(oldCripto)
            this.cargarDatos();
        },
        plazo:function(newCripto,oldCripto){
            console.log(newCripto)
            console.log(oldCripto)
            this.cargarDatos();
        }
    },
    methods:{

        async cargarDatos(){
            this.loading=true;
            //TODO implementar aquí su llamada deberemos cambiar la data de categories y de data
            var datos=[]
            var fechas=[]
            await this.axios.get('https://api.coingecko.com/api/v3/coins/'+this.cripto+'/market_chart?vs_currency='+this.moneda+'&days='+this.plazo+'').then((response)=>{
                console.log(response);
                response.data.prices.forEach(element => {
                    datos.push(element[1])
                    fechas.push(element[0])
                });


            })
            this.options={
                xaxis:{
                    categories:fechas
                }
            }
            this.series = [{
                data: datos
            }]
            this.loading=false;
        }
    },

    data: ()=>({
        loading:false,
      options: {
        xaxis: {
        categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        },
      },
      series: [{
        name: 'series-1',
        data: [30, 40, 45, 50, 49, 60, 70, 91]
      }]
    })
}
</script>
