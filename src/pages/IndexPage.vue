<template>
  <q-page class="flex flex-center">
    <div class="boxheader">
      <div class="row justify-between" style="padding: 5px">
        <div>
          <q-btn
            color="primary"
            icon="eva-person-outline"
            label="Login"
            flat
            rounded
            @click="LoginView = true"
          />
        </div>
        <q-space />
        <div>
          <q-btn-dropdown flat color="primary" label="Menu" rounded>
            <div class="row no-wrap q-pa-md">
              <div class="column">
                <div class="text-h6 q-mb-md">Settings</div>
                <q-btn
                  color="primary"
                  icon="eva-shopping-bag-outline"
                  label="Add New Product"
                  @click="AddProduct = true"
                  :disable="LoginStatus"
                  flat
                  rounded
                />
              </div>
            </div>
          </q-btn-dropdown>
        </div>
      </div>
    </div>
    <div
      style="height: 100%; width: 100%; margin: 10px; background-color: #f4f4f4"
      class="flex flex-center"
    >
      <q-carousel
        v-model="slide"
        autoplay
        swipeable
        animated
        control-color="indigo"
        navigation
        padding
        arrows
        class="text-indigo rounded-borders"
        style="height: 100%; width: 100%; background-color: #f4f4f4"
      >
        <q-carousel-slide name="s22" class="column no-wrap flex-center">
          <div class="q-mt-md text-center">
            <img
              src="https://img.global.news.samsung.com/in/wp-content/uploads/2022/02/S22_Ultra_Banner_3000x2000.jpg"
              alt=""
              srcset=""
              style="width: 1000px; height: 800px"
            />
          </div>
        </q-carousel-slide>
        <q-carousel-slide name="s22u" class="column no-wrap flex-center">
          <div class="q-mt-md text-center">
            <img
              style="width: 1000px; height: 800px"
              src="https://img.global.news.samsung.com/in/wp-content/uploads/2022/02/S22_S22_Banner_3000x2000.jpg"
            />
          </div>
        </q-carousel-slide>
        <q-carousel-slide name="pixel" class="column no-wrap flex-center">
          <div class="q-mt-md text-center">
            <img
              style="width: 1000px; height: 800px"
              src="src/assets/Logo.svg"
            />
          </div>
        </q-carousel-slide>
      </q-carousel>
    </div>

    <div style="width: 100%; height: 100%; margin: 0px 50px 0px 50px">
      <q-separator />
    </div>

    <div class="flex flex-center">
      <div v-for="i in data" :key="i" class="row">
        <q-card class="my-card" style="margin: 20px">
          <img style="width: 250px" :src="i.url" />
          <q-card-section>
            <div class="text-h6">{{ i.name }}</div>
            <div class="text-subtitle2">{{ i.description }}</div>
          </q-card-section>
          <q-card-section>
            <div class="text-bold">{{ i.price }}$</div>
            <div class="row justify-end">
              <q-btn
                color="primary"
                icon="eva-edit-2-outline"
                round
                flat
                :disable="LoginStatus"
                @click="
                  update_View(i.id, i.name, i.description, i.url, i.price)
                "
              />
              <q-btn
                color="red"
                icon="eva-trash-outline"
                flat
                round
                :disable="LoginStatus"
                @click="delete_Product(i.id)"
              /></div
          ></q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
  <q-dialog v-model="AddProduct">
    <q-card style="padding: 10px">
      <q-toolbar>
        <img src="src/assets/logo.svg" style="width: 50px; height: 50px" />

        <q-toolbar-title
          ><span class="text-weight-bold">OneShop</span></q-toolbar-title
        >
        <q-btn flat round dense icon="close" v-close-popup />
      </q-toolbar>

      <q-card-section>
        <q-input label="Product Name" v-model="Pname"> </q-input>
        <q-input label="Product img " v-model="Purl"> </q-input>
        <q-input label="Product Description" v-model="Pdescription"> </q-input>
        <q-input label="Product Price" v-model="Pprice"> </q-input>
        <q-space />
      </q-card-section>
      <q-btn
        color="primary"
        icon="check"
        label="Add Product"
        rounded
        flat
        @click="add_Product"
      />
      <q-space />
    </q-card>
  </q-dialog>
  <q-dialog v-model="UpdateProduct">
    <q-card style="padding: 10px">
      <q-toolbar>
        <img src="src/assets/logo.svg" style="width: 50px; height: 50px" />

        <q-toolbar-title
          ><span class="text-weight-bold">OneShop</span></q-toolbar-title
        >

        <q-btn flat round dense icon="close" v-close-popup />
      </q-toolbar>

      <q-card-section>
        <q-input label="Product Name" v-model="Pname"> </q-input>
        <q-input label="Product img " v-model="Purl"> </q-input>
        <q-input label="Product Description" v-model="Pdescription"> </q-input>
        <q-input label="Product Price" v-model="Pprice"> </q-input>
        <q-space />
      </q-card-section>
      <q-btn
        color="primary"
        icon="check"
        label="Update Product"
        rounded
        flat
        @click="update_Product(this.Pid)"
      />
      <q-space />
    </q-card>
  </q-dialog>
  <q-dialog v-model="LoginView">
    <q-card style="padding: 10px">
      <q-toolbar>
        <img src="src/assets/logo.svg" style="width: 50px; height: 50px" />

        <q-toolbar-title
          ><span class="text-weight-bold">OneShop</span></q-toolbar-title
        >

        <q-btn flat round dense icon="close" v-close-popup />
      </q-toolbar>

      <q-card-section>
        <q-input label="User Name" v-model="User"> </q-input>
        <q-input label="Password " v-model="Password"> </q-input>
        <q-space />
      </q-card-section>
      <q-btn
        color="primary"
        icon="check"
        label="Login"
        rounded
        flat
        @click="Login(this.User, this.Password)"
      />
      <q-space />
    </q-card>
  </q-dialog>
</template>

<script>
import { api } from "boot/axios";

export default {
  data() {
    return {
      Pname: null,
      Pdescription: null,
      Purl: null,
      Pprice: null,
      data: null,
      slide: "s22",
      AddProduct: false,
      UpdateProduct: false,
      LoginView: false,
      User: null,
      Password: null,
      LoginStatus: true,
    };
  },

  methods: {
    add_Product() {
      var data = {};
      const params = new URLSearchParams({
        Name: this.Pname,
        Description: this.Pdescription,
        Url: this.Purl,
        Price: this.Pprice,
      }).toString();
      var url = "OneShopProject_war_exploded/api/products/add?" + params;
      api
        .post(url, data, {
          headers: {
            "Content-Type": "application/json",
          },
        })
        .then((response) => {
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
      console.log(this.Pname, this.Pdescription, this.Purl, this.Pprice);
      this.Pid = "";
      this.Pname = "";
      this.Pdescription = "";
      this.Purl = "";
      this.Pprice = "";

      // location.reload();
    },
    delete_Product(IdProduct) {
      console.log(IdProduct);
      api
        .delete("OneShopProject_war_exploded/api/products/delete/", {
          id: IdProduct,

          headers: {
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Methods":
              "GET, POST, PATCH, PUT, DELETE, OPTIONS",
            "Access-Control-Allow-Headers":
              "Origin, Content-Type, X-Auth-Token",
          },
        })
        .then((response) => {
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
      location.reload();
    },
    update_View(ProductId, Name, Description, url, Price) {
      this.UpdateProduct = true;
      this.Pname = Name;
      this.Pdescription = Description;
      this.Pprice = Price;
      this.Purl = url;
      this.Pid = ProductId;
      console.log(ProductId, Name, Description, url, Price);
    },
    update_Product(ProductId) {
      api
        .put("OneShopProject_war_exploded/api/products/update/", {
          id: ProductId,
          Name: this.Pname,
          Description: this.Pdescription,
          Url: this.Purl,
          Price: this.Pprice,
        })
        .then((response) => {
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });

      console.log(
        ProductId,
        this.Pname,
        this.Pdescription,
        this.Purl,
        this.Pprice
      );
      // location.reload();
    },
    Login(user, pass) {
      this.LoginStatus = false;
    },
  },
  mounted() {
    api
      .get("OneShopProject_war_exploded/api/products/")
      .then((response) => {
        console.log(response.data);
        this.data = response.data;
      })
      .catch((e) => {
        console.log(e);
      });
  },
};
</script>
