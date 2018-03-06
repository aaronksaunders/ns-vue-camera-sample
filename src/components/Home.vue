<template>
  <Page>
    <ActionBar title="Home" />
    <StackLayout class="home">
      <Button @tap="$router.push('/counter')" text="Counter" />
      <Button @tap="$router.push('/hello')" text="Hello" />
      <StackLayout>
        <Label text="Photo Test"></Label>
        <Image :src="imageSrc" style="width:200;height:200" />
        <Button @tap="takePic()" text="TAKE PIC" />
        <Button @tap="login()" text="FB Test Login" />
        <Button @tap="getData()" text="FB Test GET DATA" />
        <button @tap="showToast()" text="Test Toast Plugin"></button>
      </StackLayout>
    </StackLayout>

  </Page>
</template>
<script>
// require("nativescript-nodeify");
import { Image } from "tns-core-modules/ui/image";
const Toast = require("nativescript-toast");
import * as camera from "nativescript-camera";
import axios from "axios";

export default {
  data() {
    return {
      surprise: false,
      imageSrc: null,
      authToken: null
    };
  },
  methods: {
    showToast() {
      const myToast = Toast.makeText("MEU VUEJS!!!", "long");
      myToast.show();
    },
    takePic: function() {
      // TEST
      camera.requestPermissions();
      camera
        .takePicture({ width: 700, height: 700, keepAspectRatio: true })
        .then(
          imageAsset => {
            console.log("Result is an image asset instance");
            this.imageSrc = imageAsset;

            uploadFile(imageAsset);
          },
          error => console.log(error)
        )
        .catch(err => {
          console.log("Error -> " + err.message);
        });
    },
    uploadFile() {
      // Initialize Firebase
      var config = {
        apiKey: "AIzaSyBjtl81OTTPAZWp92KFB01qsou39vEC2VA",
        authDomain: "nsfbapp.firebaseapp.com",
        databaseURL: "https://nsfbapp.firebaseio.com",
        projectId: "nsfbapp",
        storageBucket: "nsfbapp.appspot.com",
        messagingSenderId: "810901469042"
      };
      firebase.initializeApp(config);

      var storage = firebase.storage();

      var ref = firebase
        .storage()
        .ref()
        .child("message-1");
      var message = "This is my message.";
      ref.putString(message).then(function(snapshot) {
        console.log("Uploaded a raw string!");
      });
    },
    /**
     *
     */
    getData() {
      var instance = axios.create({
        baseURL: "https://nsfbapp.firebaseio.com",
        params: {
          auth: this.authToken
        },
        timeout: 1000,
        headers: { "Content-Type": "application/json" }
      });
      console.log("idToken", this.authToken);
      instance
        .get("/stuff.json")
        .then(
          stuff => {
            console.log("/stuff.json", stuff.data);
          },
          error => {
            console.log(error);
          }
        )
        .catch(() => {
          console.log(error);
        });
    },
    login() {
      let API_KEY = "AIzaSyBjtl81OTTPAZWp92KFB01qsou39vEC2VA";
      let url = "https://www.googleapis.com/identitytoolkit/v3/relyingparty";

      var authInstance = axios.create({
        baseURL: url,
        timeout: 1000,
        params: {
          key: API_KEY
        },
        headers: { "Content-Type": "application/json" }
      });

      authInstance
        .post(`/verifyPassword`, {
          email: "a@mail.com",
          password: "password",
          returnSecureToken: true
        })
        .then(
          stuff => {
            console.log("verifyPassword", JSON.stringify(stuff.data));
            this.authToken = stuff.data.idToken;
          },
          error => {
            console.log(error);
          }
        )
        .catch(() => {
          console.log(error);
        });
    }
  }
};
</script>