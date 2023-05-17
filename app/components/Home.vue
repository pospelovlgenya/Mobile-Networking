<template>
  <Page class="page">
    <ActionBar class="ab">
      <Label text="Networking" class="header" />
    </ActionBar>       
     
    <GridLayout rows="*, auto, auto, *, *" columns="*">
      <!-- <Label row="0" :text="show" textWrap="true" /> -->
      <TextField row="1" hint="ipv4 adress input" v-model="ip" />
      <Button row="2" text="Fint this ip" @tap="find()" />
      <!-- <Label row="3" :text="qe" textWrap="true" /> -->
        
    </GridLayout>
    
  </Page>
</template>
    
<script>
import MainData from './MainData.vue';
export default {
  data () {
    return {
      ip: '5.141.28.183',
      qe: 0,
      show: '-10',
    };
  },
  methods: {
    async find() {
      const response = await fetch("https://api.shodan.io/shodan/host/" + this.ip + "?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      const req = await response.json();
      this.$navigateTo(MainData, {props: {req: req}});
      
      
      // this.show = await req;
      // const req = await response.text();
      // const response2 = await fetch("https://api.shodan.io/shodan/host/8.8.8.8?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      // const eq = await response2.json();
      // this.qe = await response.text();
      // this.qe = await Object.keys(eq);
      // var req = Http.getString("https://api.shodan.io/shodan/host/8.8.8.8?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW")
      // var req = Http.getJSON('https://api.shodan.io/shodan/host/' + String(this.ip) + '?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW');
    },
  }
};
</script>

<style scoped lang="scss">

</style>
