<template>
  <Page>
    <ActionBar title="Counter" />
    <StackLayout>
      <Label :text="message" textWrap="true" />
      <WrapLayout>
        <Button @tap="decrement" text="-" />
        <Button @tap="increment" text="+" />
      </WrapLayout>
      <Image v-if="surprise" src="~/images/NativeScript-Vue.png" />
      <Button @tap="$router.push('/home')">Go home</Button>
    </StackLayout>
  </Page>
</template>

<script>
import { mapActions, mapState } from "vuex";

export default {
  computed: {
    // message () {
    //   return `Counter: ${this.$store.state.counter.count}`;
    // },
    // surprise () {
    //   return (this.$store.state.counter.count >= 5);
    // },
    ...mapState({
      message: state => `Counter: ${state.counter.count}`,
      surprise: state => state.counter.count >= 5
    })
  },
  methods: mapActions(["decrement", "increment"])
};
</script>

<style scoped>
Button {
  background-color: #42b883;
  width: 50%;
}
</style>