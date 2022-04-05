<template>
   <v-app>
      <v-app-bar app color="primary" dark>
         <v-spacer></v-spacer>
         <span class="mr-2">Translate</span>
      </v-app-bar>

      <v-main>
         <v-col class="d-flex" cols="12" sm="6">
            <v-select  label="Standard" dense></v-select>
         </v-col>

         <v-container fluid>
            <v-row>
               <v-col cols="12" sm="6">
                  <v-textarea
                     clearable
                     clear-icon="mdi-close-circle"
                     label="From"
                     v-model="text"
                     @keyup="chngtxt"
                  ></v-textarea>
               </v-col>
               <v-col cols="12" sm="6">
                  <v-textarea
                     clearable
                     background-color="grey lighten-2"
                     clear-icon="mdi-close-circle"
                     label="To"
                     v-model="result"
                     disabled
                  ></v-textarea>
               </v-col>
            </v-row>
         </v-container>
      </v-main>
   </v-app>
</template>

<script>
import axios from 'axios';
const baseurl= 'https://api-translate.azharimm.site'
export default {
   name: "HomeView",

   data() {
      return {
         text: null,
         result: null,
         to: "id",
         to_list: [],
         is_loading: false,
         is_error: false,
         cancel_source: null,
      };
   },

   methods: {
      chngtxt(){
         if (this.text) {
            this.is_loading = true;
            this.is_error = false;
            console.log(this.text);
            this.translate;
         } else {
            this.result = null;
         }
      },
      async translate(){
         try {
            this.cancelTranslate();
            this.cancel_source = axios.cancelToken.source();
            const result = await axios.get(`${baseurl}/translate?engine=google&text=${this.text}&to=${this.to}`,
            { cancelToken: this.cancel_source.token});
            this.result = result.data.data.result
         } catch (error) {
            console.log(error);
         }
      },
      cancelTranslate(){
         if (this.cancel_source) {
            this.cancel_source.cancel();
         }
      }
   },
};
</script>
