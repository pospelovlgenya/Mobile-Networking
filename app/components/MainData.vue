<template>
    <Page class="page">
        <ActionBar class="ab">
          <Label text="Main Info" class="header" />
        </ActionBar>
        <FlexboxLayout flexDirection="column" >
            <!-- <Label :text="req" textWrap="true" /> -->
            <Label :text="item + ': ' + req[item]" textWrap="true" v-for="item in Object.keys(req)" />
        </FlexboxLayout>
    </Page>
        
</template>
    
<script>
export default {
  props: ['req'],
  data () {
    return {};
  },
  methods: {
  }
};
</script>
    
<style scoped lang="scss">

</style>
