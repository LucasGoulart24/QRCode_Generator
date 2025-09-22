<template>
  <h1>{{ titulo }}</h1>
  <div class="Gerador">
    <div class="Barra">
      <input type="text" placeholder="Adicione o link" v-model="link">
      <button @click="GerarQR">Gerar</button>
    </div>
    <div class="QRcode" v-if="QRcodeDataUrl">
      <img :src="QRcodeDataUrl" alt="QRCode" ref="qrImagem">
    </div>
    <button @click="Download" v-if="QRcodeDataUrl">Download</button>
  </div>
</template>

<script>
import QRCode from 'qrcode';

export default {
  name: 'App',
  data() {
    return {
      titulo: 'QRCode Generator',
      link: '',
      QRcodeDataUrl: null
    }
  },
  methods: {
    async GerarQR() {
      if(!this.link) {
        alert('Por favor, insira um link!')
        return
      }
      try {
        const url = await QRCode.toDataURL(this.link);
        this.QRcodeDataUrl = url
      }catch(err) {
        console.error('Error ao gerar o QR Code:', err);
      }
    },
    Download() {
      const link = document.createElement('a');
      link.href = this.QRcodeDataUrl;
      link.download = 'qrcode.png';
      link.click();
    }
  }
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: Arial, Helvetica, sans-serif;
  background: linear-gradient(to top, #30c67c, #82f4b1);
}

h1{
  font-size: 3.5rem;
  font-weight: 400;
  color: #ffffff;
  margin: 0;
}

button{
  width: 100%;
  height: 50px;
  border: none;
  font-size: 1rem;
  cursor: pointer;
  background-color: #30c67c;
}

.Gerador{
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  background-color: #ffffff;
  box-shadow: 0px 10px 10px 0px rgba(0,0,0,0.2);
}

.Barra{
  width: 100%;
  height: 6vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.Barra input{
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  border-bottom-left-radius: 5px;
  border-top-left-radius: 5px;
  background-color: #eee;
  padding: 0 10px;
}

.Barra button{
  width: 30%;
  height: 100%;
  border: none;
  cursor: pointer;
  color: #ffffff;
  background-color: #30c67c;
  font-size: 1.2rem;
}

.QRcode{
  width: 100%;
  height: 50vh;
  border-radius: 5px;
  margin-top: 10px;
  margin-bottom: 10px;
  background-color: #eee;
}

.QRcode img{
  width: 100%;
  height: 100%;
}
</style>