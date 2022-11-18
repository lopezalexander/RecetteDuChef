<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="ion-color-dark">
        <!-- BOUTTON DE RETOUR -->
        <ion-buttons slot="start">
          <ion-back-button @click="this.router.back()"></ion-back-button>
        </ion-buttons>

        <!-- TITRE DE CATEGORIE -->
        <ion-title class="labelColor">{{ this.nomCategorie }}</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div id="ion-text-center">
        <!-- LISTE RECETTES -->
        <ion-item v-for="(item, index) in this.maListe" :key="index" :value="item.id">
          <!-- IMAGE RECETTE -->
          <ion-avatar class="imageRecette">
            <ion-img :src="item.image"></ion-img>
          </ion-avatar>

          <!-- NOM RECETTE -->
          <ion-button class="labelNomRecette" @click="() => router.push('/DetailsRecette/' + item.id)">
            <ion-label class="nomRecette"> {{ item.nom }}</ion-label>
          </ion-button>
        </ion-item>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButton,
  IonButtons,
  IonBackButton,
  IonImg,
  IonLabel,
  IonItem,
  IonAvatar,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { useRoute, useRouter } from "vue-router";

export default defineComponent({
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    IonBackButton,
    IonImg,
    IonLabel,
    IonItem,
    IonButtons,
    IonAvatar,
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const categorie = route.params.categorie;
    return { route, router, categorie };
  },
  data() {
    return {
      nomCategorie: "",
      maListe: [],
    };
  },

  ionViewDidEnter() {
    this.getCategory();
  },

  methods: {
    async getCategory() {
      let url = `https://www.themealdb.com/api/json/v1/1/filter.php?c=` + this.categorie;
      this.nomCategorie = this.categorie.toString().toUpperCase();

      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          for (let i = 0; i < data.meals.length; i++) {
            this.maListe.push({
              id: data.meals[i].idMeal,
              nom: data.meals[i].strMeal,
              image: data.meals[i].strMealThumb,
            });
          }
        });
    },
  },
});
</script>

<style scoped>
.labelNomRecette {
  --background: black !important;
  --color: var(--ion-color-primary-shade);

  border-radius: 20px;
  overflow: hidden;
  margin-left: 10px;
  height: 50px;
}

.itemRecette {
}

.imageRecette {
  width: 75px;
  height: 75px;
}

.nomRecette {
  font-size: 20px;

  padding-left: 10px;
  padding-right: 10px;
}

.labelColor {
  color: turquoise;
}
</style>
