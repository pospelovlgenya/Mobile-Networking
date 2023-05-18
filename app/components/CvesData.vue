<template>
	<Page class="page">
		<ActionBar class="ab">
			<Label text="Cves Info" class="header" />
		</ActionBar>
		
		<GridLayout rows="*, auto" columns="*">
			<ScrollView row="0" v-if="show.length" >
				<FlexboxLayout flexDirection="column" >
					<GridLayout rows="auto, auto" columns="*" v-for="item in show" :key="item.name" class="datablock">
						<Label row="0" :text="item.name + ':'" textWrap="true" />
						<Label row="1" :text="item.summary" textWrap="true" />
					</GridLayout>
				</FlexboxLayout>
			</ScrollView>
			
			<Label row="0" text="Cves не найдены" textWrap="true" v-else/>

			<FlexboxLayout row="1" flexDirection="column">
				<GridLayout rows="auto" columns="*, *, *">
					<Image col="0" src="~/assets/Search.png" height="50" width="33%" stretch="aspectFit" class="button" @tap="ToPage(0)" />
					<Image col="1" src="~/assets/Info.png" height="50" width="33%" stretch="aspectFit" class="button" @tap="ToPage(1)" />
					<Image col="2" src="~/assets/Bug.png" height="50" width="33%" stretch="aspectFit" class="button" @tap="ToPage(2)" />
				</GridLayout>
			</FlexboxLayout>
		</GridLayout>
	</Page>
</template>

<script>
import * as ApplicationSettings from '@nativescript/core/application-settings';
import Home from "./Home.vue"
import Main from "./MainData.vue"
import Cves from "./CvesData.vue"
export default {
	data () {
		return {
			Pages: [Home, Main, Cves],
			show: [],
			req: {},
		};
	},
	beforeMount() {
		if (ApplicationSettings.getString("req")) {
			this.req = JSON.parse(ApplicationSettings.getString("req"));
		}
		if (this.req['vulns']) {
			var cves = this.req['data'][0]['vulns'];
			if (cves) {
				var keys_cves = Object.keys(cves);
				for (var j = 0; j < keys_cves.length; j++) {
					this.show.push({name: keys_cves[j], summary: cves[keys_cves[j]]['summary']});
				}
			}
		}
	},
	methods: {
		ToPage(id) {
			this.$navigateTo(this.Pages[id]);
		}
	}
};
</script>

<style scoped lang="scss">

</style>
