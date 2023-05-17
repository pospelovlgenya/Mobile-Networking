<template>
  <Page class="page">
    <ActionBar class="ab">
      <Label text="Networking" class="header" />
    </ActionBar>       
     
    <GridLayout rows="*, auto, auto, *, auto, *" columns="*">
      <TextField row="1" hint="ipv4 adress input" v-model="ip" />
      <Button row="2" text="Find this ip" @tap="find()" />

			<FlexboxLayout row="4" flexDirection="column">
				<Label :text="item.ip" textWrap="true" v-for="(item, key) in history" @key="key" @tap="findhist(key)"/>
			</FlexboxLayout>
    </GridLayout>
    
  </Page>
</template>

<script>
import * as ApplicationSettings from '@nativescript/core/application-settings';
import Main from "./MainData.vue"
export default {
  data () {
    return {
      ip: '5.141.28.183',
			history: [],
    };
  },
	beforeMount() {
      if (ApplicationSettings.getString("history")) {
        this.history = Object.values(JSON.parse(ApplicationSettings.getString("history")));
      }
		},
  methods: {
    async find() {
      const response = await fetch("https://api.shodan.io/shodan/host/" + this.ip + "?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      const req = await response.json();
			if (this.history.length < 5) {
				this.history.push({ip: this.ip});
			}
			else {
				this.history.splice(0, 1);
				this.history.push({ip: this.ip});
			}
			this.save(req);
      this.ToPage();
    },
		async findhist(id) {
			const response = await fetch("https://api.shodan.io/shodan/host/" + String(this.history[id].ip) + "?key=X9bo1JfYKqcrKIzslhW90OzH3VagvbTW");
      const req = await response.json();
			this.save(req);
      this.ToPage();
		},
		save(req) {
			ApplicationSettings.setString("history", JSON.stringify(Object.assign({}, this.history)));
			ApplicationSettings.setString("req", JSON.stringify(Object.assign({}, req)));
		},
		ToPage() {
			this.$navigateTo(Main);
		}
  }
};
</script>

<style scoped lang="scss">

</style>
