<template>
  <Page class="page">
    <ActionBar class="ab">
      <Label text="Networking" class="header" />
    </ActionBar>       
     
    <GridLayout rows="*, auto, auto, *, auto, *" columns="*">
      <ActivityIndicator row="0" :busy="loadData" />

      <TextField row="1" v-model="ip" class="textbar" />
      <Label row="2" text="Find this ip" @tap="find()" class="button"/>

			<FlexboxLayout row="4" flexDirection="column">
        <Label text="Search history:" textWrap="true" width="50%" class="datablock" />
				<Label :text="item" textWrap="true" width="50%" itemWidth="50%" v-for="(item, key) in history" @key="key" @tap="findhist(key)" class="button" />
			</FlexboxLayout>
    </GridLayout>
    
  </Page>
</template>

<script>
import * as ApplicationSettings from '@nativescript/core/application-settings';
import Main from "./MainData.vue"
export default {
  data() {
    return {
      ip: '',
      history: [],
      show: {},
      loadData: false,
      regex: /^((25[0-5]|(2[0-4]|1\d|[1-9]|)\d)\.?\b){4}$/,
    };
  },
  beforeMount() {
    if (ApplicationSettings.getString("history")) {
      this.history = ApplicationSettings.getString("history").split(',') ;
    }
  },
  methods: {
    async find() {
      if (!(this.regex.test(this.ip))){
        return;
      }
      this.loadData = true;
      const response = await fetch("https://api.shodan.io/shodan/host/" + this.ip + "?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      const req = await response.json();
      if (!(this.history.includes(this.ip))) {
        if (this.history.length < 5) {
          this.history.push(this.ip);
        }
        else {
          this.history.splice(0, 1);
          this.history.push(this.ip);
        }
      }
      await this.save(req);
      this.ToPage();
    },
    async findhist(id) {
      this.loadData = true;
      const response = await fetch("https://api.shodan.io/shodan/host/" + this.history[id] + "?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      const req = await response.json();
      this.show = req;
      await this.save(req);
      this.ToPage();
    },
    async save(req) {
      ApplicationSettings.setString("history", this.history.toString());
      ApplicationSettings.setString("req", JSON.stringify(Object.assign({}, req)));
    },
    ToPage() {
      this.$navigateTo(Main);
    }
  },
};
</script>

<style scoped lang="scss">

</style>
