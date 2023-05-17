<template>
	<Page class="page">
		<ActionBar class="ab">
			<Label text="Cves Info" class="header" />
		</ActionBar>
		<GridLayout rows="*, auto" columns="*">

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
			req: [],
		};
	},
	beforeMount() {
		if (ApplicationSettings.getString("req")) {
			this.req = JSON.parse(ApplicationSettings.getString("req"));
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
