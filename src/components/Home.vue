<template>
  <Page>
    <ActionBar title="Home" />
    <StackLayout class="home">
      <Button @tap="$router.push('/counter')" text="Counter" />
      <Button @tap="$router.push('/hello')" text="Hello" />
      <StackLayout >
        <Label text="Photo Test"></Label>
        <Image  :src="imageSrc"  style="width:300;height:300"/>
        <Button @tap="takePic()" text="TAKE PIC" />
      </StackLayout>
    </StackLayout>

  </Page>
</template>
<script>

import { Image } from "tns-core-modules/ui/image";
import * as camera from "nativescript-camera";

export default {
  data() {
    return {
      surprise: false,
      imageSrc : null
    };
  },
  methods: {
    takePic: function() {
      // TEST
      camera.requestPermissions();
      camera
        .takePicture({width: 700, height: 700, keepAspectRatio: true})
        .then(
          imageAsset => {
            console.log("Result is an image asset instance");
            this.imageSrc = imageAsset;
          },
          error => console.log(error)
        )
        .catch(err => {
          console.log("Error -> " + err.message);
        });
    }
  }
};
</script>