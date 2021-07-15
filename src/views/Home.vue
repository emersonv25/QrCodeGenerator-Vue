<template>
  <div id="app">
    <v-container fluid>
      <v-row>
        <v-col cols="12"> 
          <v-card class="mx-auto" max-width="800" max-height="600">
            <v-card-title class="text--primary justify-center"> Gerador de QR CODE </v-card-title>
            <v-col cols="12" sm="12" md="12">
              <v-text-field
                label="URL"
                filled
                rounded
                dense
                v-model="qr"
              ></v-text-field>
            </v-col>
              <v-alert
                v-if="erro != ''"
                dense
                outlined
                type="error"
              >
                {{erro}}
              </v-alert>
            <v-card-actions class="align-end justify-center">
              <v-btn large color="blue" outlined @click='gerar(qr)' :loading="qrLoading"> <v-icon left>mdi-qrcode</v-icon> Gerar </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col cols="12" v-if="qrExibir == true">
          <v-card max-width="480" class="mx-auto pb-5" id="pdf">
            <v-img :src="qrImg"></v-img>
          </v-card>
            <v-layout justify-center>
              <v-card-actions>
                <v-btn left color="blue" outlined @click="pdf()">
                  <v-icon>mdi-printer</v-icon>
                  Baixar PDF
                </v-btn>
                <v-btn left color="success" :href="qrImg" outlined download="QR-Code.png">
                  <v-icon>mdi-download</v-icon>
                  Baixar PNG
                </v-btn>                
              </v-card-actions>
            </v-layout>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios';
import { jsPDF } from "jspdf";
export default {
  name: "Home",
  data: () => ({
    qr: '',
    qrImg: '',
    qrImgData: '',
    qrExibir: false,
    qrLoading: false,
    erro: '',
  }),
  methods: {
    async gerar(link){
      this.qrImg = ''
      this.erro = ''
      this.qrExibir = false
      this.qrLoading = true
      var config = { responseType: 'blob' };
      await axios.get("https://chart.googleapis.com/chart?chs=480x480&cht=qr&chl=" + link, config ).then(resp => {
        this.qrImgData = resp.data
        this.qrImg = window.URL.createObjectURL(resp.data)
        this.qrLoading = false
        this.qrExibir = true
      }).catch(err => {
        this.erro = 'Ocorreu um erro na geração do QR Code, tente novamente !',
        this.qrLoading = false
        console.log(err)
      })
    },
    pdf(){
      var qrData = this.qrImg    
      var doc = new jsPDF()
      doc.setFontSize(35)
      doc.text(20, 25, 'Aponte a câmera do seu celular')
      doc.addImage(qrData, 'JPEG', 15, 40, 180, 160)
      doc.save("QRCode.pdf");
    }
  },
};
</script>