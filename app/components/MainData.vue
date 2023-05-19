<template>
	<Page class="page">
		<ActionBar class="ab">
			<Label text="Main Info" class="header" />
		</ActionBar>

		<GridLayout rows="*, auto" columns="*">
			
			<ScrollView row="0" v-if="Object.keys(req) != 'error'">
				<FlexboxLayout flexDirection="column">
					<GridLayout rows="auto, auto" columns="*" v-for="item in need" :key="item.key" class="datablock" v-if="item.value && item.value.length">
						<Label row="0" :text="item.name + ':'" textWrap="true" />
						<Label row="1" :text="item.value" textWrap="true" />
					</GridLayout>
				</FlexboxLayout>
			</ScrollView>

			<Label row="0" text="ip не найден" textWrap="true" v-else />

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
	data() {
		return {
			Pages: [Home, Main, Cves],
			need: [
				{ key: 'ip_str', name: 'ip', value: undefined },
				{ key: 'last_update', name: 'Дата сбора сведений', value: undefined },
				{ key: 'ports', name: 'Открытые порты', value: undefined },
				{ key: 'domains', name: 'Доменные имена', value: undefined },
				{ key: 'hostnames', name: 'Хосты', value: undefined },
				{ key: 'country_name', name: 'Страна', value: undefined },
				{ key: 'city', name: 'Город', value: undefined },
			],
			req: {},
		};
	},
	beforeMount() {
		if (ApplicationSettings.getString("req")) {
			this.req = JSON.parse(ApplicationSettings.getString("req"));
			if (this.req['error'] == undefined) {
				for (var i = 0; i < this.need.length; i++) {
					switch (this.need[i].key) {
						case 'last_update':
							this.need[i].value = new Date(Date.parse(this.req[this.need[i].key]));
							break;

						case 'ports':
							this.ToStr(i);
							break;

						case 'domains':
							this.ToStr(i);
							break;

						case 'hostnames':
							this.ToStr(i)
							break;

						default:
							this.need[i].value = this.req[this.need[i].key];
							break;
					}
				}
			}
		}
	},
	methods: {
		ToStr(i) {
			this.need[i].value = '';
			for (var j = 0; j < this.req[this.need[i].key].length; j++) {
				this.need[i].value += this.req[this.need[i].key][j] + ', ';
			}
			this.need[i].value = this.need[i].value.substring(0, this.need[i].value.length - 2);
		},
		ToPage(id) {
			this.$navigateTo(this.Pages[id]);
		},
	}
};
</script>

<style scoped lang="scss"></style>
