<template>
  <v-app>
    <v-main>
      <v-card class="mx-auto" max-width="1200">
        <v-container fluid>
          <v-row dense>
            <v-col cols="12" sm="6">
              <v-card class="mx-auto" color="green" dark>
                <FormIsrExcel />
              </v-card>
            </v-col>
            <!-- <v-col cols="12" sm="6">
              <v-card class="mx-auto" color="orange" dark>
                <FormBrutoNeto />
              </v-card>
            </v-col>
            <v-col cols="12" sm="12">
              <v-card  color="#8E250F" dark>
                <FormNetoBruto />
              </v-card>
            </v-col> -->
          </v-row>
        </v-container>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
/* import FormBrutoNeto from "./components/FormBrutoNeto.vue";
import FormNetoBruto from "./components/FormNetoBruto.vue"; */
import FormIsrExcel from "./components/FormIsrExcel.vue";

export default {
  name: "App",
  components: {
    /* FormBrutoNeto,
    FormNetoBruto, */
    FormIsrExcel,
  },

  data: () => ({
    //
  }),
};
</script>
