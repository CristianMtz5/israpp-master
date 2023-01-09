<template>
  <v-container>
    <v-card-title class="text-h6 font-weight-light">
      Calculo por excel
    </v-card-title>
    <!-- <v-switch v-model="algo" :label="`Switch 1: ${algo.toString()}`"></v-switch> -->
    <v-combobox
      v-model="añoData"
      :items="años"
      label="Año"
      outlined
      dense
    ></v-combobox>
    <v-combobox
      v-model="periodoData"
      :items="periodo"
      label="Periodo"
      outlined
      dense
    ></v-combobox>
    <v-file-input
      id="archivo"
      label="File input"
      @change="subirExcel()"
      outlined
      dense
    ></v-file-input>
    <!-- <v-text-field
      class="isrData"
      v-model="isrData"
      label="Isr"
      outlined
      dense
      readonly
    ></v-text-field>
    <v-text-field
      v-model="netoData"
      label="Neto"
      outlined
      dense
      readonly
    ></v-text-field> -->
    <v-data-table 
    :headers="headers"
    :items="itemsFile"
    :items-per-page="5"
    class="elevation-1"
    ></v-data-table>
    <!-- <br> -->
    <!-- <label v-if="algo">bruto a neto</label>
    <label v-else>neto a bruto</label> -->
    <!-- <v-text-field v-model="datos.porcentaje" label="Ingreso" aria-required="true" outlined dense></v-text-field>
    <v-text-field v-model="datos.cuota" label="Ingreso" aria-required="true" outlined dense></v-text-field> -->
  </v-container>
</template>

<script>
import axios from "axios";
import readXlsFile from "read-excel-file";

export default {
  name: "FormIsrExcel",
  data() {
    return {
      //algo: true,
      añoData: "",
      periodoData: "",
      isrData: "",
      netoData: "",
      dataApi: [],
      //Items
      años: [
        "2015",
        "2016",
        "2017",
        "2018",
        "2019",
        "2020",
        "2021",
        "2022",
        "2023",
      ],
      periodo: ["Mensual", "Quincenal", "Semanal", "Diario"],
      //Items Tabla
      itemsFile: [],
      headers: [
        {
          text: "Ingresos Brutos",
          align: "start",
          value: "0"
        },
      ],
    };
  },
  methods: {
    calculaIsrExcel(brutoData, tablasIsr) {
      for (let i = 0; i < tablasIsr.data.length; i++) {
        //console.log(tablasIsr.data[i].limInf);
        let limInf = tablasIsr.data[i].limInf;
        let porcentaje = tablasIsr.data[i].porcentaje;
        let cuota = tablasIsr.data[i].cuota;
        if (
          tablasIsr.data[i].limInf < brutoData &&
          tablasIsr.data[i].limSup > brutoData
        ) {
          let base = brutoData - limInf;
          let impuestoMar = base * porcentaje;
          this.isrData = impuestoMar + cuota;
          this.isrData = this.isrData.toFixed(2);
          this.netoData = brutoData - this.isrData;
          this.netoData = this.netoData.toFixed(2);
        }
      }
    },
    getTablasIsr(brutoData) {
      axios
        .get(
          "http://localhost:8081/Isr/año/" +
            this.añoData +
            "/" +
            this.periodoData
        )
        .then((response) => {
          this.dataApi = response.data.data;
          //this.dataApi.acquisitions = this.dataApi.acquisitions.map((e) => e.data);
          this.calculaIsrExcel(brutoData, response);
        });
    },
    subirExcel() {
      let inputFile = document.getElementById("archivo");
      readXlsFile(inputFile.files[0]).then((rows) => {
        this.itemsFile = rows;
        for (let i = 0; i < this.itemsFile.length; i++) {
          //console.log(this.itemsFile[i].toString());
          this.getTablasIsr(this.itemsFile[i].toString());
        }
      });
    },
  },
};
</script>

<style>
/* .btn-container {
  margin-bottom: 27px;
} */
</style>
